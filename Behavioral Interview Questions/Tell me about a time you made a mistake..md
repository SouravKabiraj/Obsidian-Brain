# Situation
While optimising a high-volume batch job to improve processing speed, I focused solely on our system’s performance without considering the downstream external system’s capacity limits.
# Task
The goal was to reduce job runtime by 50%, but the changes inadvertently increased the load on an external API beyond its rate limits.
# Action
When the external system failed during production
Immediately rolled back the optimisation to restore service.
Collaborated with the external team to analyse their system’s thresholds.
Redesigned the job with incremental load distribution and added rate-limiting controls.
Conducted load testing in a staging environment to validate the fix.
# Result
The external system remained stable post-fix, and the job’s runtime still improved by 30% (slightly less than the original target but sustainable).

I implemented a checklist for future optimisations, including performance testing, dependency mapping and stakeholder review, preventing similar issues.

#accountability #problem-solving #learn