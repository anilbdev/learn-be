# 02b — Repo Hosting: GitHub / GitLab / Bitbucket

## Key Concepts

| Term | Definition |
|------|-----------|
| Pull Request (PR) | GitHub term — proposed change from a branch, requires review before merge |
| Merge Request (MR) | GitLab term — same concept as PR |
| Branch Protection | Rules on `main` — require PR approval, passing CI, no direct push |
| GitHub Actions | GitHub's YAML-based CI/CD pipeline system |
| GitLab CI | GitLab's built-in CI/CD — no third-party tool needed |
| Fork | Copy of someone else's repo under your account (open source flow) |
| Clone | Local copy of a repo you have access to |
| Webhook | Notification sent to external services on repo events (push, PR, merge) |

---

## Platform Comparison

| Feature | GitHub | GitLab | Bitbucket |
|---------|--------|--------|-----------|
| Owned by | Microsoft | GitLab Inc | Atlassian |
| CI/CD | GitHub Actions | Built-in (GitLab CI) | Bitbucket Pipelines |
| Security scanning | Third-party | Built-in | Third-party |
| Container registry | Yes | Built-in | Limited |
| Self-hosted | GitHub Enterprise | Yes (free tier) | Yes |
| Best for | OSS, startups, big tech | Enterprises, compliance, DevOps | Atlassian/Jira shops |

---

## Pull Request Flow (Industry Standard)

```
feature/my-feature
       |
       | git push
       v
  Remote Repo
       |
       | Open PR / MR
       v
  Automated Pipeline (YAML)
  ├── Linting
  ├── Unit tests
  ├── Integration tests
  └── Security scan
       |
       | All checks pass
       v
  Code Review (teammate approves)
       |
       | Merge
       v
    main branch
       |
       | CI/CD triggers deploy
       v
   Production
```

---

## Why PRs Instead of Direct Pushes

- No broken code reaches `main` — automated checks catch it first
- Forces code review — second pair of eyes catches bugs and design issues
- Audit trail — every change has a linked PR with discussion and context
- Branch protection enforces this at the platform level — can't bypass it

---

## Fork vs Clone

| | Fork | Clone |
|--|------|-------|
| Used for | Contributing to someone else's repo | Working on a repo you have access to |
| Creates copy | Under your GitHub account | On your local machine |
| Flow | Fork → clone fork → PR to original | Clone → branch → PR to same repo |

---

## Industry Examples

| Company | Usage |
|---------|-------|
| **Microsoft** | Hosts VS Code, TypeScript, .NET on GitHub — 100M+ developers |
| **GitLab** | Chosen by enterprises needing self-hosted, all-in-one DevOps platform |
| **Atlassian shops** | Bitbucket + Jira = issue-to-PR traceability out of the box |

---

## Interview Questions

**Q: Why would a company choose GitLab over GitHub?**
A: GitLab bundles CI/CD, security scanning, container registry, and issue tracking in one platform. Fewer vendors, self-hostable for compliance, no stitching together separate tools.

**Q: What is a Pull Request and why does every team use them?**
A: A PR is a proposed merge that triggers automated checks and requires peer review before code reaches main. It prevents broken or unreviewed code from reaching production.

**Q: What is branch protection?**
A: Rules enforced at the platform level on protected branches (usually `main`) — require PR approval, passing CI, no direct pushes. Ensures quality gates can't be bypassed.

**Q: What's the difference between a fork and a clone?**
A: Clone copies a repo to your local machine. Fork copies it to your own remote account — used in open source so you can PR back to the original without write access.
