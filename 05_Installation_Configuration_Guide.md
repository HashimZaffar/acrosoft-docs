
---

# **Acrosoft Installation & Configuration Guide**

## **Document Control**

| Field               | Detail                                    |
| ------------------- | ----------------------------------------- |
| **Product**         | Acrosoft Platform                         |
| **Guide Version**   | 1.1 (Enterprise Revision)                 |
| **Product Version** | v1.0                                      |
| **Last Updated**    | November 2025                             |
| **Author**          | Hashim Zaffar                             |
| **Reviewed By**     | —                                         |
| **Approved By**     | —                                         |
| **Status**          | Draft / Reviewed / Approved               |
| **Audience**        | Developers, DevOps, System Administrators |
| **Confidentiality** | Internal / Partner / Public               |

---

## **1. Introduction**

This guide describes how to install, configure, and deploy the **Acrosoft Platform** in local, containerized, and cloud environments.
It includes prerequisites, step-by-step procedures, validation checks, and rollback paths.

**What you will do**

* Prepare systems and accounts
* Install dependencies
* Configure environment variables and secrets
* Deploy locally, with Docker, or on AWS
* Validate health and monitoring
* Set up backup, DR, and maintenance

---

## **2. Prerequisites**

### 2.1 System Requirements

| Component   | Minimum Requirement                       |
| ----------- | ----------------------------------------- |
| **OS**      | Ubuntu 22.04 LTS / macOS 12+ / Windows 11 |
| **CPU**     | 4 cores at 2.4 GHz                        |
| **Memory**  | 8 GB RAM (16 GB recommended)              |
| **Storage** | 20 GB free disk space                     |
| **Network** | 10 Mbps stable internet                   |

### 2.2 Network and Ports

| Service                  | Port  | Direction         | Notes                           |
| ------------------------ | ----- | ----------------- | ------------------------------- |
| Web App (Next.js)        | 3000  | Inbound           | HTTP(S) to user browser         |
| API (Node.js)            | 4000  | Inbound           | REST/GraphQL                    |
| MongoDB (Atlas or local) | 27017 | Outbound to Atlas | Private peering recommended     |
| Redis (optional)         | 6379  | Inbound (private) | Cache/session                   |
| Webhooks (external)      | 443   | Inbound           | From GitHub/3rd parties via TLS |

> For production, place services behind a load balancer and restrict ports with security groups.

### 2.3 Accounts and Permissions

* **Cloud**: AWS account with permissions for EC2, S3, CloudFront, Elastic Beanstalk or EKS, IAM, CloudWatch, Secrets Manager.
* **MongoDB Atlas**: Project, cluster, IP allowlist or VPC peering.
* **GitHub**: Repo access, deploy keys, CI tokens.
* **SendGrid / Slack**: API keys with least privilege.

### 2.4 Software Dependencies

| Software             | Version              | Purpose           |
| -------------------- | -------------------- | ----------------- |
| **Node.js**          | 18.x LTS or newer    | Backend runtime   |
| **npm / yarn**       | Latest               | Package manager   |
| **MongoDB**          | 6.x (Atlas or local) | Database          |
| **Git**              | 2.35+                | Version control   |
| **Docker / Compose** | 24.x+ / v2+          | Containerization  |
| **AWS CLI**          | Latest               | Cloud deployment  |
| **EB CLI**           | Latest               | Elastic Beanstalk |
| **Postman**          | Latest               | API testing       |

---

## **3. Topology Options**

1. **Local Development**: Single machine, hot reload.
2. **Docker Compose**: Reproducible dev and QA.
3. **AWS Production**:

   * **Option A**: Elastic Beanstalk + CloudFront (simpler).
   * **Option B**: EKS (Kubernetes) with managed add-ons (advanced, roadmap).

---

## **4. Local Development Setup**

### 4.1 Clone the Repository

```bash
git clone https://github.com/acrosoft/acrosoft-platform.git
cd acrosoft-platform
```

### 4.2 Install Dependencies

```bash
npm install
# or
yarn install
```

### 4.3 Configure Environment Variables

Create `.env` at the repository root:

```env
# --- Server ---
PORT=4000
NODE_ENV=development

# --- Database ---
MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/acrosoft

# --- JWT Auth ---
JWT_SECRET=your_jwt_secret_here
JWT_EXPIRY=1h

# --- Cloud Storage ---
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_S3_BUCKET=acrosoft-bucket

# --- Integrations ---
GITHUB_API_KEY=your_github_token
SLACK_WEBHOOK_URL=https://hooks.slack.com/services/xxx/yyy/zzz
SENDGRID_API_KEY=your_sendgrid_key

# --- Observability (optional) ---
SENTRY_DSN=https://your_sentry_dsn_key
```

> Keep `.env` out of version control. Use a secrets manager in shared environments.

### 4.4 Start Dev Servers

```bash
npm run dev
```

**Expected output**

```
Server running at http://localhost:4000
Connected to MongoDB Atlas successfully.
```

**Frontend**

* Visit `http://localhost:3000`

**API Health**

* Visit `http://localhost:4000/api/v1/health`

---

## **5. Docker Setup**

### 5.1 Build and Run

```bash
docker-compose up --build
```

### 5.2 Verify Containers

```bash
docker ps
```

Expected services:

* `acrosoft-api` — backend
* `acrosoft-db` — MongoDB (if not using Atlas)
* `acrosoft-client` — frontend

### 5.3 Access

* Web app: `http://localhost:3000`
* API health: `http://localhost:4000/api/v1/health`

> For Atlas, set `MONGODB_URI` and remove the local DB service from the compose file.

---

## **6. AWS Cloud Deployment**

### 6.1 Prepare AWS Environment

```bash
aws configure
# Provide Access Key, Secret, Region (e.g., us-east-1)
```

Create S3 bucket for static assets and logs. Create an IAM user or role with least privilege.

### 6.2 Build Artifacts

```bash
npm run build
```

### 6.3 Deploy Backend with Elastic Beanstalk

```bash
eb init acrosoft-backend --platform node.js --region us-east-1
eb create acrosoft-prod
```

Set environment variables:

```bash
eb setenv NODE_ENV=production \
MONGODB_URI=mongodb+srv://prod_cluster.mongodb.net/acrosoft \
JWT_SECRET=super_secret_prod_key \
SENDGRID_API_KEY=live_key_123
```

> Store secrets in **AWS Secrets Manager** and load them at runtime where possible.

### 6.4 Deploy Frontend to S3 + CloudFront

```bash
npm run build
aws s3 sync ./build s3://acrosoft-web --delete
aws cloudfront create-invalidation --distribution-id <DIST_ID> --paths "/*"
```

### 6.5 Post-Deploy Validation

Run smoke tests:

```bash
curl -f https://api.acrosoft.io/v1/health
curl -f https://api.acrosoft.io/v1/projects -H "Authorization: Bearer <token>"
```

Check CloudWatch logs and metrics. Confirm 2xx responses and no error spikes.

---

## **7. Database Configuration**

### 7.1 Connect to MongoDB

Local:

```bash
mongod
```

Atlas quick check:

```bash
mongosh "mongodb+srv://<username>:<password>@cluster0.mongodb.net/acrosoft"
```

### 7.2 Collections

| Collection      | Purpose                 |
| --------------- | ----------------------- |
| `users`         | User profiles and roles |
| `projects`      | Project metadata        |
| `tasks`         | Task details            |
| `notifications` | In-app and email alerts |
| `activity_logs` | Auditable actions       |

### 7.3 Indexes

Create recommended indexes for performance:

```js
db.users.createIndex({ email: 1 }, { unique: true });
db.projects.createIndex({ status: 1, deadline: 1 });
db.tasks.createIndex({ projectId: 1, assignee: 1, dueDate: 1 });
db.activity_logs.createIndex({ createdAt: -1 });
```

---

## **8. Configuration Reference**

### 8.1 Required Environment Variables

| Key                 | Required | Example                       | Notes                       |
| ------------------- | -------- | ----------------------------- | --------------------------- |
| `NODE_ENV`          | Yes      | `production`                  | Affects logging and caching |
| `PORT`              | Yes      | `4000`                        | API port                    |
| `MONGODB_URI`       | Yes      | `mongodb+srv://...`           | Use SRV URI for Atlas       |
| `JWT_SECRET`        | Yes      | `***`                         | Rotate quarterly            |
| `JWT_EXPIRY`        | Yes      | `1h`                          | Token lifetime              |
| `AWS_S3_BUCKET`     | No       | `acrosoft-bucket`             | For file uploads            |
| `SENDGRID_API_KEY`  | No       | `***`                         | Email delivery              |
| `SLACK_WEBHOOK_URL` | No       | `https://hooks.slack.com/...` | Notifications               |
| `GITHUB_API_KEY`    | No       | `ghp_***`                     | Repo integration            |
| `SENTRY_DSN`        | No       | `https://...`                 | Error tracking              |

### 8.2 Secrets Management

* Use **AWS Secrets Manager** for production.
* Grant services read-only access via IAM roles.
* Never commit secrets to Git.

---

## **9. Testing and Quality Gates**

### 9.1 Unit Tests

```bash
npm run test
```

### 9.2 Integration Tests

```bash
npm run test:integration
```

### 9.3 Linting

```bash
npm run lint
```

### 9.4 Pre-deploy Checks (CI)

* Build passes
* Unit and integration tests pass
* Lint passes with no critical issues
* Image scans show no critical CVEs

---

## **10. Monitoring and Logging**

### 10.1 Sentry

Set DSN in environment. Verify an error is captured in the dashboard.

### 10.2 CloudWatch

Log groups:

```
/aws/elasticbeanstalk/acrosoft-prod/var/log/nodejs/nodejs.log
```

Dashboards:

* CPU, memory, latency, 4xx/5xx rate

### 10.3 Alerts

* API 5xx > 2% for 5 minutes
* Latency p95 > 800 ms for 5 minutes
* Error bursts in Sentry

---

## **11. Backup and Disaster Recovery**

### 11.1 Database Backups

* **Atlas**: Daily snapshots, 30-day retention, point-in-time recovery.
* Verify monthly by restoring to a staging cluster.

### 11.2 File Backups

* S3 with **Versioning** and **Lifecycle** rules to Glacier.

### 11.3 DR Objectives

| Metric  | Target     |
| ------- | ---------- |
| **RTO** | 2 hours    |
| **RPO** | 15 minutes |

### 11.4 Failover

* Cross-region read replica or snapshot restore procedure documented in runbook.

---

## **12. Operations**

### 12.1 Common Commands

| Task              | Command                |
| ----------------- | ---------------------- |
| Restart API (PM2) | `pm2 restart acrosoft` |
| View logs         | `pm2 logs acrosoft`    |
| Clear cache       | `redis-cli flushall`   |
| Update deps       | `npm update`           |
| Run migrations    | `npm run migrate`      |

### 12.2 Health and Smoke Tests

```bash
curl -f https://api.acrosoft.io/v1/health
curl -f https://api.acrosoft.io/v1/projects -H "Authorization: Bearer <token>"
```

### 12.3 Rollback Procedure

1. Identify last known good build or AMI.
2. Redeploy previous version via EB console or `eb deploy <env> --version <label>`.
3. Invalidate CloudFront cache if frontend regressed.
4. If DB migration caused issues, run down-migration: `npm run migrate:down`.
5. Verify smoke tests and monitoring.

---

## **13. Security Hardening**

* Enforce HTTPS everywhere; TLS 1.2 or newer
* WAF in front of public endpoints
* Principle of least privilege for IAM roles
* Rotate API keys and JWT secrets quarterly
* Enable 2FA on AWS, GitHub, and CI
* CORS: allow only trusted origins
* Rate limiting on all public endpoints
* Quarterly vulnerability scans and pen tests

---

## **14. Troubleshooting**

| Issue                       | Likely Cause                   | Resolution                                                     |
| --------------------------- | ------------------------------ | -------------------------------------------------------------- |
| Cannot connect to MongoDB   | Wrong URI or IP not allowed    | Verify `MONGODB_URI`, update Atlas IP allowlist or VPC peering |
| JWT validation fails        | Missing secret or wrong expiry | Set `JWT_SECRET`, check `JWT_EXPIRY`                           |
| CORS blocked                | Missing headers                | Configure allowed origins in API gateway                       |
| Build fails                 | Outdated Node/npm              | Upgrade Node to LTS 18+                                        |
| EB deploy fails             | Missing IAM or region          | Re-run `aws configure`, verify IAM permissions                 |
| Slack not receiving updates | Webhook not authorized         | Reconnect in **Settings → Integrations**                       |

---

## **15. Checklists**

### 15.1 Pre-Install

* [ ] OS meets version requirements
* [ ] Ports available and firewall configured
* [ ] Accounts created for AWS, Atlas, GitHub, SendGrid, Slack
* [ ] DNS entries planned
* [ ] SSL certificates ready or ACM configured

### 15.2 Post-Install Validation

* [ ] Web app accessible over HTTPS
* [ ] `/v1/health` returns 200
* [ ] Create project and task via UI and API
* [ ] Email and Slack notifications received
* [ ] Logs appear in CloudWatch and errors in Sentry
* [ ] Backups and lifecycle policies enabled

---

## **16. Change Management and Releases**

* Use GitHub Actions for CI and gated promotions.
* Tag releases and attach **Release Notes**.
* Require approvals for production deploys.
* Record changes in the **Change Log** and link to tickets.

---

## **17. Glossary**

| Term    | Definition               |
| ------- | ------------------------ |
| **RTO** | Recovery Time Objective  |
| **RPO** | Recovery Point Objective |
| **SSO** | Single Sign-On           |
| **WAF** | Web Application Firewall |
| **SLA** | Service Level Agreement  |

---

## **18. Changelog**

| Guide Version | Date     | Notes                                                                   |
| ------------- | -------- | ----------------------------------------------------------------------- |
| 1.0           | Nov 2025 | Initial publication                                                     |
| 1.1           | Nov 2025 | Enterprise revision, validation steps, rollback, DR, security hardening |

---

## **19. Related Documents**

* [01_Product_Requirements_Document.md](./01_Product_Requirements_Document.md)
* [02_System_Architecture.md](./02_System_Architecture.md)
* [03_API_Documentation.md](./03_API_Documentation.md)
* [04_User_Guide.md](./04_User_Guide.md)
* [06_Release_Notes.md](./06_Release_Notes.md)
* [08_Style_Guide.md](./08_Style_Guide.md)

---

**End of Document**
