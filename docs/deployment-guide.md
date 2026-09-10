# GitHub Pages Hosting & Deployment Guide

This guide provides step-by-step instructions for hosting and deploying this **MkDocs WebHelp Portal** on **GitHub Pages**.

---

## Deployment Architecture

The documentation site is built using **MkDocs** with the **Material for MkDocs** theme. Deployment can be executed in two ways:

1. **Automated Continuous Integration (CI/CD)** via **GitHub Actions** *(Recommended for production)*.
2. **Manual Command Line Deployment** using `mkdocs gh-deploy`.

---

## Method 1: Automated GitHub Actions Workflow (Recommended)

An automated GitHub Actions workflow file is included in `.github/workflows/deploy.yml`. Whenever changes are pushed to the `main` branch, GitHub Actions will build the static HTML site and publish it to the `gh-pages` branch automatically.

### Setup Instructions

1. **Push Repository to GitHub**:
    Ensure your local workspace is initialized as a git repository and pushed to your GitHub account:
    ```bash
    git init
    git add .
    git commit -m "Initial commit: Liminal Technical Writing Submission"
    git branch -M main
    git remote add origin https://github.com/YOUR_GITHUB_USERNAME/Assignment_LM.git
    git push -u origin main
    ```

2. **Configure GitHub Repository Settings**:
    - Go to your repository on GitHub: `https://github.com/Rajwar88/Assign`.
    - Click **Settings** > **Pages** (under Code and automation).
    - Under **Build and deployment**:
        - **Source**: Select `Deploy from a branch` (or `GitHub Actions`).
        - **Branch**: Select `gh-pages` branch, `/ (root)` folder.
    - Click **Save**.

3. **Verify Deployment**:
    Once the GitHub Action completes, your site will be live at:
    `https://rajwar88.github.io/Assign/`

---

## Method 2: Manual CLI Deployment

If you prefer to publish directly from your local terminal using Python and MkDocs:

### Step 1: Install Dependencies
Ensure Python 3 and MkDocs Material are installed:
```bash
pip install mkdocs mkdocs-material
```

### Step 2: Test Local Preview
Run the local dev server to verify all pages render cleanly:
```bash
mkdocs serve
```
Open your browser to `http://127.0.0.1:8000` to preview the site.

### Step 3: Publish to GitHub Pages
Run the `gh-deploy` command:
```bash
mkdocs gh-deploy --clean
```
This command automatically builds the static site into `site/`, commits it to the `gh-pages` branch, and pushes it to GitHub.

---

## Local Build Verification

To verify that the site builds with zero broken links or missing assets, execute:
```bash
mkdocs build --strict
```
If the command finishes with exit code `0`, your site is 100% compliant and ready for production hosting!
