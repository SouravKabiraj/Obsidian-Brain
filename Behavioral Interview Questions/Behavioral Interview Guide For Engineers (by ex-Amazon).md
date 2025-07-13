# Overview

| Project | Nugget | S   | T   | A   | R   | Note | Value |
| ------- | ------ | --- | --- | --- | --- | ---- | ----- |
|         |        |     |     |     |     |      |       |
## Do's and Don'ts
1) Tell recent stories
2) Share a lot of details
3) Map your stories to company values
4) Include a mix of Success, failure and lessons learned.
5) Your team's success reflects well on you.


# Prepare stories for the below themes:
- An interesting technical problem you solved.
- An interpersonal conflict you overcame.
- A time when you demonstrated leadership or ownership.
- A situation where you should have done differently in a past project.
- Do you fix things that aren’t quite right, even if you don’t have to?
- A time you acted on a piece of feedback you received.
- Something you learned recently

# Themes
An interesting technical problem you solved. 
> 1) LOS Migration, 2) DPD Job optimisation, 

An interpersonal conflict you overcame. 
> 1) MR Review, 2) Prioritisation of tech debts 

A time when you demonstrated leadership or ownership.
> 1) Wallet Adapter, 2) DB Stats optimisation

A situation where you should have done differently in a past project.
> 

Do you fix things that aren’t quite right, even if you don’t have to? 
> Database clean up job, Auto service monitor generation 

A time you acted on a piece of feedback you received.
> Speed up code review process by continuous learning

Something you learned recently
> Rule Engine: Git as Database, RAG System

# Competencies
1) Earn Trust
2) Learn and Be Curious
3) Deep Dive
4) Invent and Simplify
5) Insist on the Highest Standards
6) Think Big
7) Ownership
8) Deliver Result
9) Have Backbone
10) Disagree and commit
11) Bias for Action
12) Dealing with ambiguity
13) Data analysis, Data-driven evaluation
14) Are right, A lot
15) Customer Obsession

# Story Bank
1) Helped customer to get back money (for personal emergency) which she invested in Finzy #customer_obsession #bias_for_action #ownership #deliver_result
2) Grab Hackathon - Cost saving Debit card to Bank Linking 
   #deep_dive #invent_and_simplify 
3) Wallet Adapter implementation #insist_on_the_highest_standards #think_big #deliver_result 
4) DPD Job optimisation  #deep_dive #earn_trust 
5) MR Review Conflict  #have_backbone 
6) MLOps Integration, Bullet Loan  scoped out feature #bias_for_action #deliver_result 
7) LOS Migration #deep_dive #invent_and_simplify #ownership

# STORY 1: Future Dated Testing
## Bullet Points
1. Designed testing framework for All Lending Products
2. Process future repayments
3. Proposed and developed a time cursor mechanism
4. Fixed glitches with each iteration
5. Training sessions for QA
6. Embed the time cursor into automation pipeline
7. Designed safeguards for production
8. Reduced testing time
9. Standardised date-dependent testing
10. Added this KT in onboarding Kit 
## Situation
During the launch of our Flexi Loan product, we faced a critical testing bottleneck: validating loan repayments scheduled 250 days after creation. Manual testing by waiting 250 days was impossible without delaying the launch.

## Task
My responsibility was to design a feasible, secure, and efficient testing solution that:
- Allowed instant validation of time-sensitive repayments
- Prevented misuse in production
- Enabled seamless collaboration between Dev, QA, and Automation teams
## Action
1. Leadership & Innovation
- Proposed and developed a time cursor mechanism for test environments, letting us:
- Backdate loans (e.g., to Jan 1, 1990)
- Simulate 250 days later (e.g., Sep 8, 1990) for repayment testing.

2. Resilience & Problem-Solving
- Initial prototypes caused system glitches, but I iterated until the solution was stable.

3. Teamwork & Influence
- Documented the process for QA teams and held training sessions.
- Partnered with Automation engineers to embed the feature into CI/CD pipelines.

4. Ethical Conflict Mitigation
- Designed safeguards:
	- Disabled time cursor in production via environment checks.
	- Added audit logs for transparency.
	- Briefed stakeholders on risks and controls.
## Result
- Reduced testing time by 8 weeks, ensuring on-time launch.
- Became a standard practice for date-dependent testing.

Zero incidents of misuse due to proactive security measures.
# STORY 2: DPD Job 
## Bullet Points
1. Reduce processing time
2. Ensure data accuracy
3. Scalability to handle future growth
4. Extended the blackout window
5. Queuing system to hold repayments
6. Parallelized the accrual job
7. Coordinated with Product, QA, and Ops
8. Tested the optimization with QA
9. Update SLAs
10. Implemented logging/alerts
11. 99% reduction in accrual job time
12. Data integrity, Improved customer experience
13. Scalable foundation
## Situation
Our financial system relied on a daily accrual job that processed interest calculations for loans. Initially, it ran smoothly (15 minutes for ~10K loans). However, due to rapid company growth, loan volumes surged to 1M+, causing the job to run for over 3 hours.

Critical Problem:
- Loan repayments were blocked during accrual processing to avoid calculation errors, severely impacting customer experience.
## Task
As the lead engineer, I needed to:
1. Reduce processing time to minimize repayment delays.
2. Ensure data accuracy (no race conditions between accruals and repayments).
3. Improve scalability to handle future growth.

## Action
1. Short-Term Fix (Resilience & Risk Mitigation)
- Extended the repayment blackout window from 00:00–04:00 AM to prevent errors.
- Introduced a queuing system to hold repayments initiated during the block and process them afterward.

2. Optimization (Leadership & Innovation)
- Parallelized the accrual job, splitting work across multiple threads, reducing runtime from 3 hours → 15 minutes.
- Added a transaction token system to track pending accruals, allowing repayments to resume sooner (mean completion time: 5 minutes).

3. Collaboration (Teamwork & Influence)
- Coordinated with Product, QA, and Ops teams to:
	- Test the new logic under high-load scenarios.
	- Update SLAs for repayment processing times.
	- Documented the design for future maintenance.

4. Ethical Consideration (Conflict Prevention)
- Ensured the queuing system prioritized fairness (FIFO processing) to avoid disadvantaging any customers.
- Implemented logging/alerts for failed transactions to guarantee transparency.

## Result
- 99% reduction in accrual job time (3hr → 5min).
- Zero data integrity issues despite 10x loan volume growth.
- Improved customer experience: Repayments delayed by minutes, not hours.
- Scalable foundation: The system now handles 5M+ loans without redesign.

# Story 3: LOS Migration
## Bullet Points
1. Restore SLA
2. Reduce costs
3. Bought time for a permanent solution, Upgraded Appain M/C
4. Proposed an in-house workflow, proving Appian’s limitations
5. Demo MVP to leadership, Presented data-driven comparisons
6. Documented risks/rewards to align leadership
7. Phased migration to avoid disruptions
8. A team member promoted
## Situation
Our company initially used Appian (a no-code platform) to build our loan underwriting workflow, which worked well for low volumes (SLA: <2 mins). However, during traffic spikes (1K+ applications/minute), processing times ballooned to 2-3 hours, creating:
- Poor customer experience (delayed approvals)
- High infrastructure costs (scaling Appian required expensive upgrades)

## Task
As the Team Lead, I needed to:
1. Restore SLA compliance (<2 mins) even during peak loads.
2. Reduce costs while maintaining reliability.
3. Navigate business constraints (Appian was a strategic partner).
## Action
1. Short-Term Fix (Resilience & Teamwork)
- Collaborated with Appian support, upgraded to a larger machine—costs spiked, but bought time for a permanent solution.

2. Long-Term Solution (Leadership & Innovation)
- Proposed an in-house workflow engine after proving Appian’s limitations.
- Took tech debt: Dedicated 10 story points/sprint for an MVP, balancing it with core deliverables.
- Influenced stakeholders:
	- Presented data-driven comparisons (cost, performance at 10K/min load).
	- Addressed business relationship concerns with evidence (e.g., 50% faster processing).

3. Execution (Teamwork & Ethical Conflict)
- Led a cross-functional team (devs, QA, ops) through:
- 2-month development + 1-month testing (load/stress tests).
- Phased migration to avoid disruptions.
- Advocated for transparency: Documented risks/rewards to align leadership.

## Result

- Performance: SLA met consistently (<2 mins, even at 10K/min peaks).
- Reduced Costs: Infra savings vs. Appian (exact % if possible).
- Business Impact:
	- Improved UX: Avg. underwriting time ↓50%.
- Career growth: A team member promoted for contributions.
- Strategic win: Proved in-house solutions could coexist with vendor partnerships.

# Story 4:  Code Review Conflict
## Bullet Points
- Reviewer insisted the code writer redesign the LLD
- Code Writer argued as it will delay the project
- De-escalate
- Resolution that respected both
- Prevent recurrence
- Identified common ground
- Combine strengths of both designs
- Created an MR Review Guideline (Introduce early design syncs for high-stakes changes)
- Stronger team culture
- The guideline was adopted by Deposit and FinTrust

## Situation

At Grab, two engineers in my team escalated a heated debate during a code review:
- MR Reviewer insisted the code writer redesign the LLD (Low-Level Design) to align with their preferred approach.    
- Code Writer argued the original implementation was optimal.  
    The disagreement risked delaying the project and creating team division.

## Task
As the team lead, I needed to:
1. De-escalate tensions immediately to prevent spillover effects.    
2. Find a resolution that respected both technical merits and team dynamics.
3. Prevent recurrence by improving our review process.

## Action
1. Mediation (Leadership & Emotional Intelligence)
	- Separate 1:1 discussions: Listened to both parties to understand:
	- Reviewer’s concerns (e.g., scalability, maintainability).
	- Writer’s rationale (e.g., performance, timeline).
	- Identified common ground: Both wanted a robust, efficient solution.
2. Collaborative Problem-Solving (Teamwork & Influence)
	- Facilitated a joint session to:
	- Combine strengths of both designs (e.g., adopted the reviewer’s modular structure while preserving the writer’s core logic).
	- Agree on compromise criteria (e.g., "If X benchmark fails, we pivot to Reviewer’s approach").
3. Process Improvement (Preventive Leadership)
	- Created an MR Review Guideline to:
	- Clarify when LLD changes are mandatory vs. optional.
	- Require objective justifications (e.g., metrics, trade-offs) for design disputes.
	- Introduce early design syncs for high-stakes changes.
## Result
- Short-term: The merged solution reduced latency by 15% while improving modularity.
- Long-term:
	- 50% fewer review conflicts after implementing the new guideline.
	- Stronger team culture: Both engineers later collaborated on a high-impact project.
- Leadership impact: The guideline was adopted by 2 other teams


# STORY 5: Tech Debts over Feature

## Bullet Points:
1) 40% of our bandwidth of BE Team and hectic oncall
2) Feature, believing it was critical for customer retention
3) **Balance immediate business needs** with **long-term system health**.
4) Created DOC for RCA of top production issues
5) Collaborated with engineers prioritize tech debts
6) Created doc to compare the cost of feature delay vs Tech Debt delay
7) Proposed a win win case 70% tech debt vs 30% P0 feature 
8) Pushed back on "just ship it" pressure, Proposing a compromise sprint
9) 50% reduction in incidents in 2 weeks, Delivered the high-priority feature
10) 20% "tech debt budget
## Situation
My engineering team was in a **crisis state**:
- **40% of our bandwidth** was consumed by fixing recurring production issues, leading to:
    - **Exhausted on-call rotations** (3–5 incidents/week).
    - **Missed feature deadlines** due to constant firefighting.        
- The **product team** aggressively pushed for a new high-priority feature, believing it was critical for customer retention.    

**The Conflict**:  
Tech Team: "We can't keep stacking features on a shaky foundation—we need to address tech debt first."  
Product Team: "If we delay this feature, we risk losing market share."

## Task
As the **Tech Lead**, I needed to:
1. **Balance immediate business needs** with **long-term system health**.
2. **Align both teams** on a sustainable roadmap.
3. **Prevent ethical compromises** (e.g., sacrificing stability for short-term gains).
## Action
1. Leadership & Data-Driven Advocacy
	- Created a comprehensive document outlining:
    - **Root-cause analysis** of top production issues (e.g., 60% stemmed from unaddressed tech debt).
    - **Cost of delay**: Estimated 30% more incidents if the feature launched without fixes.
    - **Win-win proposal**: Dedicate 70% of capacity to tech debt + 30% to **one** high-priority feature.

**2. Teamwork & Influence**
- **Collaborated with engineers** to:
    - Prioritize tech debts (e.g., logging overhaul, idempotency fixes).
    - Identify **quick wins** (e.g., automated rollbacks to reduce on-call load).
- **Influenced product leaders** by:
    - Translating tech debt into business risks (e.g., "This API flakiness will cause 15% checkout failures during peak sales").
    - Aligning the roadmap to **customer-impact metrics** (e.g., "Stability improvements will reduce churn by 5%").

**3. Resilience & Ethical Conflict Mitigation**
- **Pushed back** on "just ship it" pressure by:
    - Highlighting **reputational risks** (e.g., "More outages could trigger SLA penalties").
    - Proposing a **compromise sprint** (tech debt + 1 feature) as a trial.
## Result
- Short-term:
    - **50% reduction in incidents** within 2 sprints.
    - Delivered the **high-priority feature** with zero stability regressions.
- **Long-term**:
    - **On-call fatigue dropped by 70%**, improving team morale.
    - Established a **20% "tech debt budget"** in every sprint (adopted org-wide).
    - **Stronger partnership**: Product team now invites engineers to **early roadmap planning**.

**Key Takeaway**: This conflict taught me that **transparent data, empathy for both sides, and framing tech health as a business priority** can turn adversarial debates into collaborative wins.

# STORY 6: Wallet Adapter
## Bullet Points
**Leadership:**
- Took initiative to analyze 15+ payment APIs and identify common patterns
- Created comprehensive proposal with architecture and ROI analysis
- Presented strategic vision to senior leadership with milestones
- Built and led a dedicated 4-engineer task force
- Established transparent progress dashboards for stakeholders

**Resilience:**
- Persisted through initial stakeholder skepticism
- Solved complex technical challenges in abstraction layer
- Maintained team morale during 3-month development with no immediate outputs
- Balanced framework development with BAU operations
- Delivered on ambitious timeline despite technical hurdles
**Teamwork:**
- Formed and coordinated cross-functional team of 4 engineers
- Conducted weekly alignment syncs with product/business teams
- Created developer handbook and onboarding materials
- Trained non-specialists to use the framework
- Established knowledge sharing practices
**Influence:**
- Convinced leadership to approve 3-month investment
- Translated technical solution into business ROI (90% time reduction)
- Aligned product/engineering on strategic pause for long-term gain
- Socialized progress through executive dashboards
- Made framework adoption easy through CLI tool
**Ethical Conflict:**
- Balanced CTO's aggressive timeline with engineering quality
- Allocated resources away from immediate feature work
- Ensured no compromise on payment security/compliance
- Managed tension between speed and sustainability
- Justified short-term delays with long-term value evidence
**Additional Impact Points:**
- 90% integration time reduction (2mo→2wks)
- Enabled 28+ subsequent integrations
- $4M+ estimated annual engineering savings
- CLI tool cut onboarding from 3wks→3days
- Became engineering excellence benchmark
- Accelerated ASEAN market expansion    
- Framework still in use years later

## Situation:  
During my tenure at GrabPay, our CTO established an ambitious OKR to integrate all major payment methods across ASEAN countries - a critical initiative for market expansion. The traditional integration process was painfully slow, with each new payment method requiring approximately 2 months of development time. After completing my first integration (which indeed took the expected 2 months), I was assigned another similar integration task.

## Task:  
Facing this repetitive work, I recognized a strategic opportunity. While the immediate priority was delivering integrations quickly, I saw that investing in automation could transform our long-term capabilities. However, this would require convincing stakeholders to accept short-term delays for substantial future benefits.

## Action (Demonstrating Key Points):
Leadership:  
I took initiative by:
- Conducting thorough analysis of 15+ payment APIs, identifying 80% common patterns (auth, capture, refund flows)
- Creating a comprehensive proposal detailing the framework's architecture and ROI
- Presenting to senior leadership with clear milestones and risk mitigation plans

Resilience:  
The project demanded persistence through:
- Initial skepticism from stakeholders focused on immediate deliverables
- Complex technical challenges in creating a universal abstraction layer
- Maintaining team morale during the 3-month development period with no immediate visible outputs

Teamwork & Influence:  
I collaborated effectively by:
- Building a dedicated 4-engineer task force while maintaining BAU operations
- Conducting weekly syncs with product and business teams to maintain alignment
- Creating transparent progress dashboards to build confidence in our approach

Ethical Conflict:  
We navigated tough decisions including:
- Balancing the CTO's aggressive timeline with engineering best practices
- Allocating resources away from immediate feature development
- Ensuring the solution wouldn't compromise payment security or compliance

## Result:  
Our investment paid remarkable dividends:
- 90% reduction in integration time (2 months → 2 weeks)
- Framework enabled 28+ subsequent integrations with consistent quality
- Developer Experience Revolution:
	- CLI tool reduced onboarding time from 3 weeks to 3 days
	- Handbook became part of standard developer onboarding
	- Enabled non-payment specialists to contribute to integrations
- Business Impact:
	- Accelerated Grab's market expansion across 6 ASEAN countries
	- Estimated $4M+ annual savings in engineering costs
	- Became a benchmark for engineering excellence within GrabFin


# STORY 7:  Stats Cost Optimisation
## Situation:  
As Tech Lead responsible for infrastructure cost optimization (a key OKR), I discovered our time-series database costs were consuming 23% of our monthly cloud bill - approximately $85,000/month. Initial investigations revealed:

- Over 12 million unnecessary metrics being collected daily
- Alert queries running 10x more frequently than needed
- A systemic library issue causing metric duplication across 78 microservices

## Task:  
I needed to:
1. Immediately reduce costs without disrupting monitoring
2. Solve the root cause rather than apply temporary fixes
3. Ensure organization-wide adoption of the solution

## Action (Demonstrating Key Points):
Leadership:

- Took ownership by declaring a cost optimization sprint as tech debt
- Developed a three-phase plan:
	1. Surface-level optimizations (query frequency, metric pruning)
	2. Architectural improvement (library fix)
	3. Organization-wide rollout

Resilience:
- Persisted when initial 15% cost reduction proved insufficient
- Conducted deep forensic analysis of metric collection libraries

Teamwork & Influence:
- Collaborated with SREs to identify safe optimization thresholds
- Partnered with PM to create Jira tracking dashboard
- Instituted weekly cross-team syncs with 12 other tech leads
- Developed migration playbook and troubleshooting guide

Ethical Conflict:
- Balanced cost savings against monitoring reliability risks

Innovation:
- Discovered and fixed library-level metric duplication bug
- Created automated metric audit tool to prevent regression
- Designed gradual rollout strategy with kill switches

## Result:
- 62% reduction in time-series database costs ($52,700/month saved)
- Library fix adopted by 92% of services within 2 months
- New standards implemented for metric collection
- Ongoing savings: The solution continues saving ~$630,000 annually
- Process improvement: Metric audits now part of release checklist


# STORY 8: Automatic Dashboard Creation
## Situation:  
At our company with 150+ microservices, I noticed a critical visibility gap:
- Each service team manually maintained Grafana dashboards
- 60% of dashboards were outdated or missing key metrics
- Incident resolution was delayed by 30+ minutes due to inconsistent monitoring
- Engineers wasted ~15 hours/week recreating similar dashboards

## Task:  
I set out to create a standardized, automated dashboard solution that would:
1. Provide consistent observability across all services
2. Eliminate manual dashboard maintenance
3. Preserve flexibility for service-specific metrics

## Action (Demonstrating Key Points):

Leadership:
- Proposed and championed the automation initiative as part of our SLO improvement program
- Established design principles: "Convention over configuration" for common metrics while allowing custom extensions
- Created an adoption roadmap with milestones and success metrics

Innovation & Technical Solution:

1. Pattern Identification:
	- Analyzed 85% of our services and identified universal entry/exit points
	- Designed a standardized metrics interface covering:
	- Entry: HTTP APIs (status codes, latency), gRPC methods, Kafka consumers    
	- Exit: External calls, DB queries, cache operations, message publishing

3. Library Implementation:
	- Developed an auto-instrumentation library that:
	- Generates Prometheus metrics out-of-the-box
	- Provides hooks for service-specific metrics
	- Self-registers dashboards on service startup
	- Implemented dashboard versioning to support graceful migrations

5. Visualization Automation:
	- Created template-based Grafana dashboard generator
	- Added dynamic panel configuration based on service capabilities
	- Built automated documentation for all generated dashboards

Teamwork & Influence:
	- Conducted "brown bag" sessions to demonstrate value to engineering teams
	- Worked with Tech Leads to identify pilot services
	- Established a guild of dashboard "champions" across teams
	- Created migration guides and troubleshooting playbooks

Resilience:
	- Overcinitial resistance from teams with "special snowflake" dashboards
	- Iterated based on feedback (3 major versions in 2 months)
	- Maintained legacy dashboard support during transition

Ethical Conflict:
	- Balanced standardization needs with teams' desire for customization
	- Ensured no loss of existing visibility during migration
	- Addressed security concerns about auto-created dashboards

## Result:
- 100% adoption across 150+ services within 2 months
- Zero manual dashboard maintenance overhead
- Incident detection time improved by 40%
- Saved 1,800+ engineering hours/year in dashboard maintenance
- Became the foundation for our service maturity framework
- Later extended to support automated SLO reporting


# STORY 9: Deal with Negative Review
**Project Nugget:** Turned feedback about being overly prescriptive into a more collaborative leadership style.
### **S – Situation**
During my tenure as Tech Lead for Digital Lending, I prided myself on maintaining high code quality through rigorous reviews. However, during a sprint retrospective, a senior engineer shared surprising feedback: _"Your code reviews feel like a gatekeeping exercise - you dictate solutions rather than discuss them."_ This was echoed by 2 other team members, revealing a broader perception issue.
### **T – Task**
I needed to:
1. Process this criticism without defensiveness (+)
2. Diagnose why my good intentions (quality control) were perceived as micromanagement (+)
3. Transform my approach to foster psychological safety while preserving standards (+)
### **A – Action**
####  Leadership:
- Scheduled 1:1s with all 5 team members to gather nuanced perspectives (+)
- Recognized my "teacher-student" dynamic was inhibiting junior engineers' growth
- Publicly acknowledged the feedback in our next team meeting, modeling vulnerability (+)
#### Resilience:
- Resisted the urge to justify my original approach (+)
- Experimented with 3 different review styles over 4 weeks to find the right balance
- Persisted when initial changes felt unnatural after years of habitual behavior
#### Teamwork & Influence:
- Introduced collaborative review sessions where:
	- I framed comments as open questions ("What if we...?" vs "Change this")
	- Encouraged alternatives before suggesting solutions
- Created a code review rubric co-designed with the team (+)
- Scheduled Coding Principal Discussion call to skillup the teammates.
#### Ethical Conflict:
- Balanced my responsibility for system stability with empowering the team
- Ensured architectural guardrails weren't compromised in pursuit of consensus
### **R – Result**
- Team engagement improved noticeably—feedback became a two-way street.
- Engineers started owning design decisions more confidently.
- We maintained code quality while increasing trust and collaboration across the team.

# Story 10: UnDocumented design
### **Situation:**
As a **Lead Engineer**, I was responsible for ensuring our lending platform’s design documentation matched the actual production system. While we maintained thorough documentation for major design changes, minor updates (like small feature tweaks or performance optimizations) were often implemented without being documented, as they seemed insignificant at the time.

### **Task:**
When **KPMG initiated an audit** for compliance, I realized our documentation had drifted significantly from the live system due to these undocumented changes. With only **five days** before the audit review, I had to **align all design documents with production** to avoid compliance risks.

### **Action:**
1. **Immediate Assessment:** I quickly audited the system myself, listing all undocumented changes.
2. **Team Mobilization:** I pulled in key developers to help reconstruct the logic behind each unrecorded modification.
3. **Prioritization:** Focused first on high-risk areas (e.g., interest calculations, data flows) that auditors would scrutinize.
4. **Documentation Sprint:** Worked late hours to update all design docs, adding clear notes on why changes were made.
5. **Verification:** Had a senior engineer review the updates to ensure accuracy before submission.

### **Result:**
- We **submitted compliant documentation** on time, passing the audit without major findings.
- The auditors noted our responsiveness and thoroughness in remediation.
- **Process Improvement:** After this, I enforced a rule that **no change—no matter how small—goes undocumented**. I also introduced:
    - A **lightweight change-log system** for quick updates.
    - Biweekly **documentation syncs** between engineering and QA.