# Quick Start: Xây dựng Local MVP trong 2 tuần

**Ví dụ**: E-Commerce Analytics Pipeline (Recommended project)

---

## 📋 Tuần 1: Setup & Ingest Data

### Ngày 1-2: Environment Setup

#### 1.1 Install Docker
```bash
# Windows: Download Docker Desktop
# https://www.docker.com/products/docker-desktop

# Verify
docker --version
```

#### 1.2 Create Project Structure
```bash
mkdir ecommerce-analytics
cd ecommerce-analytics

mkdir -p {data/raw,data/processed,scripts,notebooks,dashboards}
touch docker-compose.yml requirements.txt .env
```

#### 1.3 Docker Compose Setup
**File: `docker-compose.yml`**
```yaml
version: '3.8'
services:
  # PostgreSQL - Database
  postgres:
    image: postgres:15
    container_name: ecom-db
    environment:
      POSTGRES_USER: dataeng
      POSTGRES_PASSWORD: password123
      POSTGRES_DB: ecommerce
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./scripts/init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - ecom-network

  # Kafka - Message Queue
  kafka:
    image: confluentinc/cp-kafka:7.5.0
    container_name: ecom-kafka
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:29092,PLAINTEXT_HOST://localhost:9092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
    depends_on:
      - zookeeper
    networks:
      - ecom-network

  zookeeper:
    image: confluentinc/cp-zookeeper:7.5.0
    container_name: ecom-zookeeper
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
    networks:
      - ecom-network

  # Spark - Processing
  spark:
    image: bitnami/spark:3.3.1
    container_name: ecom-spark
    ports:
      - "8080:8080"
      - "4040:4040"
      - "7077:7077"
    environment:
      SPARK_MODE: master
      SPARK_RPC_AUTHENTICATION_ENABLED: no
    volumes:
      - ./scripts:/opt/spark/scripts
      - ./data:/opt/spark/data
    networks:
      - ecom-network

  # Metabase - Visualization
  metabase:
    image: metabase/metabase:latest
    container_name: ecom-metabase
    ports:
      - "3000:3000"
    environment:
      MB_DB_TYPE: postgres
      MB_DB_DBNAME: metabase
      MB_DB_HOST: postgres
      MB_DB_PORT: 5432
      MB_DB_USER: dataeng
      MB_DB_PASS: password123
    depends_on:
      - postgres
    networks:
      - ecom-network

volumes:
  postgres_data:

networks:
  ecom-network:
    driver: bridge
```

**Chạy**:
```bash
docker-compose up -d

# Verify
docker ps  # Xem tất cả container chạy
```

### Ngày 3-5: Scrape Data

#### 2.1 Install Python Dependencies
**File: `requirements.txt`**
```
requests==2.31.0
beautifulsoup4==4.12.0
selenium==4.13.0
pandas==2.1.0
pyspark==3.5.0
kafka-python==2.0.2
psycopg2-binary==2.9.9
python-dotenv==1.0.0
```

```bash
python -m pip install -r requirements.txt
```

#### 2.2 Web Scraper
**File: `scripts/01_scraper.py`**
```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import json
from datetime import datetime

class ShopeeeScraper:
    """Scrape product từ Shopee"""
    
    def __init__(self, category_url):
        self.category_url = category_url
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        }
    
    def scrape_products(self, page=1):
        """Scrape sản phẩm từ 1 page"""
        params = {
            'page': page,
            'limit': 50,
        }
        
        try:
            response = requests.get(self.category_url, headers=self.headers, params=params)
            response.raise_for_status()
            
            # Parse JSON (Shopee trả JSON API)
            data = response.json()
            
            products = []
            for item in data.get('items', []):
                products.append({
                    'product_id': item['item_basic']['itemid'],
                    'shop_id': item['item_basic']['shopid'],
                    'name': item['item_basic']['name'],
                    'price': item['item_basic']['price'] / 100000,  # Shopee lưu price*100000
                    'historical_sold': item['item_basic']['sold'],
                    'rating': item['item_basic'].get('rating', {}).get('rating_star', 0),
                    'scraped_at': datetime.now().isoformat(),
                })
            
            return products
        
        except Exception as e:
            print(f"Error scraping page {page}: {str(e)}")
            return []
    
    def scrape_all_pages(self, max_pages=3):
        """Scrape multiple pages"""
        all_products = []
        for page in range(max_pages):
            print(f"Scraping page {page + 1}...")
            products = self.scrape_products(page=page)
            all_products.extend(products)
        
        return all_products

if __name__ == "__main__":
    # Example: Scrape Shopee Electronics category
    scraper = ShopeeeScraper(
        category_url="https://shopee.vn/api/v4/search/search_items",
        # Lưu ý: Shopee API có rate limiting, nên scrape chậm
    )
    
    products = scraper.scrape_all_pages(max_pages=2)
    
    # Save to CSV (để test)
    df = pd.DataFrame(products)
    df.to_csv('data/raw/shopee_products.csv', index=False)
    print(f"Scraped {len(df)} products")
    print(df.head())
```

#### 2.3 Database Schema
**File: `scripts/init.sql`**
```sql
CREATE TABLE IF NOT EXISTS products (
    id SERIAL PRIMARY KEY,
    product_id BIGINT UNIQUE NOT NULL,
    shop_id BIGINT NOT NULL,
    name VARCHAR(500) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    historical_sold INT,
    rating DECIMAL(3, 2),
    scraped_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS daily_stats (
    id SERIAL PRIMARY KEY,
    date DATE NOT NULL,
    avg_price DECIMAL(10, 2),
    total_products INT,
    top_category VARCHAR(200),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_product_id ON products(product_id);
CREATE INDEX idx_shop_id ON products(shop_id);
CREATE INDEX idx_scraped_at ON products(scraped_at);
```

### Ngày 6-7: Load to Database

**File: `scripts/02_load_to_db.py`**
```python
import pandas as pd
import psycopg2
from psycopg2.extras import execute_batch
import os
from dotenv import load_dotenv

load_dotenv()

def load_to_postgres(csv_file):
    """Load CSV data to PostgreSQL"""
    
    conn = psycopg2.connect(
        host="localhost",
        database="ecommerce",
        user="dataeng",
        password="password123"
    )
    
    cursor = conn.cursor()
    
    # Read CSV
    df = pd.read_csv(csv_file)
    
    # Prepare batch insert
    values = [
        (
            row['product_id'],
            row['shop_id'],
            row['name'],
            row['price'],
            row['historical_sold'],
            row['rating'],
            row['scraped_at']
        )
        for _, row in df.iterrows()
    ]
    
    # Insert
    query = """
        INSERT INTO products (product_id, shop_id, name, price, historical_sold, rating, scraped_at)
        VALUES (%s, %s, %s, %s, %s, %s, %s)
        ON CONFLICT (product_id) DO UPDATE SET
            price = EXCLUDED.price,
            historical_sold = EXCLUDED.historical_sold,
            updated_at = CURRENT_TIMESTAMP
    """
    
    execute_batch(cursor, query, values, page_size=1000)
    conn.commit()
    
    print(f"Loaded {len(df)} products to database")
    cursor.close()
    conn.close()

if __name__ == "__main__":
    load_to_postgres('data/raw/shopee_products.csv')
```

**Chạy**:
```bash
python scripts/02_load_to_db.py
```

---

## 📊 Tuần 2: Processing & Visualization

### Ngày 8-10: Spark Processing

**File: `scripts/03_spark_transform.py`**
```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, avg, count, max, min, to_date, window
from datetime import datetime

spark = SparkSession.builder \
    .appName("EcommerceAnalytics") \
    .config("spark.sql.shuffle.partitions", 10) \
    .getOrCreate()

# Read from PostgreSQL
df = spark.read \
    .format("jdbc") \
    .option("url", "jdbc:postgresql://postgres:5432/ecommerce") \
    .option("dbtable", "products") \
    .option("user", "dataeng") \
    .option("password", "password123") \
    .load()

# Transformation 1: Daily stats
daily_stats = df \
    .groupBy(to_date(col("scraped_at")).alias("date")) \
    .agg(
        avg("price").alias("avg_price"),
        count("product_id").alias("total_products"),
        max("rating").alias("max_rating")
    )

daily_stats.show()

# Transformation 2: Top products
top_products = df \
    .select("name", "price", "historical_sold", "rating") \
    .filter(col("rating") > 4.5) \
    .orderBy(col("historical_sold").desc()) \
    .limit(10)

top_products.show()

# Save results
daily_stats.write \
    .mode("overwrite") \
    .parquet("data/processed/daily_stats")

top_products.write \
    .mode("overwrite") \
    .parquet("data/processed/top_products")

print("Spark transformations completed!")
```

**Chạy**:
```bash
docker-compose exec spark \
  spark-submit \
  --packages org.postgresql:postgresql:42.6.0 \
  /opt/spark/scripts/03_spark_transform.py
```

### Ngày 11-12: Dashboard

#### Tạo dashboard qua Metabase UI
```
1. Truy cập http://localhost:3000
2. Setup account
3. Connect PostgreSQL database
4. Create queries:
   - SELECT avg(price), COUNT(*) FROM products GROUP BY DATE_TRUNC('day', scraped_at)
   - SELECT name, price, historical_sold FROM products ORDER BY historical_sold DESC LIMIT 10
5. Visualize as charts
```

#### Hoặc: Streamlit Dashboard
**File: `dashboards/app.py`**
```python
import streamlit as st
import pandas as pd
import psycopg2

st.set_page_config(page_title="E-commerce Analytics", layout="wide")

@st.cache_resource
def get_db_connection():
    return psycopg2.connect(
        host="localhost",
        database="ecommerce",
        user="dataeng",
        password="password123"
    )

conn = get_db_connection()

st.title("🛒 E-Commerce Analytics Dashboard")

# Top metrics
col1, col2, col3 = st.columns(3)
with col1:
    total_products = pd.read_sql("SELECT COUNT(*) FROM products", conn).iloc[0, 0]
    st.metric("Total Products", total_products)

with col2:
    avg_price = pd.read_sql("SELECT AVG(price) FROM products", conn).iloc[0, 0]
    st.metric("Avg Price", f"₫{avg_price:,.0f}")

with col3:
    avg_rating = pd.read_sql("SELECT AVG(rating) FROM products", conn).iloc[0, 0]
    st.metric("Avg Rating", f"{avg_rating:.2f}★")

# Chart: Price distribution
st.subheader("Price Distribution")
df_price = pd.read_sql(
    "SELECT price FROM products WHERE price > 0 AND price < 50000000",
    conn
)
st.histogram(df_price['price'], bins=50)

# Chart: Top 10 products
st.subheader("Top 10 Best Sellers")
df_top = pd.read_sql(
    """
    SELECT name, historical_sold, price, rating 
    FROM products 
    ORDER BY historical_sold DESC 
    LIMIT 10
    """,
    conn
)
st.dataframe(df_top, use_container_width=True)

conn.close()
```

**Chạy**:
```bash
pip install streamlit
streamlit run dashboards/app.py
```

---

## ✅ Checkpoint: Tuần 2 End

Bây giờ em có:
- ✅ Data pipeline local hoàn chỉnh
- ✅ Real data từ Shopee
- ✅ Spark transformations
- ✅ Dashboard to explore

**Next**: Extend tính năng (real-time alerts, predictions, v.v.)

---

## 🚀 Tiếp theo (Tuần 3-4)

### Feature 1: Real-time Ingestion (Kafka)
Thay vì scrape 1 lần, make it continuous:
```python
# Producer: Scrape & send to Kafka every hour
# Consumer: Read from Kafka, process, write to DB
```

### Feature 2: Price Alerts
```python
# Nếu sản phẩm X giá giảm >20%, gửi email alert
```

### Feature 3: Prediction Model
```python
# Dự đoán price trend: ARIMA model
```

### Feature 4: Deploy to AWS (Phase 2)
```
Local Spark job → AWS EMR
Local Postgres → AWS RDS
Local dashboard → EC2 + Streamlit
```

---

## 📚 Resources
- Shopee API docs: https://open.shopee.vn/
- Spark SQL: https://spark.apache.org/docs/latest/sql-programming-guide.html
- PostgreSQL + Docker: https://hub.docker.com/_/postgres
- Streamlit: https://docs.streamlit.io/

---

## 🎯 Troubleshooting

### Docker container không start
```bash
docker-compose logs postgres  # Xem error log
docker-compose down           # Reset
docker-compose up -d --build  # Rebuild
```

### Spark job timeout
```bash
# Tăng timeout
spark.sql.execution.arrow.maxRecordsPerBatch 100
spark.executor.heartbeatInterval 60s
```

### PostgreSQL connection refused
```bash
# Wait for postgres startup
sleep 5  # trước khi chạy Python script
```

Chúc em thành công! 🚀
