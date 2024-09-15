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
### Technologies and Tools

