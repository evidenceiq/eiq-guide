# FIQ System Architecture Diagram

```mermaid
graph TB
    %% Client Layer
    WEB[Web/Mobile Client]
    
    %% API Layer
    SIQ[siqapiservice<br/>SIQ API Service]
    FWS[fiqwebservice<br/>FIQ Web Service]
    
    %% Processing Services
    STD[spstandardizeservice<br/>Standardize Service]
    SEARCH[spsearch1nservice<br/>Search Service]
    STORAGE[spstorageservice<br/>Storage Service]
    
    %% Storage Layer
    S3[AWS S3<br/>File Storage]
    DB[(Database)]
    SQS[AWS SQS<br/>Message Queue]
    ENGINE[Search Engine]
    
    %% Import Flow
    WEB -->|1. Upload Package| SIQ
    SIQ -->|2. Store Package| S3
    SIQ -->|2. Save Package Info| DB
    SIQ -->|3. Extract & Store Images| S3
    SIQ -->|3. Save Metadata| DB
    SIQ -->|3. Queue Standardization| SQS
    
    %% Standardization Flow
    SQS -->|4. Process Queue| STD
    STD -->|Get Image Info| DB
    STD -->|Read Images| S3
    STD -->|Store Standardized| S3
    STD -->|Save Results| DB
    
    %% Background Processing
    SEARCH -->|Read Data| DB
    SEARCH -->|Send Images/Info| ENGINE
    ENGINE -->|Return Results| SEARCH
    SEARCH -->|Save Results| DB
    
    %% Web Communication
    WEB -->|API Requests| FWS
    FWS -->|Data Operations| DB
    
    %% Storage Service
    STORAGE -->|Manage Files| S3
    STORAGE -->|File Metadata| DB
    
    %% Styling
    classDef client fill:#e1f5fe
    classDef service fill:#f3e5f5
    classDef storage fill:#e8f5e8
    classDef queue fill:#fff3e0
    
    class WEB client
    class SIQ,FWS,STD,SEARCH,STORAGE service
    class S3,DB,STORAGE storage
    class SQS,ENGINE queue
```

## System Components

### Services
- **siqapiservice**: Main API service handling package uploads and processing
- **fiqwebservice**: Web service for client communication and data retrieval
- **spstandardizeservice**: Image standardization processing service
- **spsearch1nservice**: Background search and matching service
- **spstorageservice**: File storage management service

### Infrastructure
- **AWS S3**: Object storage for packages and images
- **Database**: Metadata and processing results storage
- **AWS SQS**: Message queue for asynchronous processing
- **Search Engine**: External engine for image matching and search

## Process Flow

### Import Workflow
1. **Web/Mobile** uploads package to **siqapiservice**
2. **siqapiservice** uploads package to **S3**, saves package info to **database**
3. **siqapiservice** extracts data, stores original images in **S3**, saves **metadata** and **S3 locations** to **database**, queues **standardization info** to **SQS**
4. **spstandardizeservice** processes **SQS** messages, retrieves images from **S3**, performs **standardization**, saves **standardized images** to **S3** and updates **database**

### Background Processing
1. **spsearch1nservice** reads data from **database**, sends info/images to **Search Engine**
2. Waits for processing completion and saves results to **database**
3. Stores **probe, hits, and search filters** information to **database**

### Communication
1. **fiqwebservice**: Handles web requests for retrieving/saving data from/to **database**