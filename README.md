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
                  Incoming email + attachment
                              │
                              ▼
                 ┌─────────────────────────┐
                 │       AMAZON SES        │
                 │                         │
                 │ Receives incoming email │
                 │ from the company        │
                 └────────────┬────────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │        AMAZON S3         │
                 │                         │
                 │ Stores the email and    │
                 │ document attachments    │
                 └────────────┬────────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │     EVENTBRIDGE         │
                 │                         │
                 │ Detects the S3 upload   │
                 │ event and routes it     │
                 └────────────┬────────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │          SQS            │
                 │                         │
                 │ Queues processing jobs  │
                 │ and handles traffic     │
                 └────────────┬────────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │         LAMBDA          │
                 │                         │
                 │ Reads the event and     │
                 │ starts document         │
                 │ processing              │
                 └────────────┬────────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │    STEP FUNCTIONS       │
                 │                         │
                 │ Orchestrates the entire │
                 │ document workflow       │
                 └────────────┬────────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │       TEXTRACT          │
                 │                         │
                 │ Extracts text and       │
                 │ document information    │
                 └────────────┬────────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │    AI CLASSIFICATION    │
                 │                         │
                 │ Determines the document │
                 │ category                │
                 │                         │
                 │ Example: Invoice        │
                 └────────────┬────────────┘
                              │
                              ▼
                       DECISION LOGIC
                    Checks classification
                         confidence
                              │
                    ┌─────────┴─────────┐
                    │                   │
                  ≥ 90%                < 90%
                    │                   │
                    ▼                   ▼
          ┌─────────────────┐   ┌─────────────────┐
          │ AUTOMATIC ROUTE │   │  MANUAL REVIEW  │
          │                 │   │                 │
          │ Document is     │   │ Human checks   │
          │ routed to its   │   │ the document   │
          │ category        │   │                │
          └────────┬────────┘   └────────┬────────┘
                   │                     │
                   └──────────┬──────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │       DYNAMODB          │
                 │                         │
                 │ Stores classification,  │
                 │ confidence, status and │
                 │ processing information  │
                 └─────────────────────────┘