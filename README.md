# GitHub Command Line Mastery Guide

A comprehensive guide to configuring Git, authenticating via the GitHub CLI, and managing repositories like a professional.

## 1. Initial Git Configuration
Before you start committing, set your global identity. This information is attached to every commit you make.

```bash
# Set your name and email
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"

# Set the default branch name to 'main'
git config --global init.defaultBranch main
```

## 2. Authentication (GitHub CLI)
The GitHub CLI (`gh`) is the modern standard for interacting with GitHub. It replaces the need for manual Personal Access Tokens (PATs).

### Installation via macOS CLI (Homebrew)
Homebrew is the standard package manager for macOS. Use it to install the GitHub CLI:

```bash
# Update Homebrew to get the latest packages
brew update

# Install the GitHub CLI
brew install gh

# Verify the installation
gh --version
```

### The Authentication Process
Once installed, you must link your local CLI to your GitHub account:

```bash
gh auth login
```

**Follow the interactive prompts:**
1. **What account do you want to log into?** `GitHub.com`
2. **What is your preferred protocol for Git operations?** `HTTPS`
3. **Authenticate Git with your GitHub credentials?** `Yes`
4. **How would you like to authenticate GitHub CLI?** `Login with a web browser`

**Finalizing the Login:**
- The CLI will generate a **one-time, 8-character code** (e.g., `ABCD-1234`).
- Press **Enter** to open the GitHub device activation page in your default browser.
- Paste the code into the browser and click **Authorize**.
- Return to your terminal—you should see `✓ Logged in as <your-username>`.

## 3. Creating and Pushing a Repository
To take a local project and put it on GitHub:

```bash
# 1. Initialize the local folder
git init

# 2. Stage your files
git add .

# 3. Create your first commit
git commit -m "Initial commit"

# 4. Create the repo on GitHub and push (using GitHub CLI)
gh repo create my-repo-name --public --source=. --push
```

## 4. Professional Best Practices
*   **Use a .gitignore**: Always include a `.gitignore` file to prevent system files (`.DS_Store`) or secrets (`.env`) from being uploaded.
*   **Atomic Commits**: Make small, frequent commits that do one thing well.
*   **Descriptive README**: Every repository should have a `README.md` explaining what the project does and how to use it.
*   **SSH vs HTTPS**: While HTTPS is easier to set up initially with `gh auth login`, professional workflows often use SSH keys for passwordless interaction.

---
*Created by Eddie Camacho - 2026*
