
---

# **Acrosoft Release Notes**

## **Document Control**

| Field                | Detail                      |
| -------------------- | --------------------------- |
| **Product**          | Acrosoft Platform           |
| **Document Version** | 1.1 (Enterprise Revision)   |
| **Product Version**  | v1.0.0                      |
| **Release Date**     | November 3, 2025            |
| **Last Updated**     | November 2025               |
| **Author**           | Hashim Zaffar               |
| **Reviewed By**      | —                           |
| **Approved By**      | —                           |
| **Status**           | Published                   |
| **Confidentiality**  | Public / Partner / Internal |

---

## **1. Overview**

This document summarizes updates, improvements, bug fixes, and known issues for the **Acrosoft Platform**.
It is intended for stakeholders, project managers, QA, DevOps, support, and clients to track changes and plan deployments.

**Versioning:** Semantic Versioning (`MAJOR.MINOR.PATCH`).
**Channels:** Alpha → Beta → Production.
**Artifacts:** Container images, static front-end bundles, and IaC templates.

---

## **2. Release Summary**

| Version          | Release Date | Environment | Status     |
| ---------------- | ------------ | ----------- | ---------- |
| **v1.0.0**       | Nov 3, 2025  | Production  | Active     |
| **v0.9.0-beta**  | Oct 2025     | Staging     | Deprecated |
| **v0.8.0-alpha** | Aug 2025     | Development | Deprecated |

---

## **3. Release: v1.0.0 (Production)**

### 3.1 Environments

* **Frontend:** `https://app.acrosoft.io`
* **Backend API:** `https://api.acrosoft.io/v1`
* **Database:** MongoDB Atlas (Cluster-Prod-us-east-1)
* **Hosting:** AWS Elastic Beanstalk, CloudFront CDN

### 3.2 Overview

Initial production launch of the Acrosoft platform.
Focus on project management, collaboration, integrations, and analytics.

### 3.3 Highlights

* Client portal with role-based access
* Kanban task management and milestones
* Slack and GitHub integrations
* Real-time notifications and analytics dashboard

---

## **4. What Is New**

| Feature                     | Description                                       | Module         |
| --------------------------- | ------------------------------------------------- | -------------- |
| **Client Portal**           | Client login, project requests, progress tracking | Projects       |
| **Task Management**         | Kanban board with statuses and assignees          | Tasks          |
| **Roles and Permissions**   | RBAC for Admin, Manager, Developer, Client        | Auth           |
| **Slack Integration**       | Notifications to target channels                  | Integrations   |
| **GitHub Sync**             | Repos, commits, and pull requests linked          | Integrations   |
| **Analytics Dashboard**     | Velocity, deadlines, performance trends           | Analytics      |
| **Email Notifications**     | Alerts for updates and deadlines                  | Notifications  |
| **File Upload and Sharing** | S3-backed secure storage                          | Collaboration  |
| **Documentation Hub**       | Centralized links to project docs                 | Knowledge Base |

---

## **5. Improvements**

| Area              | Change                                                          |
| ----------------- | --------------------------------------------------------------- |
| **Performance**   | API responses improved by about 30 percent through caching      |
| **Security**      | JWT rotation and HTTPS enforced across all endpoints            |
| **UI/UX**         | Simplified navigation and dark mode                             |
| **CI/CD**         | Automated tests and GitHub Actions deployments                  |
| **Notifications** | Real-time in-app notifications using WebSockets                 |
| **Docs**          | Standardized PRD, SAD, API, User Guide, Install Guide templates |

---

## **6. Bug Fixes**

| Issue ID | Description                                           | Status |
| -------- | ----------------------------------------------------- | ------ |
| **#101** | 500 error when filtering projects by date             | Fixed  |
| **#104** | Slack webhook not sending updates for completed tasks | Fixed  |
| **#109** | Wrong timezone in dashboard analytics                 | Fixed  |
| **#112** | File upload failing due to missing MIME validation    | Fixed  |
| **#118** | Broken link in user settings                          | Fixed  |
| **#120** | JWT expiry not refreshing on login                    | Fixed  |

---

## **7. Security Notes**

* TLS 1.2 or later enforced
* AES-256 encryption at rest for storage
* Pen test passed with no critical findings
* Audit logging enabled for admin actions

No CVE patches included in this release beyond platform dependencies already updated during the beta cycle.

---

## **8. Compatibility and Upgrade Notes**

| Area         | Change Type         | Action                                                         |
| ------------ | ------------------- | -------------------------------------------------------------- |
| **API**      | Backward compatible | No breaking changes in `/v1`                                   |
| **Auth**     | Token behavior      | JWT rotation enabled. Ensure clients handle refresh token flow |
| **Config**   | New env keys        | `SENTRY_DSN` optional in production                            |
| **Database** | No schema breaks    | New indexes recommended for performance (see Install Guide)    |

**Client Action:** Verify OAuth scopes and webhook secrets for integrations.

---

## **9. Known Issues**

| Issue ID | Description                                    | Impact | Workaround                  | Target |
| -------- | ---------------------------------------------- | ------ | --------------------------- | ------ |
| **#201** | Email notifications delayed up to 15 minutes   | Low    | Emails still arrive         | v1.1   |
| **#205** | Project progress bar not updating in real time | Medium | Refresh page                | v1.1   |
| **#208** | Some users are logged out after about 1 hour   | Medium | Refresh token patch pending | v1.0.1 |

---

## **10. Planned Features**

| Feature               | Description                       | Status         |
| --------------------- | --------------------------------- | -------------- |
| **Stripe Billing**    | Invoicing and payment tracking    | In development |
| **AI Estimator**      | Predict project timelines         | In research    |
| **Mobile App**        | Native iOS and Android client     | Planned        |
| **Kubernetes on EKS** | Container orchestration migration | Planned        |
| **Audit Logs**        | Track admin actions and changes   | In design      |

---

## **11. Deployment Notes**

### 11.1 Pre-deployment Checks

* Tests passed for unit and integration
* Load test at 500 concurrent users
* MongoDB Atlas backups verified
* SSL certificates valid to Nov 2026
* IAM roles reviewed and scoped to least privilege

### 11.2 Post-deployment Verification

* `/v1/health` returns 200
* CDN serves latest assets with correct cache headers
* Analytics tiles load in under 5 seconds
* Slack notifications for task updates fire correctly
* New user registration and login succeed

---

## **12. Rollout Plan**

| Step            | Audience            | Detail                                               |
| --------------- | ------------------- | ---------------------------------------------------- |
| Phased canary   | 10 percent traffic  | Monitor 5xx, latency p95, and error rates for 1 hour |
| Regional expand | 50 percent traffic  | Validate notifications and webhook deliveries        |
| Full release    | 100 percent traffic | Enable remaining feature flags                       |

Feature flags used: `realtime_notifications`, `dark_mode`. Flags can be disabled in case of issues.

---

## **13. Rollback Plan**

1. Notify DevOps in **#acrosoft-ops**
2. Revert to previous Elastic Beanstalk version label
3. Invalidate CloudFront cache for frontend rollback
4. If data impact is suspected, restore from the latest Atlas snapshot
5. Announce status in **#acrosoft-status** and email impacted clients

**Estimated recovery time:** under 2 hours.

---

## **14. Release Artifacts**

| Component    | Artifact                                     | Identifier                                     |
| ------------ | -------------------------------------------- | ---------------------------------------------- |
| **API**      | Docker image                                 | `ghcr.io/acrosoft/api:v1.0.0`                  |
| **Frontend** | Static bundle checksum                       | `sha256: <bundle-hash>`                        |
| **IaC**      | EB config and CloudFront invalidation script | `iac/eb-prod-v100`, `scripts/cf-invalidate.sh` |
| **Docs**     | Tagged set                                   | `docs@v1.0.0`                                  |

> Store checksums and SBOMs in the release tag for audit.

---

## **15. Monitoring and SLOs**

| Metric           | SLO                         | Alert Threshold               |
| ---------------- | --------------------------- | ----------------------------- |
| Availability     | 99.9 percent monthly        | 5xx > 2 percent for 5 minutes |
| Latency p95      | < 800 ms                    | p95 > 800 ms for 5 minutes    |
| Error rate       | < 1 percent                 | > 1.5 percent for 10 minutes  |
| Webhook delivery | 99 percent under 30 seconds | Failure rate > 2 percent      |

Dashboards: CloudWatch and Sentry.
Alerts: Pager rotation A for production hours.

---

## **16. Support and Communication**

* Support hours: Monday to Friday, 9 AM to 6 PM UTC
* Contact: [support@acrosoft.io](mailto:support@acrosoft.io) or in-app chat
* Client communication: Release email and status page update on launch day

---

## **17. Version History**

| Version          | Release Date | Summary                               |
| ---------------- | ------------ | ------------------------------------- |
| **v1.0.0**       | Nov 2025     | Production release with core features |
| **v0.9.0-beta**  | Oct 2025     | Beta with limited users               |
| **v0.8.0-alpha** | Aug 2025     | Internal prototype                    |

---

## **18. Appendices**

### 18.1 Issue References

* Internal tracker: `ACR-101`, `ACR-104`, `ACR-109`, `ACR-112`, `ACR-118`, `ACR-120`
* Known issues: `ACR-201`, `ACR-205`, `ACR-208`

### 18.2 Glossary

| Term       | Definition                                    |
| ---------- | --------------------------------------------- |
| **RBAC**   | Role-Based Access Control                     |
| **SLO**    | Service Level Objective                       |
| **SBOM**   | Software Bill of Materials                    |
| **Canary** | Phased rollout to a small percentage of users |

---

## **19. Related Documents**

* [01_Product_Requirements_Document.md](./01_Product_Requirements_Document.md)
* [02_System_Architecture.md](./02_System_Architecture.md)
* [03_API_Documentation.md](./03_API_Documentation.md)
* [04_User_Guide.md](./04_User_Guide.md)
* [05_Installation_Configuration_Guide.md](./05_Installation_Configuration_Guide.md)
* [08_Style_Guide.md](./08_Style_Guide.md)

---

**End of Document**
