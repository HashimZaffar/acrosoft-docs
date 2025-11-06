
---
# Acrosoft Installation Configuration Guide

**Product:** Acrosoft Platform  
**Version:** v1.0  
**Last Updated:** November 2025  
**Author:** Hashim zaffar

---

## 1. Introduction
This **Installation & Configuration Guide** provides complete setup instructions for installing, configuring, and deploying the Acrosoft platform in different environments.  
It is intended for developers, DevOps engineers, and system administrators working on the Acrosoft project.

---

## 2. Prerequisites

### 2.1 System Requirements
| Component | Minimum Requirement |
|------------|---------------------|
| **OS** | Ubuntu 22.04 LTS / macOS 12+ / Windows 11 |
| **Processor** | Quad-core CPU (2.4 GHz or higher) |
| **Memory** | 8 GB RAM (16 GB recommended) |
| **Storage** | 20 GB free disk space |
| **Network** | Stable internet connection (10 Mbps+) |

### 2.2 Software Dependencies
| Software | Version | Description |
|-----------|----------|-------------|
| **Node.js** | 18.x or higher | Backend runtime |
| **npm / yarn** | Latest | Package manager |
| **MongoDB** | 6.x (Atlas or local) | Database |
| **Git** | 2.35+ | Version control |
| **Docker** | 24.x+ (optional) | Containerization |
| **AWS CLI** | Latest | Deployment management |
| **Postman** | Latest | API testing |

---

## 3. Installation Options
You can install Acrosoft using one of the following approaches:

1. **Local Development Setup** – for individual contributors  
2. **Docker Setup** – for isolated container-based development  
3. **Cloud Deployment** – for production-ready deployments

---

## 4. Local Development Setup

### 4.1 Clone the Repository
```bash
git clone https://github.com/acrosoft/acrosoft-platform.git
cd acrosoft-platform


### 4.2 Install Dependencies

```bash
npm install
# or
yarn install
```

### 4.3 Configure Environment Variables

Create a new file named `.env` in the root directory:

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
```

### 4.4 Start the Development Server

```bash
npm run dev
```

If everything is configured correctly, you should see:

```
Server running at http://localhost:4000
Connected to MongoDB Atlas successfully.
```

---

## 5. Docker Setup (Optional)

### 5.1 Build and Run Containers

```bash
docker-compose up --build
```

### 5.2 Verify Containers

```bash
docker ps
```

Expected running services:

* `acrosoft-api` – backend service
* `acrosoft-db` – MongoDB instance
* `acrosoft-client` – frontend (React app)

### 5.3 Access Application

Visit **[http://localhost:3000](http://localhost:3000)** for the web app
and **[http://localhost:4000/api/v1/health](http://localhost:4000/api/v1/health)** for API status.

---

## 6. Cloud Deployment (AWS)

### 6.1 Prepare AWS Environment

Ensure AWS CLI is configured:

```bash
aws configure
```

Provide:

* Access Key ID
* Secret Access Key
* Default region (e.g., `us-east-1`)

### 6.2 Build the Application

```bash
npm run build
```

### 6.3 Deploy Backend to AWS Elastic Beanstalk

```bash
eb init acrosoft-backend --platform node.js --region us-east-1
eb create acrosoft-prod
```

### 6.4 Deploy Frontend to AWS S3 + CloudFront

```bash
npm run build
aws s3 sync ./build s3://acrosoft-web --delete
aws cloudfront create-invalidation --distribution-id <DIST_ID> --paths "/*"
```

### 6.5 Environment Variables in Production

Set environment variables via AWS Console or CLI:

```bash
eb setenv NODE_ENV=production \
MONGODB_URI=mongodb+srv://prod_cluster.mongodb.net/acrosoft \
JWT_SECRET=super_secret_prod_key \
SENDGRID_API_KEY=live_key_123
```

---

## 7. Database Configuration

### 7.1 Connecting to MongoDB

The app uses **MongoDB Atlas**.
To connect locally, ensure MongoDB is running:

```bash
mongod
```

Test connection in terminal:

```bash
mongo "mongodb+srv://<username>@cluster0.mongodb.net/acrosoft"
```

### 7.2 Collections

| Collection      | Description                    |
| --------------- | ------------------------------ |
| `users`         | Stores user info and roles     |
| `projects`      | Holds project metadata         |
| `tasks`         | Contains task details          |
| `notifications` | Stores in-app and email alerts |
| `activity_logs` | Tracks system and user actions |

---

## 8. Running Tests

### 8.1 Unit Tests

```bash
npm run test
```

### 8.2 Integration Tests

```bash
npm run test:integration
```

### 8.3 Linting & Code Style

```bash
npm run lint
```

Ensure all tests pass before pushing to production.

---

## 9. Monitoring & Logging

### 9.1 Using Sentry

Configure **Sentry DSN** in `.env` to capture errors:

```env
SENTRY_DSN=https://your_sentry_dsn_key
```

### 9.2 AWS CloudWatch

All logs are automatically pushed to CloudWatch.
Use the AWS Console to view:

```
CloudWatch → Logs → acrosoft-api-log-group
```

---

## 10. Backup & Recovery

### 10.1 Database Backup

Automated daily backups are configured in MongoDB Atlas.
To restore:

1. Go to MongoDB Atlas → Backups
2. Select Snapshot → Restore to Cluster

### 10.2 File Backup

All uploaded assets are stored in AWS S3:

```
s3://acrosoft-bucket/uploads/
```

Enable **S3 Versioning** for file recovery.

---

## 11. Common Configuration Issues

| Issue                         | Cause                          | Resolution                                      |
| ----------------------------- | ------------------------------ | ----------------------------------------------- |
| App not connecting to MongoDB | Invalid connection string      | Verify `MONGODB_URI` in `.env`                  |
| JWT errors                    | Missing secret or wrong expiry | Check `JWT_SECRET` and `JWT_EXPIRY`             |
| CORS errors                   | API access blocked             | Enable `Access-Control-Allow-Origin` in headers |
| Build fails                   | Outdated Node/npm              | Upgrade Node to latest LTS version              |
| AWS deploy fails              | Missing credentials            | Re-run `aws configure`                          |

---

## 12. Maintenance Commands

| Task                    | Command                |
| ----------------------- | ---------------------- |
| Restart server          | `pm2 restart acrosoft` |
| View logs               | `pm2 logs acrosoft`    |
| Clear cache             | `redis-cli flushall`   |
| Update dependencies     | `npm update`           |
| Run database migrations | `npm run migrate`      |

---

## 13. Security Best Practices

* Always use HTTPS in production.
* Rotate API keys quarterly.
* Limit access to `.env` files.
* Use IAM roles with least privilege.
* Enable 2FA on AWS and GitHub accounts.
* Review logs weekly for anomalies.

---

## 11. Related Documents

* [01_Product_Requirements_Document.md](./01_Product_Requirements_Document.md)
* [02_System_Architecture.md](./02_System_Architecture.md)
* [03_API_Documentation.md](./03_API_Documentation.md)
* [04_User_Guide.md](./04_User_Guide.md)
* [05_Installation_Configuration_Guide.md](./05_Installation_Configuration_Guide.md)
* [06_Release_Notes.md](./06_Release_Notes.md)
* [08_Style_Guide.md](./08_Style_Guide.md)

---

**End of Document**