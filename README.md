# Email Attachment Classification in AWS

An AWS-based, event-driven system designed to automatically receive emails, process their attachments, classify documents using AI, and route them based on the classification result.

---

## 📌 Project Overview

Organizations often receive hundreds of emails containing different types of documents such as:

- Invoices
- Purchase Orders
- Quotations
- Receipts
- Contracts
- Resumes
- Reports

Manually opening and sorting these attachments can be time-consuming.

This project aims to automate the process using AWS services and AI.

The system receives an email, stores the attachment, extracts document information, classifies the document, and then either routes it automatically or sends it for manual review.

---

# 🏗️ Architecture

```text
                    COMPANY EMAIL
                          │
                          ▼
                   ┌─────────────┐
                   │ Amazon SES  │
                   │ Email Entry │
                   └──────┬──────┘
                          │
                          ▼
                   ┌─────────────┐
                   │  Amazon S3  │
                   │   Storage   │
                   └──────┬──────┘
                          │
                          ▼
                   ┌─────────────┐
                   │ EventBridge │
                   │ Event Route │
                   └──────┬──────┘
                          │
                          ▼
                   ┌─────────────┐
                   │    SQS      │
                   │    Queue    │
                   └──────┬──────┘
                          │
                          ▼
                   ┌─────────────┐
                   │   Lambda    │
                   │ Application │
                   │   Logic     │
                   └──────┬──────┘
                          │
                          ▼
                   ┌─────────────┐
                   │    Step     │
                   │  Functions  │
                   │ Orchestrator│
                   └──────┬──────┘
                          │
                          ▼
                   ┌─────────────┐
                   │  Textract   │
                   │ OCR / Data  │
                   │ Extraction │
                   └──────┬──────┘
                          │
                          ▼
                   ┌─────────────┐
                   │     AI      │
                   │Classification│
                   └──────┬──────┘
                          │
                          ▼
                     DECISION
                    /          \
                 ≥90%          <90%
                  /              \
                 ▼                ▼
          Automatic Route    Manual Review
                 \              /
                  \            /
                   ▼          ▼
                   ┌─────────────┐
                   │  DynamoDB   │
                   │   Results   │
                   └─────────────┘