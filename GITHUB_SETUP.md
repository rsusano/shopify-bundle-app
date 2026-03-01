# GitHub Repository Setup

## Quick Start: Push to Your GitHub Account

Follow these steps to create a new GitHub repository and push your code:

### Step 1: Create New Repository on GitHub

1. Go to [https://github.com/new](https://github.com/new)
2. Fill in the repository details:
   - **Repository name**: `shopify-bundle-app` (or your preferred name)
   - **Description**: "Production-ready Shopify bundle app with tiered discounts and market-aware pricing"
   - **Visibility**: Choose Private (recommended) or Public
   - **DO NOT** initialize with README, .gitignore, or license (we already have these)
3. Click "Create repository"

### Step 2: Connect Your Local Repository

GitHub will show you commands after creating the repo. Use these commands in your terminal:

```bash
# Remove the old Shopify template remote
git remote remove origin

# Add your new GitHub repository as origin
# Replace YOUR_USERNAME with your GitHub username
# Replace REPO_NAME with your repository name (if different from shopify-bundle-app)
git remote add origin https://github.com/YOUR_USERNAME/REPO_NAME.git

# Verify the remote was added
git remote -v
```

### Step 3: Push Your Code

```bash
# Push to your new repository
# -u sets the upstream tracking
git push -u origin main
```

If you get an authentication prompt:
- Use your GitHub username
- For password, use a **Personal Access Token** (not your GitHub password)
  - Create one at: https://github.com/settings/tokens
  - Select scopes: `repo` (full control of private repositories)

### Step 4: Verify Upload

Go to your GitHub repository URL:
```
https://github.com/YOUR_USERNAME/REPO_NAME
```

You should see:
- ✅ Complete README with features and architecture
- ✅ 67 files with all your bundle app code
- ✅ ARCHITECTURE.md with system diagrams
- ✅ All extensions (bundle-widget, discount-function)
- ✅ Database migrations
- ✅ .env.example (but NOT .env - sensitive data protected)

## Alternative: Using GitHub CLI

If you have GitHub CLI installed:

```bash
# Create repo and push in one command
gh repo create shopify-bundle-app --private --source=. --push
```

## Alternative: Using SSH (Recommended for Frequent Pushers)

If you have SSH keys set up with GitHub:

```bash
# Add remote with SSH URL
git remote add origin git@github.com:YOUR_USERNAME/REPO_NAME.git

# Push
git push -u origin main
```

## Troubleshooting

### "Repository already exists"
If you see this error when pushing:
```bash
# Force push (only if you're sure the remote repo is empty or you want to overwrite)
git push -u origin main --force
```

### Authentication Failed
- GitHub no longer accepts passwords for Git operations
- You must use a Personal Access Token or SSH key
- Generate token: https://github.com/settings/tokens

### "Permission denied"
- Make sure you're logged into the correct GitHub account
- Verify the repository belongs to you or an organization you have access to

## Next Steps After Pushing

1. **Add Repository Description** on GitHub
2. **Add Topics/Tags**: `shopify`, `shopify-app`, `bundles`, `discounts`, `remix`, `rust`
3. **Set up Branch Protection** (Settings → Branches) for main branch
4. **Add Collaborators** if working in a team (Settings → Collaborators)
5. **Configure Secrets** for GitHub Actions (if you plan CI/CD)

## For Client Projects

When forking for a specific client:

```bash
# Clone your main repository
git clone https://github.com/YOUR_USERNAME/shopify-bundle-app.git client-name-bundle-app
cd client-name-bundle-app

# Create new repo on GitHub for the client
# Then update the remote
git remote set-url origin https://github.com/YOUR_USERNAME/client-name-bundle-app.git

# Push to client repository
git push -u origin main
```

## Keeping Your Repository Private

**Important**: This repository contains:
- Your app architecture
- Business logic
- Potentially client-specific customizations

Recommended settings:
- ✅ Keep repository **Private**
- ✅ Use `.gitignore` to exclude `.env` files (already configured)
- ✅ Never commit Shopify API keys or secrets
- ✅ Use environment variables for all sensitive data

---

**Ready to push?** Run the commands in Step 2 and Step 3 above!
