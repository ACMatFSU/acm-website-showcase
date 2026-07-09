# ACMatFSU Website

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
<img width="1512" height="860" alt="image" src="https://github.com/user-attachments/assets/efb44dd3-673b-4dc9-a4a9-c488049fcdab" />

### Initiatives Section
<img width="1512" height="701" alt="image" src="https://github.com/user-attachments/assets/3fa4b7b2-16ea-48bf-bf15-9320c26e1945" />

### Leadership Section
<img width="1512" height="845" alt="image" src="https://github.com/user-attachments/assets/1ce3bce0-ffb7-429d-b134-a908d3f4e61d" />

### Projects & Sponsors Sections
<img width="1512" height="689" alt="image" src="https://github.com/user-attachments/assets/f33f4db6-30ef-4317-aba7-f2370ab12a15" />

### Get Involved Section
<img width="1512" height="505" alt="image" src="https://github.com/user-attachments/assets/5280920d-90c6-4795-bebb-1129bd2e8863" />

---

## 🙌 Contributors

| Name | GitHub |
|---|---|
| Carter Rudolph | [@CarterRudolph2005](https://github.com/CarterRudolph2005) |
| Thomas Hall | [@ThomasXiayu](https://github.com/ThomasXiayu) |
| Amy Chen | [@Amy-01999](https://github.com/Amy-01999) |
| Colton Bregoff | [@cbregoffid](https://github.com/cbregoffid) |
| Phuong Nguyen | [@ptn23](https://github.com/ptn23) |
| Bianca Blevins | [@nahible](https://github.com/nahible) |
| Mikhail Sautkin | [@msautkin](https://github.com/msautkin) |
| Sebastian Davalos | [@chumboooo](https://github.com/chumboooo) |
| Alex Smith | [@alexbsmith5](https://github.com/alexbsmith5) |
| Ivan Uribe | [@ivancuribe124](https://github.com/ivancuribe124) |

<!-- Add rows for each contributor. Replace `username` with their actual GitHub handle. -->


## 🚀 Tech Stack

- **[Vite](https://vitejs.dev/)** — Front-end build tool and dev server
- **[Tailwind CSS](https://tailwindcss.com/)** — Utility-first CSS framework for styling
- **GitHub Actions** — CI/CD pipeline for automated builds and deployments
- **JavaScript / HTML / CSS**

## Project Workflow
The site was built using an open-source-style collaborative workflow. Project Lead [Carter Rudolph](https://github.com/CarterRudolph2005) started by breaking the overall design down into discrete, well-scoped components, translating each one into a GitHub issue with deliberate priority and tags populated via the GitHub CLI. From there, the team worked issue-by-issue: developers picked up tagged issues and opened pull requests with their changes, while Project Leads [Carter Rudolph](https://github.com/CarterRudolph2005) and [Thomas Hall](https://github.com/ThomasXiayu) reviewed each PR and merged approved work into the codebase. This kept development organized, made priorities transparent to every contributor, and ensured code quality was maintained through peer review before merging.

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

> `node_modules/` and `dist/` are omitted above.

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
