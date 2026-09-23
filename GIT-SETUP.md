# Git repository setup

Canonical repository: https://github.com/helmchen81/wag-cw

Recommended remote command:

```bash
git remote add origin https://github.com/helmchen81/wag-cw.git
```

## 1. Prerequisites

Install Git on the workstation or server used to maintain the project:

```bash
git --version
```

Configure the commit identity once:

```bash
git config --global user.name "Sebastian Jeuck"
git config --global user.email "abusegame@kabanet.de"
```

## 2. Initialize the local repository

Run these commands in the project root, not in the public parent directory:

```bash
cd /path/to/dg8wastl-trainer-v3.5.1
git init -b main
git status
git add .
git status
git commit -m "Initial import v3.5.1"
git tag -a v3.5.1 -m "DG8Wa(stl's) WAG/CWA cw test trainer v3.5.1"
```

Before committing, verify that these sensitive files do not appear in `git status`:

```text
admin/data/admin-auth.json
admin/data/admin-audit.log
api/data/leaderboard.sqlite
admin/data/backups/
admin/data/uploads/
```

## 3. Create a private remote repository

Create an empty private repository in GitHub, GitLab, Azure DevOps or your self-hosted Git service. Do not add a README or `.gitignore` on the remote because this project already contains both.

Add the remote URL supplied by the provider:

```bash
git remote add origin REMOTE-URL
```

Examples:

```bash
git remote add origin git@github.com:USERNAME/dg8wastl-trainer.git
git remote add origin https://github.com/USERNAME/dg8wastl-trainer.git
```

Verify it:

```bash
git remote -v
```

## 4. Push the first version

```bash
git push -u origin main
git push origin v3.5.1
```

After the upstream is configured, later pushes normally require only:

```bash
git push
git push --tags
```

## 5. Daily workflow

Create a branch for each change:

```bash
git switch -c feature/short-description
```

After editing and testing:

```bash
git status
git diff
git add .
git commit -m "Describe the change"
git push -u origin feature/short-description
```

Merge the reviewed branch into `main`, then tag a release:

```bash
git switch main
git pull --ff-only
git merge --no-ff feature/short-description
git tag -a v3.5.2 -m "Release v3.5.2"
git push origin main
git push origin v3.5.2
```

## 6. Server deployment recommendation

Do not make the public webroot your only Git working copy. Prefer this structure:

```text
/opt/dg8wastl-trainer-repo/     private Git working copy
/var/www/dg8wastl-trainer/      deployed web application
/var/backups/dg8wastl-trainer/  release and database backups
```

Keep runtime data on deployment:

```text
api/data/leaderboard.sqlite
admin/data/admin-auth.json
admin/data/admin-audit.log
admin/data/backups/
```

Do not overwrite these paths during deployment.

## 7. Portable repository backup

Create a Git bundle containing all branches and tags:

```bash
git bundle create dg8wastl-trainer-v3.5.1.bundle --all
```

Restore it later with:

```bash
git clone dg8wastl-trainer-v3.5.1.bundle dg8wastl-trainer-restored
```