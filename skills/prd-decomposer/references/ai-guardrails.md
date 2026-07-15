# AI Guardrails

## Why This Exists

Coding agents (Claude Code, Copilot, etc.) execute exactly what the spec says. Human developers fill in implicit requirements from experience — error handling, auth checks, logging, input validation, graceful degradation. Agents don't. If the spec doesn't mention it, the agent won't build it.

AI Guardrails are domain-specific checklists of these implicit requirements. When decomposing a PRD into epics, scan each epic against the relevant checklist and include any applicable guardrails in the epic's specify or plan prompt. This prevents the classic "it works but it's not production-ready" problem where the agent builds the happy path perfectly and ignores everything else.

## How to Apply

1. After decomposing an epic, check which domains it touches (web UI, API, database, integration, AI/LLM, etc.)
2. Scan the relevant checklists below
3. For any item that applies to this epic and is NOT already covered by the PRD, add it to the epic's **AI Guardrails** section in the output
4. If the PRD already specifies a requirement, don't duplicate it in guardrails — the guardrails are for what the PRD *doesn't* say

## Domain Checklists

### Web UI / Frontend
- Loading states for async operations (spinners, skeletons, progress indicators)
- Empty states (no data yet, no results found, first-time user experience)
- Error states (API failures, network timeouts, validation errors) with user-facing messages
- Responsive behavior (mobile, tablet, desktop) unless explicitly desktop-only
- Keyboard accessibility for interactive elements
- Optimistic UI updates with rollback on failure (if applicable)
- Input validation on the client side before API calls
- Debouncing for search/filter inputs that trigger API calls
- Pagination or virtualization for large lists

### API / Backend
- Input validation and sanitization on all endpoints
- Consistent error response format with meaningful error codes
- Authentication/authorization checks on every endpoint (not just the ones the PRD mentions)
- Rate limiting for public or high-frequency endpoints
- Request logging for debugging and audit trails
- Graceful handling of downstream service failures (timeouts, retries, circuit breakers)
- Idempotency for mutating operations where retries are possible

### Database
- Migrations that are reversible (up and down)
- Indexes on columns used in WHERE clauses, JOINs, and ORDER BY
- Soft delete vs. hard delete — pick one and be consistent
- Created/updated timestamps on all entities
- Foreign key constraints for referential integrity
- Handling of concurrent writes (optimistic locking, conflict resolution)

### External Integrations
- API rate limit handling (backoff, queuing, caching)
- Credential storage (env vars or secrets manager, never hardcoded)
- Webhook signature verification for incoming webhooks
- Retry logic with exponential backoff for transient failures
- Circuit breaker pattern for unreliable external services
- Fallback behavior when the external service is down
- Data freshness indicators (when was this data last synced?)

### AI / LLM Features
- Token/cost budgeting per request or per user
- Prompt injection mitigation for user-supplied inputs
- Response validation (is the LLM output in the expected format?)
- Fallback behavior when the LLM returns garbage or times out
- Streaming vs. batch response handling
- Caching of identical or near-identical prompts to reduce cost
- User-visible confidence indicators or "AI-generated" labels where appropriate
- Guardrails on LLM output before displaying to users (content filtering, format validation)

### Security (cross-cutting)
- CORS configuration for API endpoints
- CSRF protection for state-changing operations
- Sensitive data handling (PII, credentials) — encryption at rest and in transit
- Session management (timeout, invalidation, concurrent session handling)
- Audit logging for security-relevant actions (login, permission changes, data access)

### DevOps / Infrastructure
- Health check endpoints for load balancers
- Structured logging (JSON format, correlation IDs)
- Environment-specific configuration (dev/staging/prod) via env vars
- Graceful shutdown handling (drain connections, finish in-flight requests)
- Database connection pooling

## Usage Notes

Not every guardrail applies to every epic. A purely frontend epic doesn't need database migration guidance. A simple CRUD API doesn't need LLM guardrails. Apply judgment — the goal is to catch the implicit requirements that matter for *this specific epic*, not to cargo-cult the entire list onto everything.

When adding guardrails to an epic, keep them concise. Don't rewrite the checklist — just note the specific items the agent needs to handle:

**Example:**
> **AI Guardrails:** Handle Jira API rate limits with exponential backoff. Add retry logic for transient 5xx errors. Cache sync results to avoid redundant API calls within the sync interval. Log all sync operations with timestamps for debugging stale data issues.

That tells the agent exactly what implicit requirements to build, without bloating the prompt.
