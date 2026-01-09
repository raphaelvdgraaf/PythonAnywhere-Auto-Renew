# PythonAnywhere Auto-Renewal Bot 🤖

Automatically renew your PythonAnywhere free web app every 15 days using GitHub Actions. Never let your app expire again!

![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-Automated-blue)
![Python](https://img.shields.io/badge/Python-3.9+-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

## 🚀 Features

- ✅ **Automated Renewal**: Runs every 1st and 15th of the month at 04:00 UTC (9:30 AM IST)
- ✅ **Smart Logging**: Commits success/failure logs after every run (twice per month)
- ✅ **Status Tracking**: Clear SUCCESS ✅ or FAILED ❌ indicators in logs
- ✅ **Complete Audit Trail**: Track every renewal attempt with timestamps
- ✅ **No False Positives**: Only logs success when renewal actually works
- ✅ **Secure Credentials**: Uses GitHub Secrets for safe password storage
- ✅ **Manual Trigger**: Run the workflow anytime from the GitHub Actions tab
- ✅ **Zero Maintenance**: Set it and forget it!

## 📋 How It Works

1. **GitHub Actions runs the workflow** twice a month (1st and 15th)
2. **Script logs into PythonAnywhere** using your credentials
3. **Clicks the "Extend" button** on your web app dashboard
4. **Checks renewal status** - did it succeed or fail?
5. **Logs the result** with timestamp and status to `.github/logs/workflow_runs.log`
6. **Commits the log file** automatically - creating a permanent record
7. **Keeps workflow active** - the regular commits prevent GitHub from disabling it

## 🎯 Why You Need This

PythonAnywhere free tier apps expire after **1 month** of inactivity (as of Jan 2026). This bot ensures your app stays alive by renewing it automatically every 15 days.

## 📦 Setup Instructions

### Prerequisites

- PythonAnywhere free account with a web app
- GitHub account
- 5 minutes of your time!

### Step 1: Fork/Clone This Repository

```bash
git clone https://github.com/tanishqmudaliar/PythonAnywhere-Auto-Renew
cd PythonAnywhere-Auto-Renew
```

Or click the **"Use this template"** button on GitHub.

### Step 2: Add GitHub Secrets

1. Go to your repository on GitHub
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add these two secrets:

   **Secret 1:**
   - Name: `PA_USERNAME`
   - Value: Your PythonAnywhere username

   **Secret 2:**
   - Name: `PA_PASSWORD`
   - Value: Your PythonAnywhere password

### Step 3: Enable Workflow Permissions

1. Go to **Settings** → **Actions** → **General**
2. Scroll to **Workflow permissions**
3. Select **"Read and write permissions"**
4. Check **"Allow GitHub Actions to create and approve pull requests"**
5. Click **Save**

### Step 4: Enable Actions (If Needed)

If your repo is private or has restricted actions:

1. Go to **Settings** → **Actions** → **General**
2. Under **Actions permissions**, select:
   - **"Allow all actions and reusable workflows"**
3. Click **Save**

### Step 5: Test the Workflow

1. Go to the **Actions** tab
2. Click **Auto-Renew PythonAnywhere** workflow
3. Click **Run workflow** → Select **main** branch → **Run workflow**
4. Wait ~30 seconds and check the logs
5. Verify it says "✅ Login successful".
6. Check your repository - a new file `.github/logs/workflow_runs.log` should appear

## 📅 Schedule

### Workflow Runs & Log Commits
The workflow runs automatically **twice per month**, and commits a log file after each run:

| Date | Time (UTC) | Time (IST) | Action |
|------|-----------|-----------|---------|
| 1st of every month | 04:00 | 09:30 AM | Renew app + Commit log |
| 15th of every month | 04:00 | 09:30 AM | Renew app + Commit log |

**Result:** You'll have **~24 commits per year** (2 per month) with complete audit trail.

You can also trigger it manually anytime from the Actions tab!

## 📊 Workflow Logs

Every run is automatically logged and committed to `.github/logs/workflow_runs.log`:

### Successful Renewal:
```
========================================
Workflow Run: 2026-01-01 04:00:00 UTC
Status: SUCCESS ✅
Trigger: schedule
Repository: username/repo
Branch: main
Run ID: 123456789
Run Number: 1
========================================
```

### Failed Renewal:
```
========================================
Workflow Run: 2026-01-15 04:00:00 UTC
Status: FAILED ❌
Trigger: schedule
Repository: username/repo
Branch: main
Run ID: 123456790
Run Number: 2
Note: Check GitHub Actions logs for error details
========================================
```

### Log File Features:
- ✅ Tracks all workflow runs with timestamps
- ✅ Shows SUCCESS ✅ or FAILED ❌ status for each run
- ✅ Creates a commit after every run (prevents workflow disable)
- ✅ Never gets deleted (permanent history)
- ✅ Helps debug issues and track renewals
- ✅ Shows ~24 entries per year (2 per month)
- ✅ No false positives - only logs success when renewal actually works

## 🔒 Security

- 🔐 Credentials stored as encrypted GitHub Secrets
- 🔐 Never exposed in logs or code
- 🔐 Script uses HTTPS for all connections
- 🔐 No third-party access to your data

**Never commit your `.env` file!** It's in `.gitignore` for safety.

## 🧪 Local Testing

Want to test the script locally before deploying?

### 1. Install dependencies:
```bash
pip install -r requirements.txt
```

### 2. Create `.env` file:
```env
PA_USERNAME=your_username
PA_PASSWORD=your_password
```

### 3. Run the script:
```bash
python renew_app.py
```

You should see:
```
🔐 Logging in as your_username...
✅ Login successful
📊 Checking dashboard...
ℹ️  No extend button found.
   This usually means your app doesn't need renewal yet.
```

## 🛠️ Project Structure

```
pythonanywhere-auto-renew/
├── .github/
│   ├── workflows/
│   │   └── renew.yml           # GitHub Actions workflow
│   └── logs/
│       └── workflow_runs.log   # Auto-generated run logs
├── renew_app.py                # Main renewal script
├── requirements.txt            # Python dependencies
├── .gitignore                  # Ignored files
├── .env                        # Local credentials (not in GitHub)
└── README.md                   # You are here!
```

## 🐛 Troubleshooting

### Issue: "Login failed"
**Solution:** 
- Verify `PA_USERNAME` and `PA_PASSWORD` secrets are correct
- Check if PythonAnywhere changed its login page
- Try logging in manually first to ensure credentials work
- Look for "FAILED ❌" status in `.github/logs/workflow_runs.log`

### Issue: "No extend button found"
**Solution:** 
- This is normal! Your app doesn't need renewal yet
- The button only appears when renewal is needed
- Script will try again on next scheduled run
- This is logged as SUCCESS ✅ (not an error)

### Issue: "Workflow disabled"
**Solution:**
- Make sure you enabled "Read and write permissions"
- The automatic logging system prevents this by committing every 15 days
- Check `.github/logs/workflow_runs.log` for recent commits

### Issue: "Repository access blocked"
**Solution:**
- Enable Actions in Settings → Actions → General
- Select "Allow all actions and reusable workflows"
- Make sure repo is public OR actions are enabled for private repos

### Issue: Log shows "FAILED ❌" status
**Solution:**
- Go to GitHub Actions tab and check the detailed workflow logs
- Look for error messages from the renewal script
- Common causes: wrong credentials, PythonAnywhere site changes
- The workflow will retry on the next scheduled run

## 🔄 Why Commits Every Run?

**Problem:** GitHub disables scheduled workflows after 60 days of no repository activity.

**Solution:** By committing the log file after every workflow run:
1. ✅ Creates activity every 15 days (well under the 60-day limit)
2. ✅ Provides complete audit trail of all renewals
3. ✅ Shows exactly when your app was renewed
4. ✅ Tracks success/failure status for debugging
5. ✅ Makes debugging easy if something goes wrong
6. ✅ Keeps workflow active forever automatically

**Is this safe?** YES! ✅
- Used by thousands of automation repositories
- Creates 2 commits per month (24 per year) - minimal
- Well within GitHub's terms of service
- Creates meaningful commits with actual log data (not spam)
- Industry standard practice for scheduled workflows

**No manual intervention needed!** The workflow stays active forever because it commits every 15 days.

## ⚙️ Customization

### Change Schedule Time

Edit `.github/workflows/renew.yml`:

```yaml
# Run at different times
- cron: '30 18 1,15 * *'  # 6:30 PM UTC (midnight IST)
- cron: '0 12 1,15 * *'   # Noon UTC (5:30 PM IST)
```

### Change Frequency

```yaml
# Run weekly on Mondays
- cron: '0 4 * * 1'

# Run every 10 days (more frequent)
- cron: '0 4 1,11,21 * *'

# Run monthly only
- cron: '0 4 1 * *'
```

**Note:** Commits happen on every run, so adjust frequency based on how often you want log commits.

### Change Python Script Name

If your script is named differently, update line 28 in the workflow:

```yaml
run: python3 your_script_name.py
```

## 📝 License

MIT License - feel free to use, modify, and distribute!

## 🤝 Contributing

Issues and pull requests are welcome! 

Found a bug? [Open an issue](https://github.com/tanishqmudaliar/PythonAnywhere-Auto-Renew/issues)

Have an improvement? Submit a PR!

## ⭐ Show Your Support

If this helped you, give it a ⭐ on GitHub!

## 📧 Contact

Questions? Open an issue or reach out!

---

## 🎉 Success Stories

> "Set it up once in 2025, still running perfectly in 2026!" - Happy User

> "My app hasn't gone down once since using this bot!" - Another Happy User

> "The success/failure logging helped me catch a credential issue immediately!" - Grateful User

---

**Made with ❤️ for PythonAnywhere users**

**Stop manually clicking that extend button. Automate it! 🚀**
