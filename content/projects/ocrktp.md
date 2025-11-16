---
title: OCR KTP
slug: ocrktp
description: The system provides OCR (Optical Character Recognition) capabilities for Indonesian KTP documents
longDescription: ""
cardImage: "https://github.com/narsyad/ocr-ktp-final"
tags: ["back-end", "tesseract","ocr" ]
githubUrl: https://github.com/narsyad/ocr-ktp-final
timestamp: 2025-02-24T02:39:03+00:00
featured: true
---
## Key Features

The system provides OCR (Optical Character Recognition) capabilities for Indonesian KTP documents with the following features:<cite />

- **KTP Image Processing**: Accepts image uploads (JPEG/PNG, 50KB-5MB) and extracts structured data from Indonesian identity cards  
- **Multi-stage OCR**: Uses three separate Tesseract workers to process different regions of the KTP (header, NIK number, and main content) with specialized character whitelists  
- **Adaptive Image Preprocessing**: Dynamically adjusts threshold values to optimize image quality for OCR accuracy  
- **NIK Validation & Parsing**: Validates and extracts demographic information (birthdate, gender) from the 16-digit NIK using `nik-parser`  
- **Geographic Data Enrichment**: Matches extracted location data against a PostgreSQL database to retrieve standardized province, city, district, and postal code information  
- **Rate Limiting**: Redis-based rate limiting (1 request per 14 seconds per IP)  

## Tech Stack

**Backend Framework:**<cite />
- Express.js 4.17.3 with TypeScript 4.7.3  

**OCR & Image Processing:**<cite />
- tesseract.js 3.0.3 (OCR engine)  
- jimp 0.16.2 with plugins (crop, gaussian blur, threshold)  
- Custom trained data files: `ind.traineddata` and `ocr.traineddata`<cite />

**Data Layer:**<cite />
- PostgreSQL with Sequelize 6.21.6 ORM  
- Redis 3.0.2 for caching and rate limiting  

**Supporting Libraries:**<cite />
- formidable 2.1.0 (multipart form parsing)  
- Joi 14.3.1 (validation)  
- nik-parser 1.0.1 (Indonesian ID parsing)  
- crypto-js 4.1.1 (credential encryption)  
- Winston 3.6.0 (logging)  

**Process Management:**<cite />
- PM2 in cluster mode (2 instances)<cite />

## Core Module Functions

**1. Request Handling (`OcrKtpController`)**  
- Parses multipart form data using formidable  
- Validates image mimetype (JPEG/PNG) and size (50KB-5MB)  
- Delegates to `OcrKtpService` for processing  

**2. OCR Processing (`OcrKtpService`)**  

The service implements a sophisticated multi-stage pipeline:<cite />

- **Image Preprocessing**: Resizes to 1500x767, then creates three cropped regions (header, NIK, main content) with adaptive thresholding  
- **Parallel OCR Execution**: Runs three Tesseract workers simultaneously with different language models and character whitelists  
- **Field Extraction**: Parses 13+ fields including name, birthplace, address, RT/RW, religion, marital status, occupation  
- **Data Enrichment**: Queries PostgreSQL to match extracted location strings with standardized geographic data and lookup IDs for gender, religion, and marital status  

**3. Routing (`Router.ts`)**  
- POST `/ocr-ktp` endpoint for OCR processing  
- GET `/health`, `/ping` endpoints for health checks  

## Architecture Overview

The system follows a **layered architecture** with clear separation of concerns:<cite />

```
Client → PM2 (2 instances) → Express App → Middleware (CORS, Rate Limiter) 
→ Router → Controller (validation) → Service (OCR processing) 
→ Data Layer (PostgreSQL + Redis)
```

**Key architectural patterns:**<cite />
- **Process Management**: PM2 cluster mode for load balancing and auto-restart<cite />
- **Middleware Stack**: Response time tracking, CORS, body parsing (100MB limit)<cite />
- **Parallel Processing**: Three concurrent Tesseract workers for different KTP regions  
- **Adaptive Optimization**: Dynamic threshold adjustment loop to find optimal image quality  
- **Dual Persistence**: PostgreSQL for structured data, Redis for caching/rate limiting<cite />

## Strengths

1. **Robust OCR Pipeline**: Multi-stage processing with specialized workers for different KTP regions improves accuracy<cite />
2. **Adaptive Image Processing**: Dynamic threshold adjustment ensures optimal OCR results across varying image qualities  
3. **Comprehensive Validation**: Input validation at multiple levels (Joi schema, NIK parser)  
4. **Data Enrichment**: Automatic lookup of standardized geographic and demographic data  
5. **Production-Ready**: PM2 clustering, rate limiting, structured logging, and error handling<cite />

## Potential Improvements

1. **Security Concerns**: Hardcoded encryption key `'FIT999'` should be moved to environment variables<cite />
2. **Error Handling**: Generic console.log statements should use structured logging throughout  
3. **Performance**: Infinite loop with 50-iteration limit could be optimized with binary search for threshold values  
4. **Code Quality**: Magic numbers (pixel dimensions, threshold values) should be extracted as named constants  
5. **Testing**: No test files visible; should add unit and integration tests<cite />
6. **API Documentation**: Missing OpenAPI/Swagger documentation for the `/ocr-ktp` endpoint<cite />
7. **Resource Management**: Tesseract workers should be pooled and reused instead of created per request  
8. **Rate Limiting**: Rate limiter imported but not applied to the route  

## Notes

The system is specifically designed for Indonesian KTP documents and includes domain-specific logic for parsing NIK numbers, Indonesian location hierarchies (province/city/district/subdistrict), and local demographic categories (religion, marital status).<cite /> The adaptive threshold optimization is particularly noteworthy as it automatically adjusts image preprocessing parameters to achieve optimal OCR accuracy.<cite />


