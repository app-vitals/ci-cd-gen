# @app-vitals/ci-cd-gen

> Opinionated CI/CD template generator for GitHub Actions workflows

Generate complete CI/CD workflows that you own and control - no vendor lock-in, no external dependencies.

## 🚀 Quick Start

```bash
# Coming soon!
npx @app-vitals/ci-cd-gen init
```

## ✨ What it does

- **Generates complete CI/CD workflows** - PR validation + staging/production deployment
- **No vendor lock-in** - You own all the generated code
- **Language & infrastructure agnostic** - Works with Node.js, Python, Ruby, Go, Rust + AWS, GCP, Azure
- **Docker-first** - All apps are containerized for consistency
- **GitOps workflow** - Tag for staging, GitHub release for production

## 📋 Current Status

🚧 **In Planning Phase** - See [plan.md](./plan.md) for the complete technical specification.

## 🏗️ Architecture

Template-based CLI that generates:
- `.github/workflows/ci_cd.yml` - Complete CI/CD pipeline
- `.github/workflows/hotfix.yml` - Emergency hotfix workflow
- Provider configs (`.aws/production.yml`, `.gcp/production.yml`, etc.)
- Custom hook scripts for migrations, testing, etc.

## 🎯 Why Template Generation?

Unlike GitHub Actions that create vendor dependencies, we generate workflows you fully control:

- ✅ **Full transparency** - All deployment logic visible in your repo
- ✅ **Easy customization** - Generated workflows are fully editable
- ✅ **No trust issues** - Works even if App Vitals disappears
- ✅ **Learning tool** - See best practices and understand the patterns

## 📚 Documentation

- [Complete Technical Plan](./plan.md) - Full architecture and implementation details
- [Implementation Phases](./plan.md#implementation-phases) - Development roadmap

## 🤝 Contributing

This project is in early planning stages. Check back soon for contribution guidelines!

---

Built with ❤️ by [App Vitals](https://github.com/app-vitals)