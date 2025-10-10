# Quickstart

This guide will walk you through creating and executing your first workflow in Easy IDP.

## Step 1: Login

Navigate to your Easy IDP instance and login with your admin credentials:

```
http://localhost:8000
```

## Step 2: View Available Workflows

After logging in, you'll see the **Workflows** page listing all available workflows.

For this quickstart, we'll use the example "Deploy Service" workflow that comes pre-configured.

## Step 3: Execute a Workflow

1. Click on **"Deploy Service"** workflow
2. Fill out the form:
   - **Service Name**: `my-api`
   - **Repository URL**: `https://github.com/your-org/my-api`
   - **Environment**: `development`

3. Click **"Execute Workflow"**

## Step 4: Monitor Execution

You'll be redirected to the execution page where you can see:

- ✅ Real-time step execution
- 📋 Detailed logs for each step
- ⏱️ Execution time and status
- ⚠️ Any errors or warnings

The workflow will execute each step sequentially:

```
Step 1: Validate repository access ✓
Step 2: Create deployment configuration ✓
Step 3: Deploy to cluster ✓
Step 4: Verify deployment ✓
```

## Step 5: View Results

Once complete, you can:

- View the full execution log
- See output variables from each step
- Download logs for auditing
- Re-run the workflow if needed

---

## Create Your First Workflow

Now let's create a simple custom workflow from scratch.

### Navigate to Admin Panel

Click **Admin → Workflows** in the navigation bar.

### Create New Workflow

Click **"New Workflow"** and fill out the form:

**Basic Information:**
```
Name: Hello World
Description: A simple test workflow
Version: 1.0
```

**Form Schema:**
```json
{
  "fields": [
    {
      "name": "message",
      "label": "Message",
      "type": "text",
      "required": true,
      "default": "Hello, World!"
    }
  ]
}
```

**Workflow Steps:**
```json
[
  {
    "name": "Print Message",
    "action": "shell.exec",
    "params": {
      "command": "echo",
      "args": ["{{ inputs.message }}"]
    }
  }
]
```

### Save and Test

1. Click **"Save Workflow"**
2. Navigate back to **Workflows**
3. Find your "Hello World" workflow
4. Click to execute it
5. Enter a message and click **"Execute"**

You should see your message printed in the logs! 🎉

---

## What's Next?

Now that you've created your first workflow, explore these topics:

### Learn More About Workflows

- [Action Primitives](../workflows/action-primitives.md) - Available action types
- [Creating Workflows](../workflows/creating-workflows.md) - Detailed workflow guide
- [Examples](../workflows/examples.md) - Real-world workflow examples

### Configure GitOps

- [GitOps Overview](../gitops/overview.md) - What is GitOps sync?
- [Setup Guide](../gitops/setup.md) - Configure Git repository sync

### Advanced Features

- [Global Variables](../features/global-variables.md) - Centralized configuration
- [Credentials](../features/credentials.md) - Secure secret management
- [Templates](../features/templates.md) - Dynamic file generation

---

## Quick Reference

### Common Actions

**HTTP Request:**
```json
{
  "action": "http.request",
  "params": {
    "method": "POST",
    "url": "https://api.example.com/deploy",
    "body": {"service": "{{ inputs.service_name }}"}
  }
}
```

**Shell Command:**
```json
{
  "action": "shell.exec",
  "params": {
    "command": "kubectl",
    "args": ["apply", "-f", "{{ vars.manifest_file }}"]
  }
}
```

**Template Rendering:**
```json
{
  "action": "template.render",
  "params": {
    "template": "k8s-deployment.yaml.j2",
    "output": "/tmp/deployment.yaml",
    "context": {"service": "{{ inputs.service_name }}"}
  }
}
```

### Variable Substitution

Access data in your workflows using template syntax:

- `{{ inputs.field_name }}` - User input from form
- `{{ vars.variable_name }}` - Variables set by previous steps
- `{{ globals.config_key }}` - Global configuration values
- `{{ creds.alias.key }}` - Credential values
- `{{ outputs.step_name.key }}` - Output from previous step

---

## Troubleshooting

### Workflow Fails to Execute

Check the logs for specific error messages. Common issues:

- Missing required credentials
- Invalid template syntax
- Command not in allowlist

### Form Doesn't Validate

Ensure your form schema is valid JSON and includes required fields.

### Can't See Workflow

Make sure you're logged in as admin or have the developer role.

---

Ready to build something more complex? Check out the [Creating Workflows](../workflows/creating-workflows.md) guide!
