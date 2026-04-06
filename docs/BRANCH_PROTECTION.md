# Branch Protection Rules Configuration

## Overview

This document outlines the branch protection rules that should be configured in the GitHub repository to maintain code quality and prevent accidental commits to critical branches.

---

## Protected Branches

### main (Production)

| Rule | Setting | Purpose |
|------|---------|---------|
| Require pull request reviews | ✅ Enabled | Ensure code review before production |
| Required approving reviews | 1 | Minimum one approval |
| Require status checks | ✅ Enabled | CI must pass |
| Require branches up to date | ✅ Enabled | Prevent merge conflicts |
| Dismiss stale reviews | ✅ Enabled | Re-request if new commits |
| Require conversation resolution | ✅ Enabled | All comments addressed |
| Include administrators | ✅ Enabled | Same rules for all |
| Force pushes | ❌ Disabled | Prevent accidental overwrites |
| Branch deletions | ❌ Disabled | Prevent branch deletion |

### develop (Integration)

| Rule | Setting | Purpose |
|------|---------|---------|
| Require pull request reviews | ✅ Enabled | Ensure code review |
| Required approving reviews | 1 | Minimum one approval |
| Require status checks | ✅ Enabled | CI must pass |
| Require branches up to date | ✅ Enabled | Prevent merge conflicts |
| Dismiss stale reviews | ✅ Enabled | Re-request if new commits |
| Require conversation resolution | ✅ Enabled | All comments addressed |
| Include administrators | ✅ Enabled | Same rules for all |
| Force pushes | ❌ Disabled | Protect integration branch |

---

## How to Configure in GitHub

### Steps to Enable Branch Protection

1. Navigate to repository on GitHub
2. Go to **Settings** → **Branches**
3. Click **Add branch protection rule**
4. Enter branch name pattern (e.g., `main` or `develop`)
5. Enable the rules as specified above
6. Click **Save**

### Alternative: Using GitHub CLI

```bash
# Protect main branch
gh api repos/OWNER/REPO/branches/main/protection \
  -X PUT \
  -f required_status_checks='{"strict":true,"contexts":["ci/check"]}' \
  -f required_pull_request_reviews='{"dismiss_stale_reviews":true,"required_approving_review_count":1}' \
  -f allow_force_pushes=false \
  -f allow_deletions=false
```

---

## Environment Protection

### Staging/Production Environments

| Environment | Protection Level |
|-------------|-------------------|
| Production (main) | Full protection, Lead approval required |
| Staging | Protection enabled, one approval |
| Development (develop) | Protection enabled, one approval |

---

## Status Checks Required

Before any PR can be merged, these checks must pass:

1. **CI Pipeline** - All tests must pass
2. **Code Quality** - Linting/formatting checks
3. **Security Scan** - No vulnerabilities detected
4. **Build** - Application builds successfully

---

## Review Requirements

### Required Reviewers

| Branch | Min Reviews | Special Requirements |
|--------|-------------|---------------------|
| main | 1 + Lead | Lead must approve |
| develop | 1 | Team member |
| feature/* | 1 | Team member |
| release/* | 1 + Lead | Lead must approve |

### Who Can Approve

- Engineering Lead
- Team Lead
- Senior Developers

---

## Bypass Permissions

**Do not grant bypass permissions to anyone** except in emergencies.

If emergency bypass is needed:
1. Document the reason
2. Get verbal approval from Lead
3. Create follow-up issue for review
4. Review the changes after merge

---

## Monitoring

### Branch Protection Alerts

Set up notifications for:
- Failed protection rule attempts
- Bypass permission usage
- Unusual merge patterns

---

*Last Updated: April 2026*
*Owner: Engineering Lead*