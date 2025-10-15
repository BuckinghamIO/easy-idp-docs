# Schema Changelog

All notable changes to Easy IDP schemas will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and schemas adhere to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v1] - 2025-01-15

### Added

#### Service Schema
- Initial release
- Support for `apiVersion`, `kind`, `metadata`, `spec` structure
- Metadata: `name`, `namespace`, `labels`, `annotations`
- Spec fields:
  - `type` (service, library, tool, website, infrastructure, database, cache)
  - `description`
  - `owner`
  - `teams` (array for multi-team ownership)
  - `primaryTeam` (designated on-call owner)
  - `lifecycle` (development, staging, production, deprecated)
  - `repository` (url, defaultBranch, private)
  - `dependsOn` (array of resource dependencies)
  - `links` (dashboard, docs, runbook, etc.)
  - `tags` (searchable keywords)
  - `language`, `framework`

#### Team Schema
- Initial release
- Metadata: `name`, `labels`, `annotations`
- Spec fields:
  - `displayName`
  - `description`
  - `members` (array with username, role, since)
  - `contacts` (slack, email, pagerduty)
  - `links` (wiki, jira, calendar, etc.)

#### Workflow Schema
- Initial release
- Metadata: `name`, `labels`, `annotations`
- Spec fields:
  - `displayName`
  - `description`
  - `parameters` (input parameters with types)
  - `steps` (workflow execution steps)
  - `tags`

#### Scorecard Schema
- Initial release
- Metadata: `name`, `labels`, `annotations`
- Spec fields:
  - `displayName`
  - `description`
  - `appliesTo` (resource selectors)
  - `checks` (evaluation rules)
  - `scoring` (levels: gold, silver, bronze)

### Schema Stability

- All fields marked as `required` in v1 will remain required in future v1.x versions
- New optional fields may be added in v1.x (backward compatible)
- Field removals or type changes require a new major version (v2)
- `additionalProperties: true` in `spec` allows for custom extensions

## Future Versions

### Planned for v2 (TBD)

- Enhanced dependency modeling (version constraints, optional vs required)
- Service level objectives (SLOs) as first-class schema fields
- Resource quotas and limits
- API contract references (OpenAPI, GraphQL schemas)
- Cost tracking metadata

## Deprecation Policy

- Major versions (v1, v2) supported for minimum 1 year after next version release
- Deprecation warnings added 6 months before removal
- Migration guides provided for all breaking changes
- Old schema URLs remain accessible (never 404)

## Schema URLs

All schemas are available at stable URLs:

- Service: `https://schemas.easy-idp.com/v1/service.schema.json`
- Team: `https://schemas.easy-idp.com/v1/team.schema.json`
- Workflow: `https://schemas.easy-idp.com/v1/workflow.schema.json`
- Scorecard: `https://schemas.easy-idp.com/v1/scorecard.schema.json`

These URLs will never change or return 404 errors.
