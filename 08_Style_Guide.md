
---

# **Acrosoft Style Guide**

## **Document Control**

| Field               | Detail                                      |
| ------------------- | ------------------------------------------- |
| **Product**         | Acrosoft Platform                           |
| **Guide Version**   | 1.1 (Enterprise Revision)                   |
| **Product Version** | v1.0                                        |
| **Last Updated**    | November 2025                               |
| **Author**          | Hashim Zaffar                               |
| **Reviewed By**     | —                                           |
| **Approved By**     | —                                           |
| **Status**          | Published                                   |
| **Audience**        | Writers, Engineers, PMs, Support, Marketing |
| **Confidentiality** | Public / Partner / Internal                 |

---

## **1. Introduction**

This guide defines standards for creating, maintaining, and updating Acrosoft documentation.
It ensures clarity, consistency, accessibility, and accuracy across technical and customer-facing content.

**Applies to**
Product docs, API references, user guides, knowledge base articles, onboarding, and customer communications.

---

## **2. Documentation Goals**

* **Clarity** — unambiguous and precise wording
* **Consistency** — uniform structure and terminology
* **Accessibility** — readable by all skill levels and compliant with WCAG 2.1 AA
* **Accuracy** — validated, source-controlled, and current
* **Usability** — task-oriented, outcomes first

---

## **3. Tone and Voice**

### 3.1 Voice

* Professional, friendly, confident
* Address the reader directly using **you** and **we** when appropriate
* Avoid slang, idioms, or marketing hype

### 3.2 Tone

* Empathetic, encouraging, informative, consistent

### 3.3 Examples

* ❌ “Users should endeavor to configure the server environment properly.”
* ✅ “You need to configure your server environment before you start the build.”

---

## **4. Grammar and Writing Conventions**

* Use **American English** spelling and **present tense** when possible
* Prefer **active voice**
* Keep sentences concise (target fewer than 20 words)
* Prefer simple words over complex ones (use “use” instead of “utilize”)

### 4.1 Punctuation

* Use the **Oxford comma**
* Avoid exclamation marks in technical docs
* Avoid em dashes. Use periods or commas instead
* Colons introduce examples or lists. Semicolons separate closely related ideas

### 4.2 Numbers and Units

* Spell out one through nine. Use numerals for 10 and above
* Always include units (5 MB, 3 seconds)
* Use ISO 8601 for dates and times (2025-11-08T12:30:00Z)

### 4.3 Inclusive and Clear Language

* Prefer gender-neutral terms
* Avoid ableist language
* Prefer “allow” or “let” over “whitelist/blacklist” (use “allowlist/blocklist”)

---

## **5. Formatting Standards**

### 5.1 Headings

Follow Markdown heading hierarchy:

```
# H1 — Document Title
## H2 — Major Section
### H3 — Subsection
#### H4 — Minor Heading
```

Each document must start with a metadata block (see Section 10).

### 5.2 Emphasis

| Purpose                 | Markdown    | Example                |
| ----------------------- | ----------- | ---------------------- |
| Emphasis or UI elements | `**bold**`  | Click **Save changes** |
| Terms or references     | `*italics*` | Use *npm install*      |
| Commands or code        | backticks   | Run `npm start`        |

### 5.3 Lists

* Use bulleted lists for unordered items
* Use numbered lists for steps
* Start each step with an **imperative verb**

**Example**

```markdown
1. Click **Settings**.
2. Select **Integrations**.
3. Click **Connect Slack**.
```

### 5.4 Tables

Use tables for structured data:

```markdown
| Parameter | Type | Description |
|---|---|---|
| `JWT_SECRET` | string | Secret for token signing |
```

### 5.5 Code Blocks

Use fenced code blocks with a language tag:

```bash
npm run build
```

```json
{ "status": "success" }
```

---

## **6. Visuals and Media**

### 6.1 Screenshots

* High resolution, cropped to the relevant area
* No sensitive or personal data
* Use neutral light theme unless feature requires dark mode
* Provide concise captions

### 6.2 Diagrams

* Use **draw.io**, **PlantUML**, or **Excalidraw**
* Keep shapes simple and labels short
* Store source files alongside exported PNG/SVG

**Alt text**

```markdown
![Acrosoft system context diagram](./assets/architecture.png "Frontend, API, DB, and integrations")
```

### 6.3 Video (optional)

* Max 3 minutes, with captions
* Host on the docs portal or official channel

---

## **7. Naming Conventions**

### 7.1 Files and Folders

* **kebab-case** for file names:
  `01_product_requirements_document.md`, `03_api_documentation.md`

### 7.2 Variables and Commands

* Match code identifiers precisely. Use `code` style for all variable names and CLI flags

### 7.3 API Resources

* Plural nouns and lowercase kebab paths
* Version prefix in the URL: `/v1/projects`
* Avoid verbs in paths. Use HTTP methods for actions

---

## **8. Repository Structure**

```
Acrosoft-Documentation/
├── 01_Product_Requirements_Document.md
├── 02_System_Architecture.md
├── 03_API_Documentation.md
├── 04_User_Guide.md
├── 05_Installation_Configuration_Guide.md
├── 06_Release_Notes.md
├── 07_Troubleshooting_FAQ.md
└── 08_Style_Guide.md
```

Each file includes: version, author, last updated date, and related docs.

---

## **9. Language and Terminology**

### 9.1 Preferred Terms

| Preferred               | Avoid               | Reason                      |
| ----------------------- | ------------------- | --------------------------- |
| **Sign in**             | Log in              | Simpler and more consistent |
| **Client**              | Customer            | Industry-consistent         |
| **Dashboard**           | Control panel       | Simpler                     |
| **Configuration**       | Setup               | More precise                |
| **Allowlist/Blocklist** | Whitelist/Blacklist | Inclusive language          |

### 9.2 First Use of Acronyms

Spell out on first use, then use acronym
Examples: *Representational State Transfer (REST)*, *Continuous Integration (CI)*

### 9.3 Product and Tech Casing (enforced)

Use the casing below exactly throughout docs:

* .NET, .NET Core, .NET Framework, modern .NET, Legacy .NET
* C#, C# 11, .NET 7, .NET 5
* Visual Studio, Visual Studio Code, Visual Studio 2022 for Windows, Visual Studio 2022 for Mac
* JetBrains Rider, GitHub Codespaces
* .NET Interactive Notebooks, Markdown, PowerShell, F#, SQL, SQL Server
* Windows, Windows 10, Windows 11 Arm, macOS, Linux, Ubuntu, Red Hat Enterprise Linux (RHEL)
* CLR, CoreCLR, BCL, GAC
* ASP.NET, WPF, WCF, SignalR, gRPC, MVC, Blazor, Entity Framework (EF) 6, Entity Framework Core, UWP, Microsoft Store, Azure Cosmos DB, Oracle, Xcode

---

## **10. Document Metadata and Front Matter**

Every document begins with a metadata block:

```markdown
**Product:** <Product Name>  
**Version:** vX.Y  
**Last Updated:** <Month YYYY>  
**Author:** <Name>
```

Optionally add:

* **Audience:** role(s)
* **Status:** Draft, Reviewed, Approved, Published
* **Confidentiality:** Public, Partner, Internal

---

## **11. API Documentation Standards**

* Use **OpenAPI 3.1** source of truth
* Provide request and response examples with minimal, realistic data
* Show authentication headers and status codes
* Document pagination, filtering, sorting, idempotency, and rate limiting
* Standardize errors using **RFC 7807** problem details
* Provide `curl` and one typed language example (TypeScript) when helpful

**Endpoint example**

```http
GET /v1/projects?status=active&page=1&limit=20
Authorization: Bearer <token>
```

**Error model**

```json
{
  "type": "https://docs.acrosoft.io/errors/not-found",
  "title": "Resource not found",
  "status": 404,
  "detail": "Project p999 does not exist.",
  "instance": "/v1/projects/p999"
}
```

---

## **12. CLI and Code Examples**

* Use realistic placeholders and keep them consistent
* One task per code block
* Wrap long commands at logical breakpoints
* Never hard-code secrets or tokens

**CLI**

```bash
aws cloudfront create-invalidation --distribution-id <DIST_ID> --paths "/*"
```

**TypeScript**

```ts
const res = await fetch("/v1/projects", { headers: { Authorization: `Bearer ${token}` }});
```

---

## **13. Accessibility and Localization**

* WCAG 2.1 AA contrast and keyboard support for examples and UI captures
* Use descriptive link text (“View installation guide”)
* All images require alt text
* Avoid color-only distinctions in diagrams
* Internationalization: default to UTC timestamps or label local time zones clearly

---

## **14. Links, Cross-References, and Anchors**

* Prefer relative links within the docs repo
* Link titles should describe the target
* For headings, use stable, lowercase hyphenated anchors
* Validate links in CI

---

## **15. Review, Approval, and Versioning**

### 15.1 Workflow

1. Draft content and include metadata
2. Open a **GitHub Pull Request**
3. Peer review for accuracy and tone
4. Technical review by SME when needed
5. Update **Last Updated** date and merge after approval

### 15.2 Versioning Policy

| Change         | Increment | Example |
| -------------- | --------- | ------- |
| Minor text fix | +0.0.1    | 1.0.1   |
| Feature update | +0.1.0    | 1.1.0   |
| Major rewrite  | +1.0.0    | 2.0.0   |

---

## **16. Quality Gates and Doc Linting**

* Validate Markdown structure and links in CI
* Enforce spell-check with technical dictionary
* Run an accessibility scan on sample pages
* Block merges on broken links or missing metadata

---

## **17. Reusable Templates**

### 17.1 Section Template

```markdown
## [Section Title]
### Purpose
What this section covers.

### Steps
1. Step one
2. Step two
3. Step three

### Example
Code snippet or screenshot.

### Related
- Link to background or APIs
```

### 17.2 Table Template

```markdown
| Field | Type | Description |
|---|---|---|
| Example | string | Short explanation |
```

### 17.3 Front Matter Snippet

```markdown
**Product:** Acrosoft Platform  
**Version:** v1.0  
**Last Updated:** November 2025  
**Author:** <Name>
```

---

## **18. Security and Privacy in Docs**

* Redact tokens, secrets, and PII
* Use placeholders such as `<token>` and `<DIST_ID>`
* Anonymize screenshots and logs
* Follow retention rules for embedded data

---

## **19. SEO and Discoverability (Docs Portal)**

* One H1 per page that includes the task outcome
* Intro paragraph explains who the page is for and what they can achieve
* Use descriptive titles and concise summaries for navigation cards
* Prefer short URLs that reflect task names

---

## **20. Maintenance and Lifecycle**

* Review each top-level doc at least once per quarter
* Archive deprecated content to `/archive/` and add a banner at the top of the page
* Track changes in **Changelog** tables or commit messages

---

## **21. Example Term Bank (Acrosoft-specific)**

| Term                  | Definition                                          |
| --------------------- | --------------------------------------------------- |
| **Acrosoft Platform** | SaaS for software delivery and client collaboration |
| **Client Portal**     | Authenticated area for clients to manage projects   |
| **Project Dashboard** | Overview of milestones, tasks, and KPIs             |
| **Analytics**         | Velocity, completion rate, issue trends             |
| **Integrations**      | Slack, GitHub, SendGrid, Stripe (planned)           |

---

## **22. Changelog**

| Guide Version | Date     | Notes                                                                    |
| ------------- | -------- | ------------------------------------------------------------------------ |
| 1.0           | Nov 2025 | Initial publication                                                      |
| 1.1           | Nov 2025 | Enterprise revision, metadata, accessibility, API/CLI rules, doc linting |

---

## **23. Related Documents**

* [01_Product_Requirements_Document.md](./01_Product_Requirements_Document.md)
* [02_System_Architecture.md](./02_System_Architecture.md)
* [03_API_Documentation.md](./03_API_Documentation.md)
* [04_User_Guide.md](./04_User_Guide.md)
* [05_Installation_Configuration_Guide.md](./05_Installation_Configuration_Guide.md)
* [06_Release_Notes.md](./06_Release_Notes.md)
* [07_Troubleshooting_FAQ.md](./07_Troubleshooting_FAQ.md)

---

**End of Document**
