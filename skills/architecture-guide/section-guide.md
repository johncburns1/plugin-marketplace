# Section Guide - Architecture Discovery Questions

Comprehensive guide for gathering information for each section of the architecture document.

## 1. System Overview

**Purpose**: One-paragraph summary of what the system does and its primary architectural style

**Questions**:

- What is this system in one sentence?
- What's the primary architectural pattern? (monolith, microservices, serverless, event-driven)
- What's the deployment model? (cloud, on-prem, hybrid, edge)

**Example**:
> TaskFlow is a lightweight ticket management system using a modular monolith architecture deployed as a containerized web application. It prioritizes simplicity and fast iteration over distributed complexity.

**Tips**:

- Keep to 2-3 sentences max
- Name the architectural style explicitly
- State deployment model upfront

## 2. Functional Requirements

**Purpose**: The 3-5 core capabilities that drive architectural decisions

**Questions**:

- What are the absolute must-have capabilities?
- What user actions must the system support?
- What data transformations or processing must occur?
- Which features have the highest traffic/load?

**Format**: Numbered list, one line each

**Example**:

1. Users can create, update, and close tickets
2. System assigns tickets to team members with notifications
3. Users can search and filter across all tickets
4. System tracks ticket history and generates activity feeds

**Tips**:

- Only include requirements that affect architecture
- Skip CRUD unless it has special requirements
- Focus on what drives technical complexity

## 3. Non-Functional Requirements

Break into categories. For each, document the requirement and its architectural implication.

### Scale & Performance

**Questions**:

- How many concurrent users? Now vs 2-year projection?
- What's acceptable latency for key operations?
- Any batch processing or bulk operations?
- Peak vs average load ratio?

**Example**:

| Requirement | Target | Implication |
|-------------|--------|-------------|
| Concurrent users | 100 now, 1000 in 2 years | Stateless API, horizontal scaling |
| API latency | < 200ms p95 | Caching layer, query optimization |
| Search latency | < 500ms | Search index, async indexing |

### Reliability & Availability

**Questions**:

- What uptime is required? (99%, 99.9%, 99.99%)
- What's the business cost of downtime?
- Recovery time objective (RTO)?
- Recovery point objective (RPO)?

**Example**:

| Requirement | Target | Implication |
|-------------|--------|-------------|
| Availability | 99.9% (8.7 hrs/year downtime) | Health checks, auto-restart |
| RTO | < 1 hour | Automated recovery, backups |
| RPO | < 5 minutes | Transaction logs, point-in-time recovery |

### Data Consistency (CAP Theorem)

**Questions**:

- Must reads always return the latest write?
- Can you tolerate stale data? For how long?
- What happens during a network partition?
- Any operations that require strong consistency?

**Example**:

> **Choice**: AP (Availability + Partition Tolerance)
>
> Eventual consistency is acceptable for ticket list views. Strong consistency required only for ticket creation/updates within a single user session.

**Tips**:

- Most systems don't need strong consistency everywhere
- Identify which operations truly need it
- Document the consistency boundary

### Security & Compliance

**Questions**:

- What compliance frameworks apply? (SOC2, HIPAA, GDPR, PCI-DSS)
- Data classification? (public, internal, confidential, restricted)
- Authentication requirements? (SSO, MFA, API keys)
- Authorization model? (RBAC, ABAC, simple ownership)
- Encryption requirements? (at-rest, in-transit, end-to-end)
- Audit logging requirements?

**Example**:

| Requirement | Approach |
|-------------|----------|
| Auth | SSO via OIDC, API keys for integrations |
| Authorization | Simple team-based RBAC |
| Encryption | TLS in transit, encrypted at rest |
| Audit | Log all mutations with actor, timestamp |

### Environment Constraints

**Questions**:

- Cloud provider preference or mandate?
- On-prem requirements?
- Network restrictions? (VPN, firewall, air-gapped)
- Existing infrastructure to integrate with?
- Team expertise and operational capacity?
- Budget constraints?

**Example**:

- Cloud-agnostic design, initial deploy to AWS
- Must integrate with existing corporate SSO
- Small team (3 engineers) - minimize operational complexity
- Limited budget - prefer managed services over self-hosted

## 4. Capacity Estimation

**Purpose**: Back-of-envelope math to validate architectural choices

**Questions**:

- How many users? (DAU, MAU)
- Actions per user per day?
- Data size per entity?
- Data retention requirements?
- Growth rate?

**Format**: Show your math with assumptions

**Example**:

```
Users: 1,000 DAU (10,000 MAU)
Actions: 50 actions/user/day
Peak factor: 3x average

Requests:
- Average: 1,000 × 50 / 86,400 = ~0.6 req/sec
- Peak: ~2 req/sec
- Conclusion: Single instance sufficient for MVP

Storage:
- Tickets: 100 per team × 100 teams = 10,000 tickets
- Size: ~5KB per ticket (with history)
- Total: ~50MB
- Growth: 2x per year
- Conclusion: Any database handles this easily

Bandwidth:
- Average response: 10KB
- Peak: 2 req/sec × 10KB = 20 KB/sec
- Conclusion: Negligible
```

**Tips**:

- Round liberally—order of magnitude matters
- State assumptions explicitly
- Identify what changes at 10x scale

## 5. Core Entities

**Purpose**: The 3-7 domain objects that the system manages

**Questions**:

- What are the main "things" in your domain?
- How do they relate to each other?
- Which are read-heavy vs write-heavy?
- Which have complex lifecycles or state machines?

**Format**: Simple list with relationships

**Example**:

```
Entities:
- User: authenticated actor, belongs to Team
- Team: group of users, owns Tickets
- Ticket: core work item, assigned to User, has Comments
- Comment: discussion on Ticket, created by User
- Activity: audit log entry, references Ticket + User

Key Relationships:
- Team 1:N Users
- Team 1:N Tickets
- Ticket 1:N Comments
- Ticket N:1 User (assignee)

Access Patterns:
- Tickets: Heavy read (list views), moderate write
- Comments: Append-only, read with ticket
- Activity: Write-heavy, read for audit only
```

**Tips**:

- This is NOT a database schema
- Focus on conceptual entities
- Note access patterns (read vs write)

## 6. Core Interfaces

**Purpose**: Key boundaries and contracts between components

**Questions**:

- What does the outside world call? (public API)
- Any internal service boundaries?
- Third-party integrations?
- Event contracts (if async)?
- Data import/export interfaces?

**Format**: List interfaces with brief descriptions

**Example**:

```
External Interfaces:
- REST API: CRUD for tickets, users, teams
- Webhook receiver: GitHub integration for commit linking
- Webhook sender: Notify external systems on ticket changes

Internal Interfaces:
- Notification service: accepts events, handles delivery
- Search service: accepts index updates, handles queries

Third-Party:
- OIDC Provider: authentication
- Email service: notification delivery
- Object storage: file attachments
```

**Tips**:

- Focus on contracts, not implementations
- Note sync vs async
- Identify external dependencies

## 7. High-Level Design

**Purpose**: Visual representation of major components and data flow

**Questions**:

- What are the major components/services?
- How do they communicate?
- Where does data flow?
- Where does state live?

**Format**: ASCII or Mermaid diagram with 5-10 components max

**Example**:

```
┌──────────────────────────────────────────────────────────┐
│                        Clients                           │
│              (Web App, Mobile, API Consumers)            │
└─────────────────────────┬────────────────────────────────┘
                          │ HTTPS
                          ▼
┌──────────────────────────────────────────────────────────┐
│                      API Gateway                          │
│                (Auth, Rate Limiting, Routing)            │
└─────────────────────────┬────────────────────────────────┘
                          │
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
    ┌──────────┐   ┌──────────┐   ┌──────────┐
    │  Ticket  │   │   User   │   │  Search  │
    │ Service  │   │ Service  │   │ Service  │
    └────┬─────┘   └────┬─────┘   └────┬─────┘
         │              │              │
         └──────────────┼──────────────┘
                        ▼
              ┌──────────────────┐
              │    Data Store    │
              │  (Primary + Index)│
              └──────────────────┘
```

**Tips**:

- Keep to one diagram if possible
- Show data flow direction
- Label communication protocols
- Don't show implementation details

## 8. Component Deep Dives

**Purpose**: Additional detail on 1-3 complex or critical components

**Questions**:

- Which components have tricky requirements?
- Where are the scaling bottlenecks?
- What patterns apply? (CQRS, event sourcing, saga, etc.)
- What are the failure modes?

**Format**: Brief section for each component

**Example**:

```markdown
### Notification System

**Challenge**: Reliable delivery across multiple channels (email, webhook, in-app)

**Approach**:
- Async processing via message queue
- At-least-once delivery with idempotency keys
- Dead letter queue for failed deliveries
- Retry with exponential backoff

**Trade-off**: Eventual delivery (seconds delay) vs complexity of sync delivery
```

**Tips**:

- Only deep-dive on what's architecturally interesting
- Keep each deep-dive to 5-10 lines
- Focus on patterns and trade-offs

## 9. Trade-offs & Decisions

**Purpose**: Document the "why" behind architectural choices

**Questions**:

- What alternatives were considered?
- What trade-offs were made?
- What constraints drove the decision?
- What would you reconsider at 10x scale?

**Format**: Table or decision records

**Example**:

| Decision | Choice | Alternative | Why |
|----------|--------|-------------|-----|
| Architecture | Modular monolith | Microservices | Small team, fast iteration > independent scaling |
| Database | Relational | Document store | Strong consistency for core data, familiar to team |
| Search | Separate index | Database queries | Fast full-text search at scale |
| Async | Message queue | Direct calls | Decouple notification delivery from request path |

**Tips**:

- Every decision has trade-offs—make them explicit
- Note what would change the decision
- This is the most valuable section for future readers

## 10. Future Considerations

**Purpose**: What's explicitly deferred and why

**Questions**:

- What's out of scope for v1?
- What would you add at 10x scale?
- What technical debt are you accepting?
- What would require a re-architecture?

**Format**: Brief list

**Example**:

- Multi-region deployment: Not needed until global user base
- Event sourcing: Consider if audit requirements increase
- Service extraction: Extract notification service first if scaling independently
- Read replicas: Add when read load exceeds single instance

**Tips**:

- Be honest about accepted limitations
- Note the trigger for revisiting each item
- Keep brief—this is a one-pager
