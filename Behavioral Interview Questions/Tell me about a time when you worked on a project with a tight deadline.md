**Project Nugget:** Balanced speed, scope, and quality under tight ML integration deadline.

**Situation:**  
During a critical GrabFin initiative in Q1 2024, we faced a high-stakes challenge:
- Business needed an ML-powered loan scoring model integrated within **4 weeks** to capitalize on a market opportunity
- The MLOps platform had **no performance SLAs** (P99 latency unknown) 
- Initial load tests showed the synchronous approach could fail during peak traffic (200+ RPS)

**Task:**  
As Technical Lead, I needed to:
1. Deliver a production-ready solution within the immovable deadline
2. Mitigate risks from unproven ML infrastructure
3. Balance immediate needs with long-term system health
4. Maintain team morale under intense pressure

**Action (Demonstrating Key Points):**

**Leadership:**
- Conducted rapid **architectural spike** comparing 3 approaches
- Presented executives with a **risk-weighted decision matrix**:
    |Option|Speed|Risk|Future Proofing|
    |---|---|---|---|
    |Sync|Fast|High|Poor|
    |Async|Slow|Low|Excellent|
    |Hybrid|Medium|Medium|Good|
- Proposed and gained buy-in for the **phased hybrid approach**
    

**Resilience:**
- Managed team burnout through:
    - **Daily standups** to surface blockers
- Pivoted quickly when initial integration tests revealed API throttling issues

**Teamwork & Influence:**
- Formed a **tiger team** with:
    - 2 backend engineers
    - 1 ML engineer
    - 1 SRE
- Negotiated with MLOps team for temporary priority support
- Aligned product managers on percentage of application for which we will fetch ML Score.

**Ethical Conflict:**
- Balanced business pressure against engineering ethics:
    - Insisted on **circuit breakers** despite timeline pressure
    - Implemented **graceful degradation** to protect user experience

**Innovation:**

- Designed a **transitional architecture**:
    - Phase 1: Synchronous API with:
        - Queue-based retry mechanism
        - Thorough monitoring (latency buckets, error budgets)
    - Phase 2: Asynchronous redesign completed 3 weeks post-launch

**Result:**
- **Met the 4-week deadline** with 2 days buffer
- **Zero production incidents** during critical launch period
- Post-migration achieved:
    - **40% latency reduction** (P99 from 1200ms → 720ms)
    - **5x throughput** at 1/3 the infrastructure cost
- Established **new patterns** for ML integration that were adopted across 3 other teams
- Team retention: **100% of members stayed** post-project (vs. 30% turnover in similar pressured projects)