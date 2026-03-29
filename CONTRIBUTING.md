# Contributing to LoanFlow 360

This document covers branching conventions, pull request process, and local development setup for the LoanFlow 360 platform.

---

## Branching Strategy

### Branch Naming

All branches must follow this pattern:

```
<TICKET-ID>-<short-kebab-case-description>
```

| Segment | Rules |
|---|---|
| `TICKET-ID` | Jira ticket ID in all caps, e.g. `SCRUM-301` |
| `short-kebab-case-description` | Lowercase, words separated by hyphens, no spaces or special characters, 3–6 words max |

### Branch Types

| Type | Pattern | Example |
|---|---|---|
| Epic | `SCRUM-###-<epic-description>` | `SCRUM-276-Foundation-and-Project-Setup` |
| Story / task | `SCRUM-###-<description>` | `SCRUM-280-repository-folder-structure` |
| Bug fix | `SCRUM-###-fix-<description>` | `SCRUM-318-fix-token-expiry-handling` |
| Hotfix | `hotfix/<description>` | `hotfix/null-pointer-loan-status` |

### Branching Rules

- **Epic branches** are cut from `main`.
- **Story and task branches** are cut from their parent epic branch.
- Merge story/task branches back into the epic branch via PR.
- The epic branch is merged into `main` when the epic is complete and approved.
- Never commit directly to `main` or an epic branch.
- One ticket per branch. Do not bundle unrelated tickets.
- Keep branches short-lived — merge or close within the scope of the ticket.

### Example Flow

```
main
└── SCRUM-276-Foundation-and-Project-Setup        ← epic branch, cut from main
    ├── SCRUM-280-repository-folder-structure      ← story branch, cut from epic
    └── SCRUM-281-contribution-standards           ← story branch, cut from epic
```

---

## Pull Request Process

### PR Title

```
SCRUM-### Short description of change
```

Example: `SCRUM-301 Add loan application submission endpoint`

The `.github/workflows/pr-checks.yml` action validates this format automatically on every PR open, edit, or push.

### PR Description

When you open a PR, GitHub will auto-populate the body from `.github/PULL_REQUEST_TEMPLATE.md`. Fill in the **What**, **Why**, and **How to Test** sections, then work through both checklists before requesting review.

### Merge Rules

- Minimum **one approval** required before merging.
- All review comments must be resolved or replied to before merge.
- Target the correct base branch: epic branch for story/task PRs, `main` for epic PRs.
- Use **Squash and Merge**. The squash commit message must follow the PR title format.
- Delete the branch after merge.

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
