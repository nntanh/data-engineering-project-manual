# 🚀 START HERE: Lộ trình để làm project Data Engineering

**Dành cho**: Sinh viên năm cuối, muốn làm project thực tế, cost-effective để show off nhà tuyển dụng

---

## ⏰ Total Timeline: 3-6 tháng

```
Phase 1: Planning & Setup       (Tuần 1-2)    → $0
Phase 2: MVP Local              (Tuần 3-4)    → $0
Phase 3: Enhance Features       (Tuần 5-8)    → $0-20
Phase 4: AWS Extension          (Tuần 9+)     → $10-15/tháng
Phase 5: Interview Prep         (Tuần 13+)    → $0
```

---

## 🎯 Step 1: Chọn Project (Tuần 1)

### **Option A: Khuyến cáo** ⭐
👉 **E-Commerce Analytics Pipeline** - Scrape Shopee/Lazada, analyze trends

**Why**: 
- Real-world problem (sellers want insights)
- Data flow phức tạp đủ để impress
- Cost-effective
- Easy to extend with predictions

→ **Read**: `01-Project-Ideas.md`

---

## 💰 Step 2: Quyết định Chi phí & Architecture (Tuần 1)

### **Khuyến cáo**: Hybrid Model
```
Phase 1-2: LOCAL (máy em)          → $0
Phase 3-4: LOCAL + minimal AWS     → $10-20/tuần khi process
Phase 5+:  Optimized (S3 + RDS)    → $10-15/tháng
```

→ **Read**: `02-Cost-Architecture-Guide.md`

**Action**: Quyết định:
- [ ] Local-only? (Nếu máy yếu hoặc không muốn tốn tiền)
- [ ] Hybrid? (Recommended - $0-20/tháng)
- [ ] Full cloud? (Không khuyến cáo cho sinh viên)

---

## 🛠️ Step 3: Cài Đặt & Xây MVP (Tuần 2-4)

### Theo hướng dẫn:
→ **Read & Follow**: `03-Quick-Start-Local-MVP.md`

**Timeline**:
```
Day 1-2:   Docker setup (postgres, kafka, spark, metabase)
Day 3-5:   Web scraper
Day 6-7:   Load data to PostgreSQL
Day 8-10:  Spark transformations
Day 11-12: Dashboard (Metabase/Streamlit)
```

**After**: Em sẽ có data pipeline local chạy được

---

## ✨ Step 4: Enhance Features (Tuần 5-8)

### Options (chọn 2-3 cái):

#### A. Real-time Streaming
```
Mục tiêu: Scrape & update data every 30 minutes (not once)
Tools: Kafka producer scheduling + Lambda/cron job
Difficulty: ⭐⭐
Time: 1-2 tuần
```

#### B. Data Quality & Monitoring
```
Mục tiêu: Alert nếu pipeline fail
Tools: Great Expectations + Prometheus + Slack alerts
Difficulty: ⭐⭐
Time: 1 tuần
```

#### C. ML Prediction Model
```
Mục tiêu: Predict top products next week
Tools: Prophet (time-series forecasting)
Difficulty: ⭐⭐⭐
Time: 2-3 tuần
```

#### D. Advanced Analytics
```
Mục tiêu: Cohort analysis, buyer behavior, price elasticity
Tools: SQL, Pandas, Jupyter
Difficulty: ⭐⭐
Time: 2 tuần
```

**Recommendation**: Pick A + B (streaming + monitoring) = production-ready

---

## ☁️ Step 5: Deploy to AWS (Tuần 9+, Optional)

### Minimal AWS Setup
```
Local:
├─ Development, testing
└─ Docker Compose

AWS:
├─ S3: Data backup ($0.50/tháng cho 5GB)
├─ EMR (on-demand): Run Spark jobs 1-2x/tuần ($10-20 khi chạy)
├─ RDS (optional): Managed database ($10/tháng)
└─ Lambda: Scheduled ingestion jobs (free tier)

Total: $10-15/tháng steady-state
```

### How:
1. Copy local Spark job to AWS EMR
2. Setup S3 backup for data
3. Use RDS instead of local PostgreSQL (optional)
4. Schedule jobs with AWS Glue / Lambda

**Cost optimization**:
- Use spot instances (50% cheaper)
- Set billing alerts
- Turn off resources when not needed

---

## 🎤 Step 6: Interview Prep (Tuần 13+)

### Deliverables:
```
☐ GitHub repo: Clean code, good README, commit history
☐ Architecture diagram: Show system design
☐ Demo: Live or video (2-3 min)
☐ Metrics: "50K products, <2min latency, $15/month"
☐ Explanation: 30-sec pitch ready
☐ Stories: 3 "failure & fix" stories
```

→ **Read**: `04-Interview-Portfolio-Tips.md`

**Practice**:
- [ ] Explain project in 30 seconds
- [ ] Explain in 2 minutes
- [ ] Answer "Why [technology]?"
- [ ] Answer "What would you do differently?"
- [ ] Answer "How does [component] work?"
- [ ] Record yourself, watch playback

---

## 📋 Checklist: Before Starting

### Preparation
- [ ] Máy em có ≥8GB RAM (recommend) để chạy Docker
- [ ] Có internet (ổn định) để scrape data
- [ ] Python 3.9+ installed
- [ ] Docker & Docker Compose installed
- [ ] Commit time: 3-6 tháng (20-30 hrs/week)

### Tools to Install
```bash
# Windows
choco install docker-desktop python git

# Mac
brew install docker python git

# Linux
sudo apt install docker.io python3-pip git
```

### AWS Account (Optional, Phase 2)
- Free tier account: https://aws.amazon.com/free
- Free $100 credit: Check if em vừa sign up
- Billing alerts: Set limit to $20

---

## 🎓 Files Breakdown

| File | Purpose | When to read |
|------|---------|------------|
| **00-START-HERE.md** | Lộ trình tổng quan | Now |
| **01-Project-Ideas.md** | 4 ý tưởng project | Tuần 1: Chọn topic |
| **02-Cost-Architecture-Guide.md** | Full vs Hybrid vs Local | Tuần 1: Chi phí planning |
| **03-Quick-Start-Local-MVP.md** | Hướng dẫn code (detail) | Tuần 2-4: Implement |
| **04-Interview-Portfolio-Tips.md** | Cách demo & kể project | Tuần 13+: Interview prep |

---

## 💡 Advice bổ sung cho em/bạn em

### 1. Start MVP ASAP
Không nên planning lâu. Pick 1 project, code ngay. 
- Tuần 1: Decision
- Tuần 2: Setup
- Tuần 3: MVP running

### 2. Prioritize: Features, not Perfection
MVP mindset:
- ✅ E-commerce pipeline scraping + loading (1-2 tuần)
- ✅ Spark transform + dashboard (1 tuần)
- ✅ Show it works
- ❌ NOT: Make it 100% perfect

Can always improve after.

### 3. Version Control Early
```bash
git init
git add . && git commit -m "Initial commit: local dev environment"
# Commit every 2-3 days showing progress
```

GitHub commits show your journey, not just final code.

### 4. Document as You Go
- README: Update after each phase
- Architecture diagram: Draw it, even rough
- Comments in code: Not everywhere, just key decisions

### 5. Cost Monitoring
If using AWS:
```
AWS Console → Billing → Set alert at $20/month
Check weekly during Phase 3-4
```

### 6. Don't Get Stuck on Tech Choices
Not sure between Kafka vs RabbitMQ? Both fine.
Not sure between Streamlit vs Dash? Both fine.

**Just pick one and move on.** You learn by building, not debating.

### 7. Make it Realistic
Scrape real data, build real alerts, solve real problems.
- Not: "I made a data pipeline with dummy data"
- But: "I scraped 50K real products, found that XYZ sellers dominate"

### 8. Test Locally First
Before moving to AWS, make sure it works locally:
- [ ] Docker compose up -d
- [ ] Pipeline runs end-to-end
- [ ] Dashboard shows data
- [ ] Can restart without data loss

### 9. Prepare for Questions
Recruiter will ask:
- "Why Kafka?" → Learn answer
- "How would you scale to 1B events?" → Have answer
- "What went wrong?" → Have failure story ready

### 10. Network + Share
- Post on Dev.to / Medium about what you learned
- Share GitHub link with friends
- Tell recruiters: "I built an e-commerce analytics pipeline, here's the GitHub repo"

More important than AWS credentials for junior role.

---

## 🎯 Expected Outcome

After 3-6 months:
```
Portfolio:
├─ GitHub repo: Clean, well-documented
├─ Live dashboard: Demo link
└─ Metrics: "50K products, <2min latency, $15/month"

Skills:
├─ Data pipeline design (ETL/ELT)
├─ Cloud basics (Docker, AWS optional)
├─ Distributed systems (Kafka, Spark)
├─ SQL + Python
└─ Problem-solving & architecture

Interview-ready:
├─ Can explain architecture
├─ Can discuss trade-offs
├─ Can show live demo
├─ Can answer "why" questions
└─ Can discuss costs & scalability
```

This is **more than enough** for junior DE roles.

---

## ❓ FAQ

### Q: Phải có AWS knowledge before?
**A**: No. AWS Phase là optional (tuần 9+). Học on-the-job.

### Q: Phải có Spark/Kafka experience?
**A**: No. This project teaches you. 

### Q: Phải là bán hàng online kiếp?
**A**: No. E-commerce just example. Apply same pattern to any domain:
- Social media analytics
- Stock market analysis  
- Weather forecasting
- GitHub trends

### Q: Phải tốt công nghệ gì?
**A**: Just Docker + Python + SQL. We'll cover the rest.

### Q: Bao lâu để giỏi?
**A**: After 3 months: Competent junior
After 6 months: Solid junior, interview-ready
After 12 months: Mid-level (if you keep building)

### Q: Có thêm resource nào?
**A**: Yes, xem mỗi file đều có links & resources.

---

## 🚀 Let's Go!

**Next step**: 
1. Read `01-Project-Ideas.md`
2. Pick 1 project
3. Read `02-Cost-Architecture-Guide.md`
4. Read `03-Quick-Start-Local-MVP.md`
5. Start coding!

---

## 📞 Nếu cần help

**Issues during implementation**?
- Check error messages carefully
- Google the error
- Read Docker logs: `docker-compose logs [service]`
- Check GitHub issues for that tool

**Có câu hỏi về decision**?
- Refer back to cost guide
- Ask: "What's the minimum viable choice?"
- Move fast, iterate later

---

## ⭐ Remember

> "A completed MVP beats a perfect plan."
> "Shipping > Shipping-ready"
> "Done is better than perfect."

Good luck! You got this! 🚀

---

**Last update**: July 2026
**Author**: For junior Data Engineers pursuing portfolio projects
**Status**: Ready to use - Customize based on your situation

