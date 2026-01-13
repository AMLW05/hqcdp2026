# HQCDP Dashboard - Cloudflare Pages Deployment Guide

## 🚀 Step-by-Step Deployment

### Prerequisites
- GitHub account
- Cloudflare account (free tier works!)
- Git installed locally

---

## Part 1: Push to GitHub (Private Repo)

### Option A: Using GitHub CLI (Recommended)

```bash
# Navigate to project folder
cd /Users/angelawestmoreland/hqcdp-cloudflare

# Create private GitHub repository and push
gh repo create hqcdp-dashboard --private --source=. --push

# Verify it worked
gh repo view
```

### Option B: Using GitHub Website

1. Go to https://github.com/new
2. Repository name: `hqcdp-dashboard`
3. **Important**: Select **Private** (keep your data private!)
4. Do NOT initialize with README (we already have files)
5. Click "Create repository"

Then push your code:
```bash
cd /Users/angelawestmoreland/hqcdp-cloudflare
git remote add origin https://github.com/YOUR_USERNAME/hqcdp-dashboard.git
git branch -M main
git push -u origin main
```

---

## Part 2: Deploy to Cloudflare Pages

### Step 1: Login to Cloudflare
1. Go to https://dash.cloudflare.com
2. Login with your account
3. If you don't have an account, sign up (free!)

### Step 2: Connect to GitHub
1. In Cloudflare dashboard, click **Workers & Pages** (left sidebar)
2. Click **Create Application**
3. Select **Pages** tab
4. Click **Connect to Git**
5. Click **GitHub** (connect your GitHub account if first time)
6. Authorize Cloudflare to access your GitHub
7. Select **Only select repositories** → Choose `hqcdp-dashboard`
8. Click **Install & Authorize**

### Step 3: Configure Build Settings
1. Select `hqcdp-dashboard` repository
2. Click **Begin setup**
3. Configure project:
   - **Project name**: `hqcdp-dashboard` (or choose custom name)
   - **Production branch**: `main`
   - **Framework preset**: None
   - **Build command**: (leave empty)
   - **Build output directory**: `/`
   - **Root directory**: `/` (or leave empty)

4. Click **Save and Deploy**

### Step 4: Wait for Deployment
- First deployment takes 1-2 minutes
- Watch the build log for any errors
- Once complete, you'll see: "Success! Your site is live!"

### Step 5: Get Your URL
Your dashboard is now live at:
```
https://hqcdp-dashboard.pages.dev
```

Or with custom project name:
```
https://YOUR-PROJECT-NAME.pages.dev
```

---

## Part 3: Add Access Protection (Highly Recommended!)

Your boss needs to see the dashboard, but you want to control who can access it.

### Option A: Cloudflare Zero Trust Access (Best Option)

**Benefits:**
- Professional authentication
- Email-based access control
- No passwords to remember
- Free for up to 50 users

**Setup:**

1. In Cloudflare dashboard, go to **Zero Trust** (left sidebar)
2. If first time: Click **Get Started** and set up your team name
3. Go to **Access → Applications**
4. Click **Add an application**
5. Select **Self-hosted**
6. Configure:
   - **Application name**: HQCDP Dashboard
   - **Session duration**: 24 hours (or your preference)
   - **Application domain**:
     - Subdomain: `hqcdp-dashboard`
     - Domain: `pages.dev` (or your custom domain)
     - Path: (leave empty)

7. Click **Next**

8. Create policy:
   - **Policy name**: Authorized Users
   - **Action**: Allow
   - **Session duration**: 24 hours
   - Under **Configure rules**:
     - **Selector**: Emails
     - **Value**: Add emails (your email, your boss's email, etc.)
     - Click **+Add include** for multiple emails

9. Click **Next** → **Add application**

**Now anyone visiting the dashboard must authenticate with their email!**

### Option B: Cloudflare Access with Domain (Free Method)

If you want simpler email auth without Zero Trust:

1. Go to **Workers & Pages** → Your project
2. Click **Settings** tab
3. Scroll to **Access Policy**
4. Click **Enable Access Policy**
5. Add email addresses that should have access

---

## Part 4: Custom Domain (Optional)

Want to use your own domain like `hqcdp.yourschool.edu`?

1. In your Cloudflare dashboard, go to **Workers & Pages**
2. Click your `hqcdp-dashboard` project
3. Go to **Custom domains** tab
4. Click **Set up a custom domain**
5. Enter your domain: `hqcdp.yourschool.edu`
6. Follow DNS setup instructions
7. Wait for SSL certificate (automatic, ~5 minutes)

---

## Part 5: Testing

### Test Supervisor View (Boss View)
1. Open your dashboard URL
2. You should see **Supervisor View** by default
3. Check:
   - ✅ KPI cards show correct numbers
   - ✅ Pipeline status displays stage distribution
   - ✅ Upcoming deadlines show courses
   - ✅ Filters work (search, stage, CTE)

### Test Admin View (Your Backend)
1. Click **Admin Mode** button in header
2. Check:
   - ✅ Can see all courses in table format
   - ✅ Click "Add Course" and add a test course
   - ✅ Expand a course card and edit fields inline
   - ✅ Export CSV and JSON work
   - ✅ Changes persist after refresh

---

## Part 6: Updating Your Dashboard

### When You Need to Make Changes

```bash
# Navigate to project
cd /Users/angelawestmoreland/hqcdp-cloudflare

# Make your changes to index.html or other files

# Commit and push
git add .
git commit -m "Updated dashboard with new features"
git push

# Cloudflare automatically rebuilds and deploys!
# Check build status in Cloudflare dashboard
```

**Auto-Deploy:** Every push to `main` branch triggers automatic redeployment!

---

## 🔒 Security Checklist

- [ ] GitHub repository is set to **Private**
- [ ] Cloudflare Zero Trust Access is configured
- [ ] Authorized email addresses added
- [ ] Tested that unauthenticated users cannot access
- [ ] Boss can successfully login and view
- [ ] You can access Admin Mode
- [ ] Data exports work correctly

---

## 📊 Importing Your Existing Data

You have two options:

### Option 1: Manual Entry (Recommended for <20 courses)
1. Switch to Admin Mode
2. Click "Add Course" button
3. Fill in course details
4. Repeat for each course

### Option 2: Bulk Import via Browser Console

1. Export your CSV to JSON format
2. Open your dashboard in browser
3. Press F12 to open Developer Console
4. Run this command:

```javascript
// Prepare your courses array
const myCourses = [
  {
    id: 'BUS-115',
    title: 'Business Law I',
    instructor: 'Dr. Lewis',
    instructorEmail: 'lewis@ftcc.edu',
    department: 'Business',
    complexity: 'Medium',
    cte: true,
    stage: 2,
    progress: 35,
    totalModules: 12,
    modulesCompleted: 4,
    deadline: '2025-12-15',
    notes: 'Working on module 3',
    documents: [
      { name: 'Course Map', url: '', status: 'Received' },
      { name: 'Syllabus', url: '', status: 'Received' },
      { name: 'HIDOC Sheet', url: '', status: 'Pending' }
    ]
  },
  // ... add more courses
];

// Import
localStorage.setItem('hqcdp_courses_v2', JSON.stringify({
  courses: myCourses,
  lastUpdated: new Date().toISOString(),
  version: '2.0'
}));

// Reload page
location.reload();
```

---

## 🆘 Troubleshooting

### "Build failed" on Cloudflare
- Check that `index.html` is in repository root
- Verify no syntax errors in HTML
- Check Cloudflare build logs for specific error

### "Access Denied" when visiting dashboard
- Check Zero Trust policy includes your email
- Try incognito/private browsing mode
- Clear browser cache
- Wait 5 minutes for DNS propagation

### Data not showing up
- Check browser console for errors (F12)
- Verify localStorage is enabled in browser
- Try different browser
- Export/import data as backup

### Changes not appearing after push
- Check Cloudflare deployment status
- Wait 2-3 minutes for rebuild
- Hard refresh browser (Ctrl+Shift+R or Cmd+Shift+R)
- Check that commit was pushed to `main` branch

---

## 📞 Quick Commands Reference

```bash
# View current status
cd /Users/angelawestmoreland/hqcdp-cloudflare
git status

# Push changes
git add .
git commit -m "Your change description"
git push

# View GitHub repo
gh repo view

# Check Cloudflare deployment
# Visit: https://dash.cloudflare.com/[your-account]/pages

# Test locally
open index.html
```

---

## 🎉 Success Checklist

Your deployment is complete when:

- [ ] Dashboard is live at Cloudflare Pages URL
- [ ] Supervisor View shows clean KPIs for boss
- [ ] Admin View allows you to manage courses
- [ ] Access protection requires authentication
- [ ] Your boss can successfully login and view
- [ ] Data persists between sessions
- [ ] CSV/JSON export works
- [ ] You can push updates via Git

---

**Need Help?**

1. Check build logs in Cloudflare dashboard
2. Review browser console for JavaScript errors
3. Test in incognito mode to rule out cache issues
4. Verify GitHub repository is private and connected

**Questions about specific features?** See README.md for detailed documentation.

---

**Deployment Date**: January 2026
**Version**: 2.0
