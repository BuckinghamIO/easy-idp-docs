# Service Schema

Services are the core resource in Easy IDP. They represent any software component in your organization: APIs, libraries, tools, databases, websites, and infrastructure.

## Schema URL

```
https://schemas.easy-idp.com/v1/service.schema.json
```

## Basic Example

```yaml
# yaml-language-server: $schema=https://schemas.easy-idp.com/v1/service.schema.json
---
apiVersion: idp.buckingham.io/v1
kind: Service

metadata:
  name: payment-api
  labels:
    env: production

spec:
  type: service
  description: Payment processing API
  teams:
    - payment-team
  lifecycle: production
  repository:
    url: https://github.com/company/payment-api
```

## Fields Reference

### `metadata`

#### `metadata.name` **(required)**

- **Type**: `string`
- **Pattern**: `^[a-z0-9-]+$` (lowercase, hyphens only)
- **Length**: 1-63 characters
- **Description**: Unique service identifier

**Examples:**
```yaml
name: payment-api        ✅
name: user-service       ✅
name: PaymentAPI         ❌ (no uppercase)
name: payment_api        ❌ (no underscores)
```

#### `metadata.namespace`

- **Type**: `string`
- **Default**: `default`
- **Description**: Logical namespace for grouping services

**Examples:**
```yaml
namespace: default       # Default namespace
namespace: production    # Production services
namespace: experimental  # Experimental services
```

#### `metadata.labels`

- **Type**: `object`
- **Description**: Key-value pairs for filtering and selection

**Examples:**
```yaml
labels:
  env: production
  criticality: high
  language: python
  team: payment
```

#### `metadata.annotations`

- **Type**: `object`
- **Description**: Non-identifying metadata (not used for queries)

**Examples:**
```yaml
annotations:
  docs.url: https://docs.company.com/payment-api
  oncall.rotation: weekly
  cost.center: engineering
```

### `spec`

#### `spec.type` **(required)**

- **Type**: `string`
- **Allowed values**: 
  - `service` - Backend/API service
  - `library` - Shared library or package
  - `tool` - CLI tool or utility
  - `website` - Frontend website/webapp
  - `infrastructure` - Infrastructure component
  - `database` - Database
  - `cache` - Cache (Redis, Memcached)

**Examples:**
```yaml
type: service        # REST API, gRPC service
type: library        # npm package, Python library
type: website        # React app, marketing site
type: database       # PostgreSQL, MongoDB
```

#### `spec.description`

- **Type**: `string`
- **Max length**: 500 characters
- **Description**: Brief service description

**Examples:**
```yaml
description: Payment processing API with Stripe integration
description: User authentication and authorization service
description: Redis cache for session storage
```

#### `spec.owner`

- **Type**: `string`
- **Description**: GitHub organization or username

**Examples:**
```yaml
owner: CompanyName
owner: PlatformTeam
owner: john-doe
```

#### `spec.teams` (Multi-team ownership)

- **Type**: `array` of `string`
- **Min items**: 1
- **Description**: Teams that own/support this service

**Examples:**
```yaml
# Single team
teams:
  - platform-team

# Multi-team (shared ownership)
teams:
  - platform-team    # Primary
  - data-team        # Analytics access
  - security-team    # Security reviews
```

#### `spec.primaryTeam`

- **Type**: `string`
- **Description**: Primary team for on-call (defaults to first in `teams`)

**Examples:**
```yaml
teams:
  - platform-team
  - data-team
primaryTeam: platform-team  # On-call responsibility
```

#### `spec.lifecycle`

- **Type**: `string`
- **Default**: `development`
- **Allowed values**:
  - `development` - In development
  - `staging` - Staging environment
  - `production` - Production
  - `deprecated` - Being phased out

**Examples:**
```yaml
lifecycle: production      # Live service
lifecycle: development     # WIP
lifecycle: deprecated      # Legacy system
```

#### `spec.repository`

- **Type**: `object`
- **Description**: Source code repository information

**Fields:**

- **`url`** (string, required): Git repository URL
- **`defaultBranch`** (string, default: `main`): Main branch name
- **`private`** (boolean, default: `false`): Whether repo is private

**Examples:**
```yaml
repository:
  url: https://github.com/company/payment-api
  defaultBranch: main
  private: true
```

#### `spec.dependsOn`

- **Type**: `array` of objects
- **Description**: Dependencies on other resources

**Each dependency:**

- **`kind`** (string, required): Resource type
- **`name`** (string, required): Resource name
- **`namespace`** (string, optional): Resource namespace

**Examples:**
```yaml
dependsOn:
  - kind: Service
    name: postgres-primary
  - kind: Service
    name: redis-cache
  - kind: Service
    name: auth-service
    namespace: security
```

#### `spec.links`

- **Type**: `object`
- **Description**: Links to external resources

**Common links:**
- `dashboard` - Monitoring dashboard (Grafana, Datadog)
- `docs` - Documentation
- `runbook` - Runbook/playbook
- `logs` - Log viewer
- `slack` - Team Slack channel
- `repository` - Source code
- `metrics` - Metrics dashboard

**Examples:**
```yaml
links:
  dashboard: https://grafana.company.com/d/payment-api
  docs: https://docs.company.com/payment-api
  runbook: https://wiki.company.com/runbooks/payment-api
  logs: https://logs.company.com/payment-api
  slack: "#team-payment"
  metrics: https://datadog.company.com/dashboard/payment-api
```

#### `spec.tags`

- **Type**: `array` of `string`
- **Description**: Searchable tags/keywords

**Examples:**
```yaml
tags:
  - api
  - payment
  - stripe
  - pci-compliant
  - microservice
```

#### `spec.language`

- **Type**: `string`
- **Description**: Primary programming language

**Examples:**
```yaml
language: Python
language: TypeScript
language: Go
```

#### `spec.framework`

- **Type**: `string`
- **Description**: Primary framework

**Examples:**
```yaml
framework: FastAPI
framework: React
framework: Express
```

## Complete Example

```yaml
# yaml-language-server: $schema=https://schemas.easy-idp.com/v1/service.schema.json
---
apiVersion: idp.buckingham.io/v1
kind: Service

metadata:
  name: payment-api
  namespace: default
  labels:
    env: production
    criticality: high
    language: python
    team: payment
  annotations:
    docs.url: https://docs.company.com/payment-api
    oncall.rotation: weekly
    cost.center: engineering

spec:
  # Core info
  type: service
  description: Payment processing API with Stripe integration
  
  # Ownership (multi-team)
  owner: PaymentTeam
  teams:
    - payment-team      # Primary owner
    - platform-team     # Platform support
    - data-team         # Analytics access
  primaryTeam: payment-team  # On-call responsibility
  
  # Lifecycle
  lifecycle: production
  
  # Source code
  repository:
    url: https://github.com/company/payment-api
    defaultBranch: main
    private: true
  
  # Dependencies
  dependsOn:
    - kind: Service
      name: postgres-primary
    - kind: Service
      name: redis-cache
    - kind: Service
      name: auth-service
  
  # Links
  links:
    dashboard: https://grafana.company.com/d/payment-api
    docs: https://docs.company.com/payment-api
    runbook: https://wiki.company.com/runbooks/payment-api
    logs: https://logs.company.com/payment-api
    slack: "#team-payment"
    repository: https://github.com/company/payment-api
    metrics: https://datadog.company.com/dashboard/payment-api
  
  # Metadata
  tags:
    - api
    - payment
    - stripe
    - pci-compliant
  language: Python
  framework: FastAPI
```

## Multi-Team Services

Services can be owned by multiple teams:

```yaml
spec:
  # Shared database example
  teams:
    - user-platform    # Primary owner
    - infra-data       # Infrastructure support
  primaryTeam: user-platform  # Responsible for on-call
```

**Use cases:**
- Shared infrastructure (databases, caches)
- Cross-functional services
- Platform components with multiple stakeholders

The **primary team** handles:
- On-call rotations
- Incident response
- Production changes

## Extensions

The schema allows `additionalProperties: true` in `spec`, enabling custom fields:

```yaml
spec:
  # Standard fields
  type: service
  teams: [payment-team]
  
  # Custom extensions
  slos:
    availability: 99.9%
    latency_p95: 200ms
  
  cost:
    monthly_budget: 5000
    alerts_threshold: 6000
  
  compliance:
    pci_dss: true
    sox: true
```

## See Also

- [Team Schema](team.md) - Define teams
- [Examples](examples.md) - More examples
- [GitOps Setup](/gitops/setup/) - Configure GitOps
