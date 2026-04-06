# Fashion Fest

A modern e-commerce platform for fashion events and shopping.

## Project Overview

- **Project Type:** E-commerce Web Application
- **Tech Stack:** (To be determined based on project requirements)
- **Repository:** https://github.com/ngaw-dev/fashion-fest

## Quick Start

```bash
# Clone the repository
git clone https://github.com/ngaw-dev/fashion-fest.git
cd fashion-fest

# Install dependencies (to be determined)
# npm install or pnpm install
```

## Documentation

- [Team Roles & Responsibilities](docs/TEAM_ROLES.md)
- [Git Workflow Best Practices](docs/GIT_WORKFLOW.md)
- [Branch Protection Rules](docs/BRANCH_PROTECTION.md)

## Git Workflow

This project follows a structured Git workflow. Please review the [Git Workflow Guide](docs/GIT_WORKFLOW.md) before contributing.

### Branch Structure

```
main          # Production (protected)
├── develop   # Integration (protected)
│   ├── feature/*   # Feature branches
│   ├── bugfix/*    # Bug fix branches
│   └── release/*   # Release branches
```

### Key Conventions

- **Branch Naming:** `type/TICKET-ID-description` (e.g., `feature/FF-123-add-cart`)
- **Commits:** Conventional commits with type (feat, fix, docs, etc.)
- **PRs:** Use PR template in `.github/PULL_REQUEST_TEMPLATE.md`

## Team

See [Team Roles](docs/TEAM_ROLES.md) for the complete organizational structure.

## License

MIT