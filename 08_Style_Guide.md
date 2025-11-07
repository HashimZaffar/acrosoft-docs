
---
# Acrosoft Style Guide

**Product:** Acrosoft Platform  
**Version:** v1.0  
**Last Updated:** November 2025  
**Author:** Hashim Zaffar

---

## 1. Introduction
This **Style Guide** provides standards for creating, maintaining, and updating documentation across the Acrosoft ecosystem.  
It ensures clarity, consistency, and professionalism in all technical and customer-facing materials.

All writers, engineers, and contributors must follow these guidelines when producing:
- Product documentation  
- API references  
- User guides  
- Knowledge base articles  
- Marketing or onboarding content  

---

## 2. Documentation Goals
- **Clarity:** Content must be unambiguous and precise.  
- **Consistency:** Use uniform style, structure, and terminology across all documents.  
- **Accessibility:** Ensure documentation is easy to read for all skill levels.  
- **Accuracy:** Keep information current, validated, and technically correct.  
- **Usability:** Documentation must guide readers toward successful outcomes.

---

## 3. Tone and Voice

### 3.1 Voice
- Use a **professional, friendly, and confident** voice.  
- Speak directly to the reader — use **“you”** and **“we”** when appropriate.  
- Avoid corporate jargon or overly technical phrasing unless absolutely necessary.

### 3.2 Tone
- **Empathetic:** Acknowledge user goals and challenges.  
- **Encouraging:** Guide users positively through instructions.  
- **Informative:** Focus on clarity, not marketing.  
- **Consistent:** Keep tone steady across sections, even if multiple authors contribute.

### 3.3 Examples
❌ *Incorrect:* “Users should endeavor to configure the server environment properly.”  
✅ *Correct:* “You need to configure your server environment before starting the build.”

---

## 4. Grammar and Writing Conventions

### 4.1 General Rules
- Use **American English** spelling.
- Write in **active voice** (e.g., “The system sends a confirmation email,” not “A confirmation email is sent by the system.”).
- Use **present tense** unless describing version history or deprecated features.
- Keep sentences under 20 words where possible.

### 4.2 Punctuation
- Use the **Oxford comma**.
- Avoid exclamation marks in technical writing.
- Use em dashes (—) sparingly and for emphasis only.
- Colons introduce examples or lists; semicolons separate related ideas.

### 4.3 Numbers & Measurements
- Spell out numbers one through nine; use numerals for 10 and above.
- Always include units (e.g., “5 MB,” not “5”).

---

## 5. Formatting Standards

### 5.1 Headings
Use proper Markdown heading hierarchy:

# H1 – Document Title  
## H2 – Major Section  
### H3 – Subsection  
#### H4 – Minor Heading

Each document must begin with a level-1 title that includes:

* Product name
* Version number
* Author
* Last updated date

---

### 5.2 Text Emphasis

| Purpose                               | Markdown        | Example                |
| ------------------------------------- | --------------- | ---------------------- |
| **Bold** for UI elements or emphasis  | `**text**`      | Click **Save Changes** |
| *Italics* for terms or variable names | `*text*`        | Use *npm install*      |
| `Code` for commands                   | `` `command` `` | Run `npm start`        |

---

### 5.3 Lists

* Use **bulleted lists** for unordered items.
* Use **numbered lists** for sequential steps.
* Each step should start with an **imperative verb** (e.g., “Click,” “Run,” “Open”).

**Example:**

```markdown
1. Click **Settings**.  
2. Select **Integrations**.  
3. Click **Connect Slack**.
```

---

### 5.4 Tables

Tables should be used for structured data such as configuration keys or feature comparisons.

```markdown
| Parameter | Type | Description |
|------------|------|-------------|
| `JWT_SECRET` | String | Secret used for token encryption |
| `MONGODB_URI` | String | Connection string for database |
```

---

### 5.5 Code Blocks

For commands, configuration, or code examples, use fenced code blocks with the appropriate language syntax:

```bash
npm run build
```

```json
{
  "status": "success",
  "message": "Project created successfully"
}
```

---

## 6. Visuals and Media

### 6.1 Screenshots

* Use high-resolution images only.
* Crop to focus on relevant content.
* Avoid showing user-sensitive data.
* Annotate images with labels or arrows only when necessary.

### 6.2 Diagrams

* Use clear diagrams for architecture, data flow, or system logic.
* Supported tools: **draw.io**, **PlantUML**, or **Excalidraw**.
* Always include alt text (for accessibility):

  ```markdown
  ![Acrosoft Architecture Diagram](./assets/architecture.png "Acrosoft System Overview")
  ```

### 6.3 Videos (Optional)

If including video tutorials:

* Keep under 3 minutes.
* Add closed captions.
* Host on the official Acrosoft YouTube or docs portal.

---

## 7. Naming Conventions

### 7.1 File Naming

Use **kebab-case** for filenames:

```
01_product_requirements_document.md  
03_api_documentation.md  
05_installation_configuration_guide.md
```

### 7.2 Variable & Command Names

* Maintain consistency between documentation and code.
* Use monospace (`code`) for all variable or command references.

### 7.3 API Endpoint Naming

Use REST best practices:

* Use **plural nouns** (e.g., `/projects`, `/tasks`, `/users`)
* Use **lowercase with hyphens** (e.g., `/user-profile`)
* Include version prefix (e.g., `/v1/projects`)

---

## 8. File Structure

All official documentation should follow this structure:

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

Each file should include:

* Version number
* Author name
* Last updated date
* References to related docs

---

## 9. Language and Terminology

### 9.1 Use Consistent Terms

| Preferred Term    | Avoid         | Reason                    |
| ----------------- | ------------- | ------------------------- |
| **Sign in**       | Log in        | More user-friendly        |
| **Client**        | Customer      | Industry-consistent       |
| **Dashboard**     | Control Panel | Simpler term              |
| **End user**      | Consumer      | Broader usage             |
| **Configuration** | Setup         | More precise in tech docs |

### 9.2 Abbreviations

* Spell out acronyms the first time they appear:

  * *Representational State Transfer (REST)*
  * *Continuous Integration (CI)*

---

## 10. Accessibility Guidelines

* Maintain **WCAG 2.1 AA** compliance.
* Use descriptive link text:

  * ✅ “View installation guide”
  * ❌ “Click here”
* Include alt text for all images.
* Avoid relying solely on color to convey meaning.

---

## 11. Review and Approval Process

1. Draft documentation must undergo **peer review** before publishing.
2. Use **GitHub pull requests** for changes.
3. Each doc must include a changelog or commit message referencing the update.
4. The documentation lead reviews for:

   * Accuracy
   * Formatting consistency
   * Tone alignment
   * Compliance with this guide

---

## 12. Version Control & Updates

### 12.1 Versioning Policy

| Change Type    | Version Increment | Example |
| -------------- | ----------------- | ------- |
| Minor text fix | +0.0.1            | 1.0.1   |
| Feature update | +0.1.0            | 1.1.0   |
| Major rewrite  | +1.0.0            | 2.0.0   |

### 12.2 Update Checklist

Before publishing updates:

* ✅ Verify technical accuracy
* ✅ Update “Last Updated” date
* ✅ Cross-check links and filenames
* ✅ Validate Markdown formatting
* ✅ Review by at least one SME (Subject Matter Expert)

---

## 13. Example Templates

### Section Template

## [Section Title]
### Purpose
Describe what this section covers.

### Steps
1. Step one
2. Step two
3. Step three

### Example
Provide code or screenshot example if relevant.

### Table Template


| Field | Type | Description |
|--------|------|-------------|
| Example | String | Short explanation |

---

## 14. References

* [Microsoft Writing Style Guide](https://learn.microsoft.com/en-us/style-guide/welcome/)
* [Google Developer Documentation Style Guide](https://developers.google.com/style)
* [The Chicago Manual of Style](https://www.chicagomanualofstyle.org/)
* [Acrosoft Documentation Standards](./README.md)

---

**End of Document**
