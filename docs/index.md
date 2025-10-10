# Welcome to Easy IDP

Easy IDP is a **GitOps-native Internal Developer Platform** that enables developers to create and execute workflows through guided forms.

!!! tip "Quick Start"
    New to Easy IDP? Start with our [Installation Guide](getting-started/installation.md) or jump straight to the [Quickstart](getting-started/quickstart.md).

## What is Easy IDP?

Easy IDP simplifies platform engineering by providing a self-service portal for common developer workflows. Instead of writing scripts or managing complex CI/CD pipelines, developers fill out simple forms that execute predefined workflows.

### Key Features

✨ **GitOps-First**  
Configuration as code with Git version control, code review, and rollback capabilities.

🔄 **Workflow Management**  
Create workflows with dynamic forms - no complex YAML or scripting required.

🔑 **Credential Management**  
Secure storage and templating of sensitive credentials with automatic secret redaction.

🌐 **Global Variables**  
Centralized configuration values accessible across all workflows.

⚡ **Action Primitives**  
Powerful, composable actions: HTTP requests, shell commands, templates, and flow control.

📊 **Real-time Execution**  
Step-by-step workflow execution with live status updates and detailed logging.

## Why Easy IDP?

Traditional internal developer platforms are complex and require significant infrastructure:

- ❌ Kubernetes clusters
- ❌ Multiple services and dependencies
- ❌ Weeks of setup time
- ❌ Dedicated platform engineering team

Easy IDP takes a different approach:

- ✅ Single Docker container
- ✅ No Kubernetes required
- ✅ Set up in minutes
- ✅ Maintainable by small teams

## Use Cases

### Service Deployment
Deploy new services with pre-configured workflows that handle repository creation, CI/CD setup, and initial deployment.

### Infrastructure Provisioning
Provision databases, DNS records, SSL certificates, and other infrastructure through simple forms.

### Developer Onboarding
Create workflows that set up developer environments, grant access, and configure tools automatically.

### Compliance & Governance
Enforce policies and standards through workflow templates with built-in validation.

## Quick Links

<div class="grid cards" markdown>

-   :material-download:{ .lg .middle } __Installation__

    ---

    Get Easy IDP up and running in minutes

    [:octicons-arrow-right-24: Install now](getting-started/installation.md)

-   :material-rocket-launch:{ .lg .middle } __Quickstart__

    ---

    Create your first workflow and see Easy IDP in action

    [:octicons-arrow-right-24: Get started](getting-started/quickstart.md)

-   :material-git:{ .lg .middle } __GitOps Setup__

    ---

    Configure GitOps sync with your platform-config repository

    [:octicons-arrow-right-24: Setup guide](gitops/setup.md)

-   :material-code-braces:{ .lg .middle } __API Reference__

    ---

    Integrate Easy IDP with your tools and services

    [:octicons-arrow-right-24: View API docs](api/authentication.md)

</div>

## Architecture

Easy IDP follows a simple, straightforward architecture:

```
┌─────────────────────────────────────────┐
│  Platform Config Repository (Git)       │
│  ├── globals.json                       │
│  ├── workflows/                         │
│  └── catalog/                           │
└─────────────┬───────────────────────────┘
              │
              │ GitOps Sync (Pull-based)
              │
              ▼
┌─────────────────────────────────────────┐
│  Easy IDP Application                   │
│  ├── Web UI (Forms)                     │
│  ├── Workflow Engine                    │
│  ├── Credential Store                   │
│  └── Execution Logs                     │
└─────────────────────────────────────────┘
```

## Community & Support

- **GitHub**: [BuckinghamIO/easy-idp](https://github.com/BuckinghamIO/easy-idp)
- **Issues**: [Report bugs or request features](https://github.com/BuckinghamIO/easy-idp/issues)
- **Discussions**: [Ask questions and share ideas](https://github.com/BuckinghamIO/easy-idp/discussions)

## License

Easy IDP is open source software licensed under the MIT License.

---

Ready to get started? Head over to the [Installation Guide](getting-started/installation.md)! 🚀
