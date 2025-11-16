---
title: Golang Startup
slug: gostartup
description: This is a crowdfunding platform
longDescription: ""
cardImage: "https://github.com/narsyad/golang_startup"
tags: ["back-end", "golang"]
githubUrl: https://github.com/narsyad/golang_startup
timestamp: 2025-02-24T02:39:03+00:00
featured: true
---

## Key Features

This is a crowdfunding platform with the following core features:

- **User Management**: Registration, login, email availability checking, and avatar upload   
- **Campaign Management**: Create, update, list, and view campaign details with image uploads   
- **Transaction Processing**: Create transactions, process payments via Midtrans payment gateway, and handle payment notifications   
- **JWT Authentication**: Token-based authentication for protected endpoints  

## Tech Stack  

**Core Technologies:**
- **Language**: Go 1.17
- **Web Framework**: Gin (v1.7.7) for HTTP routing and middleware
- **ORM**: GORM (v1.22.3) with MySQL driver (v1.2.0)
- **Authentication**: JWT tokens via `dgrijalva/jwt-go` (v3.2.0)
- **Password Hashing**: bcrypt via `golang.org/x/crypto`
- **Payment Gateway**: Midtrans Snap Gateway via `veritrans/go-midtrans`
- **Validation**: `go-playground/validator` (v10.9.0)
- **Configuration**: Viper (v1.9.0) for environment variables
- **URL Slugs**: `gosimple/slug` (v1.11.2)

## Core Module Functions

### 1. User Module  

- **RegisterUser**: Creates new user with bcrypt password hashing and default "user" role  
- **LoginUser**: Authenticates users by email/password comparison  
- **IsEmailAvailable**: Checks email uniqueness for registration  
- **UploadAvatar**: Handles user profile image uploads

### 2. Campaign Module  

- **GetCampaigns**: Lists all campaigns or filters by user ID  
- **CreateCampaign**: Creates new campaigns with slug generation
- **UpdateCampaign**: Updates campaign details with ownership validation  
- **SaveCampaignImage**: Manages campaign images with primary flag support  

### 3. Transaction Module  

- **CreateTransaction**: Initiates transaction with "pending" status, generates Midtrans payment URL  
- **ProcessPayment**: Handles Midtrans webhook notifications, updates transaction status ("paid"/"cancelled"), and increments campaign statistics (BackerCount, CurrentAmount)  
- **GetTransactionCampaignById**: Retrieves campaign transactions with authorization check  
- **GetTransactionUserById**: Retrieves user's transaction history  

### 4. Payment Module  

- **GetPaymentUrl**: Integrates with Midtrans Snap Gateway to generate payment URLs  

## Architecture

The application follows a **three-tier layered architecture** with strict unidirectional dependency flow:  

### Layer Structure:

1. **Handler Layer** (Presentation): HTTP request/response handling, input validation, response formatting  

2. **Service Layer** (Business Logic): Business rules, validation, orchestration between repositories and external services  

3. **Repository Layer** (Data Access): Database operations via GORM, CRUD operations  

### Dependency Injection Flow:
- Database connection → Repositories → Services → Handlers → Routes
- All dependencies are injected through constructors in `main.go`
- Services depend on repository interfaces (not implementations) for testability

### Key Patterns:
- **Repository Pattern**: Abstracts database operations
- **Interface-based Design**: Enables loose coupling and mocking
- **Middleware**: JWT authentication middleware for protected routes  

## Notes

The transaction system implements a multi-phase commit pattern where transactions are created with "pending" status, payment URLs are generated via Midtrans, and webhook notifications asynchronously update transaction status and campaign statistics. The system uses denormalized campaign statistics (BackerCount, CurrentAmount) for performance, though it lacks idempotency guards for duplicate webhook notifications.<cite /></cite />

