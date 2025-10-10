# Easy IDP Documentation

Official documentation for Easy IDP - GitOps-native Internal Developer Platform.

## 📚 View Documentation

**Live Documentation:** https://buckinghamio.github.io/easy-idp-docs/

## 🚀 Local Development

### Prerequisites

- Python 3.x
- pip

### Setup

```bash
# Install MkDocs Material
pip install mkdocs-material

# Serve locally
mkdocs serve
```

Visit http://localhost:8000 to view the documentation.

## 📝 Contributing

We welcome contributions to improve the documentation!

### Adding New Pages

1. Create a new Markdown file in the appropriate `docs/` subdirectory
2. Add the page to the navigation in `mkdocs.yml`
3. Test locally with `mkdocs serve`
4. Submit a pull request

### Documentation Structure

```
docs/
├── getting-started/     # Installation and quickstart guides
├── workflows/           # Workflow creation and management
├── gitops/             # GitOps configuration and setup
├── features/           # Feature documentation
├── api/                # API reference
└── guides/             # Step-by-step guides
```

### Style Guide

- Use clear, concise language
- Include code examples
- Add warnings/notes using admonitions
- Test all commands and code snippets
- Include screenshots where helpful

## 🛠️ Built With

- [MkDocs](https://www.mkdocs.org/) - Static site generator
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) - Material Design theme
- [GitHub Pages](https://pages.github.com/) - Hosting

## 📦 Deployment

Documentation is automatically deployed to GitHub Pages when changes are pushed to the `main` branch.

The deployment is handled by GitHub Actions (see `.github/workflows/docs.yml`).

## 📄 License

This documentation is part of the Easy IDP project and is licensed under the MIT License.

## 🔗 Links

- [Easy IDP Repository](https://github.com/BuckinghamIO/easy-idp)
- [Report Documentation Issues](https://github.com/BuckinghamIO/easy-idp-docs/issues)
- [Easy IDP Discussions](https://github.com/BuckinghamIO/easy-idp/discussions)
