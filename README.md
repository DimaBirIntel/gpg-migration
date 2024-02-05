# Migrating to GPG-Signed Commits

This guide is intended for developers moving to GPG-signed commits to enhance the security and integrity of our codebase. GPG signing allows others to verify that your commits come from a trusted source and have not been tampered with.

## Setting Up GPG

### 1. **Install GPG**

First, ensure you have GPG installed on your system. If not, you can install it using your package manager.

For Ubuntu/Debian-based systems:

```bash
sudo apt-get update
sudo apt-get install gnupg
```

For Red Hat/Fedora systems:

```bash
sudo yum install gnupg
```

For macOS (using [Homebrew](https://brew.sh/)):

```bash
brew install gnupg
```

### 2. **Generate a GPG Key Pair**

Generate a new GPG key pair with the following command:

```bash
gpg --full-generate-key
```

Follow the prompts to select the key type, size, and expiration. Use your Git email address when prompted.

### 3. **List GPG Keys**

After generating your key, list your GPG keys:

```bash
gpg --list-secret-keys --keyid-format=long
```

Look for your key ID, which follows the `/` in the `sec` line (e.g., `3AA5C34371567BD2`).

### 4. **Configure Git to Use Your GPG Key**

Set your GPG key in Git with your key ID:

```bash
git config --global user.signingkey YOUR_KEY_ID
```

### 5. **Sign Commits**

Enable commit signing by default:

```bash
git config --global commit.gpgsign true
```

## Signing Old Commits

To sign old commits, you can use the `git rebase` command. **Note**: This should be done with caution, especially on shared branches, as it will rewrite history.

### 1. **Interactive Rebase**

For a specific number of commits (e.g., the last 5 commits), use:

```bash
git rebase -i HEAD~5 --signoff
```

Replace `5` with the number of commits you wish to update. In the interactive editor, replace `pick` with `edit` for each commit you want to sign or resign.

### 2. **Sign Each Commit**

When git stops for each commit, sign it with:

```bash
git commit --amend --no-edit -S
```

Then, continue the rebase:

```bash
git rebase --continue
```

Repeat for each commit.

### 3. **Force Push** (if necessary)

If you're updating commits already pushed to a remote repository, you'll need to force push. **Be very cautious with this step**, as it can disrupt the history for others working on the project.

```bash
git push --force
```

## Share Your Public Key

Ensure you've added your GPG public key to your GitHub, GitLab, or Bitbucket account to show your commits as verified. Export your GPG key:

```bash
gpg --armor --export YOUR_EMAIL_ADDRESS
```

Copy the output and add it to your account settings on the respective platform.
