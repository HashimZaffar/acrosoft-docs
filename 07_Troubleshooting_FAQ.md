
---

# **Acrosoft Troubleshooting & FAQ**

## **Document Control**

| Field               | Detail                           |
| ------------------- | -------------------------------- |
| **Product**         | Acrosoft Platform                |
| **Guide Version**   | 1.1 (Enterprise Revision)        |
| **Product Version** | v1.0                             |
| **Last Updated**    | November 2025                    |
| **Author**          | Hashim Zaffar                    |
| **Reviewed By**     | —                                |
| **Approved By**     | —                                |
| **Status**          | Published                        |
| **Audience**        | Clients, Developers, QA, Support |
| **Confidentiality** | Public / Partner / Internal      |

---

## **1. Purpose**

Help users **identify, diagnose, and resolve** common issues on the Acrosoft Platform.
If your issue is not listed, contact **[support@acrosoft.io](mailto:support@acrosoft.io)** or use in-app **Support Chat**.

---

## **2. Before You Start: Self-Help Checklist**

1. **Status page**: check outages at **[https://status.acrosoft.io](https://status.acrosoft.io)**
2. **Hard refresh**: clear cache and cookies, or try **Incognito**
3. **Browser**: use a supported version and disable extensions temporarily
4. **Connection**: stable internet 5 Mbps or higher
5. **Permissions**: confirm your **role** and **project access**
6. **Reproduce**: note steps, expected vs actual results, time, and screenshots

---

## **3. Standard Triage Flow (Support Use)**

1. **Classify impact**

   * Critical: outage or data loss
   * High: major feature broken
   * Medium: degraded experience
   * Low: question or cosmetic
2. **Collect diagnostics** (see Section 15)
3. **Check recent changes**: deployments, feature flags, config
4. **Verify health**

   * API: `GET https://api.acrosoft.io/v1/health`
   * App load via CDN
5. **Review logs**: Sentry, CloudWatch, application logs
6. **Apply fix or escalate** (Section 16)

---

## **4. Login and Authentication**

### 4.1 “Invalid Credentials”

**Cause**: Wrong email or password.
**Fix**

* Verify credentials
* Use **Forgot Password**
* Ask support to check account status

### 4.2 “Session Expired”

**Cause**: JWT lifetime reached.
**Fix**

* Sign out and sign in again
* Ensure cookies are enabled and not blocked

### 4.3 “Too Many Login Attempts”

**Cause**: Rate limit exceeded.
**Fix**

* Wait 15 minutes
* Reset password if needed
* Support can unlock accounts as required

### 4.4 SSO Cannot Complete

**Cause**: IdP misconfiguration or clock skew.
**Fix**

* Ask Admin to validate SSO metadata and certificate
* Ensure device time is auto synced
* Try a normal login to isolate SSO

---

## **5. Projects and Tasks**

### 5.1 Project Missing from Dashboard

**Cause**: Filters or permissions.
**Fix**

* Clear filters. Check Active and Completed
* Confirm project is not Archived
* Ask PM to verify access

### 5.2 “Failed to Create Project”

**Cause**: Required field or date format error.
**Fix**

* Complete all required fields
* Use `YYYY-MM-DD` dates

### 5.3 Tasks Do Not Update

**Cause**: Temporary sync delay or network issue.
**Fix**

* Refresh after a few seconds
* Check internet connection
* Report with project ID if repeatable

### 5.4 File Upload Fails

**Cause**: Size, format, or S3 delay.
**Fix**

* Max 20 MB per file
* Allowed: PDF, DOCX, ZIP, PNG, JPG
* Retry after 5 minutes

---

## **6. Integrations**

### 6.1 Slack Notifications Not Received

**Cause**: Revoked webhook or channel mapping.
**Fix**

1. **Settings → Integrations → Slack**
2. Reconnect and select the project channel
3. Send a test message

### 6.2 GitHub Not Syncing

**Cause**: Token scope or repo permission.
**Fix**

* Reconnect with token that has `repo` and `workflow` scopes
* Check GitHub webhooks on the repository
* Confirm the repo is mapped to the correct project

### 6.3 Email Delays

**Cause**: Queue backlog or SendGrid throttling.
**Fix**

* Check spam or Promotions
* Delays over 15 minutes appear in **Analytics → Alerts**
* If chronic, contact support

---

## **7. Performance and Connectivity**

### 7.1 Slow Dashboard

**Cause**: Cache or heavy traffic.
**Fix**

* Clear cache or use Incognito
* Use latest browser
* Check **status.acrosoft.io** for maintenance

### 7.2 “Server Unreachable”

**Cause**: Network or API downtime.
**Fix**

* Verify internet
* Check: `https://api.acrosoft.io/v1/health`
* Share timestamp and console logs with support

---

## **8. Security and Access**

### 8.1 “Access Denied”

**Cause**: RBAC restriction.
**Fix**

* Confirm your role
* Request updated permissions from Admin or PM

### 8.2 2FA Issues

**Cause**: Time drift or code mismatch.
**Fix**

* Sync device time
* Re add 2FA using an authenticator app
* Store backup codes safely

---

## **9. API and Webhooks**

### 9.1 401 Unauthorized

**Cause**: Expired or missing token.
**Fix**

* Use `Authorization: Bearer <jwt>`
* Refresh token via `/auth/refresh`

### 9.2 429 Too Many Requests

**Cause**: Rate limit exceeded.
**Fix**

* Read headers `X-RateLimit-*`
* Back off and retry after reset time

### 9.3 Webhook Not Received

**Cause**: Target endpoint blocked or signature mismatch.
**Fix**

* Verify secret and compute HMAC of the raw body
* Check `X-Acrosoft-Signature` and `X-Acrosoft-Event-ID`
* Confirm your endpoint returns `2xx` within 5 seconds

**Test registration**

```http
POST /webhooks
Content-Type: application/json

{
  "url": "https://clientapp.io/webhook",
  "events": ["project.updated", "task.assigned"],
  "secret": "your-secret"
}
```

---

## **10. Dev and Deployment**

### 10.1 Cannot Connect to Database

**Cause**: Wrong `MONGODB_URI` or allowlist.
**Fix**

* Check `.env` and credentials
* Ensure Atlas IP allowlist or VPC peering
  **Quick check**

```bash
mongosh "mongodb+srv://<user>:<pass>@cluster.mongodb.net/acrosoft"
```

### 10.2 Environment Variables Not Loading

**Cause**: Missing `.env` or process not restarted.
**Fix**

* `.env` must be at repo root
* Restart server:

```bash
npm run dev
```

### 10.3 Docker Container Crashes

**Cause**: Port conflict or memory limits.
**Fix**

```bash
docker stop $(docker ps -aq)
docker-compose up --build
```

---

## **11. Known Limits**

| Item            | Limit                            |
| --------------- | -------------------------------- |
| File size       | 20 MB per file                   |
| Comment length  | 10,000 characters                |
| Webhook timeout | 5 seconds per delivery           |
| Rate limit      | 120 requests per minute per user |

---

## **12. Supported Browsers**

| Browser | Version |
| ------- | ------- |
| Chrome  | 110+    |
| Firefox | 100+    |
| Edge    | 110+    |
| Safari  | 15+     |

---

## **13. FAQs**

**Can clients message developers**
Yes. Use in app chat in each project, or Slack if connected.

**How often is data backed up**
Daily Atlas snapshots with 30 day retention. S3 uses versioning.

**Can I export project data**
Yes. **Projects → Export → CSV or PDF**.

**How do I report a bug**
**Help → Report Issue**. Include steps, screenshot, browser, OS, and time.

**Is there a mobile app**
Mobile app is planned for Q2 2026.

**What is uptime**
Target uptime is **99.9 percent**. See **status.acrosoft.io**.

---

## **14. Troubleshooting Playbooks (Support Use)**

### 14.1 Authentication Failures

1. Reproduce on a clean browser profile
2. Check auth logs in Sentry for the user email
3. Confirm JWT clock skew and token lifetime
4. If SSO: validate IdP metadata and cert dates
5. Provide root cause and fix steps

### 14.2 Slow Pages

1. Capture browser network waterfall and p95 load time
2. Compare CDN vs origin latency
3. Check CloudWatch for 5xx or throttles
4. Verify Redis or cache health if enabled
5. Create a Jira ticket with metrics

### 14.3 Webhook Delivery

1. Search `X-Acrosoft-Event-ID` in logs
2. Verify receiver returned `2xx`
3. Validate signature against stored secret
4. Retry event if delivery failed

---

## **15. Data to Include When Contacting Support**

Copy, fill, and paste into Support Chat or email.

```
Issue summary:
Steps to reproduce:
Expected result:
Actual result:
Timestamp (UTC):
Project ID / Task ID:
User email:
Browser and version:
Screenshots / HAR (if possible):
```

---

## **16. Severity and Escalation**

| Severity | Example                  | First Response  | Escalation                  |
| -------- | ------------------------ | --------------- | --------------------------- |
| Critical | System outage, data loss | 1 hour          | On-call SRE and Engineering |
| High     | Major feature broken     | 4 hours         | Product and Engineering     |
| Medium   | Degraded UX              | 1 business day  | Support lead                |
| Low      | Questions or cosmetic    | 2 business days | Support queue               |

**Escalation paths**

* Internal Slack: `#acrosoft-ops` for incidents, `#acrosoft-support` for tickets
* External: [support@acrosoft.io](mailto:support@acrosoft.io)

---

## **17. Privacy and Security**

* Encrypt data in transit and at rest
* Do not share secrets or full tokens in tickets
* Use redacted logs for reproduction details
* Follow org policy for data retention and deletion

---

## **18. Changelog**

| Guide Version | Date     | Notes                                             |
| ------------- | -------- | ------------------------------------------------- |
| 1.0           | Nov 2025 | Initial FAQ                                       |
| 1.1           | Nov 2025 | Enterprise revision, triage, playbooks, templates |

---

## **19. Related Documents**

* [01_Product_Requirements_Document.md](./01_Product_Requirements_Document.md)
* [02_System_Architecture.md](./02_System_Architecture.md)
* [03_API_Documentation.md](./03_API_Documentation.md)
* [04_User_Guide.md](./04_User_Guide.md)
* [05_Installation_Configuration_Guide.md](./05_Installation_Configuration_Guide.md)
* [06_Release_Notes.md](./06_Release_Notes.md)
* [08_Style_Guide.md](./08_Style_Guide.md)

---

**End of Document**
