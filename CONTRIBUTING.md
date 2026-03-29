# Contributing to LoanFlow 360

This document covers branching conventions, pull request process, and local development setup for the LoanFlow 360 platform.

---

## Branching Strategy

### Branch Naming

All branches must be prefixed with the Jira ticket number followed by a short kebab-case description:

```
<TICKET-ID>-<short-description>
```

Examples:
- `SCRUM-276-foundation-and-project-setup`
- `SCRUM-301-loan-application-rest-api`
- `SCRUM-315-document-upload-s3-integration`

### Branch Types

| Type | Pattern | Purpose |
|---|---|---|
| Feature | `SCRUM-###-<description>` | New feature work tied to a Jira story or task |
| Bug fix | `SCRUM-###-fix-<description>` | Bug fixes tied to a Jira ticket |
| Hotfix | `hotfix/<description>` | Critical production fixes (rare, requires lead approval) |

### Rules

- Branch from `main` unless explicitly told otherwise.
- Keep branches short-lived. Merge or close them within the scope of the ticket.
- Do not commit directly to `main`.

---

## Pull Request Process

### Before Opening a PR

- Ensure your branch is up to date with `main`.
- Verify the service or module builds locally without errors.
- Run any existing tests and confirm they pass.
- Remove debug output, commented-out code, and TODO comments not tracked in Jira.

### PR Title Format

Use the Jira ticket ID as the prefix:

```
SCRUM-### Short description of change
```

Example: `SCRUM-301 Add loan application submission endpoint`

### PR Description

Include:
1. **What** — a brief summary of what changed.
2. **Why** — the Jira ticket link or context for the change.
3. **How to test** — steps a reviewer can use to verify the change locally.

### Review Requirements

- At least one approval is required before merging.
- Address all review comments or mark them as resolved with a reply before merging.
- Squash commits on merge to keep `main` history clean.

### Merge

- Use **Squash and Merge** from GitHub.
- The squash commit message should follow the PR title format above.

---

## Local Development Setup

### Prerequisites

Ensure the following are installed before starting:

| Tool | Version | Notes |
|---|---|---|
| Java | 17+ | Use SDKMAN or your system package manager |
| Maven | 3.8+ | Or use the `./mvnw` wrapper in each service |
| Node.js | 18+ | Use nvm for version management |
| npm | 9+ | Comes with Node.js |
| Docker | 24+ | Required to run infrastructure dependencies |
| Docker Compose | v2+ | Bundled with Docker Desktop |
| Git | 2.30+ | Standard version control |

### Clone the Repository

```bash
git clone <repository-url>
cd loanflow-360
```

### Infrastructure Dependencies

All infrastructure services (Kafka, Zookeeper, MySQL, Redis, MongoDB) are managed via Docker Compose. Start them before running any application service:

```bash
docker compose -f infra/docker-compose.yml up -d
```

To stop:

```bash
docker compose -f infra/docker-compose.yml down
```

### Backend Services

Each backend service lives under `backend/<service-name>/`. To build and run a service:

```bash
cd backend/<service-name>
./mvnw spring-boot:run
```

Or build the JAR first:

```bash
./mvnw clean package -DskipTests
java -jar target/<service-name>-*.jar
```

Environment configuration is managed through `.env` files. Copy the example and fill in local values:

```bash
cp .env.example .env
```

> `.env` files are gitignored. Never commit secrets or local credentials.

### Frontend Application

The React frontend lives under `frontend/`:

```bash
cd frontend
npm install
npm run dev
```

The development server runs on `http://localhost:5173` by default. The frontend proxies API requests to the API Gateway.

### Service Port Reference

| Service | Default Port |
|---|---|
| API Gateway | 8080 |
| Identity Service | 8081 |
| Loan Application Service | 8082 |
| Document Service | 8083 |
| Underwriting Service | 8084 |
| Notification / Audit Service | 8085 |
| Frontend Dev Server | 5173 |
| MySQL | 3306 |
| Redis | 6379 |
| Kafka | 9092 |
| MongoDB | 27017 |

### Running Tests

From any backend service directory:

```bash
./mvnw test
```

From the frontend:

```bash
npm test
```

---

## Questions

If you have questions about setup or workflow, reach out in the team channel or open a Jira comment on the relevant ticket.
