
---
# Acrosoft Troubleshooting FAQ

**Product:** Acrosoft Platform  
**Version:** v1.0  
**Last Updated:** November 2025  
**Author:** [Your Name]

---

## 1. Introduction
This **Troubleshooting & FAQ Guide** provides solutions to common issues encountered on the Acrosoft platform.  
It is designed for clients, developers, and support engineers to quickly identify, diagnose, and resolve common technical and functional problems.

If you cannot find your issue here, please contact **support@acrosoft.io** or use the in-app **Support Chat**.

---

## 2. General Troubleshooting Process
Follow these steps before escalating an issue:

1. **Check the Status Page:**  
   Visit [status.acrosoft.io](https://status.acrosoft.io) to verify if there are known outages.
2. **Clear Browser Cache:**  
   Clear cookies and refresh to ensure you are viewing the latest version.
3. **Try Incognito Mode:**  
   Browser extensions may interfere with functionality.
4. **Confirm Internet Connection:**  
   Ensure stable connectivity (minimum 5 Mbps).
5. **Check Permissions:**  
   Access may be restricted based on your role (Admin, Manager, Client).
6. **Reproduce the Issue:**  
   Note the exact steps, browser, and timestamp when reporting to support.

---

## 3. Login & Authentication Issues

### Issue 3.1 – “Invalid Credentials” Error
**Cause:** Incorrect email or password.  
**Solution:**
- Double-check your login credentials.
- Use **Forgot Password** to reset credentials.
- If still unresolved, contact support to verify your account status.

---

### Issue 3.2 – “Session Expired” Message
**Cause:** JWT token expired (after 60 minutes).  
**Solution:**
- Log out and sign back in.
- If this occurs frequently, ensure cookies are not blocked in your browser settings.

---

### Issue 3.3 – “Too Many Login Attempts”
**Cause:** Exceeded login rate limit.  
**Solution:**
- Wait 15 minutes and retry.
- Use **password reset** if you forgot credentials.
- Contact support if your account is temporarily locked.

---

## 4. Project & Task Management Issues

### Issue 4.1 – Project Not Visible on Dashboard
**Cause:** Insufficient permissions or project filters.  
**Solution:**
- Check if your role allows project access.  
- Use the **filter menu** (Active / Completed).  
- Confirm with your Project Manager if the project is archived.

---

### Issue 4.2 – “Failed to Create Project” Error
**Cause:** Missing required fields or invalid date format.  
**Solution:**
- Ensure all mandatory fields (name, start date, deadline) are filled.
- Dates must follow ISO format (`YYYY-MM-DD`).

---

### Issue 4.3 – Tasks Not Updating or Stuck
**Cause:** Network latency or server sync delay.  
**Solution:**
- Refresh the page after a few seconds.
- Check internet connectivity.
- If the issue persists, contact support with project ID.

---

### Issue 4.4 – Files Won’t Upload
**Possible Causes:**
- File size exceeds 20 MB.
- Unsupported format.
- S3 bucket temporarily unavailable.

**Solution:**
- Compress large files before uploading.
- Accepted formats: PDF, DOCX, ZIP, PNG, JPG.
- Retry in 5 minutes; AWS storage sync may take a moment.

---

## 5. Integration Issues

### Issue 5.1 – Slack Notifications Not Received
**Cause:** Revoked or expired webhook URL.  
**Solution:**
1. Go to **Settings → Integrations → Slack**.
2. Reconnect your Slack workspace.
3. Test notification delivery using the “Send Test Message” button.

---

### Issue 5.2 – GitHub Repositories Not Syncing
**Cause:** Invalid token or repository permissions.  
**Solution:**
- Re-authenticate GitHub from **Settings → Integrations**.
- Ensure your GitHub token includes `repo` and `workflow` scopes.
- Confirm webhook configuration in GitHub repo settings.

---

### Issue 5.3 – Email Notifications Delayed
**Cause:** Queue backlog or SendGrid throttling.  
**Solution:**
- Check spam folder or “Promotions” tab.
- If consistent, contact support to verify SendGrid status.
- Delays over 15 minutes are logged in **System Analytics → Alerts**.

---

## 6. Performance & Connectivity

### Issue 6.1 – Slow Dashboard Loading
**Cause:** High traffic or cached session data.  
**Solution:**
- Clear browser cache.
- Switch to incognito mode.
- Use the latest browser version.
- Check **status.acrosoft.io** for ongoing maintenance.

---

### Issue 6.2 – “Server Unreachable” Error
**Cause:** API downtime or network issue.  
**Solution:**
- Verify internet connection.
- If error persists, check **API health** at `https://api.acrosoft.io/v1/health`.
- Contact support with error timestamp and browser console log.

---

## 7. Deployment & Dev Environment Issues

### Issue 7.1 – “Cannot Connect to Database”
**Cause:** Incorrect `MONGODB_URI` in `.env`.  
**Solution:**
- Verify credentials in `.env` file.
- Test connection manually:
  ```bash
  mongo "mongodb+srv://<user>:<pass>@cluster.mongodb.net/acrosoft"

---

### Issue 7.2 – Environment Variables Not Loading

**Cause:** Missing or misconfigured `.env` file.
**Solution:**

* Ensure `.env` is in the project root directory.
* Restart the server:

  ```bash
  npm run dev
  ```

---

### Issue 7.3 – Docker Container Crashes on Start

**Cause:** Port conflicts or memory limits.
**Solution:**

* Stop previous containers:

  ```bash
  docker stop $(docker ps -aq)
  ```
* Restart with fresh build:

  ```bash
  docker-compose up --build
  ```

---

## 8. Security & Access

### Issue 8.1 – “Access Denied” When Viewing Resource

**Cause:** RBAC restriction.
**Solution:**

* Confirm your assigned role (Admin, Manager, Client).
* Contact your project administrator to request access.

---

### Issue 8.2 – Two-Factor Authentication (2FA) Not Working

**Cause:** Incorrect verification code or desynced time.
**Solution:**

* Ensure your device time is auto-synced.
* Re-add your 2FA code using Google Authenticator or Authy.
* Backup recovery codes during setup for emergencies.

---

## 9. FAQs

### Q1. Can clients directly communicate with developers?

Yes — clients can message assigned developers using the **in-app chat** under each project, or via the **Slack integration** if enabled.

---

### Q2. How often is data backed up?

Data is backed up **daily** on MongoDB Atlas and retained for 30 days.
All files on AWS S3 have versioning enabled for recovery.

---

### Q3. Can I export project data?

Yes, go to **Projects → Export → CSV/PDF**.
Export includes project metadata, tasks, and timelines.

---

### Q4. What’s the maximum file upload size?

20 MB per file.
Use ZIP archives for multiple uploads.

---

### Q5. How do I report a bug?

1. Navigate to **Help → Report Issue**.
2. Include:

   * Description of the issue
   * Screenshot (if available)
   * Browser, OS, and timestamp
3. Click **Submit**.
   The support team will acknowledge within 24 hours.

---

### Q6. What browsers are supported?

| Browser | Minimum Version |
| ------- | --------------- |
| Chrome  | 110+            |
| Firefox | 100+            |
| Edge    | 110+            |
| Safari  | 15+             |

---

### Q7. Does Acrosoft have an API?

Yes, see [03_API_Documentation.md](./03_API_Documentation.md) for detailed API reference.

---

### Q8. Is there a mobile app?

The mobile app is currently in development and scheduled for **Q2 2026** release.

---

### Q9. How do I reset my password?

1. Click **Forgot Password** on the login page.
2. Enter your registered email.
3. Check your inbox for a reset link (valid for 15 minutes).

---

### Q10. What is the platform uptime?

Acrosoft maintains **99.9% uptime**.
All incidents and maintenance updates are listed at [status.acrosoft.io](https://status.acrosoft.io).

---

## 10. Contact Support

For additional help:

| Channel           | Details                                           |
| ----------------- | ------------------------------------------------- |
| **Email**         | [support@acrosoft.io](mailto:support@acrosoft.io) |
| **Chat**          | In-app live chat (bottom-right corner)            |
| **Status Page**   | [status.acrosoft.io](https://status.acrosoft.io)  |
| **Documentation** | [docs.acrosoft.io](https://docs.acrosoft.io)      |

Response time: **Within 24 business hours**

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