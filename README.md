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

### Extended requirement assumption/clarification
1. ~~The players are not distributed across the globe - properly they are based on the same country~~ => Players are distributed across the globe as X company is targeting international market
2. The system is able to support thousands of concurrent players & number of Active players join the game around 65,000
4. Read:Write ratio: 1:5
5. The players can view the top 5/10 players on the leaderboard in real-time
6. A leaderboard should display the current standings of all participants => The player can view another specific player’s rank and score
7. As players submit answers, their scores should be updated in real-time => receive score updates through push notifications
#### Non-functional requirement
1. Scalability
2. Performance
3. Reliability
4. Maintainability
5. Monitoring and Observability

### Capacity estimation

### High Level Design
### Database Design
### API Design
### Components Design
### Data Flow
### Technologies and Tools

