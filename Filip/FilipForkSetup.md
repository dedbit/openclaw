# OpenClaw Fork Setup Guide

This guide explains how to set up your own fork of OpenClaw in a new location while maintaining the ability to pull updates from the official repository. Your original project at `d:\dev\12c\OpenClaw` will remain unchanged.

## Current Repository URLs

- **Official OpenClaw Repository**: `https://github.com/openclaw/openclaw.git`
- **Your Fork**: `https://github.com/YOUR_USERNAME/openclaw.git` (replace YOUR_USERNAME)
- **Current Project Location**: `d:\dev\12c\OpenClaw` (official OpenClaw clone)

## Setup Steps

### 1. Fork the Repository on GitHub

1. Go to https://github.com/openclaw/openclaw
2. Click the **Fork** button in the top-right corner
3. Select your GitHub account as the destination
4. Wait for GitHub to create your fork at `https://github.com/YOUR_USERNAME/openclaw`

### 2. Clone Your Fork to a New Location

Choose a new directory for your fork (e.g., `d:\dev\12c\OpenClaw-Fork` or `d:\dev\12c\MyOpenClaw`):

```powershell
# Navigate to your dev directory
cd d:\dev\12c

# Clone YOUR fork
git clone https://github.com/YOUR_USERNAME/openclaw.git OpenClaw-Fork

# Navigate into the new directory
cd OpenClaw-Fork
```

### 3. Add Official OpenClaw as Upstream Remote

```powershell
# Add the official OpenClaw repository as 'upstream'
git remote add upstream https://github.com/openclaw/openclaw.git

# Verify the configuration
git remote -v
```

Expected output:
```
origin    https://github.com/YOUR_USERNAME/openclaw.git (fetch)
origin    https://github.com/YOUR_USERNAME/openclaw.git (push)
upstream  https://github.com/openclaw/openclaw.git (fetch)
upstream  https://github.com/openclaw/openclaw.git (push)
```

### 4. Copy Relevant Configurations from Original Project

Copy your personal configurations from the original project to your new fork:

```powershell
# Copy OpenClaw configuration
Copy-Item -Path "d:\dev\12c\OpenClaw\.openclaw" -Destination "d:\dev\12c\OpenClaw-Fork\" -Recurse -Force

# Copy environment-specific files if you have them
Copy-Item -Path "d:\dev\12c\OpenClaw\openclaw-config-updated.json" -Destination "d:\dev\12c\OpenClaw-Fork\" -ErrorAction SilentlyContinue
Copy-Item -Path "d:\dev\12c\OpenClaw\paired-cli-device.json" -Destination "d:\dev\12c\OpenClaw-Fork\" -ErrorAction SilentlyContinue
Copy-Item -Path "d:\dev\12c\OpenClaw\openclaw.podman.env" -Destination "d:\dev\12c\OpenClaw-Fork\" -ErrorAction SilentlyContinue

# Copy any Docker customizations
Copy-Item -Path "d:\dev\12c\OpenClaw\docker-compose-filip.yml" -Destination "d:\dev\12c\OpenClaw-Fork\" -ErrorAction SilentlyContinue
Copy-Item -Path "d:\dev\12c\OpenClaw\docker-setup-filip.sh" -Destination "d:\dev\12c\OpenClaw-Fork\" -ErrorAction SilentlyContinue

# Copy Filip-specific documentation
Copy-Item -Path "d:\dev\12c\OpenClaw\FilipInstall.md" -Destination "d:\dev\12c\OpenClaw-Fork\" -ErrorAction SilentlyContinue
Copy-Item -Path "d:\dev\12c\OpenClaw\FilipNotes.md" -Destination "d:\dev\12c\OpenClaw-Fork\" -ErrorAction SilentlyContinue
```

**Important Notes:**
- The `.openclaw` directory contains credentials, sessions, and local configuration
- Do NOT commit `.openclaw` to Git (it's already in `.gitignore`)
- Review copied files and ensure no sensitive data is accidentally committed

### 5. Install Dependencies in Your New Fork

```powershell
cd d:\dev\12c\OpenClaw-Fork

# Install dependencies (uses pnpm as per repo guidelines)
pnpm install

# Verify the installation
pnpm openclaw --version
```

## Project Structure

After setup, you'll have:
- **Original Project**: `d:\dev\12c\OpenClaw` - official OpenClaw (read-only, for reference and updates)
- **Your Fork**: `d:\dev\12c\OpenClaw-Fork` - your personal fork (where you work and push changes)

## Daily Workflow

**Always work in your fork directory**: `cd d:\dev\12c\OpenClaw-Fork`

### Pushing Your Changes

Push your changes to **your fork**:

```powershell
cd d:\dev\12c\OpenClaw-Fork

git push origin main
# or
git push origin YOUR_BRANCH_NAME
```

### Pulling Updates from Official OpenClaw

When the official OpenClaw repository has updates you want to pull:

```powershell
cd d:\dev\12c\OpenClaw-Fork

# Fetch the latest changes from official OpenClaw
git fetch upstream

# Merge the official changes into your local main branch
git checkout main
git merge upstream/main

# Or use rebase to keep a cleaner history (recommended)
git checkout main
git rebase upstream/main

# Push the updates to your fork on GitHub
git push origin main
```

### Updating Your Original Project (Optional)

If you want to keep the original project updated as a reference:

```powershell
cd d:\dev\12c\OpenClaw
git pull origin main
```

### Syncing Your Fork via GitHub Web UI (Alternative)

GitHub also provides a convenient web UI option:

1. Go to your fork: `https://github.com/YOUR_USERNAME/openclaw`
2. If your fork is behind, you'll see a message like "This branch is X commits behind openclaw:main"
3. Click **Sync fork** → **Update branch**
4. Then pull the changes locally: `git pull origin main`

## Common Workflows

### Creating a Feature Branch

```powershell
cd d:\dev\12c\OpenClaw-Fork

# Make sure you're on main and it's up to date
git checkout main
git fetch upstream
git merge upstream/main

# Create a new feature branch
git checkout -b feature/my-new-feature

# Work on your changes, then push to your fork
git push origin feature/my-new-feature
```

### Contributing Back to Official OpenClaw

When you want to contribute your changes back to the official repository:

1. Push your feature branch to your fork: `git push origin feature/my-feature`
2. Go to your fork on GitHub: `https://github.com/YOUR_USERNAME/openclaw`
3. Click **Contribute** → **Open pull request**
4. Select your feature branch and create a PR to `openclaw/openclaw:main`
5. Follow the PR template and guidelines in `.github/pull_request_template.md`

### Keeping Your Fork Clean

Periodically sync with upstream to avoid divergence:

```powershell
cd d:\dev\12c\OpenClaw-Fork

# Weekly or before starting new work
git checkout main
git fetch upstream
git rebase upstream/main
git push origin main --force-with-lease
```

### Copying Updated Configs from Original Project

If you've made configuration changes in the original project that you want in your fork:

```powershell
# Copy specific config files
Copy-Item -Path "d:\dev\12c\OpenClaw\.openclaw\config.json" -Destination "d:\dev\12c\OpenClaw-Fork\.openclaw\" -Force

# Or copy the entire .openclaw directory
Copy-Item -Path "d:\dev\12c\OpenClaw\.openclaw" -Destination "d:\dev\12c\OpenClaw-Fork\" -Recurse -Force
```

## Troubleshooting

### If `git remote add upstream` fails with "already exists"

Remove it first, then re-add:
```powershell
cd d:\dev\12c\OpenClaw-Fork
git remote remove upstream
git remote add upstream https://github.com/openclaw/openclaw.git
```

### If you accidentally cloned the wrong repository

Delete the directory and start over:
```powershell
Remove-Item -Path "d:\dev\12c\OpenClaw-Fork" -Recurse -Force
# Then follow step 2 again
```

### Merge conflicts when syncing

If you have local commits that conflict with upstream:
```powershell
cd d:\dev\12c\OpenClaw-Fork
git fetch upstream
git checkout main
git merge upstream/main
# Resolve conflicts in your editor
git add .
git commit
git push origin main
```

### Missing configuration after cloning

If you forgot to copy configs:
```powershell
# Copy the entire .openclaw directory
Copy-Item -Path "d:\dev\12c\OpenClaw\.openclaw" -Destination "d:\dev\12c\OpenClaw-Fork\" -Recurse -Force
```

## Files to Copy vs. Not Copy

### ✅ Safe to Copy (Personal Configurations)
- `.openclaw/` - Your credentials, sessions, and local config
- `openclaw-config-updated.json` - Your custom config overrides
- `paired-cli-device.json` - Device pairing info
- `docker-compose-filip.yml` - Your Docker customizations
- `docker-setup-filip.sh` - Your setup scripts
- `FilipInstall.md`, `FilipNotes.md` - Your personal documentation
- `openclaw.podman.env` - Your environment variables

### ❌ Do NOT Copy (Generated/Build Artifacts)
- `node_modules/` - Will be regenerated by `pnpm install`
- `dist/` - Build output, regenerated on build
- `.git/` - Git history (already cloned from your fork)
- `pnpm-lock.yaml` - May differ; let pnpm manage it
- Any `.log` files or temporary files

### ⚠️ Copy with Caution (May Need Updates)
- Custom scripts in `scripts/` - Review before using
- Modified GitHub workflows in `.github/` - Only if you need custom CI

## Reference Commands

```powershell
# View all remotes
git remote -v

# Fetch from upstream without merging
git fetch upstream

# See what branches exist remotely
git branch -r

# Check your current branch and status
git status

# View commit history
git log --oneline --graph --all --decorate
```

## Notes

- **origin**: Your personal fork (read/write)
- **upstream**: Official OpenClaw repository (read-only for most users)
- Always pull from `upstream` and push to `origin`
- Follow the repository guidelines in `AGENTS.md` and `CONTRIBUTING.md`
- Use the commit script: `scripts/committer "<msg>" <file...>` for proper commit formatting

## Repository Structure Reminder

When working with the codebase:
- Source code: `src/`
- Tests: colocated `*.test.ts`
- Documentation: `docs/`
- Extensions/plugins: `extensions/*`
- Mobile apps: `apps/android/`, `apps/ios/`, `apps/macos/`

## Further Reading

- Official contributing guide: `CONTRIBUTING.md`
- Agent/development guidelines: `AGENTS.md`
- Release process: `docs/reference/RELEASING.md`
- Testing guide: `docs/testing.md`
