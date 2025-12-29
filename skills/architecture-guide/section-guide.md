# Section Guide - Architecture Discovery Questions

Detailed questions for gathering information for each section of ARCHITECTURE.md.

For document generation principles, see [SKILL.md](SKILL.md).

## 1. System Overview

**Questions**:

- What is this system in one sentence?
- What's the primary architectural pattern? (monolith, microservices, serverless, event-driven)
- What's the deployment model? (cloud, on-prem, hybrid, edge)

**Example**:
> TaskFlow is a lightweight ticket management system using a modular monolith architecture deployed as a containerized web application.

## 2. Functional Requirements

**Questions**:

- What are the 3-5 core capabilities that drive architecture?
- What user actions must the system support?
- Which features have the highest traffic/load?

**Example**:

1. Users can create, update, and close tickets
2. System assigns tickets to team members with notifications
3. Users can search and filter across all tickets

## 3. Non-Functional Requirements

### Scale & Performance

- How many concurrent users? (now vs 2-year projection)
- What's acceptable latency for key operations?
- Peak vs average load ratio?

### Reliability & Availability

- What uptime is required? (99%, 99.9%, 99.99%)
- Recovery time objective (RTO)?
- Recovery point objective (RPO)?

### Data Consistency

- Strong vs eventual consistency acceptable?
- Which operations require strong consistency?
- CAP theorem trade-off: CP or AP?

### Security & Compliance

- Compliance frameworks? (SOC2, HIPAA, GDPR, PCI-DSS)
- Authentication requirements? (SSO, MFA, API keys)
- Authorization model? (RBAC, ABAC)
- Encryption requirements?

### Environment Constraints

- Cloud, on-prem, or hybrid?
- Existing infrastructure to integrate with?
- Team size and expertise?
- Budget constraints?

## 4. Capacity Estimation

**Questions**:

- How many users? (DAU, MAU)
- Actions per user per day?
- Data size per entity and growth rate?
- Peak vs average load factor?

**Example** (show math with assumptions):

```text
Users: 1,000 DAU
Actions: 50/user/day → ~0.6 req/sec average, ~2 req/sec peak
Storage: 10,000 entities × 5KB = ~50MB, growing 2x/year
Conclusion: Single instance sufficient for MVP
```

## 5. Core Entities

**Questions**:

- What are the 3-7 main "things" in your domain?
- How do they relate to each other?
- Which are read-heavy vs write-heavy?

**Example**:

```text
User: Authenticated actor, belongs to Team
Team: Groups Users, owns Tickets
Ticket: Core work item, assigned to User

Relationships: Team 1:N Users, Team 1:N Tickets
Access: Tickets (read-heavy), Comments (append-only)
```

## 6. Core Interfaces

**Questions**:

- What does the outside world call? (public API)
- Any internal service boundaries?
- Third-party integrations needed?
- Sync or async communication?

**Example**:

```text
External: REST API (CRUD), Webhooks (events)
Internal: Notification service (async), Search service (async)
Third-Party: Identity provider, Object storage
```

## 7. High-Level Design

**Questions**:

- What are the major components (5-10 max)?
- How do they communicate?
- Where does data flow?

Create a simple ASCII or Mermaid diagram showing components and data flow.

## 8. Component Deep Dives

For 1-3 complex or critical components:

- What's the architectural challenge?
- What pattern solves it? (CQRS, event sourcing, saga, etc.)
- What's the trade-off?

**Example**:

```text
Notification System
Challenge: Reliable multi-channel delivery
Approach: Async processing, at-least-once delivery, retry pattern
Trade-off: Eventual delivery vs synchronous complexity
```

## 9. Trade-offs & Decisions

**Questions**:

- What alternatives were considered?
- What trade-offs were made?
- What would change at 10x scale?

**Example**:

| Decision | Choice | Alternative | Why |
|----------|--------|-------------|-----|
| Architecture | Modular monolith | Microservices | Small team, fast iteration > independent scaling |
| Database | Relational | Document store | Strong consistency, familiar to team |

## 10. Operational Requirements

### Observability & Monitoring

**Questions**:

- What metrics matter most? (latency, throughput, error rates, business KPIs)
- What needs dashboards vs alerts?
- What debug visibility is needed? (logs, traces, profiles)
- Log retention requirements? (compliance, debugging)
- Distributed tracing needed?

**Example**:

```text
Monitoring:
- API latency (p50, p95, p99) - alert if p95 > 500ms
- Error rate by endpoint - alert if > 1%
- Queue depth - alert if > 1000

Logging:
- Structured logs, 30-day retention
- Request/response tracing for debugging
- Correlation IDs across services
```

### Error Handling & Resilience

**Questions**:

- How should the system handle partial failures?
- Which operations degrade gracefully vs fail fast?
- Retry strategy? (exponential backoff, max attempts)
- Timeout values for external calls?
- Circuit breaker patterns needed?
- Fallback behaviors?

**Example**:

```text
Resilience Patterns:
- External API calls: 3 retries with exponential backoff, 5s timeout
- Circuit breaker on payment gateway (open after 5 failures)
- Graceful degradation: Serve cached data if search is down
- Fail fast: Reject requests if database unavailable
```

### Deployment & Release Strategy

**Questions**:

- Deployment frequency? (continuous, daily, weekly)
- Zero-downtime required?
- Deployment pattern? (blue/green, canary, rolling)
- Feature flags needed?
- Rollback strategy?
- Database migration approach?

**Example**:

```text
Deployment:
- Rolling deployments with health checks
- Canary releases for major changes (5% → 50% → 100%)
- Feature flags for new features
- Automated rollback on error rate spike
- Database migrations run before deployment with backward compatibility
```

### Disaster Recovery & Backup

**Questions**:

- Backup frequency and retention?
- Recovery time objective (RTO)?
- Recovery point objective (RPO)?
- Multi-region failover needed?
- Disaster recovery testing frequency?

**Example**:

```text
DR Strategy:
- Database: Automated daily backups, 30-day retention
- RTO: 4 hours (restore from backup)
- RPO: 24 hours (daily backups acceptable)
- Single region sufficient for MVP
- Quarterly DR drills
```

### Cost Analysis

**Questions**:

- What are the main cost drivers?
- Cost per user/request estimates?
- Budget constraints affecting design?
- Cost optimization priorities?
- Cost monitoring approach?

**Example**:

```text
Cost Drivers:
- Primary: Compute (API servers) ~60%
- Secondary: Database storage ~25%, Third-party APIs ~15%
- Estimated: $0.50/user/month at 10K users = $5K/month
- Optimization: Auto-scaling, reserved instances after growth validated
```

### API Design

**Questions**:

- API style? (REST, GraphQL, gRPC, event-driven)
- Versioning strategy? (URL, header, content negotiation)
- Sync vs async operations?
- Pagination approach?
- Rate limiting needed?
- API documentation approach?

**Example**:

```text
API Design:
- REST API with JSON
- Versioning via URL path (/v1/, /v2/)
- Async for long-running operations (webhooks for completion)
- Cursor-based pagination for large lists
- Rate limiting: 1000 req/hour per API key
- OpenAPI/Swagger documentation
```

## 11. Future Considerations

**Questions**:

- What's out of scope for v1?
- What would you add at 10x scale?
- What triggers revisiting each decision?

**Example**:

- Multi-region: Not needed until global user base
- Read replicas: Add when read load exceeds single instance
- Service extraction: Extract notification service first if scaling independently
