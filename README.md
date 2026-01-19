# 📊 n8n Trips Data Automation

An end-to-end **workflow automation system** built using **n8n** to streamline trip data management for a tourism / car rental business.  
The system automates trip data collection, centralized record keeping, duty slip generation, and email delivery — with zero manual intervention after setup.

---

## 🚀 Project Overview

This project implements a **server-side automation pipeline** that:

1. Accepts trip details via an n8n-hosted form  
2. Appends each trip as a new row into an existing CSV file stored on Google Drive  
3. Dynamically generates a duty slip using HTML templating  
4. Converts the HTML into an image using a headless rendering engine  
5. Sends the generated duty slip as an email attachment using SMTP  

The solution ensures **data consistency**, **auditability**, and **operational efficiency**.

---

## 🧠 Architecture Summary

```Form Submission (HTTP Trigger)
│
├── CSV Pipeline
│ ├── Download existing trips.csv (Google Drive API)
│ ├── Append new row (JavaScript processing)
│ └── Upload updated CSV (overwrite same file)
│
└── Document Pipeline
├── Generate HTML duty slip (templating)
├── Convert HTML → Image
└── Send email with attachment (SMTP)
```

Both pipelines run **in parallel** from a single form submission.

---

## 🔧 Technical Implementation Details

### 1. Data Ingestion
- Uses **n8n “On Form Submission” trigger**
- Emits structured JSON containing trip details
- Acts as the single source of truth for downstream nodes

### 2. Persistent CSV Storage (Google Drive)
- Uses **Google Drive API v3**
- Downloads an existing `trips.csv` file by File ID
- Ensures all trip records are stored in one continuously growing file

### 3. CSV Append Logic
- Implemented using a **JavaScript Function node**
- Responsibilities:
  - Decode Base64 CSV content to UTF-8
  - Validate or create CSV headers
  - Escape values safely (quotes, commas)
  - Append a new row
  - Re-encode CSV to Base64 for upload

This avoids file duplication and guarantees atomic updates.

### 4. Duty Slip Generation
- Uses an **HTML template** with dynamic placeholders
- n8n expression engine (`{{$json[...]}}`) injects runtime values
- Template mirrors a real-world duty slip layout

### 5. HTML to Image Rendering
- Uses a Chromium-based headless renderer
- Converts styled HTML + CSS into a PNG image
- Ensures consistent visual output across executions

### 6. Email Delivery
- Uses **SMTP Email Send node**
- Authenticated via Gmail App Password
- Sends multipart MIME email with binary attachment
- Works independently of Gmail OAuth limitations

---

## 🛠️ Tech Stack

| Component | Technology |
|--------|------------|
| Workflow engine | n8n (Docker) |
| Backend runtime | Node.js (inside n8n) |
| Storage | Google Drive (CSV) |
| Rendering | Headless Chromium (HTML → Image) |
| Email | SMTP (Gmail App Password) |
| Data formats | JSON, CSV, Base64 |
| Authentication | OAuth 2.0 (Drive), SMTP auth |

---

## 📂 Repository Structure
```
├── trips-data-automation.json # Exported n8n workflow
├── README.md # Project documentation
└── .gitignore # Excludes secrets and runtime files
```

---

## 🔧 Technical Implementation Details

### 1. Data Ingestion
- Uses **n8n “On Form Submission” trigger**
- Emits structured JSON containing trip details
- Acts as the single source of truth for downstream nodes

### 2. Persistent CSV Storage (Google Drive)
- Uses **Google Drive API v3**
- Downloads an existing `trips.csv` file by File ID
- Ensures all trip records are stored in one continuously growing file

### 3. CSV Append Logic
- Implemented using a **JavaScript Function node**
- Responsibilities:
  - Decode Base64 CSV content to UTF-8
  - Validate or create CSV headers
  - Escape values safely (quotes, commas)
  - Append a new row
  - Re-encode CSV to Base64 for upload

This avoids file duplication and guarantees atomic updates.

### 4. Duty Slip Generation
- Uses an **HTML template** with dynamic placeholders
- n8n expression engine (`{{$json[...]}}`) injects runtime values
- Template mirrors a real-world duty slip layout

### 5. HTML to Image Rendering
- Uses a Chromium-based headless renderer
- Converts styled HTML + CSS into a PNG image
- Ensures consistent visual output across executions

### 6. Email Delivery
- Uses **SMTP Email Send node**
- Authenticated via Gmail App Password
- Sends multipart MIME email with binary attachment
- Works independently of Gmail OAuth limitations

---

## 🔐 Security Considerations

- No credentials or secrets are committed
- OAuth tokens and SMTP passwords are stored only in n8n credential store
- `.gitignore` explicitly excludes sensitive files
- Google OAuth client secrets must be regenerated if compromised

---

## ⚙️ Setup Requirements (High Level)

- n8n running locally or on a server (Docker recommended)
- Google Cloud project with:
  - Google Drive API enabled
  - OAuth Client (Web application)
- Gmail account with App Password enabled
- Existing `trips.csv` file in Google Drive

> Detailed setup steps are intentionally excluded from the repository to avoid leaking sensitive configuration.

---

## ✅ Key Outcomes

- Single source of truth for trip records
- No duplicate files created on Google Drive
- Automated, consistent duty slip generation
- Reduced manual operational workload
- Easily extensible for PDF generation, WhatsApp delivery, or database storage

---

## 🔮 Future Enhancements

The current implementation provides a reliable automation pipeline for trip data handling.  
The following enhancements can further improve scalability, security, and production readiness:

### 1. Database-Backed Storage
- Replace CSV-based storage with a relational database (PostgreSQL/MySQL)
- Enable concurrent writes, indexing, and complex queries
- Improve long-term scalability and data integrity

### 2. PDF-Based Duty Slip Generation
- Generate duty slips directly as PDFs instead of images
- Support printing, digital signing, and archival compliance
- Improve document portability and professional presentation

### 3. Role-Based Access Control (RBAC)
- Introduce authentication and authorization layers
- Define roles such as Admin, Operations, and Finance
- Restrict workflow access based on user roles

### 4. Multi-Channel Notifications
- Extend delivery channels beyond email
- Integrate WhatsApp Business API, Slack, or SMS gateways
- Improve real-time communication with stakeholders

### 5. Centralized Error Handling & Monitoring
- Add workflow-level error handlers and retry mechanisms
- Integrate logging and alerting for failures
- Improve observability and operational reliability

### 6. Workflow Versioning & Rollbacks
- Store workflow versions in Git automatically
- Enable rollback to previous stable versions
- Improve deployment safety and traceability

### 7. Input Validation & Data Sanitization
- Validate form inputs such as timestamps, vehicle numbers, and distances
- Prevent malformed or inconsistent data entry
- Ensure clean downstream processing

### 8. Analytics & Reporting Dashboard
- Build dashboards for trip statistics and vehicle utilization
- Enable business insights through aggregated metrics
- Support decision-making for operations and finance teams

### 9. Cloud-Native Deployment
- Deploy n8n on cloud platforms (AWS/GCP/Azure)
- Use managed databases, object storage, and secrets managers
- Improve availability, security, and scalability

### 10. AI-Assisted Automation
- Introduce AI for trip summarization and anomaly detection
- Predict high-usage vehicles or driver fatigue risks
- Enhance automation intelligence using ML models

---

## 🔒 License & Usage Restrictions

© 2026 Hrishab Pachange. All Rights Reserved.

This project is **proprietary and confidential**.

❌ Unauthorized copying, modification, distribution, or use of this project, in whole or in part, is **strictly prohibited**.

This repository is provided **for reference and evaluation purposes only**.  
No permission is granted to:
- Use this project commercially
- Redistribute or resell the workflow
- Claim this project as your own
- Deploy it in another environment without explicit written consent

Any misuse may result in legal action.

For permissions or inquiries, contact the author directly.

---
> Designed and implemented as part of a real-world automation use case.
---

## 👤 Author

**Hrishab Pachange**  
AI/ML Engineer Intern & Backend Developer Intern at Plainsurf Solutions  
Computer Science Student  

**Technical Focus:**  
Golang • Python • FastAPI • n8n & AI Automation • RESTful APIs • Microservices • Cloud Computing • Generative AI

- GitHub: https://github.com/hrishabpachange
- LinkedIn: www.linkedin.com/in/hrishab-pachange-1667702b1

> © 2026 Hrishab Pachange. All rights reserved.

