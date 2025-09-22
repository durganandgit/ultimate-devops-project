# DevOps Project Progress Log

This document tracks the steps I followed while implementing the [Ultimate DevOps Project](https://github.com/iam-veeramalla/ultimate-devops-project-demo).

---

## Step 1: Repository Setup
- Cloned original repo and removed history.
- Created my own repo: [durganandgit/ultimate-devops-project](https://github.com/durganandgit/ultimate-devops-project).
- Pushed initial commit with clean history.

```bash
git clone https://github.com/iam-veeramalla/ultimate-devops-project-demo.git
cd ultimate-devops-project-demo
git remote remove origin
git remote add origin https://github.com/durganandgit/ultimate-devops-project.git

# Reset history
rm -rf .git
git init
git branch -M main
git remote add origin https://github.com/durganandgit/ultimate-devops-project.git
git add .
git commit -m "Initial commit - starting my DevOps project"
git push -u origin main --force

