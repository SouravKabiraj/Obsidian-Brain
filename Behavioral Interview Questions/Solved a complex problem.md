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