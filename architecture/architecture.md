# Email Attachment Classification in AWS

## 1. Project Overview

This project is an AWS-based system designed to automatically receive emails, extract their attachments, process the attached documents, classify them using AI, and route them based on the classification result.

The system is designed for organizations that receive a large number of emails containing documents such as:

- Invoices
- Purchase Orders
- Quotations
- Receipts
- Contracts
- Resumes
- Reports
- Other business documents

The goal is to reduce manual document sorting and create an automated, scalable document-processing workflow.

---

# 2. High-Level Architecture

```text
Company Email
      ↓
Amazon SES
      ↓
Amazon S3
      ↓
Amazon EventBridge
      ↓
Amazon SQS
      ↓
AWS Lambda
      ↓
AWS Step Functions
      ↓
Amazon Textract
      ↓
AI Classification
      ↓
Decision
   ↙       ↘
≥90%      <90%
 ↓          ↓
Automatic  Manual
Routing    Review
   ↘       ↙
    Amazon DynamoDB
