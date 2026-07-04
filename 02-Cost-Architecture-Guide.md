# Chi Phí & Architecture: Full Cloud vs Hybrid vs Local

## 🎯 Tóm tắt nhanh
**Khuyến nghị cho sinh viên**: **Hybrid - Local MVP + AWS optional**
- Bắt đầu local (máy/Docker) → hoàn toàn free
- Extend lên AWS chỉ khi cần scale (1-2 tháng sau)
- Tiết kiệm 80-90% chi phí vs full cloud từ ngày 1

---

## Option 1️⃣: FULL LOCAL (0$ / tháng)

### Công nghệ
```
Kafka/RabbitMQ → local Docker
Spark → local (standalone mode)
PostgreSQL → local Docker
Metabase/Superset → local Docker
```

### Chi phí: **$0**
- Máy tính em đã có
- Tất cả tools open-source
- Internet + điện = coi như có rồi

### Ưu điểm ✅
- **Cost**: Hoàn toàn free
- **Control**: Full control trên data
- **Privacy**: Không lo data bị leak
- **Learning**: Hiểu rõ mọi component

### Nhược điểm ❌
- **Scalability**: Bị giới hạn RAM/CPU máy
- **Demo remote**: Khó demo cho recruiter từ xa (phải share screen)
- **Availability**: Em shut down = pipeline down
- **Resource**: Máy nóng quạt to khi chạy full pipeline 🔥

### Thích hợp cho
- MVP, learning, development environment
- Nếu máy em khỏe (≥8GB RAM, SSD)

---

## Option 2️⃣: FULL CLOUD (AWS) ($50-200+/tháng) 🚨

### Công nghệ
```
S3 → Data Lake
AWS Glue / Lambda → Ingestion & Processing
Redshift → Data Warehouse
QuickSight / Looker → Visualization
RDS → Metadata store
```

### Chi phí tính toán (E-Commerce project)
```
S3:           $5-10/tháng     (1-10GB data)
Lambda:       $10-20/tháng    (ingestion jobs)
Glue:         $15-30/tháng    (processing)
Redshift:     $20-50/tháng    (smallest node RA3.xlplus)
QuickSight:   $10-15/tháng    (basic tier)
Data transfer: $5-15/tháng    (internet charges)
────────────────────────────────
Total:        $65-140+/tháng
```

### Ưu điểm ✅
- **Scalability**: Can handle massive data
- **Production-ready**: Giống công ty thật
- **24/7 available**: Always on, em sleep vẫn running
- **Share link**: Demo dễ - send URL to recruiter

### Nhược điểm ❌
- **Chi phí cao**: $65-140/tháng là lớn cho sinh viên
- **Complexity**: Phức tạp hơn, dễ setup sai
- **Lock-in**: Kiến trúc AWS-specific
- **Learning curve**: Phải học AWS services

### Thích hợp cho
- Nếu em có học bổng AWS / budget
- Nếu muốn thực sự production-grade (ít)

---

## Option 3️⃣: HYBRID (Recommended) ⭐ ($5-20/tháng)

### Chiến lược
```
Phase 1 (tuần 1-4): Local development
├─ Docker Compose: Kafka, Spark, PostgreSQL
├─ Local notebook/script development
└─ Cost: $0

Phase 2 (tuần 5-8): Mini-cloud extend
├─ Spark jobs → AWS EMR (on-demand, tắt sau)
├─ Data → S3 (persistent backup)
├─ Cost: $10-15/tuần khi chạy, $0 khi off

Phase 3 (tuần 9+): Optimize
├─ S3 + RDS (managed database, cheaper than local)
├─ Lambda jobs thay Spark (serverless)
├─ Cost: $5-15/tháng steady-state
```

### Chi phí breakdown (tuần 5-8 processing week)
```
EC2 (m5.xlarge, 2-3 ngày):     $15-20
S3 (dữ liệu thô):              $2-5
RDS (optional, nếu cần):       $10-15
────────────────────────────
Active week: ~$30-40 (tắt sau)
Idle week: ~$2-5 (chỉ S3 storage)

Average/tháng: $10-15
```

### Ưu điểm ✅
- **Cost**: $10-15/tháng (affordable)
- **Flexibility**: Local dev + cloud scale
- **Demo**: Có URL để share, nhưng vẫn cheap
- **Learning**: Học cả local + AWS architecture
- **Portfolio**: "I optimized costs by using hybrid approach" → good story

### Nhược điểm ❌
- **Complexity**: Phải quản lý 2 environments
- **Data sync**: Local → Cloud cần tooling
- **Not 24/7**: Nếu em tắt EMR để tiết kiệm, pipeline run time > local

### Thích hợp cho
- **Sinh viên có budget hạn chế** (BEST CHOICE)
- Muốn học both local + cloud
- Cần demo URL nhưng khôn tiền

---

## 📊 So sánh trực tiếp

| Tiêu chí | Local | Hybrid | Full Cloud |
|---------|-------|--------|-----------|
| **Chi phí** | $0 | $10-15/tháng | $65-140/tháng |
| **Scalability** | Máy ~ 8GB RAM | Local + AWS | Unlimited |
| **Demo remote** | ❌ Screen share | ✅ URL | ✅ URL |
| **24/7 uptime** | ❌ | ~✅ | ✅ |
| **Learning value** | ⭐⭐⭐ Local deep | ⭐⭐⭐⭐ Both | ⭐⭐⭐ Cloud only |
| **Setup complexity** | ⭐ Easy | ⭐⭐⭐ Medium | ⭐⭐⭐⭐ Hard |
| **Recruit story** | "Local MVP" | "Cost-optimized hybrid" | "Production ready" |

---

## 🚀 Chi tiết Hybrid Approach (CHO E-COMMERCE PROJECT)

### Phase 1: Local Development (Tháng 1-1.5)
**Mục tiêu**: MVP hoàn toàn local, learn basics

```yaml
# docker-compose.yml mẫu
services:
  kafka:
    image: confluentinc/cp-kafka:7.x
    ports: ["9092:9092"]
    
  spark:
    image: bitnami/spark:3.x
    ports: ["8080:8080", "4040:4040"]
    
  postgres:
    image: postgres:15
    volumes: ["./data:/var/lib/postgresql/data"]
    ports: ["5432:5432"]
    
  metabase:
    image: metabase/metabase:latest
    ports: ["3000:3000"]
```

**Cost**: $0

---

### Phase 2: Cloud Experiment (Tuần 5-8, xử lý lớn)
**Mục tiêu**: Test Spark job trên cloud, backup data

```bash
# Khi cần chạy processing lớn
aws ec2 run-instances --instance-type m5.xlarge --region us-east-1
# Chạy Spark job
spark-submit --master yarn my_processing_job.py
# Upload result to S3
aws s3 sync ./output s3://my-bucket/processed/
# TẮT EC2 sau xong (QUAN TRỌNG!)
aws ec2 terminate-instances --instance-ids i-xxxxx
```

**Cost/week**: $15-20 (chỉ khi chạy)

---

### Phase 3: Optimize & Finalize (Tuần 9+)
**Mục tiêu**: AWS infra stable, local for dev

```
┌─ Local (Development)
│  ├─ Docker Compose all-in-one
│  ├─ Notebook experiments
│  └─ Cost: $0
│
├─ AWS S3 (Persistent Data)
│  ├─ Raw data lake (~5GB) = $0.12/tháng
│  ├─ Processed data = $0.24/tháng
│  └─ Cost: ~$0.50/tháng
│
└─ AWS RDS (optional, DB backup)
   ├─ db.t3.micro tier
   ├─ $6-10/tháng
   └─ Tự động backup

Total steady-state: $10-15/tháng
```

---

## 💡 Concrete Decision Tree

```
Bạn hỏi: "Em nên chọn gì?"

1. Em có máy khỏe (≥8GB RAM, SSD)? 
   → CÓ: Start LOCAL (Phase 1)
   → KHÔNG: Buy/borrow hoặc skip project này

2. Em có AWS credit (từ FCJ)?
   → CÓ: HYBRID (tận dụng credit)
   → KHÔNG: Vẫn HYBRID nhưng limited (chỉ $10-15/tháng)

3. Em muốn demo URL cho recruiter?
   → CÓ: HYBRID từ tuần 5
   → KHÔNG: Local vẫn okie (screen share)

4. Em có thời gian optimize infra?
   → CÓ: Spend time learning AWS, Phase 2-3
   → KHÔNG: Stay LOCAL, focus on data logic

Recommendation: HYBRID → $0-20/tháng, learn both worlds, impress recruiter
```

---

## 🎯 Cost-Saving Tips

### Tip 1: AWS Free Tier ✅
```
- 1 tháng free (nếu account mới)
- 1 million Lambda requests/tháng (free tier)
- 5GB S3 storage (free)
```

### Tip 2: Spot instances (50% cheaper)
```bash
# EC2 spot 70% giá rẻ hơn
aws ec2 request-spot-instances \
  --spot-price "0.05" \
  --instance-type m5.xlarge
# Risk: AWS tắt nếu price tăng, nhưng okie cho batch jobs
```

### Tip 3: Cron jobs instead of always-on
```bash
# Thay vì EC2 chạy 24/7
# Dùng AWS Glue scheduled jobs (pay-per-job)
# Tiết kiệm 90%
```

### Tip 4: Compress data
```bash
# Parquet vs CSV: 80% smaller = 80% cheaper S3
df.to_parquet('output.parquet')  # not .to_csv()
```

### Tip 5: Set up billing alerts
```
AWS Console → Billing → Create alert khi $20/tháng
→ Kill job nếu unexpected cost
```

---

## 🎓 Kết luận

**Recommended path cho sinh viên**:
```
Tuần 1-4:   LOCAL (Docker Compose MVP)        → Cost: $0
Tuần 5-8:   HYBRID (EMR on-demand processing) → Cost: $10-20/tuần
Tuần 9+:    HYBRID (S3 + RDS optimized)       → Cost: $10-15/tháng
```

**Total investment**: ~$50-100 cho cả project (1-2 tháng hoạt động)
**Story để kể ở interview**: 
> "I built a scalable E-commerce analytics system using hybrid cloud architecture. Started with local Docker development for rapid iteration, then extended to AWS EMR for batch processing, optimizing costs while maintaining production readiness."

Chi phí hợp lý, kỹ năng rộng, story hay = **Perfect portfolio piece** 🎯
