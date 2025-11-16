---
title: Bank Extractor
slug: bankextractor
description: This repository is a Node.js/TypeScript microservice for extracting financial data from bank statement PDFs and performing credit analysis.
longDescription: ""
cardImage: "https://github.com/narsyad"
tags: ["back-end", "typescript","nodejs","express"]
githubUrl: https://github.com/narsyad
timestamp: 2025-02-24T02:39:03+00:00
featured: true
---

# Repository Analysis: ts-bank-statement-extractor

This repository is a Node.js/TypeScript microservice for extracting financial data from bank statement PDFs and performing credit analysis.  

## Key Features

**1. Multi-Bank PDF Extraction**: Supports 15+ Indonesian banks with multiple format versions including BCA, BNI, BRI, Mandiri, BSI, CIMB Niaga, OCBC NISP, Danamon, Muamalat, Permata, BTPN, and others.   The system automatically detects bank statement versions and routes to appropriate parsers.  

**2. PDF Tampering Detection**: Validates PDF authenticity by checking producer metadata for editing tools (ITEXT, IBEX, FPDF, etc.) and comparing creation vs modification dates.  

**3. Credit Engine Analysis**: Processes extracted transaction data to generate credit reports, summaries, and Excel exports for financial assessment.  

**4. Excel Report Generation**: Converts extracted data into structured Excel workbooks with summary and transaction detail sheets.  

## Tech Stack

**Runtime & Framework**:
- Node.js with TypeScript 4.8.3
- Express.js 4.17.3 web framework  

**PDF Processing**:
- `pdf-parse` (1.1.1) and `pdfreader` (2.0.0) for text extraction  
- `formidable` (1.2.6) for file upload handling  

**Database & Caching**:
- PostgreSQL with Sequelize ORM (6.23.0)  
- Redis 3.0.2 for session management  

**Security & Validation**:
- JWT authentication (jsonwebtoken 9.0.0)  
- Joi 17.6.0 for request validation  
- crypto-js for encryption  

**Observability**:
- OpenTelemetry SDK (0.48.0) with gRPC trace export to Tempo  
- Winston logger with daily file rotation  

**Output Generation**:
- xlsx (0.18.0) for Excel generation  
- archiver (5.3.1) for file compression  

## Core Module Functions

**BanksStatementService** (`service/BanksStatementService.ts`): Main orchestrator that:
1. Validates PDF metadata and checks for tampering  
2. Extracts PDF content using appropriate parser  
3. Detects bank statement version by scanning content patterns  
4. Routes to bank-specific processors (e.g., BCA, BNI, BRI)  
5. Returns structured JSON with account info, transactions, balances, and tampering flags  

**Bank-Specific Processors** (e.g., `BCA_V2`, `BNI_V5`, `BRI`): Each implements `processContent()` to parse bank-specific PDF formats and extract:
- Account number
- Period (month/year)
- Opening/closing balances
- Debit/credit mutations
- Transaction details (date, description, amounts)    

**Credit Engine** (`BcaCreditEngine`): Aggregates multi-month statements, sorts chronologically, and generates credit analysis reports with debit/credit categorization.  

## Architecture

**Layered Architecture**:
```
HTTP Client → Express App → Router → Middleware → Controller → Service → Database/Redis
```

**Request Flow**:
1. Express middleware stack: response-time, CORS, body-parser (100MB limit)
2. Router maps endpoints to controllers
3. `serviceMiddleware` authenticates via HEADER_TOKEN or JWT+Redis
4. Controllers delegate to service layer
5. Services process PDFs and interact with PostgreSQL/Redis
6. Responses returned as JSON or Excel files<cite />

**Key Endpoints**:
- `POST /bank-statement/extract`: Extract PDF data
- `POST /credit-engine/data-viewer`: View credit data
- `POST /credit-engine/data-analysis`: Perform analysis
- `POST /credit-engine/excel`: Generate Excel reports
- `GET /health`: Health check<cite />

**Deployment**: Docker containers orchestrated by Docker Swarm, managed by PM2 in cluster mode (2 instances), with GitHub Actions CI/CD triggered by version tags.<cite />

## Notes

The system is production-ready with comprehensive observability (OpenTelemetry tracing to Tempo, Winston logging), security hardening (JWT auth, PDF tampering detection), and support for 15+ bank formats with automatic version detection. The modular bank processor design allows easy addition of new banks by implementing the `processContent()` interface.<cite />

