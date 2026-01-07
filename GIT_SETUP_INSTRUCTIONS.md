# Git Repository Setup Instructions
## hr-assistant-without-rai

Follow these steps to create and publish your repository to GitHub.

---

## 📋 Prerequisites

- GitHub account
- Git installed on your machine
- Access to CyberAutomatedSystems GitHub organization

---

## 🚀 Step-by-Step Setup

### Step 1: Create Repository on GitHub

1. **Navigate to CyberAutomatedSystems**
   - Go to https://github.com/CyberAutomatedSystems
   - Click "Repositories" tab
   - Click the green "New" button

2. **Configure Repository**
   - **Repository name**: `hr-assistant-without-rai`
   - **Description**: 
     ```
     HR Knowledge Assistant demo showing problems without Responsible AI controls. Educational demonstration for AEPS Accelerate.
     ```
   - **Visibility**: 
     - Choose "Public" (for partner/customer sharing)
     - OR "Private" (for internal use only)
   - **Initialize**: 
     - ⚠️ DO NOT check "Add a README file"
     - ⚠️ DO NOT check "Add .gitignore"
     - ⚠️ DO NOT check "Choose a license"
     - (We already have these files)

3. **Create Repository**
   - Click "Create repository"
   - You'll see a page with setup instructions - keep this open

---

### Step 2: Prepare Local Repository

1. **Navigate to Your Demo Folder**
   ```bash
   cd /path/to/hr-assistant-without-rai
   ```

2. **Verify Files Present**
   ```bash
   ls -la
   ```
   
   You should see:
   - `README.md`
   - `hr_assistant.py`
   - `deploy-portal.md`
   - `deploy-arm.json`
   - `deploy-arm.md`
   - `requirements.txt`
   - `.gitignore`
   - `LICENSE`

3. **Initialize Git Repository**
   ```bash
   git init
   ```

4. **Add All Files**
   ```bash
   git add .
   ```

5. **Check What's Being Added**
   ```bash
   git status
   ```
   
   Should show all files in green (staged for commit).
   Should NOT show `.env` files (excluded by .gitignore).

6. **Make Initial Commit**
   ```bash
   git commit -m "Initial commit: HR Assistant WITHOUT RAI demo

   - Simple Azure OpenAI implementation without protections
   - Demonstrates bias, vulnerability, and hallucination
   - Portal and ARM deployment guides included
   - Part of AEPS Accelerate partner enablement"
   ```

---

### Step 3: Connect to GitHub

1. **Add Remote Repository**
   
   Replace with your actual repository URL from Step 1:
   
   ```bash
   git remote add origin git@github.com:CyberAutomatedSystems/hr-assistant-without-rai.git
   ```
   
   **OR if using HTTPS:**
   ```bash
   git remote add origin https://github.com/CyberAutomatedSystems/hr-assistant-without-rai.git
   ```

2. **Verify Remote**
   ```bash
   git remote -v
   ```
   
   Should show:
   ```
   origin  git@github.com:CyberAutomatedSystems/hr-assistant-without-rai.git (fetch)
   origin  git@github.com:CyberAutomatedSystems/hr-assistant-without-rai.git (push)
   ```

3. **Rename Branch to Main** (if needed)
   ```bash
   git branch -M main
   ```

4. **Push to GitHub**
   ```bash
   git push -u origin main
   ```

5. **Verify on GitHub**
   - Refresh your repository page
   - You should see all files uploaded
   - README.md should display on the main page

---

### Step 4: Configure Repository Settings (Optional)

1. **Add Topics/Tags** (for discoverability)
   - On your GitHub repo page, click ⚙️ next to "About"
   - Add topics:
     - `azure`
     - `azure-openai`
     - `responsible-ai`
     - `demo`
     - `education`
     - `python`
   - Click "Save changes"

2. **Update Description** (if needed)
   - Same settings area
   - Ensure description is clear

3. **Add Website Link** (optional)
   - Link to companion repo or documentation

---

## ✅ Verification Checklist

After pushing, verify:

- [ ] All files visible on GitHub
- [ ] README.md displays correctly on main page
- [ ] Deployment guides (deploy-portal.md, deploy-arm.md) are accessible
- [ ] ARM template (deploy-arm.json) is present
- [ ] .gitignore is working (no .env files visible)
- [ ] LICENSE file is present
- [ ] Repository description is clear
- [ ] Topics/tags are added (if public)

---

## 🔧 Troubleshooting

### Issue: "Permission denied (publickey)"
**Solution:** Set up SSH keys or use HTTPS
```bash
# Remove SSH remote
git remote remove origin

# Add HTTPS remote
git remote add origin https://github.com/CyberAutomatedSystems/hr-assistant-without-rai.git

# Try push again
git push -u origin main
```

### Issue: "Repository not found"
**Solution:** Verify repository name and organization
```bash
# Check remote URL
git remote -v

# Update if wrong
git remote set-url origin git@github.com:CyberAutomatedSystems/hr-assistant-without-rai.git
```

### Issue: Files not showing up
**Solution:** Check gitignore isn't blocking them
```bash
# See what's being tracked
git ls-files

# Check gitignore rules
git check-ignore -v <filename>
```

---

## 📝 Making Updates

After initial setup, to update the repository:

1. **Make changes to files**
   ```bash
   # Edit your files
   code README.md  # or any editor
   ```

2. **Stage changes**
   ```bash
   git add .
   # or specific files
   git add README.md
   ```

3. **Commit changes**
   ```bash
   git commit -m "Update: Description of what changed"
   ```

4. **Push to GitHub**
   ```bash
   git push
   ```

---

## 🎯 Next Steps

Once repository is live:

1. **Test the Deployment Guides**
   - Follow deploy-portal.md to verify it works
   - Test ARM template deployment
   - Document any issues

2. **Share with Team**
   - Announce in Teams/Slack
   - Share repository link
   - Get feedback

3. **Prepare for Demo**
   - Test all 3 queries
   - Verify output shows problems
   - Ready for presentation

4. **Create WITH RAI Repository**
   - Follow same process for companion repo
   - Link them together in READMEs

---

## 📧 Quick Commands Reference

```bash
# Clone your repo (for others to use)
git clone https://github.com/CyberAutomatedSystems/hr-assistant-without-rai.git

# Check status
git status

# View commit history
git log --oneline

# Create a branch (for development)
git checkout -b feature/new-improvement

# Switch back to main
git checkout main

# Pull latest changes
git pull

# View remote repositories
git remote -v
```

---

## 🎉 Success!

Your repository is now live at:
```
https://github.com/CyberAutomatedSystems/hr-assistant-without-rai
```

Team members and partners can now:
- ✅ Clone the repository
- ✅ Follow deployment guides
- ✅ Run the demo
- ✅ See what problems occur without RAI

---

## 📚 Additional Resources

- [GitHub Docs - Creating a Repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
- [Git Basics](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics)
- [GitHub CLI](https://cli.github.com/) - Alternative to web interface

---

**Ready to proceed with hr-assistant-with-rai next!** 🚀
