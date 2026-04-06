# Git Workflow Best Practices

## Overview

This document outlines the Git workflow and best practices for the Fashion Fest development team. Following these guidelines ensures clean, traceable, and collaborative development.

## Technology Stack

| Layer | Technology |
|-------|------------|
| Backend | Laravel (PHP) |
| Frontend | Next.js (React) |
| UI Components | shadcn/ui |
| Styling | Tailwind CSS |

---

## Team Roles & Responsibilities

| Role | Git Responsibility |
|------|-------------------|
| **CTO** | Architecture decisions, tech stack standards |
| **Engineering Lead** | Code review approval, merge to main/production branches, enforce standards |
| **Frontend Developer** | Next.js features, UI components, shadcn/ui, PRs for frontend |
| **Backend Developer** | Laravel API, database changes, service integrations |
| **QA Tester** | Test branch management, validate features before release |
| **UI/UX Designer** | Design asset management, design system updates |
| **Product Expert** | Product feedback, acceptance criteria validation |

---

## Branch Strategy

### Branch Types

```
main          # Production-ready code (protected)
├── develop   # Integration branch for features (protected)
│   ├── feature/TICKET-123-short-description
│   ├── bugfix/TICKET-456-fix-login-issue
│   ├── hotfix/TICKET-789-urgent-security-patch
│   └── release/v1.0.0
```

### Branch Naming Convention

| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feature/TICKET-ID-description` | `feature/FF-123-add-shopping-cart` |
| Bugfix | `bugfix/TICKET-ID-description` | `bugfix/FF-456-fix-payment-error` |
| Hotfix | `hotfix/TICKET-ID-description` | `hotfix/FF-789-security-patch` |
| Release | `release/vMAJOR.MINOR.PATCH` | `release/v1.0.0` |
| Experiment | `experiment/description` | `experiment/new-payment-gateway` |

**Rules:**
- Use kebab-case (lowercase with hyphens)
- Include ticket ID when available
- Keep descriptions concise (3-5 words max)
- No spaces, no special characters except hyphens

---

## Commit Guidelines

### Commit Message Format

```
type(TICKET-ID): Short description

Longer description if needed.

Closes #TICKET-ID
```

### Types

| Type | Description |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Code style (formatting, no logic) |
| `refactor` | Code refactoring |
| `test` | Adding/updating tests |
| `chore` | Maintenance, dependencies |
| `perf` | Performance improvement |

### Examples

```
feat(FF-123): Add user authentication flow

- Implement JWT token generation
- Add login/logout endpoints
- Create auth middleware

Closes #FF-123

---

fix(FF-456): Resolve payment gateway timeout

- Add retry mechanism with exponential backoff
- Increase timeout from 5s to 30s
- Add proper error handling

Closes #FF-456

---

docs(FF-789): Update API documentation

- Add new endpoints
- Fix broken examples
```

### Commit Rules

- **Atomic commits**: One logical change per commit
- **Imperative mood**: "Add feature" not "Added feature"
- **Max 72 chars** for first line
- **Reference ticket**: Always include TICKET-ID when applicable
- **Test first**: Commit tests with the code they validate

---

## Pull Request Workflow

### PR Creation

1. **Before creating PR:**
   - [ ] All tests pass locally
   - [ ] Code follows style guidelines
   - [ ] No console.log or debug code
   - [ ] Self-review completed
   - [ ] Documentation updated if needed

2. **Create PR with:**
   - Clear title following convention
   - Detailed description using template
   - Link to related tickets
   - Screenshots for UI changes

### PR Title Format

```
[TYPE] TICKET-ID: Brief description

Examples:
[Feature] FF-123: Add shopping cart functionality
[Bugfix] FF-456: Fix payment timeout issue
[Hotfix] FF-789: Patch security vulnerability
```

### PR Description Template

```markdown
## Summary
Brief description of what this PR does.

## Changes
- Change 1
- Change 2
- Change 3

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests pass
- [ ] Manual testing completed

## Screenshots
If applicable, add screenshots here.

## Checklist
- [ ] Code follows project standards
- [ ] No breaking changes
- [ ] Documentation updated
- [ ] Linked to related issues
```

### Review Process

| Step | Who | Action |
|------|-----|--------|
| 1 | Author | Create PR, request review |
| 2 | Code Reviewer | Review code, leave comments |
| 3 | Author | Address feedback, respond |
| 4 | Reviewer | Approve or request changes |
| 5 | Lead | Final approval, merge |

### Review Requirements

- **Minimum 1 approval** from code reviewer
- **Lead approval** required for:
  - Production branch merges
  - Breaking changes
  - Security-related changes
- **All checks must pass** before merge (CI/CD)
- **No unresolved comments**

---

## Merge Strategies

### Feature Branches ( squash merge recommended)

```
git checkout develop
git merge --squash feature/FF-123-feature-name
```

**Benefits:**
- Clean history on develop
- Each PR = one commit
- Easy to revert entire feature

### Hotfix Branches ( merge commit)

```
git checkout main
git merge --no-ff hotfix/FF-789-fix
```

**Benefits:**
- Preserves exact timeline
- Easier to track hotfix in history

### Release Branches ( merge commit)

```
git checkout main
git merge --no-ff release/v1.0.0
```

---

## Branch Protection Rules

### main (Production)

- [ ] Require pull request reviews before merging
- [ ] Require at least 1 approval
- [ ] Require status checks to pass
- [ ] Require branches to be up to date
- [ ] Include administrators in restrictions
- [ ] Require conversation resolution before merge
- [ ] Do not allow force pushes
- [ ] Do not allow deletions

### develop (Integration)

- [ ] Require pull request reviews before merging
- [ ] Require at least 1 approval
- [ ] Require status checks to pass
- [ ] Require branches to be up to date
- [ ] Include administrators in restrictions
- [ ] Require conversation resolution before merge
- [ ] Do not allow force pushes

---

## Daily Workflow

### Starting New Work

```bash
# Update local develop
git checkout develop
git pull origin develop

# Create feature branch
git checkout -b feature/FF-123-feature-name
```

### During Development

```bash
# Stage and commit changes
git add -A
git commit -m "feat(FF-123): Add feature description"

# Push to remote (first time)
git push -u origin feature/FF-123-feature-name
```

### Before Creating PR

```bash
# Rebase on latest develop (cleaner history)
git fetch origin
git rebase origin/develop

# If conflicts, resolve and continue
git add -A
git rebase --continue

# Force push after rebase (safe for feature branches)
git push --force-with-lease
```

### After PR Approval

```bash
# Squash and merge via GitHub UI
# Or use command line:
git checkout develop
git merge --squash feature/FF-123-feature-name
git commit -m "feat(FF-123): Complete feature name"
git push origin develop

# Delete feature branch
git branch -d feature/FF-123-feature-name
git push origin --delete feature/FF-123-feature-name
```

---

## Common Commands Reference

```bash
# Create branch
git checkout -b feature/FF-123-description

# Commit with type
git commit -m "feat(FF-123): Add feature"

# Amend last commit (before push)
git commit --amend

# Interactive rebase (squash commits)
git rebase -i HEAD~3

# Stash changes
git stash
git stash pop

# View branch diff
git log develop..feature/FF-123 --oneline

# Undo last commit (keep changes)
git reset --soft HEAD~1
```

---

## Anti-Patterns to Avoid

❌ **Don't do this:**
- Committing directly to main/develop
- Using ambiguous branch names (e.g., "fix", "update", "mywork")
- Writing vague commit messages ("fixed stuff", "updated")
- Force pushing to protected branches
- Creating massive PRs with unrelated changes
- Leaving commented-out code in commits
- Committing secrets or credentials

✅ **Do this instead:**
- Create feature branches from develop
- Use clear, descriptive names
- Write meaningful commit messages
- Use --force-with-lease for feature branches only
- Keep PRs focused and small
- Clean up before committing
- Use environment variables for secrets

---

## Resources

- [GitHub Flow](https://guides.github.com/introduction/flow/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [GitKraken Learning](https://www.gitkraken.com/learn/git)

---

*Last Updated: April 2026*
*Owner: Engineering Lead*