
---

# **Acrosoft User Guide**

## **Document Control**

| Field               | Detail                                        |
| ------------------- | --------------------------------------------- |
| **Product**         | Acrosoft Platform                             |
| **Guide Version**   | 1.1 (Enterprise Revision)                     |
| **Product Version** | v1.0                                          |
| **Last Updated**    | November 2025                                 |
| **Author**          | Hashim Zaffar                                 |
| **Reviewed By**     | —                                             |
| **Approved By**     | —                                             |
| **Status**          | Draft / Reviewed / Approved                   |
| **Audience**        | Clients, Project Managers, Developers, Admins |
| **Confidentiality** | Public / Partner / Internal                   |

---

## **1. About This Guide**

### 1.1 Purpose

Help all roles use Acrosoft to plan work, collaborate, and track delivery.

### 1.2 What You Will Learn

* Sign up, sign in, and secure your account
* Create and manage projects and tasks
* Communicate with your team and clients
* View analytics and export reports
* Connect Slack and GitHub
* Configure notifications, roles, and permissions

---

## **2. Quick Start by Role**

### 2.1 Client

1. Create account and verify email
2. Create a project request
3. Review milestones on the Dashboard
4. Comment on tasks and approve deliverables
5. Export a weekly report

### 2.2 Project Manager

1. Create project or accept client request
2. Add team members and set roles
3. Define milestones and tasks
4. Connect Slack and GitHub
5. Monitor velocity and adjust deadlines

### 2.3 Developer

1. Join assigned project
2. Review specs and files
3. Update task status on Kanban board
4. Link commits and pull requests
5. Resolve comments and move tasks to Done

### 2.4 Admin

1. Configure SSO and 2FA policies
2. Manage org roles and permissions
3. Set notification defaults and retention limits
4. Review audit logs and access reports
5. Maintain integrations

---

## **3. System Requirements**

| Component    | Minimum                                        |
| ------------ | ---------------------------------------------- |
| **Browser**  | Chrome 110+, Firefox 100+, Edge 110+           |
| **OS**       | Windows 10+, macOS 12+, Ubuntu 22.04+          |
| **Internet** | 5 Mbps stable connection                       |
| **Screen**   | 1366×768 or higher                             |
| **Mobile**   | Latest iOS or Android browser (responsive web) |

> Tip: Enable cookies and local storage for best experience.

---

## **4. Accessing Acrosoft**

### 4.1 Create an Account

1. Go to **[https://app.acrosoft.io](https://app.acrosoft.io)**
2. Select **Sign Up**
3. Choose **Client** or **Team**
4. Enter name, email, password
5. Verify email to activate

### 4.2 Sign In

* Visit **[https://app.acrosoft.io/login](https://app.acrosoft.io/login)**
* Enter email and password

### 4.3 Single Sign-On (Optional)

* Supported IdPs: Microsoft Entra ID, Google Workspace, Okta
* Admins can enable under **Admin → Security → SSO**

### 4.4 Two-Factor Authentication

* Go to **Settings → Security**
* Enable **2FA** with an authenticator app
* Save backup codes in a safe place

### 4.5 Password Reset

* Click **Forgot Password** on the login page
* Check email and follow the link

---

## **5. Navigating the Interface**

### 5.1 Main Areas

| Area          | What You See                                   |
| ------------- | ---------------------------------------------- |
| **Dashboard** | Projects, tasks, alerts, and high-level KPIs   |
| **Projects**  | Project list, search, filters, and statuses    |
| **Tasks**     | Kanban and list views with quick edits         |
| **Messages**  | Project rooms and direct messages              |
| **Analytics** | Velocity, completion rate, trend charts        |
| **Settings**  | Profile, security, notifications, integrations |

### 5.2 Global Actions

* Search bar for projects and tasks
* Create menu: **New Project**, **New Task**
* Bell icon: notifications and reminders
* Profile menu: account, theme, sign out

---

## **6. Projects**

### 6.1 Create a Project

1. **Projects → Create New Project**
2. Enter Title, Description, Dates, Budget (optional)
3. Choose Service Category
4. Submit

### 6.2 Project Details

* **Overview**: status, deadlines, team
* **Tasks & Milestones**
* **Commits**: linked GitHub activity
* **Files**: specs and attachments
* **Messages**: project room and mentions

### 6.3 Status Workflow

| Status           | Use When                               |
| ---------------- | -------------------------------------- |
| **Planning**     | Scope and milestones are being defined |
| **In Progress**  | Work is active                         |
| **Under Review** | QA and stakeholder review              |
| **Completed**    | Work is delivered and closed           |
| **Archived**     | Hidden from active lists, read-only    |

> PMs update via **Project → Status → Update**.

### 6.4 Team and Access

* **Add Members**: **Project → Team → Add**
* Assign roles per project: Client, Developer, Manager, Viewer

### 6.5 Files and Deliverables

* Upload PDF, DOCX, ZIP, PNG
* Max file size: **20 MB** per file
* Versioning: new uploads keep history

---

## **7. Tasks**

### 7.1 Create a Task

1. Open a project
2. **Tasks → Add Task**
3. Title, Description, Due Date, Assignee
4. Save

### 7.2 Update Status

* Drag cards on the Kanban board
* States: **To Do**, **In Progress**, **Blocked**, **Completed**

### 7.3 Comments and Mentions

* Use `@username` to notify someone
* Attach files and paste links
* Code blocks are supported in comments

### 7.4 Bulk Actions

* Multi-select to change assignee, due date, or status for several tasks

### 7.5 Good Practices

* One clear owner per task
* Short, action-oriented titles
* Acceptance criteria in the description

---

## **8. Communication**

### 8.1 Messages

* **Messages → Project Room** for each project
* Share files, links, and quick updates

### 8.2 Slack Integration

1. **Settings → Integrations → Connect Slack**
2. Approve access
3. Choose target channel per project
4. You will receive updates for task changes, mentions, and deploys

### 8.3 Email Preferences

* **Settings → Notifications**
* Toggle digests, mentions, task updates, and reminders

---

## **9. Analytics and Reports**

### 9.1 Reports

* **Analytics → Reports**
* Filter by Project and Timeframe
* Metrics: Completion rate, velocity, open vs closed tasks, issue trends

### 9.2 Export

* **Export CSV** for spreadsheets
* **Export PDF** for stakeholders
* Exports include applied filters and timestamps

---

## **10. Integrations**

### 10.1 GitHub

* **Settings → Integrations → Connect GitHub**
* Authorize and pick repositories
* Commits and pull requests appear in project activity

### 10.2 Slack

* See section 8.2

### 10.3 Email (SendGrid)

* System uses email for alerts and digests
* No setup needed for end users

---

## **11. Account and Security**

### 11.1 Profile

* **Settings → Profile**
* Update name, photo, contact info, timezone

### 11.2 Security

* Change password and enable 2FA
* View active sessions and sign out of other devices

### 11.3 Roles and Permissions

| Role                | Access Highlights                      |
| ------------------- | -------------------------------------- |
| **Admin**           | All organization settings and projects |
| **Project Manager** | Project setup, team, milestones        |
| **Developer**       | Assigned projects and tasks            |
| **Client**          | Read and comment on own projects       |

> Admins control role assignment under **Admin → Users**.

---

## **12. Notifications**

* Triggered by project updates, task events, mentions, file uploads, deadlines
* Channels: in-app, email, Slack
* Configure under **Settings → Notifications**

---

## **13. Accessibility and Localization**

* Keyboard navigation supported
* Contrast-friendly themes
* Timezone support per user profile
* Date and time in ISO 8601

---

## **14. Troubleshooting**

| Issue              | Cause                             | Fix                                        |
| ------------------ | --------------------------------- | ------------------------------------------ |
| Cannot sign in     | Wrong password or expired session | Use **Forgot Password** or sign in again   |
| Project missing    | No access or wrong filter         | Clear filters or contact a PM              |
| File upload fails  | Over 20 MB or blocked type        | Compress file or split archives            |
| No notifications   | Disabled or muted channel         | Re-enable in **Settings → Notifications**  |
| Slack not updating | App not authorized                | Reconnect integration and reselect channel |

---

## **15. FAQs**

**Q:** Can I change a project owner
**A:** Yes. PMs can update owner in **Project → Team**.

**Q:** How do I restore an archived project
**A:** Admin or PM can unarchive from **Projects → Filters → Archived → Restore**.

**Q:** Can clients upload files
**A:** Yes, within their projects and tasks.

**Q:** Do comments notify by email
**A:** Yes, if mentions or your notification rule is enabled.

---

## **16. Best Practices**

* Keep specs, wireframes, and acceptance criteria attached to tasks
* Review progress weekly and close done work
* Use mentions for decisions and approvals
* Keep statuses accurate to improve analytics
* Export monthly reports for stakeholders

---

## **17. Support and SLA**

* **Email:** [support@acrosoft.io](mailto:support@acrosoft.io)
* **In-App Chat:** bottom-right widget
* **Docs:** [https://docs.acrosoft.io](https://docs.acrosoft.io)

**Hours:** Monday to Friday, 9 AM to 6 PM UTC

**Severity and Targets**

| Severity | Example               | First Response  |
| -------- | --------------------- | --------------- |
| Critical | System outage         | 1 hour          |
| High     | Major feature broken  | 4 hours         |
| Medium   | Degraded experience   | 1 business day  |
| Low      | Questions or requests | 2 business days |

---

## **18. Data Privacy**

* GDPR-aligned handling
* Data encrypted in transit and at rest
* Export and deletion available upon request through support

---

## **19. Keyboard Shortcuts**

| Action             | Shortcut       |
| ------------------ | -------------- |
| New Task           | `N`            |
| Search             | `/`            |
| Open Notifications | `G` then `N`   |
| Save               | `Ctrl/Cmd + S` |

---

## **20. Limits and Retention**

| Item            | Limit                                                                         |
| --------------- | ----------------------------------------------------------------------------- |
| Max file size   | 20 MB per file                                                                |
| Comment size    | 10,000 characters                                                             |
| Webhook timeout | 5 seconds per delivery                                                        |
| Data retention  | Project files retained for active projects; archived for 12 months by default |

> Admins can request custom retention policies.

---

## **21. Glossary**

| Term      | Definition                         |
| --------- | ---------------------------------- |
| **PRD**   | Product Requirements Document      |
| **API**   | Application Programming Interface  |
| **MVP**   | Minimum Viable Product             |
| **UI/UX** | User Interface and User Experience |
| **RBAC**  | Role-Based Access Control          |
| **SSO**   | Single Sign-On                     |

---

## **22. Changelog**

| Guide Version | Date     | Notes                                                       |
| ------------- | -------- | ----------------------------------------------------------- |
| 1.0           | Nov 2025 | Initial user guide                                          |
| 1.1           | Nov 2025 | Enterprise revision, added SSO, 2FA, SLA, limits, shortcuts |

---

## **23. Related Documents**

* [01_Product_Requirements_Document.md](./01_Product_Requirements_Document.md)
* [02_System_Architecture.md](./02_System_Architecture.md)
* [03_API_Documentation.md](./03_API_Documentation.md)
* [05_Installation_Configuration_Guide.md](./05_Installation_Configuration_Guide.md)
* [06_Release_Notes.md](./06_Release_Notes.md)
* [08_Style_Guide.md](./08_Style_Guide.md)

---

**End of Document**
