# Action Primitives

Easy IDP provides a set of powerful action primitives that can be composed to create complex workflows.

## Overview

Actions are the building blocks of workflows. Each action performs a specific task such as making an HTTP request, executing a shell command, or rendering a template.

## Available Actions

### http.request

Make HTTP/HTTPS requests to external APIs or services.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `method` | string | Yes | HTTP method (GET, POST, PUT, DELETE, etc.) |
| `url` | string | Yes | Full URL to request |
| `headers` | object | No | HTTP headers |
| `body` | object/string | No | Request body |
| `timeout` | int | No | Request timeout in seconds (default: 30) |

**Example:**

```json
{
  "name": "Create GitHub Repository",
  "action": "http.request",
  "params": {
    "method": "POST",
    "url": "https://api.github.com/orgs/{{ globals.github_org }}/repos",
    "headers": {
      "Authorization": "Bearer {{ creds.github.token }}",
      "Accept": "application/vnd.github.v3+json"
    },
    "body": {
      "name": "{{ inputs.repo_name }}",
      "private": true,
      "auto_init": true
    }
  }
}
```

**Output:**

```json
{
  "status_code": 201,
  "body": {...},
  "headers": {...}
}
```

---

### shell.exec

Execute shell commands on the server.

!!! warning "Security"
    Commands must be in the allowlist for security. Contact your administrator to add new commands.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `command` | string | Yes | Command to execute |
| `args` | array | No | Command arguments |
| `cwd` | string | No | Working directory |
| `env` | object | No | Environment variables |

**Example:**

```json
{
  "name": "Deploy to Kubernetes",
  "action": "shell.exec",
  "params": {
    "command": "kubectl",
    "args": [
      "apply",
      "-f",
      "{{ vars.manifest_path }}",
      "-n",
      "{{ inputs.namespace }}"
    ],
    "env": {
      "KUBECONFIG": "/etc/kubernetes/config"
    }
  }
}
```

**Output:**

```json
{
  "exit_code": 0,
  "stdout": "deployment.apps/my-service created",
  "stderr": ""
}
```

---

### template.render

Render Jinja2 templates to files.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `template` | string | Yes | Template file name |
| `output` | string | Yes | Output file path |
| `context` | object | Yes | Template variables |

**Example:**

```json
{
  "name": "Generate Kubernetes Manifest",
  "action": "template.render",
  "params": {
    "template": "k8s-deployment.yaml.j2",
    "output": "/tmp/deployment.yaml",
    "context": {
      "service_name": "{{ inputs.service_name }}",
      "image": "{{ inputs.image }}",
      "replicas": "{{ inputs.replicas }}"
    }
  }
}
```

**Template Example** (`templates/k8s-deployment.yaml.j2`):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ service_name }}
spec:
  replicas: {{ replicas }}
  template:
    spec:
      containers:
      - name: {{ service_name }}
        image: {{ image }}
```

**Output:**

```json
{
  "output_path": "/tmp/deployment.yaml",
  "size_bytes": 512
}
```

---

### flow.set_vars

Set or derive variables for use in subsequent steps.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `vars` | object | Yes | Variables to set (key-value pairs) |

**Example:**

```json
{
  "name": "Set Derived Variables",
  "action": "flow.set_vars",
  "params": {
    "vars": {
      "full_service_name": "{{ inputs.environment }}-{{ inputs.service_name }}",
      "manifest_path": "/tmp/{{ inputs.service_name }}-deployment.yaml",
      "timestamp": "{{ now() | datetime('%Y%m%d-%H%M%S') }}"
    }
  }
}
```

**Output:**

```json
{
  "vars_set": ["full_service_name", "manifest_path", "timestamp"]
}
```

---

### flow.assert

Assert conditions with fail-fast behavior.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `condition` | string | Yes | Boolean expression to evaluate |
| `error_message` | string | No | Custom error message on failure |

**Example:**

```json
{
  "name": "Validate Service Name",
  "action": "flow.assert",
  "params": {
    "condition": "{{ inputs.service_name | regex_match('^[a-z0-9-]+$') }}",
    "error_message": "Service name must be lowercase alphanumeric with dashes"
  }
}
```

**Output:**

```json
{
  "passed": true
}
```

---

## Variable Substitution

All action parameters support template variable substitution using Jinja2 syntax.

### Available Variables

**`inputs`** - User input from workflow form:
```
{{ inputs.service_name }}
{{ inputs.environment }}
```

**`vars`** - Variables set by `flow.set_vars`:
```
{{ vars.full_service_name }}
{{ vars.timestamp }}
```

**`globals`** - Global configuration values:
```
{{ globals.github_org }}
{{ globals.cluster_domain }}
```

**`creds`** - Credential values:
```
{{ creds.github.token }}
{{ creds.aws.access_key }}
```

**`outputs`** - Output from previous steps:
```
{{ outputs.create_repo.body.id }}
{{ outputs.deploy.exit_code }}
```

### Jinja2 Filters

**String manipulation:**
```
{{ inputs.name | upper }}
{{ inputs.name | lower }}
{{ inputs.name | title }}
{{ inputs.name | replace(' ', '-') }}
```

**Regex:**
```
{{ inputs.email | regex_match('.*@example\\.com') }}
{{ inputs.version | regex_search('\\d+\\.\\d+') }}
```

**JSON:**
```
{{ outputs.api_call.body | tojson }}
{{ vars.data | fromjson }}
```

**Date/Time:**
```
{{ now() }}
{{ now() | datetime('%Y-%m-%d') }}
```

---

## Error Handling

All actions return a status indicating success or failure. If an action fails:

1. Workflow execution stops immediately
2. Error details are logged
3. Workflow status set to "failed"
4. User is notified

**Example error output:**

```json
{
  "error": "Command 'kubectl' returned non-zero exit code 1",
  "details": {
    "exit_code": 1,
    "stderr": "Error: deployment not found"
  }
}
```

---

## Best Practices

### 1. Use Meaningful Step Names

```json
// ❌ Bad
{"name": "Step 1", "action": "http.request", ...}

// ✅ Good
{"name": "Create GitHub Repository", "action": "http.request", ...}
```

### 2. Validate Input Early

```json
// Add validation steps at the beginning
{
  "name": "Validate Service Name",
  "action": "flow.assert",
  "params": {
    "condition": "{{ inputs.service_name | length > 0 }}"
  }
}
```

### 3. Set Intermediate Variables

```json
// Instead of complex nested templates
{
  "name": "Prepare Variables",
  "action": "flow.set_vars",
  "params": {
    "vars": {
      "repo_url": "https://github.com/{{ globals.github_org }}/{{ inputs.repo_name }}"
    }
  }
}
```

### 4. Use Descriptive Error Messages

```json
{
  "name": "Check Cluster Access",
  "action": "flow.assert",
  "params": {
    "condition": "{{ creds.kubernetes.token | length > 0 }}",
    "error_message": "Kubernetes credentials not configured. Please set up credentials in Admin panel."
  }
}
```

---

## Examples

See the [Examples](examples.md) page for complete workflow examples using these action primitives.

## API Reference

For detailed API information, see the [API Documentation](../api/workflows.md).
