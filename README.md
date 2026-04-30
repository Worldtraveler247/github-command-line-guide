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

## 4. SSH Workflow — Cloning, Editing, and Pushing
The `gh auth login` flow in Section 2 sets up HTTPS authentication, which is fine for most users. Many professionals prefer **SSH** because it skips browser prompts and works headlessly (CI runners, scripts, remote servers). This section walks through the full end-to-end SSH workflow on a real repo.

### 4.1 Prerequisites — Generate and Register an SSH Key
If you've never set up an SSH key for GitHub, do this once:

```bash
# Generate a modern Ed25519 key
ssh-keygen -t ed25519 -C "your-email@example.com"

# Copy the PUBLIC key to your clipboard (macOS)
cat ~/.ssh/id_ed25519.pub | pbcopy
```

Then in your browser: **GitHub → Settings → SSH and GPG keys → New SSH key**, paste, save.

Verify GitHub recognizes you:

```bash
ssh -T git@github.com
# > Hi <your-username>! You've successfully authenticated, but GitHub does not provide shell access.
```

### 4.2 The Full Workflow (Annotated Shell History)
The `git@github.com:user/repo.git` URL is the SSH form. Compare with the HTTPS form `https://github.com/user/repo.git` — same repo, different protocol. The numbers on the left are line numbers from `history` output, preserved so you can follow along step by step.

```bash
   235  git clone git@github.com:remoteworker365/ansible_tutorial.git
   236  ls
   237  cd ansible_tutorial/
   238  ls
   239  cat README.md
   240  vi README.md
   241  git config --global user.name "remote worker"
   242  git config --global user.email "eddie.camacho@gmail.com
   243  git config --global user.email "eddie.camacho@gmail.com"
   244  cat ~/.gitconfig
   245  ls
   246  vi README.md
   247  cat README.md
   248  git status
   249  git diff README.md
   250  git add README.md
   251  git status
   252  git commit -m "updated readme file, initial commit"
   253  git push origin main
```

### 4.3 What Each Step Does
| Line | Command | Purpose |
|---|---|---|
| **235** | `git clone git@github.com:...` | Clone via SSH — note the `git@github.com:` syntax (no `https://`). |
| **236–239** | `ls`, `cd`, `ls`, `cat README.md` | Inspect what was cloned and read the existing README. |
| **240** | `vi README.md` | Open the README in `vi` to edit. (`i` enters insert mode, `Esc` then `:wq` saves and quits.) |
| **241–243** | `git config --global user.name/email` | Set your global git identity. **Note:** line 242 is missing the closing quote — line 243 is the corrected version. Real shell history, real typo, real fix. |
| **244** | `cat ~/.gitconfig` | Verify the config landed. `~/.gitconfig` is where global git settings live. |
| **245–247** | `ls`, `vi`, `cat` | Re-list, re-edit, re-read to confirm changes. |
| **248** | `git status` | Show what changed since the last commit (working tree vs index vs HEAD). |
| **249** | `git diff README.md` | Show the actual line-by-line edits to README.md. |
| **250** | `git add README.md` | Stage the file for commit. |
| **251** | `git status` | Confirm it moved to the staging area. |
| **252** | `git commit -m "..."` | Create the commit with a message. |
| **253** | `git push origin main` | Push the commit to the `main` branch on the remote named `origin`. |

### 4.4 Common SSH Gotchas
*   **`Permission denied (publickey)`** — Your SSH key isn't loaded into the agent. Run `ssh-add ~/.ssh/id_ed25519`. On macOS, add it permanently with `ssh-add --apple-use-keychain ~/.ssh/id_ed25519`.
*   **Stuck in `vi`?** Press `Esc`, type `:q!` and Enter to quit without saving. `:wq` saves and quits.
*   **Wrong identity in commits?** `git config --get user.email` inside the repo shows what's actually being used. The `--global` setting can be overridden per-repo with `git config user.email "..."` (no `--global`).
*   **Pushing to a different default branch?** Some repos use `master` instead of `main`. Run `git branch --show-current` to check.

## 5. Professional Best Practices
*   **Use a .gitignore**: Always include a `.gitignore` file to prevent system files (`.DS_Store`) or secrets (`.env`) from being uploaded.
*   **Atomic Commits**: Make small, frequent commits that do one thing well.
*   **Descriptive README**: Every repository should have a `README.md` explaining what the project does and how to use it.
*   **SSH vs HTTPS**: HTTPS (Section 2) is easier to set up initially via `gh auth login`. SSH (Section 4) is the preferred workflow for headless environments, automation, and avoiding repeated auth prompts.

---
*Created by Eddie Camacho - 2026*
