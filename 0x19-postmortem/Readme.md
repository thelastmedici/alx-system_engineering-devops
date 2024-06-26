## Postmortem: Web Application Outage

### Issue Summary
**Duration of Outage:**
- Start: June 20, 2024, 2:15 PM UTC
- End: June 20, 2024, 4:45 PM UTC
- Total Duration: 2 hours 30 minutes

**Impact:**
- Service affected: User authentication and login service.
- User Experience: Users were unable to log into their accounts, resulting in a complete service disruption for 70% of our user base.
- Affected Users: Approximately 70% of active users experienced the issue.

**Root Cause:**
- A misconfiguration in the database connection pool settings caused the authentication service to exceed its maximum connection limit, leading to a denial of service.

### Timeline

- **2:15 PM:** Issue detected through automated monitoring alerts indicating a spike in authentication failures.
- **2:20 PM:** On-call engineer begins investigation, confirms the issue through user reports and logs.
- **2:30 PM:** Initial assumption: Recent code deployment might have caused the issue. Code rollback initiated.
- **2:45 PM:** Code rollback completed; issue persists. Focus shifts to infrastructure.
- **3:00 PM:** Database logs reviewed; no recent changes observed. Suspected network issue.
- **3:20 PM:** Network team investigates; no anomalies found. Misleading path abandoned.
- **3:40 PM:** Escalation to senior engineers. Detailed review of application logs.
- **4:00 PM:** Root cause identified as a misconfiguration in the database connection pool settings.
- **4:15 PM:** Configuration updated to increase connection pool size. 
- **4:30 PM:** Authentication service restarted.
- **4:45 PM:** Service restored. Monitoring confirms resolution, and affected users can log in again.

### Root Cause and Resolution

**Root Cause:**
The root cause of the outage was a misconfiguration in the database connection pool settings. The maximum number of database connections was set too low, which caused the connection pool to be exhausted quickly under high user load. This led to authentication requests being queued and eventually timing out, resulting in a service outage.

**Resolution:**
To resolve the issue, the database connection pool settings were reviewed and updated to accommodate a higher number of connections. The maximum connection limit was increased from 100 to 500, allowing the authentication service to handle more concurrent connections without exhausting the pool. After updating the configuration, the authentication service was restarted, which restored normal functionality.

### Corrective and Preventative Measures

**Improvements and Fixes:**
1. **Increase Database Connection Pool Size:** Update and monitor database connection pool settings to ensure they can handle peak loads.
2. **Enhance Monitoring:** Implement more granular monitoring for database connection metrics, including connection pool utilization and wait times.
3. **Load Testing:** Conduct regular load testing to identify potential bottlenecks and configuration issues in the authentication service.
4. **Incident Response Training:** Provide additional training for the engineering team on identifying and resolving database-related issues.

**Task List:**
- [ ] Update database connection pool settings to a higher limit.
- [ ] Implement detailed monitoring for database connection pool metrics.
- [ ] Schedule regular load testing sessions for the authentication service.
- [ ] Conduct a training session on database connection management and incident response.
- [ ] Review and update incident response documentation to include steps for database connection issues.

By implementing these measures, we aim to prevent similar outages in the future and improve the overall reliability and resilience of our authentication service.
