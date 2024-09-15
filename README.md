# Real-Time-Vocabulary-Quiz
## Requirement: 
This feature will allow users to answer questions in real-time, compete with others, and see their scores updated live on a leaderboard.

## Acceptance Criteria

1. **User Participation**:
   - Users should be able to join a quiz session using a unique quiz ID.
   - The system should support multiple users joining the same quiz session simultaneously.

2. **Real-Time Score Updates**:
   - As users submit answers, their scores should be updated in real-time.
   - The scoring system must be accurate and consistent.

3. **Real-Time Leaderboard**:
   - A leaderboard should display the current standings of all participants.
   - The leaderboard should update promptly as scores change.
     
### Part 1: System Design

1. **System Design Document**:
   - **Architecture Diagram**: Create an architecture diagram illustrating how different components of the system interact. This should include all components required for the feature, including the server, client applications, database, and any external services.
   - **Component Description**: Describe each component's role in the system.
   - **Data Flow**: Explain how data flows through the system from when a user joins a quiz to when the leaderboard is updated.
   - **Technologies and Tools**: List and justify the technologies and tools chosen for each component.

## System Design Proposal

### Extended assumption/requirement
1. ~~The players are not distributed across the globe - properly they are based on the same country~~ => Players are distributed across the globe as X company is targeting international market
2. Scale of the system:
   - Each player can generate 10 requests/minute
3. Scale of data: lenghth of questions
   - Assuming each question is ~1KB in size => estimate to support 10GB amount of questions (~10 million questions)
5. Read:Write ratio
   - During active gameplay, reads (fetching questions, leaderboard) are more frequent than writes (submitting answers, updating scores)
      Estimation: 10:1
6. How fast is enought & SLA:
   - 99.99% system uptime
8. How much downtime can we tolerate:
   - zero downtime
   - Unplanned downtime could occur due to:
   -    Major cloud provider outage: ~4 hours/year (99.95% uptime)
   -    Database failover: ~5 minutes/occurrence, maybe 2-3 times/year
   -    Critical bug: rollback strategies
10. The players can view the top 5/10 players on the leaderboard in real-time
11. A leaderboard should display the current standings of all participants => The player can view another specific player’s rank and score
12. As players submit answers, their scores should be updated in real-time => receive score updates through push notifications
#### Non-functional requirement
1. Scalability
2. Performance
3. Reliability
4. Maintainability
5. Monitoring and Observability

### Capacity estimation

### High Level Design
### Components Design
1. Client Browser:
> [!NOTE] The user interface where players interact with the quiz application.
2. Cloudflare CDN:
> [!NOTE] Caches and serves static assets globally, reducing latency for users.
3. AWS Load Balancer:
> [!NOTE] Distributes incoming traffic across multiple API servers for better performance and reliability.
4. API Gateway/Express:
> [!NOTE] Handles HTTP requests, routes them to appropriate services, and manages API endpoints.
5. WebSocket Server:
> [!NOTE] Manages real-time bidirectional communication between the server and clients for instant updates.
6. Authentication Service:
> [!NOTE] Manages user registration, login, and token-based authentication.
7. Quiz Service:
> [!NOTE] Handles quiz-related operations including creating, retrieving, and submitting quizzes.
8. Scoring Service:
> [!NOTE] Calculates and updates user scores based on quiz submissions.
9. Leaderboard Service:
> [!NOTE] Manages and updates the leaderboard based on user scores.
10. Redis Cache Quiz:
> [!NOTE] Caches quiz questions for faster retrieval and reduced database load.
11. Redis Cache Leaderboard:
> [!NOTE] Stores real-time leaderboard data for quick access and updates.
12. Redis Cache Progress:
> [!NOTE] Stores user progress in quizzes to handle disconnections and rejoin scenarios.
13. Quiz PostgreSQL DB:
> [!NOTE] Persistent storage for quiz data, questions, and user information.
14. Leaderboard PostgreSQL DB:
> [!NOTE] Stores historical leaderboard data for long-term analysis and retrieval.
15. Kafka Queue:
> [!NOTE] Manages asynchronous communication between services, ensuring scalability and fault tolerance.
16. Retry Queue:
> [!NOTE] Stores failed operations for later retry, improving system reliability.
17. Retry Worker:
> [!NOTE] Processes failed operations from the Retry Queue, attempting to resolve them.
18. APM (New Relic/Datadog):
> [!NOTE] Monitors application performance, helping identify and diagnose issues.
19. ELK Stack:
> [!NOTE] Collects, processes, and visualizes log data for system monitoring and troubleshooting.
20. Prometheus:
> [!NOTE] Collects and stores time-series data for system metrics.
21. Grafana:
> [!NOTE] Visualizes metrics from Prometheus, providing real-time system dashboards.
22. Flyway:
> [!NOTE] Manages database migrations, ensuring consistent database schema across environments.
### Data Flow
I.  User joins a quiz:
   > [!IMPORTANT]
   > 1. Client sends request to API Gateway via Load Balancer.
   > 2. API Gateway routes request to Quiz Service.
   > 3. Quiz Service retrieves quiz data from Redis Cache Quiz or Quiz PostgreSQL DB.
   > 4. Quiz data is sent back to the client.

II. User answers questions:
> [!IMPORTANT]
> 1. Client sends answers to API Gateway.
> 2. Quiz Service receives answers and publishes to Kafka Queue.
> 3. Scoring Service consumes message from Kafka, calculates score.
> 4. Score is published back to Kafka Queue.


III. Leaderboard update:
> [!IMPORTANT]
> 1. Leaderboard Service consumes score update from Kafka Queue.
> 2. Updates Redis Cache Leaderboard and Leaderboard PostgreSQL DB.
> 3. Publishes leaderboard update to Kafka Queue.
> 4. WebSocket Server consumes leaderboard update and broadcasts to connected clients.

IV.Real-time updates:
> [!IMPORTANT]
> 1. Client receives updates via WebSocket connection.
> 2. Leaderboard component in the client updates in real-time.
### Technologies and Tools. (Notes: building on the cloud is 100% worth consideration which can be explored down the line)
> [!NOTE]
> **Frontend**: React (React Native for mobile)
> Justification: Component-based architecture, efficient rendering, large ecosystem.

> [!NOTE]
> **CDN**: Cloudflare
> Justification: Global presence, easy setup, DDoS protection.

> [!NOTE]
> **Load Balancer**: AWS ELB
> Justification: Scalable, integrates well with AWS services, supports health checks.

> [!NOTE]
>** API Server**: Node.js/Express.js
> Justification: Lightweight, flexible, large ecosystem of middleware.

> [!NOTE]
> **WebSocket**: Socket.io
> Justification: Reliable real-time communication, fallback options for older browsers.

> [!NOTE]
> **Authentication**: JWT
> :Stateless, scalable, secure for token-based authentication.

> [!NOTE]
> **Databases**: PostgreSQL
> : ACID compliant, supports complex queries, extensible.

> [!NOTE]
> **Caching**: Redis
> : In-memory data structure store, supports complex data types, pub/sub mechanism.

> [!NOTE]
> **Message Queue**: Kafka
> : High-throughput, distributed, fault-tolerant.

> [!NOTE]
> **Containerization**: Docker
> : Consistent environments, easy scaling and deployment.

> [!NOTE]
> **Orchestration**: Kubernetes
> : Automated deployment, scaling, and management of containerized applications.

> [!NOTE]
> **CI/CD**: GitHub
> : Automates building, testing, and deployment processes.

> [!NOTE]
> **API Documentation**: Swagger
> : Interactive documentation, supports OpenAPI specification.

> [!NOTE]
> **Monitoring**: New Relic/Datadog, ELK Stack, Prometheus, Grafana
> : Comprehensive monitoring suite covering APM, logging, and metrics visualization.

> [!NOTE]
> **Database Migration**: Flyway
> : Version control for databases, supports SQL and Java migrations.
