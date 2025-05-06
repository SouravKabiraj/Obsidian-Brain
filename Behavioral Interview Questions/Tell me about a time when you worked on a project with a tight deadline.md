**Situation:**  
We had to integrate an ML scoring model into our loan system under a tight 4-week deadline, but MLOps API performance SLAs were unclear.  

**Task:**  
Decide between a sync (simple but risky) or async (scalable but slower) approach without delaying launch.  

**Action:**  
- Proposed a 2-phase plan: **sync API for MVP** (meet deadline), then **optimize to async post-launch**.  
- Clearly documented trade-offs (e.g., latency risks) and got buy-in from leadership.  
- Added monitoring to catch issues early.  

**Result:**  
- Delivered on time with the MVP; later improved latency by 40% with async.  
- Leadership trusted the approach due to transparency and results.  

**Key Highlight:**  
Balanced speed vs. quality by planning for both short-term wins *and* long-term fixes.


[IMP_NOTE]
```
Iron Triangle:
Project Management Iron Triangle**

In project management, the Iron Triangle refers to the three key constraints that affect every project:

- Scope – The work that needs to be done.
- Time – The schedule or deadline.
- Cost – The budget allocated.
    

The principle states that changing one constraint affects the others. For example:

- If scope increases, time or cost must also increase.
- If time is reduced, cost may rise or scope may shrink.

This model helps managers balance trade-offs to deliver a successful project.
```
