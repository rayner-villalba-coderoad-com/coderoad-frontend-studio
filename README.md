# CodeRoadFrontend and Mobile Engineering Hub

A centralized knowledge base for **CodeRoad's** Frontend and Mobile engineering teams. Built with [Hugo](https://gohugo.io/) and the [Blowfish](https://blowfish.page/) theme, it provides structured job descriptions, learning resources, and career paths for engineering roles across Frontend, iOS, and Mobile disciplines.

## 📋 Content Overview

| Section | Description |
|---|---|
| **Frontend Engineering** | Job descriptions for Frontend roles (L1–L6): Junior → Director |
| **iOS Engineering** | Job descriptions for iOS roles (L1–L6): Junior → Director |
| **Mobile Engineering** | Job descriptions for Mobile roles (L1–L6): Junior → Director (React Native & Flutter) |
| **Learning Resources** | Curated roadmaps, tutorials, books, courses, and community links |

All content is organized using Hugo's **taxonomy system** — browse by [Categories](categories/) (Frontend / iOS / Mobile Engineering) or filter by [Tags](tags/) (e.g., `React`, `Swift`, `Flutter`, `Senior`, `L3`).

## 🛠️ Tech Stack

- **Static Site Generator:** [Hugo](https://gohugo.io/) (v0.156.0 extended)
- **Theme:** [Blowfish](https://blowfish.page/) (git submodule)
- **Hosting:** AWS S3 (static website hosting)
- **CI/CD:** GitHub Actions (auto-deploy on push to `main`)

## 🚀 Getting Started

### Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) v0.156.0+
- [Git](https://git-scm.com/)

### Local Development

```bash
# Clone the repository (with theme submodule)
git clone --recurse-submodules <your-repo-url>
cd frontend-engineering-hub

# Start the development server
hugo server -D

# Open http://localhost:1313 in your browser
```

### Build for Production

```bash
hugo --minify
```

The static output will be generated in the `public/` directory.

## 📁 Project Structure

```
frontend-engineering-hub/
├── .github/workflows/
│   └── deploy.yml              # CI/CD pipeline (Hugo build → S3 deploy)
├── archetypes/                 # Hugo content templates
├── assets/
│   └── css/
│       └── custom.css          # Custom CSS overrides
├── config/_default/
│   ├── hugo.toml               # Core Hugo settings & taxonomies
│   ├── languages.en.toml       # Language & author config
│   ├── menus.en.toml           # Navigation menu structure
│   ├── params.toml             # Blowfish theme parameters
│   ├── markup.toml             # Markdown rendering settings
│   └── module.toml             # Hugo module config
├── content/
│   ├── _index.md               # Homepage
│   ├── fe_job_description/     # Frontend Engineering JDs (L1–L6)
│   ├── ios_job_description/    # iOS Engineering JDs (L1–L6)
│   ├── mobile_job_descriptions/# Mobile Engineering JDs (L1–L6)
│   └── learning_resources/     # Curated learning materials
├── static/                     # Static assets
├── themes/blowfish/            # Blowfish theme (git submodule)
└── hugo.toml                   # Root Hugo config
```

## ✍️ Adding Content

### New Job Description

Create a new markdown file in the appropriate section directory with Hugo frontmatter:

```markdown
---
title: "Role Title (Level X)"
date: 2026-03-09
categories: ["Frontend Engineering"]
tags: ["Senior", "L3", "React", "TypeScript"]
summary: "One-line description of the role."
weight: 3
---

# Role Title (Level X)

Your content here...
```

- **`categories`** — One of: `Frontend Engineering`, `iOS Engineering`, `Mobile Engineering`
- **`tags`** — Seniority level + tech stack keywords
- **`weight`** — Controls ordering within section lists (L1=1, L2=2, etc.)

### New Learning Resource

Add content to `content/learning_resources/index.md` following the existing section structure.

## 🚢 CI/CD & Deployment

The project uses **GitHub Actions** to deploy automatically to **AWS S3** on every push to `main`.

### Pipeline Overview

```
Push to main → Checkout (with submodules) → Install Hugo → Build → Deploy to S3
```

### Required GitHub Variables

Configure these in **Settings → Variables → Actions**:

| Variable | Description |
|---|---|
| `AWS_ACCOUNT_ID` | AWS Account ID for OIDC-based IAM role |
| `S3_BUCKET_NAME` | Target S3 bucket name |

### AWS Setup

1. **S3 Bucket** — Create a bucket with static website hosting enabled
2. **IAM Role** — Create a role named `githubconnect` with:
   - Trust policy for GitHub OIDC provider
   - `s3:PutObject`, `s3:DeleteObject`, `s3:ListBucket` permissions on the bucket
3. **Optional: CloudFront** — Uncomment the CloudFront invalidation step in `deploy.yml` and add a `CLOUDFRONT_DISTRIBUTION_ID` variable

### Manual Deploy

```bash
# Build locally
hugo --minify

# Sync to S3
aws s3 sync public/ s3://YOUR_BUCKET_NAME --delete --cache-control "max-age=86400"
```

## ⚙️ Configuration

Key configuration files and what they control:

| File | Purpose |
|---|---|
| `config/_default/hugo.toml` | Taxonomies (categories, tags, series), build settings |
| `config/_default/params.toml` | Theme options: layout, TOC, breadcrumbs, taxonomy display |
| `config/_default/menus.en.toml` | Navigation menu structure |
| `config/_default/languages.en.toml` | Site title, logo, author info |
| `assets/css/custom.css` | CSS overrides (logo sizing, etc.) |

## 📄 License

This project is private to CodeRoad.
