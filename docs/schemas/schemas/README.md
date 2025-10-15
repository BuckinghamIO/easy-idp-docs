# Easy IDP Schemas

This directory contains the official JSON schemas for Easy IDP resources.

## 📋 Available Schemas

### v1 (Current)

| Resource | Schema URL | Description |
|----------|-----------|-------------|
| Service | [`v1/service.schema.json`](v1/service.schema.json) | Service catalog entries |
| Team | [`v1/team.schema.json`](v1/team.schema.json) | Team definitions |
| Workflow | [`v1/workflow.schema.json`](v1/workflow.schema.json) | Automated workflows |
| Scorecard | [`v1/scorecard.schema.json`](v1/scorecard.schema.json) | Quality scorecards |

## 🚀 Usage

### Reference in YAML Files

Add this comment at the top of your YAML files for IDE validation:

```yaml
# yaml-language-server: $schema=https://schemas.easy-idp.com/v1/service.schema.json
---
apiVersion: idp.buckingham.io/v1
kind: Service
metadata:
  name: my-service
```

### VSCode Configuration

Add to your `.vscode/settings.json`:

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

This enables:
- ✅ Auto-completion
- ✅ Inline documentation
- ✅ Real-time validation
- ✅ Error highlighting

## 📖 Examples

See the [`../examples/`](../examples/) directory for complete working examples:

- [`service-basic.yaml`](../examples/service-basic.yaml) - Simple service definition
- [`service-full.yaml`](../examples/service-full.yaml) - Full-featured service with all options
- [`team-basic.yaml`](../examples/team-basic.yaml) - Team definition
- [`multi-document.yaml`](../examples/multi-document.yaml) - Multiple resources in one file

## 📚 Documentation

Full documentation available at: https://buckinghamio.github.io/easy-idp-docs/

## 🔄 Versioning

### Semantic Versioning

Schemas follow semantic versioning:

- **Major version (v1 → v2)**: Breaking changes
  - Field removals
  - Type changes
  - Required field additions
  
- **Minor version (v1.0 → v1.1)**: Backward-compatible additions
  - New optional fields
  - New enum values
  - Relaxed constraints
  
- **Patch version (v1.0.0 → v1.0.1)**: Documentation fixes
  - No schema changes

### Stability Promise

- Schema URLs are permanent and will never return 404
- Major versions supported for 1 year after next version release
- 6 months deprecation notice before breaking changes
- Migration guides provided for all major version upgrades

## 🤝 Contributing

Found a bug or have a suggestion? Please open an issue or PR at:
https://github.com/BuckinghamIO/easy-idp-docs

### Schema Change Guidelines

When proposing schema changes:

1. **Backward compatible** (minor version):
   - ✅ Add optional fields
   - ✅ Add enum values
   - ✅ Relax constraints

2. **Breaking changes** (major version):
   - ⚠️ Remove fields
   - ⚠️ Change field types
   - ⚠️ Make optional fields required
   - ⚠️ Stricter validation

## 📜 License

MIT License - See [LICENSE](../LICENSE) for details
