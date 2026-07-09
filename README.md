# ACM at FSU — Website

This is a showcase repository for the official website of the **Association for Computing Machinery (ACM) chapter at Florida State University**, live at [fsu.acm.org](https://fsu.acm.org).

> **Note:** This repo mirrors the codebase for portfolio/showcase purposes. The active development repository is kept private, and changes are deployed automatically via CI/CD.

<!-- ============================================= -->
<!-- SCREENSHOTS -->
<!-- ============================================= -->

## 📸 Screenshots

<!--
Add screenshots below by replacing the placeholder image links.
Suggested: store images in a `screenshots/` folder in this repo, then reference them like:
![Home Page](./screenshots/home.png)
-->

### Hero Section


### Initiatives Section


### Leadership Section


### Projects & Sponsors Sections


### Get Involved Section


---

## 🚀 Tech Stack

- **[Vite](https://vitejs.dev/)** — Front-end build tool and dev server
- **[Tailwind CSS](https://tailwindcss.com/)** — Utility-first CSS framework for styling
- **GitHub Actions** — CI/CD pipeline for automated builds and deployments
- **JavaScript / HTML / CSS**

## 🏗️ Project Structure

```
.
├── .github/
│   └── workflows/
│       └── deploy.yml          # CI/CD workflow — builds & deploys on push to `prod`
├── src/
│   ├── assets/
│   │   ├── acmLogo.png
│   │   ├── data/
│   │   │   └── config.ts       # Site content/config (site copy, links, etc.)
│   │   ├── headshots/          # Officer/member headshots
│   │   └── projects/           # Project showcase images
│   ├── code_archive/
│   ├── components/
│   │   ├── CTABar.tsx
│   │   ├── FloatingIsland.tsx
│   │   ├── FooterSection.tsx
│   │   ├── HeroHeadline.tsx
│   │   ├── HeroSection.tsx
│   │   ├── HeroSubcopy.tsx
│   │   ├── JoinSection.tsx
│   │   ├── LeadershipTree.tsx
│   │   ├── PillarCard.tsx
│   │   ├── PillarsSection.tsx
│   │   ├── ProjectCard.tsx
│   │   ├── ProjectsSection.tsx
│   │   ├── SectionHeader.tsx
│   │   ├── SponsorSection.tsx
│   │   ├── Stampbadge.tsx
│   │   ├── TechTag.tsx
│   │   ├── ThemeShowcase.tsx
│   │   └── SponsorRow.tsx
│   ├── App.tsx
│   ├── index.css
│   ├── main.tsx
│   └── vite-env.d.ts
├── index.html
├── tailwind.config.ts
├── vite.config.ts
├── postcss.config.js
├── tsconfig.json
├── package.json
├── CODEOWNERS
└── README.md
```

> `node_modules/` and `dist/` are omitted above (installed/generated locally — see [Getting Started](#️-getting-started)).

## 🔄 CI/CD Pipeline

This project uses **GitHub Actions** to automate deployment to cPanel hosting on every push to the `prod` branch:

1. Code is pushed/merged to `prod`
2. GitHub Actions workflow triggers automatically
3. Node.js 20 is set up and dependencies are installed with `npm ci`
4. The Vite production build runs (`npm run build`), generating the `dist/` folder
5. The contents of `dist/` are deployed to the server's `public_html/` directory via secure FTP, using [`SamKirkland/FTP-Deploy-Action`](https://github.com/SamKirkland/FTP-Deploy-Action)
6. Sensitive/system paths (`.htaccess`, `.well-known/`, `.tmb/`) are excluded from the sync so backend routes and cPanel system folders stay untouched

Workflow file: `.github/workflows/deploy.yml`

```yaml
name: Deploy React App to cPanel

on:
  push:
    branches:
      - prod

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Check out the repository code
      - name: Checkout Repository
        uses: actions/checkout@v4

      # Step 2: Set up Node.js environment
      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      # Step 3: Install dependencies and build production assets
      - name: Install Dependencies & Build
        run: |
          npm ci
          npm run build

      # Step 4: Deploy to A2 Hosting via Secure FTP
      - name: Deploy Assets via SFTP
        uses: SamKirkland/FTP-Deploy-Action@v4.3.5
        with:
          server: ${{ secrets.DEPLOY_HOST }}
          username: ${{ secrets.DEPLOY_USER }}
          password: ${{ secrets.DEPLOY_FTP_PASSWORD }}
          port: 21 # Connects through standard cPanel FTP channels
          local-dir: ./dist/
          server-dir: ./public_html/
          
          exclude: |
            **/.well-known/**
            **/.tmb/**
            **/.htaccess
```

**Required repository secrets:**

| Secret | Description |
|---|---|
| `DEPLOY_HOST` | FTP host for the cPanel/A2 Hosting server |
| `DEPLOY_USER` | FTP username |
| `DEPLOY_FTP_PASSWORD` | FTP password |

## 🌐 Live Site

Visit the live website: **[fsu.acm.org](https://fsu.acm.org)**

## 👥 About ACM at FSU

As the largest student-run computing organization at FSU, ACM aims to bridge the gap between students and the tech industry through weekly technical interview practice sessions, networking events, and club-wide projects.

<!--## 📄 License

 Add license information here, e.g. MIT License -->

## 📬 Contact Us

For questions, reach out via the [ACM at FSU website](https://fsu.acm.org) or this [contact link](mailto:contact@fsu.acm.org).
