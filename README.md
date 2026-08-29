<h1 style="font-size: 40px;">CMS - 100xDevs</h1>

Open source repo for app.100xdevs.com

## Recommendation develop tool

| Area | Recommendation | Why it fits this repo |
| --- | --- | --- |
| Runtime / local services | Docker or Docker Desktop | This repo includes Docker setup for PostgreSQL and app services via `docker-compose.yml` and `setup.sh`. |
| IDE | VS Code | Best fit for a Next.js + TypeScript + Prisma + Tailwind project with Git integration and terminal support. |
| AI coding support | GitHub Copilot and similar AI tools | Helpful for code understanding, refactoring, repetitive task generation, and PR review assistance. |
| Version control | Git + GitHub | Required for fork workflow, PRs, branch protection, and team collaboration. |
| Package manager | pnpm | This project explicitly uses pnpm in `package.json` and setup instructions. |
| Database tooling | PostgreSQL client / DB UI | Useful for inspecting local data during Prisma and app development. |
| Terminal / debugging | zsh or VS Code terminal + browser dev tools | Needed for running dev scripts, logs, and frontend debugging. |

Suggested stack: Docker + VS Code + GitHub Copilot + pnpm + GitHub workflow.

## Running Locally

> [!NOTE]  
> This project uses [pnpm](https://pnpm.io/) only as a package manager.

1. Clone the repository:

```bash
git clone https://github.com/code100x/cms.git
```

2. Navigate to the project directory:

```bash
cd cms
```
# Instant Docker Setup

> [!NOTE]  
> Your Docker Demon should be online

1. Running Script for Instant setup

```
# Gives permission to execute a setup file
chmod +x setup.sh

# Runs the setup script file
./setup.sh
```

# Traditional Docker Setup

(Optional) Start a PostgreSQL database using Docker:

```bash
docker run -d \
--name cms-db \
-e POSTGRES_USER=myuser \
-e POSTGRES_PASSWORD=mypassword \
-e POSTGRES_DB=mydatabase \
-p 5432:5432 \
postgres
``` 



1. Create a .env file:

   - Copy `.env.example` and rename it to `.env`.


2. Install dependencies:

```bash
pnpm install
```

3. Run database migrations:

```bash
pnpm prisma:migrate
```

4. Generate prisma client

```bash
pnpm prisma generate
```

5. Seed the database:

```bash
pnpm db:seed
```

6. Start the development server:

```bash
pnpm dev
```

## Usage

1. Access the application in your browser:

```bash
http://localhost:3000
```

2. Login using any of the following provided user credentials:

- Email: `testuser@example.com`, Password: `123456`

- Email: `testuser2@example.com`, Password: `123456`

## Contributing

This repository is configured as a fork workflow for a small team working from different machines. The official source repository remains the upstream project, while this fork is the working repo for the team.

### Repository roles

- `upstream` = source repository: `https://github.com/code100x/cms.git`
- `origin` = this fork repo
- `develop` = shared integration branch for pull requests
- `feature/developer-chen` = branch for `vaychen`
- `feature/developer-dai` = branch for `daiwei2026`

> This documentation describes the project-level contribution model for the repository admin and team members. Certain repository maintenance actions are limited to the project admin.

### Recommended branch strategy

Use this flow in the current setup:

1. Keep `main` synced with upstream
2. Create a `develop` branch from the latest `main`
3. Each developer creates a feature branch from `develop`
4. Open pull requests to `develop`, not directly to `main`
5. Periodically sync `main` into `develop`
6. Merge `develop` into `main` only when the shared branch is stable and ready

This keeps the project stable while still allowing multiple developers to work in parallel.

### Admin-only repository operations

The following actions are reserved for the project admin and should not be performed by ordinary contributors such as `daiwei2026`:

- update or push directly to `develop`
- sync `main` from `upstream/main`
- merge upstream changes into the fork `main`
- manage protected branch rules and repo permissions
- perform other repo maintenance actions that affect repository-level governance

For normal development work, contributors should use feature branches and pull requests only.

### Setup for this project

#### 1) Add upstream and sync the fork

```bash
git remote add upstream https://github.com/code100x/cms.git
git fetch upstream

git checkout main
git pull --ff-only upstream/main
git push origin main
```

#### 2) Create the shared base branch

```bash
git checkout -b develop
git push -u origin develop
```

#### 3) Each developer creates their own feature branch

For `vaychen`:

```bash
git checkout develop
git pull origin develop
git checkout -b feature/developer-chen
git push -u origin feature/developer-chen
```

For `daiwei2026`:

```bash
git checkout develop
git pull origin develop
git checkout -b feature/developer-dai
git push -u origin feature/developer-dai
```

When working on different machines (Mac, Windows, Linux), each developer should keep their local branch updated from the shared remote branch:

```bash
git fetch origin
git checkout develop
git pull origin develop
```

### Pull request workflow

- Developer creates a feature branch from `develop`
- Developer pushes the branch to `origin`
- Developer opens a PR to `develop`
- Reviewer approves the PR
- Merge into `develop`

This means the shared review branch is always the base, and `main` stays clean and mostly synchronized with upstream.

### Updating from upstream main

When the upstream project updates, pull the latest changes into your fork main and then bring them into `develop`:

```bash
git fetch upstream
git checkout main
git merge --ff-only upstream/main
git push origin main

git checkout develop
git merge origin/main
git push origin develop
```

This keeps the shared branch aligned with the latest source project updates without losing work in feature branches.

### Branch protection recommendation

To keep the PR flow reliable, protect the shared branch in GitHub. In this project, the branch protection rule name is set as:

- `develop-branch-pr-requirements`

This rule is intended for the repository admin to enforce the shared review process for `develop`.

The expected configuration for this rule is:

- require a pull request before merging
- require at least 1 approval
- dismiss stale approvals on new commits
- require status checks to pass before merging
- require branch to be up to date before merging
- restrict direct pushes to protected branches

This ensures that `develop` is only updated through reviewed pull requests and that maintenance actions remain controlled by the admin.

### Notes for contributors

- Do not push directly to `main` unless you are intentionally syncing from upstream and you are the designated admin
- Do not open PRs directly to `main` for normal feature work
- Feature branches should be short-lived and merged back into `develop`
- Keep `develop` fresh by merging `main` regularly under admin control
- Ordinary contributors such as `daiwei2026` should not perform admin-level repository maintenance actions

> Suggested development setup: use VS Code as the shared IDE for this project so both developers can work in the same editor environment, keep consistent project settings, and use the Git integration cleanly for branch management and PR review.

