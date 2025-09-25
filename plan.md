# App Vitals CI/CD - Opinionated CI/CD Platform

## Overview

An opinionated, Docker-based CI/CD template generator that creates GitHub Actions workflows providing a standardized CI/CD experience across different infrastructure providers and programming languages. Companies own their CI/CD workflows with no vendor lock-in.

## Core Principles

- **Template Generation**: Creates CI/CD workflows you own and control
- **No Vendor Lock-in**: Generated workflows work independently of App Vitals
- **Docker-first**: All applications are containerized for consistency
- **Infrastructure Agnostic**: Templates for AWS, GCP, Azure, and others
- **Language Agnostic**: Templates for any programming language/framework
- **Convention over Configuration**: Sensible defaults with full customization
- **GitOps**: Deployments triggered by GitHub releases
- **Quality Gates**: Integrated linting and testing in CI/CD pipeline

## Architecture

### Core Components

1. **CLI Template Generator**
   - `@app-vitals/ci-cd-gen` - NPM package for generating CI/CD workflows
   - Interactive prompts for language, provider, and service selection
   - Template engine for generating customized CI/CD workflows
   - Validation and best practices enforcement

2. **Template Library**
   - Language templates (Node.js, Python, Ruby, Go, Rust)
   - Package manager templates (npm, yarn, pip, poetry, bundle)
   - Framework templates (Next.js, Django, Rails, FastAPI)
   - Infrastructure provider templates (AWS, GCP, Azure, Kubernetes)
   - Service templates (ECS, Cloud Run, Lambda, etc.)

3. **Generated CI/CD Components**
   - Quality gates (lint, test, coverage)
   - Docker build and optimization
   - Infrastructure deployment
   - Rollback capabilities
   - Hotfix workflows

### Deployment Triggers

- **Staging**: Creating and pushing a `v*` tag (e.g., `v1.2.3`) → triggers staging deployment
- **Production**: Creating a GitHub release from that tag → triggers production deployment
- **Hotfixes**: Creating a release from hotfix tags `v*-hotfix.*` → deploy directly to production

Workflow:
1. Create and push git tag: `git tag v1.2.3 && git push origin v1.2.3` → **deploys to staging**
2. Test staging deployment, then create GitHub release → **deploys to production**
3. Rollback = create new release from previous tag

### CI/CD Flow

**PR Events (Validation):**
```
1. Trigger (PR creation/update)
   ↓
2. Lint & Code Quality Checks
   ↓
3. Test Execution
   ↓
4. Docker Build (validation only)
   ↓
5. Report Results
```

**Tag Events (Staging Deployment):**
```
1. Trigger (Git tag push)
   ↓
2. Lint & Code Quality Checks
   ↓
3. Test Execution
   ↓
4. Pre-build Hooks
   ↓
5. Docker Build & Optimization
   ↓
6. Post-build Hooks (security scanning)
   ↓
7. Pre-deploy Hooks
   ↓
8. Deploy to Staging
   ↓
9. Post-deploy Hooks
```

**Release Events (Production Deployment):**
```
1. Trigger (GitHub release creation)
   ↓
2. Lint & Code Quality Checks
   ↓
3. Test Execution
   ↓
4. Pre-build Hooks
   ↓
5. Docker Build & Optimization
   ↓
6. Post-build Hooks (security scanning)
   ↓
7. Pre-deploy Hooks
   ↓
8. Deploy to Production
   ↓
9. Post-deploy Hooks
```

## Template Generation

### CLI Usage

```bash
# Initialize CI/CD workflows
npx @app-vitals/ci-cd-gen init

# Or with options
npx @app-vitals/ci-cd-gen init \
  --language=nodejs \
  --package-manager=npm \
  --framework=nextjs \
  --provider=aws \
  --service=ecs \
  --app-name=my-app

# Generate additional workflows
npx @app-vitals/ci-cd-gen add hotfix    # Creates separate hotfix.yml workflow
npx @app-vitals/ci-cd-gen add rollback  # Adds rollback job to existing workflow
```

### Interactive Prompts

```
? What's your application name? my-nextjs-app
? What language are you using? (Use arrow keys)
❯ Node.js
  Python
  Ruby
  Go
  Rust

? What package manager? (Use arrow keys)
❯ npm
  yarn
  pnpm

? What framework? (Use arrow keys)
❯ Next.js
  React
  Express
  None

? What cloud provider? (Use arrow keys)
❯ AWS
  GCP
  Azure
  Kubernetes

? What service type? (Use arrow keys)
❯ ECS
  Fargate
  Lambda
  EC2
```

### Generated Output

The CLI generates a complete, self-contained `.github/workflows/ci_cd.yml` with all CI/CD logic inline - handles both PR validation and deployment with no external dependencies on App Vitals infrastructure.

### Provider-Specific Configuration

#### AWS (`.aws/production.yml`)
```yaml
service_type: "ecs" # ecs, fargate, lambda, ec2
cluster: "production-cluster"
task_definition: "my-app-production"
```

#### GCP (`.gcp/production.yml`)
```yaml
service_type: "cloud_run" # cloud_run, gke, compute_engine
region: "us-central1"
service_name: "my-app-production"
```

## Directory Structure

```
.github/
  workflows/
    ci_cd.yml               # Main CI/CD workflow (PR checks + deployment)
.aws/                       # AWS-specific configs
  staging.yml
  production.yml
.gcp/                       # GCP-specific configs
  staging.yml
  production.yml
scripts/                    # Custom deployment scripts
  hooks/
    pre-build/
    post-build/
    pre-deploy/
    post-deploy/
  providers/
    aws/
    gcp/
    azure/
```

## Implementation Phases

### Phase 1: CLI Foundation
- [ ] NPM package setup and CLI framework
- [ ] Interactive prompt system
- [ ] Template engine and file generation
- [ ] Basic language templates (Node.js, Python)
- [ ] Basic provider templates (AWS ECS, GCP Cloud Run)
- [ ] Generated workflow validation

### Phase 2: Template Library Expansion
- [ ] Additional language templates (Ruby, Go, Rust)
- [ ] Package manager variations (yarn, pnpm, poetry, bundle)
- [ ] Framework-specific templates (Next.js, Django, Rails)
- [ ] Additional AWS services (Fargate, Lambda)
- [ ] Additional GCP services (GKE, Cloud Functions)

### Phase 3: Advanced Workflow Features
- [ ] Hotfix workflow generation
- [ ] Rollback workflow generation
- [ ] Multi-environment templates
- [ ] Custom hook integration
- [ ] Docker optimization templates

### Phase 4: Developer Experience
- [ ] Configuration file detection and migration
- [ ] Workflow updating and versioning
- [ ] Template validation and linting
- [ ] Documentation generation

### Phase 5: Enterprise Features
- [ ] Azure and Kubernetes templates
- [ ] Security scanning templates
- [ ] Compliance and audit templates
- [ ] Multi-region deployment templates

## Template System Design

### Language-Specific Templates

The CLI generates workflows based on your language and package manager choices. Each combination produces specific commands in the generated workflow:

**Node.js Templates:**
```bash
# npm template generates:
npm ci
npm run build
npm test

# yarn template generates:
yarn install
yarn build
yarn test
```

**Python Templates:**
```bash
# pip template generates:
pip install -r requirements.txt
python -m pytest

# poetry template generates:
poetry install
poetry run pytest
```

**Framework Extensions:**
```bash
# Django framework adds:
python manage.py migrate
python manage.py collectstatic

# Next.js framework adds:
npm run build  # optimized for Next.js
```

### Generated Custom Hooks

The CLI can generate hook scripts that you customize:

**Generated hook structure:**
```bash
scripts/
  hooks/
    pre-build.sh     # Generated with framework-specific logic
    post-build.sh    # Generated with security scanning templates
    pre-deploy.sh    # Generated with migration commands
    post-deploy.sh   # Generated with smoke test templates
```

**Example generated pre-deploy.sh:**
```bash
#!/bin/bash
# Generated by @app-vitals/ci-cd-gen for Node.js + Prisma

echo "Running database migrations..."
npx prisma migrate deploy

echo "Warming application cache..."
curl -X POST "$APP_URL/api/cache/warm"
```

## Error Handling & Rollback

- **Rollback via GitHub Releases**: Create a new release from a previous tag
- **Manual rollback**: GitHub Actions workflow dispatch to deploy specific release
- **Automated rollback**: Optional workflow to auto-rollback on deployment failure

## Hotfix Strategy

Critical fixes need to bypass the normal staging flow while maintaining safety:

### Hotfix Flow
1. **Branch from production release**: Create hotfix branch from production tag (e.g., `v1.2.3`)
2. **Make minimal fix**: Only include the critical fix, no other changes
3. **Tag hotfix**: Use pattern like `v1.2.3-hotfix.1`
4. **Skip staging environment**: Deploy directly to production
5. **Quality gates by default**: Run lint, test, and build checks
6. **Selective opt-outs**: Allow skipping specific checks for true emergencies

### Hotfix Configuration
```yaml
# .github/workflows/hotfix.yml - generated separate workflow for hotfixes
name: Hotfix Deploy
on:
  release:
    types: [published]

jobs:
  hotfix:
    if: contains(github.event.release.tag_name, 'hotfix')
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      # Quality gates (can be skipped with emergency_mode)
      - name: Install dependencies
        run: npm ci

      - name: Lint
        if: env.EMERGENCY_MODE != 'true'
        run: npm run lint

      - name: Test
        if: env.EMERGENCY_MODE != 'true'
        run: npm test

      # Docker build and deploy directly to production (skip staging)
      - name: Build and Deploy
        run: |
          # Generated deployment logic here
          docker build -t my-app .
          # AWS ECS deployment commands...
```

### Emergency Overrides
- `emergency_mode: "true"`: Skip lint and test for critical outages
- Manual approval still required for production deployment

## Security Considerations

- Secrets management via GitHub Actions secrets
- Container image scanning
- RBAC for deployment permissions
- Audit trail for all deployments

## Getting Started

### Quick Start

```bash
# Install the CLI
npm install -g @app-vitals/ci-cd-gen

# Navigate to your project
cd my-nextjs-app

# Generate CI/CD workflows
npx @app-vitals/ci-cd-gen init
```

### Complete Example

For a Node.js app deploying to AWS ECS:

**1. Run the generator:**
```bash
$ npx @app-vitals/ci-cd-gen init
? What's your application name? my-nextjs-app
? What language are you using? Node.js
? What package manager? npm
? What framework? Next.js
? What cloud provider? AWS
? What service type? ECS

✓ Generated .github/workflows/ci_cd.yml
✓ Generated .github/workflows/hotfix.yml
✓ Generated .aws/production.yml
✓ Generated README-deployment.md

🎉 CI/CD workflows ready! Your team owns and controls all the code.
```

**2. Customize generated files if needed (they're fully editable)**

**3. Deploy:**
```bash
# Create and push tag
git tag v1.0.0
git push origin v1.0.0

# Create GitHub release (triggers deployment)
gh release create v1.0.0 --title "Release v1.0.0" --notes "Initial release"
```

**4. Your CI/CD workflows work independently - no App Vitals dependency!**