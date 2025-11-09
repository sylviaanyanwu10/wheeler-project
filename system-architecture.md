# Wheeler System Architecture
## Overview
Wheeler is a peer-to-peer car sharing platform that connects car owners with renters through a secure digital interface. The system is designed around trust, simplicity, and convenience, with a focus on removing friction from the car rental process.
## Technology Stack
### Frontend
- **Mobile Application**: Native iOS and Android applications
  - Swift for iOS
  - Kotlin for Android
  - Cross-platform framework (React Native or Flutter) for future scalability
- **UI Components**: Material Design for Android, Human Interface Guidelines for iOS
- **Maps Integration**: Google Maps API for location services
- **Image Processing**: Image compression and optimization for car photos
### Backend
- **Server**: Node.js with Express.js framework
- **API**: RESTful API with JSON responses
- **Authentication**: JWT (JSON Web Tokens) for secure user authentication
- **File Storage**: AWS S3 for storing car images and documents
- **Real-time Communication**: WebSockets for instant notifications and messaging
### Database
- **Primary Database**: PostgreSQL for structured data (users, cars, bookings, payments)
- **Cache Layer**: Redis for session management and frequently accessed data
- **Search Engine**: Elasticsearch for efficient car search and filtering
### Third-party Integrations
- **Payment Gateway**: PayStack for secure payment processing
- **Verification Service**: Third-party KYC service for identity verification
- **Insurance API**: Integration with insurance providers for short-term coverage (LeadWay)
- **Push Notifications**: Firebase Cloud Messaging (FCM) or Apple Push Notification Service (APNS)
## System Components
### User Management Service
- Handles user registration, authentication, and profile management
- Implements KYC verification process
- Manages user roles (owner/renter)
### Car Listing Service
- Manages car information, photos, availability, and pricing
- Handles search and filtering functionality
- Maintains car status (available, booked, maintenance)
### Booking Service
- Manages the booking lifecycle from request to completion
- Handles car handover checklist and pickup instructions
- Tracks booking status and history
### Payment Service
- Manages secure payment processing through escrow
- Handles payment release to owners after successful rentals
- Processes refunds and dispute resolutions
### Insurance Service
- Integrates with insurance providers for short-term coverage
- Manages insurance activation and deactivation
- Handles claims processing
### Rating and Review Service
- Manages user ratings and reviews
- Calculates credibility scores based on historical data
- Provides recommendation algorithms
## Component Communication
### API Gateway
- Single entry point for all client requests
- Handles request routing, composition, and protocol translation
- Implements rate limiting and authentication
### Service Communication
- Internal services communicate through REST APIs
- Asynchronous communication handled by message queues (RabbitMQ or Kafka)
- Event-driven architecture for booking lifecycle management
### Data Flow
1. User actions trigger API calls to the API Gateway
2. Gateway routes requests to appropriate services
3. Services process requests and interact with the database
4. Responses flow back through the gateway to the client
5. Critical events trigger notifications through the messaging system
## Security Measures
### Authentication and Authorization
- JWT-based authentication with refresh tokens
- Role-based access control
- OAuth 2.0 integration for third-party login options
### Data Protection
- End-to-end encryption for sensitive data
- PCI DSS compliance for payment processing
- GDPR compliance for user data handling
### Fraud Prevention
- Machine learning algorithms for suspicious activity detection
- Multi-factor authentication for high-risk operations
- Real-time monitoring and alerting system
## Technical Feasibility
### Scalability
- Microservices architecture allows independent scaling of components
- Horizontal scaling with containerization (Docker and Kubernetes)
- Database sharding for handling large volumes of data
### Performance
- Caching strategies for frequently accessed data
- CDN for static assets and images
- Database indexing for efficient queries
### Development Approach
- MVP focus on core features: user authentication, car listings, booking, payment, and ratings
- Agile development methodology with iterative releases
- Continuous integration and deployment (CI/CD) pipeline
### Cost Considerations
- Cloud-based infrastructure (AWS or Google Cloud) for cost-effective scaling
- Serverless functions for infrequent operations to reduce costs
- Asset-light model eliminates need for physical infrastructure
The Wheeler system architecture is designed to be technically feasible through a lean MVP approach, focusing on core functionality while ensuring security, scalability, and performance. The microservices architecture allows for incremental development and future expansion without major architectural changes.


```mermaid
graph LR
    %% Define Styles for different component types
    classDef userStyle fill:#E3F2FD,stroke:#2196F3,color:#0D47A1
    classDef clientStyle fill:#E8F5E9,stroke:#4CAF50,color:#1B5E20
    classDef gatewayStyle fill:#FFF3E0,stroke:#FF9800,color:#E65100
    classDef serviceStyle fill:#F3E5F5,stroke:#9C27B0,color:#4A148C
    classDef dataStyle fill:#ECEFF1,stroke:#607D8B,color:#263238
    classDef externalStyle fill:#FFEBEE,stroke:#F44336,color:#B71C1C
    %% Users
    EndUser[End Users]:::userStyle
    Admin[Admin Users]:::userStyle
    %% Client-Side
    WebApp[Web Application<br>React/Vue]:::clientStyle
    MobileApp[Mobile App<br>iOS/Android]:::clientStyle
    AdminPanel[Admin Dashboard]:::clientStyle
    %% API Gateway
    Gateway[API Gateway<br>(Authentication, Routing, Rate Limiting)]:::gatewayStyle
    %% Backend Microservices
    UserService[User Management Service<br>(Profiles, Preferences)]:::serviceStyle
    NotificationService[Notification Service<br>(Processing, Delivery)]:::serviceStyle
    TemplateService[Template Service<br>(Content Management)]:::serviceStyle
    EmailService[Email Service<br>(SMTP Integration)]:::serviceStyle
    PushService[Push Notification Service<br>(FCM/APNS Integration)]:::serviceStyle
    AnalyticsService[Analytics Service<br>(Tracking, Reporting)]:::serviceStyle
    %% Message Queue for Async Communication
    MessageQueue[Message Queue<br>(RabbitMQ)]:::serviceStyle
    %% Data Storage
    PostgreSQL[(PostgreSQL<br>Primary DB)]:::dataStyle
    Redis[(Redis<br>Cache & Rate Limiting)]:::dataStyle
    S3[(AWS S3<br>File Storage)]:::dataStyle
    %% Third-Party Integrations
    SendGrid[SendGrid<br>Email Service]:::externalStyle
    FCM[Firebase Cloud Messaging<br>Push Service]:::externalStyle
    APNS[Apple Push Notification<br>Service]:::externalStyle
    WebPush[Web Push<br>Service]:::externalStyle
    AnalyticsAPI[Google Analytics<br>API]:::externalStyle
    %% Connections - User to Client
    EndUser -- "HTTPS API" --> WebApp
    EndUser -- "HTTPS API" --> MobileApp
    Admin -- "HTTPS API" --> AdminPanel
    %% Connections - Client to Gateway
    WebApp -- "HTTPS Request" --> Gateway
    MobileApp -- "HTTPS Request" --> Gateway
    AdminPanel -- "HTTPS Request" --> Gateway
    %% Connections - Gateway to Services
    Gateway -- "Routes to" --> UserService
    Gateway -- "Routes to" --> NotificationService
    Gateway -- "Routes to" --> TemplateService
    Gateway -- "Routes to" --> AnalyticsService
    %% Connections - Services to Data Stores
    UserService -- "Read/Write" --> PostgreSQL
    UserService -- "Cache/Session" --> Redis
    UserService -- "Store Avatars" --> S3
    NotificationService -- "Read/Write" --> PostgreSQL
    NotificationService -- "Cache Status" --> Redis
    NotificationService -- "Publish Events" --> MessageQueue
    TemplateService -- "Read/Write" --> PostgreSQL
    TemplateService -- "Cache Templates" --> Redis
    EmailService -- "Read/Write" --> PostgreSQL
    EmailService -- "Process Queue" --> MessageQueue
    EmailService -- "Send Email" --> SendGrid
    PushService -- "Read/Write" --> PostgreSQL
    PushService -- "Process Queue" --> MessageQueue
    PushService -- "Send Push" --> FCM
    PushService -- "Send Push" --> APNS
    PushService -- "Send Push" --> WebPush
    AnalyticsService -- "Read/Write" --> PostgreSQL
    AnalyticsService -- "Cache Metrics" --> Redis
    AnalyticsService -- "Track Events" --> AnalyticsAPI
    %% Asynchronous Communication Flow
    NotificationService -.->|'Notification Created'| MessageQueue
    EmailService -.->|'Email Delivery Status'| MessageQueue
    PushService -.->|'Push Delivery Status'| MessageQueue
    MessageQueue -.->|'Update Analytics'| AnalyticsService
```




