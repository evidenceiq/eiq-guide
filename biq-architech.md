# BIQ System Architecture Diagram

## Overview
```mermaid
graph TB
    %% External Components
    CT[Capture Tool] --> BUAS[bsuploadapiservice]
    API_CLIENT[API Client] --> BAPI[biqapiservice]
    WEB[Web Interface] --> BRDB[bsremotedbservice]
    WEB --> EIQ[eiqhelperservice]
    
    %% Import Flow
    BUAS --> S3[(S3 Storage)]
    BUAS --> DB[(Database)]
    BIS[bsimportservice] --> DB
    BIS --> S3
    BIS --> SQS[(SQS Queue)]
    BCAP[bscaptureservice] --> SQS
    BCAP --> DB
    BCAP --> S3
    
    %% Background Services
    BCSA[bscsaservice] --> DB
    BCSA --> ENG[ENGINE]
    BS1N[bssearch1nservice] --> DB
    BS1N --> ENG
    BSST[bsstorageservice] --> DB
    BSST --> S3
    BSTAT[bsstatservice] --> DB
    BMAIL[bsmailservice] --> DB
    BMAIL --> USERS[Reception/Firearm Examiners]
    
    %% Communication Services
    BRDB --> DB
    BAPI --> DB
    EIQ --> DB
    EIQ --> REPORTS[PDF Reports]
    
    %% Styling
    classDef service fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef storage fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef external fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef engine fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class BUAS,BIS,BCAP,BCSA,BS1N,BSST,BSTAT,BMAIL,BRDB,BAPI,EIQ service
    class S3,DB,SQS storage
    class CT,API_CLIENT,WEB,USERS external
    class ENG,REPORTS engine
```

## Detailed Import Process Flow
```mermaid
sequenceDiagram
    participant CT as Capture Tool
    participant BUAS as bsuploadapiservice
    participant S3 as S3 Storage
    participant DB as Database
    participant BIS as bsimportservice
    participant SQS as SQS Queue
    participant BCAP as bscaptureservice
    
    CT->>BUAS: 1. Upload package
    BUAS->>S3: 2. Upload package to S3
    BUAS->>DB: 2. Save package info
    
    BIS->>DB: 3. Read package info
    BIS->>S3: 3. Retrieve package
    BIS->>S3: 3. Push original images
    BIS->>DB: 3. Save metadata & S3 locations
    BIS->>SQS: 3. Push circle info
    
    BCAP->>SQS: 4. Read circle info
    BCAP->>DB: 4. Get image info
    BCAP->>S3: 4. Read image data
    BCAP->>S3: 4. Save cut circle images
    BCAP->>DB: 4. Save circle image locations
```

## Background Services Architecture
```mermaid
graph LR
    subgraph "Background Processing"
        BCSA[bscsaservice<br/>CSA Processing]
        BS1N[bssearch1nservice<br/>Search Processing]
        BSST[bsstorageservice<br/>Data Management]
        BSTAT[bsstatservice<br/>Statistics]
        BMAIL[bsmailservice<br/>Report Generation]
    end
    
    subgraph "Data Layer"
        DB[(Database)]
        S3[(S3 Storage)]
        SQS[(SQS Queue)]
    end
    
    subgraph "External Systems"
        ENG[Processing Engine]
        USERS[Reception/<br/>Firearm Examiners]
    end
    
    BCSA --> DB
    BCSA --> ENG
    BS1N --> DB
    BS1N --> ENG
    BSST --> DB
    BSST --> S3
    BSST --> SQS
    BSTAT --> DB
    BMAIL --> DB
    BMAIL --> USERS
    
    classDef service fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef storage fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef external fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class BCSA,BS1N,BSST,BSTAT,BMAIL service
    class DB,S3,SQS storage
    class ENG,USERS external
```

## Communication Services Layer
```mermaid
graph TB
    subgraph "Client Layer"
        WEB[Web Interface]
        API_CLIENT[API Client]
    end
    
    subgraph "Communication Services"
        BRDB[bsremotedbservice<br/>Database Access]
        BAPI[biqapiservice<br/>API Gateway]
        EIQ[eiqhelperservice<br/>Helper Services]
    end
    
    subgraph "Data & Processing"
        DB[(Database)]
        PDF[PDF Reports]
        IMG_PROC[Image Processing<br/>Pattern Matching<br/>MFA Flow]
    end
    
    WEB --> BRDB
    WEB --> EIQ
    API_CLIENT --> BAPI
    
    BRDB --> DB
    BAPI --> DB
    EIQ --> DB
    EIQ --> PDF
    EIQ --> IMG_PROC
    
    classDef client fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef service fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef data fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef processing fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class WEB,API_CLIENT client
    class BRDB,BAPI,EIQ service
    class DB,PDF data
    class IMG_PROC processing
```

## Component Responsibilities

### Import Services
- **bsuploadapiservice**: Handles package uploads and initial S3 storage
- **bsimportservice**: Processes packages, extracts data, manages image storage
- **bscaptureservice**: Processes circle cutting from uploaded images

### Background Processing Services  
- **bscsaservice**: CSA (Cartridge Case Analysis) processing with external engine
- **bssearch1nservice**: 1:N search processing and result management
- **bsstorageservice**: Data sharing, RBT, auditing, and annotation processing
- **bsstatservice**: Statistics calculation for gallery, auditing, RBT, contracts
- **bsmailservice**: Report generation and email distribution

### Communication Services
- **bsremotedbservice**: Database access layer for web interface
- **biqapiservice**: API gateway for external clients
- **eiqhelperservice**: PDF generation, real-time image processing, MFA flow