---
title: Belimang Kopi
slug: belimangkopi
description: belimang-kopi is a Spring Boot-based e-commerce platform designed for multi-merchant food and beverage ordering with integrated geospatial capabilities.
longDescription: ""
cardImage: "https://github.com/narsyad/belimang-kopi"
tags: ["back-end", "java","spring-boot","docker","kubernetes","grafana","minio" ]
githubUrl: https://github.com/narsyad/belimang-kopi
timestamp: 2025-11-16T02:39:03+00:00
featured: true
---
## Key Features

This project implements a **location-based food ordering platform** with the following core capabilities:

1. **Geospatial Merchant Discovery**: Finds nearby merchants within 3km radius using PostGIS spatial queries  

2. **Route-Optimized Delivery**: Implements TSP (Traveling Salesman Problem) nearest neighbor algorithm for multi-merchant order routing  

3. **Role-Based Authentication**: Separate admin and user authentication flows with JWT tokens  

4. **Order Estimation & Placement**: Two-phase ordering (estimate first, then confirm) with atomic flag updates to prevent race conditions  

5. **Image Storage**: S3-compatible object storage via MinIO with presigned URLs for direct client uploads  

## Technology Stack

### Core Framework
- **Spring Boot 3.5.6** with Java 21   
- **Spring Web MVC** for REST API  
- **Spring Data JPA** with Hibernate ORM  
- **Spring Security** for authentication/authorization  

### Database & Persistence
- **PostgreSQL** with **PostGIS** extension for geospatial operations  
- **Hibernate Spatial 6.5.0** for JTS geometry types  
- **Flyway** for database migrations  

### Security
- **JWT (jjwt 0.12.5)** for stateless authentication  
- **BCrypt** password hashing with strength 4  

### Storage & Observability
- **MinIO 8.5.17** for S3-compatible object storage  
- **Micrometer + Prometheus** for metrics export  
- **Spring Boot Actuator** for health checks  

### Build & Deployment
- **Gradle 8.x** with GraalVM native image support  
- **Docker Compose** for local development (app + PostgreSQL + MinIO)  
- **Kubernetes** manifests for production deployment  

## Architecture Overview

```mermaid
graph TB
    Client[HTTP Client]
    
    subgraph "Security Layer"
        Guard[@Guard Annotation]
        JWTFilter[JWT Filter]
    end
    
    subgraph "API Layer"
        AuthCtrl[AuthController]
        OrderCtrl[OrderController]
        MerchantCtrl[MerchantController]
    end
    
    subgraph "Service Layer"
        AuthSvc[AuthServiceImpl]
        OrderSvc[OrderService<br/>TSP Algorithm]
        MerchantSvc[MerchantService]
    end
    
    subgraph "Data Layer"
        UserRepo[UserRepository]
        OrderRepo[OrderRepository]
        MerchantRepo[MerchantRepository<br/>PostGIS Queries]
    end
    
    subgraph "External Services"
        PostgreSQL[(PostgreSQL + PostGIS)]
        MinIO[MinIO Object Storage]
    end
    
    Client --> Guard
    Guard --> JWTFilter
    JWTFilter --> AuthCtrl
    JWTFilter --> OrderCtrl
    JWTFilter --> MerchantCtrl
    
    AuthCtrl --> AuthSvc
    OrderCtrl --> OrderSvc
    MerchantCtrl --> MerchantSvc
    
    AuthSvc --> UserRepo
    OrderSvc --> OrderRepo
    OrderSvc --> MerchantRepo
    MerchantSvc --> MerchantRepo
    
    UserRepo --> PostgreSQL
    OrderRepo --> PostgreSQL
    MerchantRepo --> PostgreSQL
    
    MerchantSvc --> MinIO
```

## Code Quality & Patterns Analysis

### Architectural Patterns

1. **Layered Architecture (N-Tier)**: Clear separation between Controller → Service → Repository → Database   

2. **Repository Pattern**: Spring Data JPA abstracts database access with both derived queries and native SQL  

3. **DTO Pattern**: Request/Response DTOs decouple API contracts from domain entities  

4. **Dependency Injection**: Constructor-based DI throughout (via Lombok `@RequiredArgsConstructor`)  

5. **Custom Annotation Pattern**: `@Guard` for declarative role-based access control  

### Design Strengths

- **Atomic Operations**: Uses PostgreSQL `RETURNING` clause to prevent race conditions in order placement  
- **Geospatial Optimization**: Leverages PostGIS spatial indexes for efficient proximity searches  
- **Centralized Exception Handling**: `@ControllerAdvice` maps exceptions to HTTP status codes  
- **Configuration Externalization**: Environment variable overrides for all sensitive configs  
- **Database Versioning**: Flyway migrations ensure reproducible schema evolution  

## Notes

The project demonstrates solid understanding of Spring Boot ecosystem and geospatial computing. The TSP implementation for delivery routing is a notable algorithmic feature. The architecture follows standard Spring Boot conventions with clear layer separation. The use of PostGIS for location-based queries and MinIO for object storage shows thoughtful technology selection for the problem domain. The codebase would benefit from extracting business logic into smaller, testable components and adding comprehensive test coverage.