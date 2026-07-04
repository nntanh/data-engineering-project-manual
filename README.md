# 📚 Data Engineering Project Guide

**Bộ hướng dẫn hoàn chỉnh** để sinh viên năm cuối xây dựng project DE thực tế, cost-effective, interview-ready.

---

## 🎯 Bắt đầu từ đâu?

### ⏰ Có 2 phút? 
👉 Đọc: **00-START-HERE.md** (lộ trình tổng quan)

### ⏰ Có 30 phút?
1. **00-START-HERE.md** - Lộ trình
2. **01-Project-Ideas.md** - Chọn topic
3. **02-Cost-Architecture-Guide.md** - Chi phí (hybrid recommended)

### ⏰ Có 2+ giờ?
Đọc toàn bộ:
1. **00-START-HERE.md**
2. **01-Project-Ideas.md**
3. **02-Cost-Architecture-Guide.md**
4. **03-Quick-Start-Local-MVP.md** (start coding!)
5. **05-Architecture-Decision-Framework.md** (khi có quyết định khó)
6. **04-Interview-Portfolio-Tips.md** (sau 3 tháng)

---

## 📋 File Guide

| File | Purpose | Read when |
|------|---------|-----------|
| **00-START-HERE.md** | Lộ trình tổng quan, timeline | First |
| **01-Project-Ideas.md** | 4 ý tưởng project + so sánh | Tuần 1: Chọn topic |
| **02-Cost-Architecture-Guide.md** | Full cloud vs hybrid vs local | Tuần 1: Budget planning |
| **03-Quick-Start-Local-MVP.md** | Code hướng dẫn chi tiết | Tuần 2-4: Implementation |
| **04-Interview-Portfolio-Tips.md** | Portfolio & interview prep | Tuần 13+: Before applying |
| **05-Architecture-Decision-Framework.md** | Tech choice framework | When stuck on decision |
| **README.md** | This file (navigation) | Now |

---

## 🚀 Quick Start (5 min)

```bash
# 1. Read START HERE
open 00-START-HERE.md

# 2. Pick project (recommend: E-Commerce Analytics)
# 3. Decide budget (recommend: Hybrid, $0-20/month)
# 4. Install Docker
# 5. Follow 03-Quick-Start-Local-MVP.md

# 6. Run your first pipeline
docker-compose up -d
python scripts/02_load_to_db.py
streamlit run dashboards/app.py
```

---

## 🎓 What You'll Learn

### After 2 weeks (MVP):
- Data scraping & API integration
- Message queues (Kafka basics)
- SQL & relational databases
- Basic Docker

### After 4 weeks (Extended):
- Distributed processing (Spark)
- Data transformation & aggregations
- Dashboard visualization
- End-to-end pipeline thinking

### After 8 weeks (Production):
- Streaming architecture
- Error handling & monitoring
- Cost optimization
- Production deployment patterns

### After 12 weeks (Interview-ready):
- Architecture trade-offs
- System design thinking
- Production-grade code
- Interview storytelling

---

## 💰 Cost Reality

| Approach | Cost | Setup time | Interview value |
|----------|------|-----------|-----------------|
| **Local only** | $0 | 3 hours | ⭐⭐⭐ (good, but demo limited) |
| **Hybrid (recommended)** | $10-20/month | 5 hours | ⭐⭐⭐⭐ (best balance) |
| **Full cloud** | $65-140/month | 1 day | ⭐⭐⭐⭐⭐ (overkill for student) |

**Recommendation**: Hybrid approach
- Phase 1-2: Local (0$)
- Phase 3+: S3 + optional EMR ($10-20/month when needed)

---

## 🛠️ Tech Stack Recommendations

```
Recommended (E-Commerce Analytics):
├─ Scraping: Python + BeautifulSoup
├─ Streaming: Apache Kafka (local Docker)
├─ Processing: PySpark
├─ Storage: PostgreSQL (local) + S3 (AWS optional)
├─ Visualization: Streamlit
├─ Infrastructure: Docker Compose (local) + AWS EMR (optional)
└─ Cost: $0-20/month

Why this stack?
✅ Industry standard (real companies use this)
✅ Cost-effective
✅ Scalable (can grow from MVP to production)
✅ Good learning value
✅ Impressive for interviews
```

---

## 📊 Project Recommendations

### Pick 1 of these 4:

#### 1️⃣ **E-Commerce Analytics** ⭐ (RECOMMENDED)
- Scrape Shopee/Lazada
- Analyze product trends, seller performance
- **Why**: Real problem, great learning, easily scalable
- **Timeline**: 3-6 months
- **Cost**: $0-20/month
- **Difficulty**: ⭐⭐⭐

#### 2️⃣ GitHub Analytics
- Analyze public repos, language trends
- **Why**: Technical focus, good for tech hiring
- **Timeline**: 2-4 months
- **Cost**: $0-5/month
- **Difficulty**: ⭐⭐⭐

#### 3️⃣ Air Quality Monitoring
- Real-time pollution tracking + forecasting
- **Why**: ML + streaming, cool demo
- **Timeline**: 3-5 months
- **Cost**: $0-10/month
- **Difficulty**: ⭐⭐⭐⭐

#### 4️⃣ Personal Finance Aggregator
- Aggregate bank accounts, crypto, stocks
- **Why**: Quick to MVP, personally useful
- **Timeline**: 3-6 weeks
- **Cost**: $0
- **Difficulty**: ⭐⭐

**Recommendation**: Pick option 1 (E-Commerce) for best balance of difficulty, learning, and interview appeal.

---

## 📈 Expected Timeline

```
Week 1-2:   Planning + Setup                  $0
Week 3-4:   MVP (scraper + DB + dashboard)    $0
Week 5-8:   Features (streaming, monitoring)  $0-20
Week 9-12:  AWS deployment + optimization     $10-15/month
Week 13+:   Interview prep
─────────────────────────────────────────────────
Total: 3-6 months, $50-100 investment (or $0 if local only)
```

---

## ✅ Milestones

### ✓ Week 2: Decision made
- [ ] Project chosen
- [ ] Architecture planned
- [ ] Budget decided
- [ ] Docker installed

### ✓ Week 4: MVP working
- [ ] Scraper running
- [ ] Data in database
- [ ] Dashboard showing results
- [ ] Code on GitHub

### ✓ Week 8: Production-ready
- [ ] Real-time pipeline
- [ ] Error handling
- [ ] Monitoring setup
- [ ] Demo ready

### ✓ Week 13: Interview-ready
- [ ] GitHub repo perfect
- [ ] Demo polished
- [ ] Interview story prepared
- [ ] Resume updated

---

## 🎤 Interview Story

After 3-6 months, here's what to tell recruiters:

### 30-second pitch:
> "I built an end-to-end data pipeline for e-commerce analytics. Scraped 50K products from Shopee, used Kafka for real-time ingestion, Spark for processing, and created a Streamlit dashboard. The entire system cost <$20/month by using a hybrid cloud approach."

### 2-minute explanation:
> "The system has three stages. First, a Python scraper collects product data every hour and publishes to Kafka. Second, Spark jobs aggregate metrics (price trends, best sellers) and write to PostgreSQL. Third, a Streamlit dashboard visualizes the insights.
>
> What was interesting was optimizing cost while maintaining scalability. I started with everything local, then strategically moved to AWS only for heavy processing jobs, keeping the baseline cost at $15/month.
>
> The architecture also taught me important tradeoffs: Kafka provides decoupling (if scraper is slow, processor doesn't block), Spark provides scalability (can handle 100x more data without rewriting), and PostgreSQL gives me ACID guarantees for data quality."

---

## 📚 Key Resources

### By Topic

#### Getting started with DE concepts
- Fundamentals of Data Engineering (book)
- YouTube: Data Engineering Simplified

#### Docker
- https://docs.docker.com/
- Play with Docker: https://labs.play-with-docker.com/

#### Kafka
- Kafka official docs: https://kafka.apache.org/
- YouTube: Kafka in 100 Seconds

#### PySpark
- Official docs: https://spark.apache.org/docs/latest/api/python/
- YouTube: PySpark Tutorial

#### SQL & Databases
- PostgreSQL docs: https://www.postgresql.org/docs/
- Mode SQL Analytics: https://mode.com/sql-tutorial/

#### AWS
- AWS Free tier: https://aws.amazon.com/free
- AWS Learning Path: https://aws.amazon.com/training/
- A Cloud Guru (Udemy)

### By Goal

**Just want to pass interviews?**
→ Focus on the Big Picture Architecture. Read: "Designing Data-Intensive Applications" ch 1-3

**Want to actually understand the code?**
→ Follow 03-Quick-Start-Local-MVP.md completely

**Want to optimize for cost?**
→ Read 02-Cost-Architecture-Guide.md thoroughly

**Want to prepare talking points?**
→ Read 04-Interview-Portfolio-Tips.md

**Stuck on architecture decision?**
→ Read 05-Architecture-Decision-Framework.md

---

## 🆘 Troubleshooting

### "I don't know how to start"
→ Read 00-START-HERE.md, pick one project, follow 03-Quick-Start-Local-MVP.md step by step

### "Docker/Spark/Kafka seems too complicated"
→ Totally normal. Learn one piece at a time. Start with Docker, then Postgres, then add Kafka. Don't learn everything at once.

### "I'm running out of money/have no AWS credit"
→ Use LOCAL only (docker compose). No AWS needed for MVP. Cost = $0.

### "I want to deploy to AWS but don't understand how"
→ Read 02-Cost-Architecture-Guide.md Phase 3. It's simpler than you think.

### "My demo crashed during interview"
→ Have backup: Screenshots + video ready. Explain what happened calmly.

### "I don't know what to add next"
→ Pick from: Real-time alerts, ML prediction, or API endpoint for consumption.

---

## 🎯 Interview Gotchas (Be Prepared!)

**Q: "Why did you choose Kafka over RabbitMQ?"**
→ Answer: "Kafka handles higher throughput, has better exactly-once semantics, and is more industry standard for this data volume"

**Q: "How would you scale this to 1 billion events?"**
→ Answer: "Switch from local Spark to AWS EMR auto-scaling cluster, or even better, Flink for true streaming"

**Q: "What went wrong?"**
→ Have 3 failure stories ready. Show how you debugged and fixed.

**Q: "How much does this cost?"**
→ Answer: "In production: $15/month. In development: free local Docker setup"

**Q: "Show me the code"**
→ Link: GitHub repo. Highlight: scraper logic, Spark transformation, error handling

---

## 🏆 Success Metrics

After following this guide:

- [ ] You have working project
- [ ] You can explain it in 30 seconds
- [ ] You can explain it in 5 minutes with architecture diagrams
- [ ] You know why every technology choice was made
- [ ] You have 3 "debugging failure" stories
- [ ] Your GitHub shows good commit history
- [ ] Your code has no scary warnings/TODOs
- [ ] You're confident demoing it
- [ ] You can answer variations ("What if 10x data?")

If you check all boxes: **Ready for interviews!** 🚀

---

## 📞 Questions?

**"Which project should I pick?"**
→ Pick E-Commerce Analytics (option 1). Most balanced.

**"How long will this take?"**
→ MVP: 4 weeks, Full: 8-12 weeks, Interview-ready: 16+ weeks

**"What if I fail?"**
→ You'll learn a LOT by trying. Failure is part of the journey.

**"Can I do this part-time?"**
→ Yes. 20-30 hours/week for 3-6 months works great.

**"Do I need to know Kubernetes/Data Science/MLOps?"**
→ No. This guide is pure Data Engineering. You'll learn what you need on the job.

**"Is this enough for a job?"**
→ This project + interview prep = solid junior DE candidate. Not senior, but respectable portfolio.

---

## 🚀 Next Step

**Pick ONE and do it:**

### Option A: I'm ready to start coding NOW
→ Read 03-Quick-Start-Local-MVP.md and follow it

### Option B: I want to plan first
→ Read 00-START-HERE.md + 01-Project-Ideas.md + 02-Cost-Architecture-Guide.md

### Option C: I'm stuck on a decision
→ Read 05-Architecture-Decision-Framework.md

### Option D: I already have a project and want interview tips
→ Read 04-Interview-Portfolio-Tips.md

---

## 📊 Quick Decision Tree

```
Q: Do I have 2-3 months free?
├─ YES → Start now!
└─ NO → Adjust timeline to 6-12 months

Q: Do I have $0-20/month budget?
├─ YES → Hybrid approach (recommended)
└─ NO → Go local-only ($0)

Q: Am I solo or in a team?
├─ SOLO → Focus on learning + portfolio
└─ TEAM → Focus on collaboration + docs

Q: Do I know Python + SQL basics?
├─ YES → Ready to start
└─ NO → Learn basics first (1-2 weeks)
```

---

## 🎓 By the End

You'll have:
```
✅ Working data pipeline (production-grade)
✅ GitHub portfolio project
✅ Demo ready for recruiter
✅ Interview stories prepared
✅ Understanding of: Docker, Kafka, Spark, SQL, Cloud
✅ Confidence in system design conversations
✅ Cost optimization experience
```

**That's all you need for junior DE role interviews!** 🏆

---

**Version**: 1.0
**Last updated**: July 2026
**Best for**: Final year students, career switchers, bootcamp grads
**Effort**: 3-6 months, 20-30 hrs/week recommended

**Good luck! You've got this! 🚀**

---

P.S. After you finish: 
- Share on LinkedIn
- Post on Dev.to / Medium
- Tell your friends
- Help others do the same

Spread the knowledge! 📚
