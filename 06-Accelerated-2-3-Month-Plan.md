# ⚡ Accelerated Plan: 2-3 Tháng + $5-15/tháng + Recruiter Strategy

**Dành cho**: Ai muốn ship nhanh, keep cost thấp, ready demo cho recruiter

---

## 🎯 Goals
- ✅ **Timeline**: 2-3 tháng (thay vì 3-6)
- ✅ **Cost**: $5-15/tháng (thay vì $0-20)
- ✅ **Recruiter**: GitHub public + live demo at interview (not live URL on CV)

---

## 📅 Aggressive Timeline

### **Week 1-2: MVP Local (0$)**
```
Day 1-3:    Docker setup + database schema
Day 4-7:    Python scraper (1000 products first)
Day 8-10:   Load to PostgreSQL
Day 11-14:  Simple Spark transform + Jupyter notebook
────────────────────────────────────────────
Output: Working pipeline, local only
Demo: Jupyter notebook showing data
```

### **Week 3-4: Streaming + Dashboard (0$)**
```
Day 15-17:  Kafka setup (local)
Day 18-21:  Kafka producer/consumer
Day 22-24:  Streamlit dashboard
Day 25-28:  Testing + fixes
────────────────────────────────────────────
Output: Full streaming pipeline
Demo: Live dashboard (local)
```

### **Week 5-6: AWS Deployment ($5-15)**
```
Day 29-35:  Setup RDS (managed PostgreSQL) + S3
Day 36-42:  Move Spark job to EMR (on-demand, not always-on)
────────────────────────────────────────────
Cost: Only when processing (on-demand)
Run 2x/week Spark job (~2 hours) = ~$5-10/month
```

### **Week 7-9: Monitoring + Optimization ($5-10)**
```
Day 43-49:  Add error handling, logging
Day 50-56:  Cost optimization (spot instances, auto-scaling)
Day 57-63:  Demo preparation
────────────────────────────────────────────
Finalize: Production-ready pipeline
Cost: S3 storage + occasional EMR = $5-15/month
```

---

## 💰 Cost Breakdown: $5-15/month (Aggressive)

### **Phase 1-2: Local (0$)**
```
Week 1-4: Everything local Docker
├─ Postgres: Local container
├─ Kafka: Local container
├─ Spark: Local container
└─ Cost: $0
```

### **Phase 3: AWS Minimal ($5-15/month)**
```
S3 Storage:
├─ Raw data (5GB): $0.12/month
├─ Processed data (2GB): $0.05/month
└─ Sub-total: ~$0.20/month

RDS (PostgreSQL, db.t3.micro):
├─ Instance: $6-10/month
├─ Storage (20GB): $2/month
└─ Sub-total: $8-12/month

EMR (Spark processing, on-demand only):
├─ 1 m5.xlarge instance (2 hours/week): $0.20/hour
├─ 2 weeks × 2 hours × 0.20 = $0.80/month
└─ Data transfer: $1-2/month

Lambda (scraper trigger):
├─ Free tier: 1M invocations
└─ $0 (within free tier)

Total: $8-14/month (STEADY STATE)
```

### **How to stay at $5-15**:
1. ✅ **Don't run EMR 24/7** → Run only 2x/week for batch processing
2. ✅ **Use spot instances** → 50% cheaper when running EMR
3. ✅ **Set billing alert** → $20/month max
4. ✅ **Turn off RDS** → Can skip RDS, use S3 + local Postgres (saves $8!)

---

## 🎯 Cost-Optimized Architecture (2-3 month version)

### **Option A: Ultra-cheap ($5-8/month)**
```
Local:
├─ Python scraper (cron job on laptop)
├─ PostgreSQL local
└─ Streamlit local (only run when needed)

AWS:
├─ S3: Data backup only ($0.20/month)
└─ NO RDS, NO EMR

Cost: ~$5/month (just S3 storage)
Trade-off: Laptop must be on for scraper, can't scale easily
```

### **Option B: Balanced ($5-15/month) ⭐ RECOMMENDED**
```
Local (Development):
├─ Python scraper
├─ PostgreSQL 
└─ Streamlit

AWS (Production):
├─ S3: Data lake
├─ RDS: Managed database ($10-12/month)
├─ EMR: On-demand Spark (2x/week, $0.50-1/month)
└─ Lambda: Scheduled scraper (free tier)

Cost: $10-15/month
Benefit: Production-grade, scalable, always available
```

### **Option C: Minimum viable ($5/month)**
```
Skip RDS. Use DuckDB (file-based) instead:

Local + S3:
├─ Python scraper → DuckDB file
├─ Spark job (local computer) → S3
├─ Streamlit dashboard reads from S3
└─ No RDS needed

Cost: ~$5/month (just S3 storage)
Trade-off: Can't query from cloud yet, but MVP-level
```

**Recommendation: Option B** (best balance)

---

## 🚀 Realistic 2-3 Month Breakdown

### **Month 1: MVP Local + GitHub**
```
Week 1-2:   Setup + MVP scraper/DB/Spark
Week 3-4:   Streamlit dashboard
Goal:       Working pipeline, push to GitHub

Deliverable:
├─ GitHub repo public
├─ README with architecture
├─ Running on local laptop
└─ Commits show progression

Cost: $0
```

### **Month 2: AWS Deployment + Production-ifying**
```
Week 5-6:   Migrate to RDS + S3
Week 7-8:   Fix bugs, add monitoring, optimize cost
Goal:       Production infrastructure ready

Deliverable:
├─ Data persisted on AWS
├─ Scheduled EMR jobs
├─ GitHub updated with AWS architecture
└─ Demo works from laptop

Cost: $5-15/month steady-state
```

### **Month 3: Polish + Interview Prep**
```
Week 9-10:  Final optimizations
Week 11-12: Portfolio polish, demo prep
Goal:       Interview-ready

Deliverable:
├─ Perfect GitHub README
├─ Metrics documented (latency, cost, scale)
├─ Interview stories prepared
├─ Demo video pre-recorded (backup)

Cost: $5-15/month
```

---

## 📱 How to Show Off Recruiter: GitHub + Demo Strategy

### **Strategy: NOT put live URL on CV**

#### ❌ Don't do this:
```
CV says: "E-Commerce Analytics Pipeline: dashboard.myproject.com"
Problem:
- URL dies after project (cost, maintenance)
- Recruiter visits, something broken → bad impression
- Can't control when they visit (might be down)
- "Why did you leave it running? Costs $X/month?"
```

#### ✅ DO THIS INSTEAD:

**CV says:**
```
## Projects

### E-Commerce Analytics Pipeline
- Built scalable data pipeline: scrape → Kafka → Spark → dashboard
- GitHub: github.com/yourname/ecommerce-analytics
- Tech: Python, Kafka, PySpark, PostgreSQL, AWS
- Impact: 50K products, <2min latency, $15/month cost-optimized
```

**Why this is better:**
- GitHub link shows code (most important!)
- Recruiter sees commit history, code quality
- Shows you maintained it
- No broken URL
- Costs nothing (GitHub free)

---

## 🎤 At Interview: How to Demo

### **Format 1: Live Demo (if confident)**
```
Recruiter: "Show me how it works"
You: 
  1. Open terminal, run: docker-compose up -d
  2. Wait 30 seconds for services to start
  3. Open browser → http://localhost:3000 (Streamlit)
  4. Run scraper, show data flowing
  5. Click filters in dashboard
  
Risk: Network issues, Docker takes time to start
Mitigation: Test 30min before call, have video backup
```

### **Format 2: Pre-recorded Video (safest)**
```
Recruiter: "Show me how it works"
You:
  1. "I prepared a short demo video" 
  2. Play 2-3 min video showing:
     ├─ Terminal: scraper running
     ├─ Kafka: messages flowing
     ├─ Spark: job processing
     └─ Dashboard: results visualized
  3. Then explain architecture, answer questions
  
Benefit: No technical issues, you control the narrative
Time to create: ~30 min (screen record + basic editing)
```

### **Format 3: Screenshots + Narration (most reliable)**
```
Recruiter: "Tell me about the pipeline"
You:
  1. Share screen with slides
  2. Show architecture diagram
  3. Show code snippets (key parts)
  4. Show dashboard screenshots
  5. Show dashboard metrics
  
  Explain: "The system has 3 stages..."
  
Benefit: No execution, just talking
Time: ~2 min explanation
```

**Recommendation: Format 2 (video) + Format 3 (slides)**
- Video shows it actually works
- Slides/narration explain the thinking

---

## 📸 What to Include in Demo/CV

### **CV mentions:**
```
E-Commerce Analytics Pipeline
- Scraped 50K+ products from Shopee using Python + Selenium
- Real-time ingestion via Apache Kafka (1M+ events/day throughput)
- Batch processing with PySpark (distributed computing)
- Stored in PostgreSQL (50M+ row analytics queries <2s)
- Interactive Streamlit dashboard for trend analysis
- Cost-optimized: $15/month using AWS hybrid approach (local + EMR)
- GitHub: github.com/yourname/ecommerce-analytics
```

### **GitHub README should show:**
```
1. Architecture diagram (visual)
2. Quick start (can run locally)
3. Tech stack + WHY each choice
4. Performance metrics (latency, throughput)
5. Cost breakdown (show you're smart about money)
6. How to run demo locally
7. Future improvements (roadmap)
```

### **Demo should emphasize:**
```
1. Real data (50K products, not dummy)
2. Working end-to-end (scraper → DB → dashboard)
3. Scale story ("If 10x data, here's how we'd scale")
4. Error handling (show you thought about reliability)
5. Cost awareness (explain why you chose this architecture)
```

---

## 🎬 Interview Q&A Prep (for 2-3 month version)

### **Q: "This is impressive! How long did it take?"**
A: "2.5 months, working part-time. Started with local MVP in 2 weeks, then migrated to AWS over the next month, optimized in the final month."

### **Q: "Why these 2-3 months instead of more polished?"**
A: "I prioritized learning + shipping. The core pipeline is solid and production-ready. Future work would add ML predictions and real-time alerting, but the MVP demonstrates the core skills."

### **Q: "Why not just keep it running on AWS 24/7?"**
A: "Cost-conscious decision. Running EMR 24/7 would cost $400+/month. Instead, I run batch jobs on-demand (2x/week), keeping it at $15/month. This teaches cost optimization thinking."

### **Q: "Can you show me live?"**
A: (Option 1) "Sure, let me start the local Docker services" *starts Docker*
(Option 2) "I have a pre-recorded demo to avoid network issues. Here it is." *plays video*

### **Q: "What would you do next?"**
A: "Add real-time anomaly detection for price drops, integrate with Slack alerts, and extend to other e-commerce platforms (Lazada, TikTok Shop)."

### **Q: "What challenges did you face?"**
A: "Biggest was Kafka initially confusing (offset management). Solved by reading docs + experimenting. Also, cost management - realized full AWS was too expensive, so designed hybrid approach."

---

## ✅ 2-3 Month Checklist

### **End of Month 1:**
- [ ] GitHub repo public + first commit
- [ ] Local MVP working (scraper + DB + basic viz)
- [ ] README started
- [ ] Can demo locally

### **End of Month 2:**
- [ ] RDS + S3 setup (AWS)
- [ ] EMR job working (test run)
- [ ] Streamlit dashboard polished
- [ ] GitHub updated with AWS architecture
- [ ] Cost stable at $10-15/month

### **End of Month 3:**
- [ ] README perfect
- [ ] Demo video recorded (2-3 min)
- [ ] Metrics documented (latency, cost, scale)
- [ ] 3 interview stories prepared
- [ ] CV updated with link
- [ ] Ready to apply!

---

## 🚀 "Ready to Apply" Checklist

Before sending CV:

```
CV Mentions:
☐ Project name: E-Commerce Analytics Pipeline
☐ GitHub link: github.com/yourname/ecommerce-analytics
☐ 1-liner: "Built scalable data pipeline ingesting 50K products"
☐ Tech stack: Kafka, Spark, PostgreSQL, AWS
☐ Impact: "<2min latency, 1M events/day throughput, $15/month cost"

GitHub Repo:
☐ README with architecture diagram
☐ Clear project structure (scripts/, dashboards/, docs/)
☐ Setup instructions (someone can clone & run locally)
☐ Good commit history (shows work over time)
☐ Code is clean (no debug prints, TODOs)
☐ Tech decision documented (why Kafka? why Spark?)

Demo Ready:
☐ Local setup works (docker-compose up -d works)
☐ Video pre-recorded (2-3 min backup)
☐ Slides ready (architecture + code snippets)
☐ Can explain every component
☐ Have 3 "failure & fix" stories

Interview Prep:
☐ 30-sec elevator pitch
☐ 2-min explanation ready
☐ Answers to "why [tech]?" questions
☐ Cost optimization story
☐ Scale-to-10x plan
```

---

## 💡 Pro Tips for 2-3 Month Sprint

### **Tip 1: Skip optional stuff**
```
❌ Skip: Kubernetes, GraphQL, complex ML models
✅ Do: Core pipeline, error handling, demo

Skip: "Perfect code architecture"
Do: Working MVP that solves the problem
```

### **Tip 2: Aggressive scope management**
```
Week 1: 1000 products
Week 2-3: Scale to 50K products (if it works)
Don't: Build for 1B records from day 1
```

### **Tip 3: Use templates & boilerplate**
```
Docker Compose: Copy from Docker docs
Spark job: Copy starter template, customize
Streamlit: Use example from docs, modify

Don't reinvent: Use existing patterns
```

### **Tip 4: Test AWS before committing**
```
Week 5-6: Spin up RDS for 1 day, test, turn off
Cost: ~$0.50
Purpose: Verify before month-long commitment

Don't: Setup RDS on day 1, forget about it for 3 months ($300 bill!)
```

### **Tip 5: Document decisions early**
```
Day 1: Create DECISIONS.md
Record: "Why Kafka?" "Why Spark?" "Why not Kubernetes?"

Later: Recruiter asks, you have answers ready
Interview: You can explain architecture thinking
```

---

## 🎯 Expected Results After 2-3 Months

### **What you'll have:**
```
✅ Working data pipeline (production-grade)
✅ 50K real data points
✅ End-to-end working (scraper → DB → visualization)
✅ Cost-optimized to $5-15/month
✅ Public GitHub repo with good README
✅ Demo video ready
✅ Architecture understanding
✅ Interview stories prepared
```

### **What recruiters will see:**
```
✅ Real project (not tutorial)
✅ Technical depth (Kafka, Spark, AWS)
✅ Cost awareness (junior engineers often miss this)
✅ Problem-solving (architecture decisions)
✅ Communication (good GitHub README + demo)
```

### **This is enough for:**
```
✅ Junior DE interviews
✅ Data analyst interviews
✅ Analytics engineering interviews
⚠️ Mid-level: Would need more projects or deeper system design
```

---

## ⏱️ Time Commitment Realistic?

**2-3 months aggressive timeline requires:**
```
Recommended: 20-30 hrs/week
├─ Weeks 1-4: 30 hrs/week (MVP sprint)
├─ Weeks 5-8: 20 hrs/week (AWS migration)
└─ Weeks 9-12: 15 hrs/week (polish)

Workable: 15 hrs/week (slower, 4-5 months)
Tight: 40+ hrs/week (burnout risk, not recommended)
```

**Timeline is aggressive but feasible if:**
- ✅ Committed to daily progress
- ✅ Don't get stuck on perfectionism
- ✅ Ask for help (Stack Overflow, GitHub issues)
- ✅ Focus on MVP first

---

## 🎓 Final Recommendation

**For 2-3 month timeline + $5-15 cost + interview-ready:**

```
Timeline: 12 weeks aggressive
├─ Week 1-2:   MVP local (0$)
├─ Week 3-4:   Streaming dashboard (0$)
├─ Week 5-6:   AWS setup (RDS + S3 + EMR test) ($5-10)
├─ Week 7-8:   Cost optimization ($5-15 steady)
├─ Week 9-12:  Polish + demo prep ($5-15)
└─ Total: $20-60 for entire project (3-4 months cost)

Deliverable: GitHub repo + demo video + interview prep

Interview pitch: "Built production-grade e-commerce analytics pipeline in 2.5 months, cost-optimized to $15/month"

This will impress junior DE hiring managers! 🚀
```

---

**Good luck! Shoot for 2-3 months, you can do it! 💪**
