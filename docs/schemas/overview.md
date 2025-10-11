# Schema Reference

Easy IDP uses **YAML resources** with JSON Schema validation to define your platform configuration. This approach provides strong typing, IDE support, and clear versioning.

## 📋 Available Resources

| Resource | Description | Schema |
|----------|-------------|--------|
| **Service** | Services, libraries, tools, and infrastructure components | [View Schema](https://schemas.easy-idp.com/v1/service.schema.json) |
| **Team** | Team definitions with members and contacts | [View Schema](https://schemas.easy-idp.com/v1/team.schema.json) |
| **Workflow** | Automated workflows and actions | [View Schema](https://schemas.easy-idp.com/v1/workflow.schema.json) |
| **Scorecard** | Quality metrics and service maturity evaluation | [View Schema](https://schemas.easy-idp.com/v1/scorecard.schema.json) |

## 🏗️ Resource Structure

All resources follow the same pattern:

```yaml
apiVersion: idp.buckingham.io/v1  # API version
kind: Service                      # Resource type

metadata:
  name: my-resource               # Unique identifier
  namespace: default              # Optional: namespace
  labels:                         # Key-value labels
    env: production
  annotations:                    # Non-identifying metadata
    docs: https://example.com

spec:
  # Resource-specific configuration
  type: service
  description: My service description
  # ... more fields
```

## 🎯 Key Concepts

### API Version

```yaml
apiVersion: idp.buckingham.io/v1
```

- Defines which schema version to use
- Allows backward-compatible evolution
- Current version: **v1**

### Kind

```yaml
kind: Service
```

- Resource type: `Service`, `Team`, `Workflow`, `Scorecard`
- Determines which schema to validate against

### Metadata

Common across all resources:

- **`name`**: Unique identifier (lowercase-with-hyphens)
- **`namespace`**: Logical grouping (optional, default: `default`)
- **`labels`**: Key-value pairs for filtering and selection
- **`annotations`**: Additional metadata (not used for selection)

### Spec

Resource-specific configuration. Each resource type has different fields:

- **Service**: `type`, `teams`, `repository`, `dependencies`, etc.
- **Team**: `displayName`, `members`, `contacts`, etc.
- **Workflow**: `parameters`, `steps`, etc.
- **Scorecard**: `checks`, `scoring`, etc.

## 📁 File Organization

### Option 1: Individual Files (Recommended)

```
platform-config/
├── services/
│   ├── payment-api.yaml
│   ├── user-service.yaml
│   └── auth-api.yaml
├── teams/
│   ├── platform-team.yaml
│   └── payment-team.yaml
└── workflows/
    └── deploy-service.yaml
```

**Benefits:**
- Clear ownership (CODEOWNERS)
- No merge conflicts
- Easy to find specific resources

### Option 2: Multi-Document Files

```yaml
# services/payment-system.yaml
---
apiVersion: idp.buckingham.io/v1
kind: Service
metadata:
  name: payment-api
---
apiVersion: idp.buckingham.io/v1
kind: Service
metadata:
  name: payment-worker
---
apiVersion: idp.buckingham.io/v1
kind: Service
metadata:
  name: payment-db
```

**Benefits:**
- Logically related resources together
- Atomic changes (all-or-nothing)
- Copy-paste friendly

## ✨ IDE Integration

### VSCode

Add to `.vscode/settings.json`:

```json
{
  "yaml.schemas": {
    "https://schemas.easy-idp.com/v1/service.schema.json": "services/*.yaml",
    "https://schemas.easy-idp.com/v1/team.schema.json": "teams/*.yaml",
    "https://schemas.easy-idp.com/v1/workflow.schema.json": "workflows/*.yaml",
    "https://schemas.easy-idp.com/v1/scorecard.schema.json": "scorecards/*.yaml"
  }
}
```

Or use inline comments:

```yaml
# yaml-language-server: $schema=https://schemas.easy-idp.com/v1/service.schema.json
---
apiVersion: idp.buckingham.io/v1
kind: Service
# ... VSCode now provides autocomplete and validation!
```

### Features

- ✅ **Autocomplete**: Suggest valid fields as you type
- ✅ **Validation**: Real-time error checking
- ✅ **Documentation**: Hover over fields for descriptions
- ✅ **Enum values**: Show available options for choice fields

## 🔄 Versioning

Schemas use semantic versioning:

- **v1**: Current stable version
- **v2**: Next major version (breaking changes)
- **v1.1**: Minor updates (backward compatible)

See [Versioning](versioning.md) for details on our stability promises.

## 📚 Next Steps

- [Service Schema](service.md) - Service catalog entries
- [Team Schema](team.md) - Team definitions
- [Workflow Schema](workflow.md) - Automation
- [Scorecard Schema](scorecard.md) - Quality metrics
- [Examples](examples.md) - Working examples

## 🔗 External Resources

- [JSON Schema Documentation](https://json-schema.org/)
- [YAML Language Specification](https://yaml.org/spec/)
- [Kubernetes Resource Model](https://kubernetes.io/docs/concepts/overview/working-with-objects/) (inspiration)
