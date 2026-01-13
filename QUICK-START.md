# HQCDP Dashboard - Quick Start Guide

## 🎯 What You Have

A **dual-mode dashboard** for course development tracking:

### 👁️ **Supervisor View** (Boss-Facing)
- Clean, simple executive dashboard
- KPI cards: Total courses, CTE count, certified, at-risk, avg progress
- Visual pipeline showing stage distribution
- Deadline timeline
- Easy filters (search, stage, CTE/Non-CTE)

### ⚙️ **Admin View** (Your Backend)
- Full course management (add, edit, delete)
- Inline editing - click any field to edit
- Bulk export (CSV + JSON)
- Private notes section
- Quick entry form for new courses
- Document tracking per course

---

## 🚀 Deploy in 5 Minutes

### 1. Push to GitHub (Private)
```bash
cd /Users/angelawestmoreland/hqcdp-cloudflare
gh repo create hqcdp-dashboard --private --source=. --push
```

### 2. Deploy to Cloudflare
1. Go to https://dash.cloudflare.com
2. Click **Workers & Pages** → **Create Application** → **Pages**
3. Connect to Git → Select `hqcdp-dashboard`
4. Click **Save and Deploy**
5. Done! Your URL: `https://hqcdp-dashboard.pages.dev`

### 3. Add Access Protection
1. In Cloudflare: **Zero Trust** → **Access** → **Applications**
2. Add application for your dashboard URL
3. Create policy: Allow specific emails (yours + boss)
4. Save

Now only authorized users can access!

---

## 📊 Import Your Data

### Option 1: Manual Entry (Best for <20 courses)
1. Open dashboard → Switch to **Admin Mode**
2. Click **Add Course** button
3. Fill in details, click **Add**
4. Repeat for each course

### Option 2: Bulk Import from CSV
1. Open dashboard in browser
2. Press **F12** (Developer Console)
3. Use the converter script in `MIGRATION-HELPER.md`
4. Paste your CSV data
5. Run script → auto-imports all courses!

---

## 🎨 How to Use

### For You (Admin):
1. **Quick Updates**:
   - Switch to Admin Mode
   - Expand any course card
   - Click any field to edit inline
   - Changes save automatically

2. **Add New Course**:
   - Click "Add Course" button
   - Fill in form (only Course ID required!)
   - System auto-calculates progress

3. **Export Data**:
   - Click "Export CSV" for spreadsheets
   - Click "Export JSON" for backup
   - Both buttons in header

4. **Track Progress**:
   - View pipeline status
   - See deadline timeline
   - Filter by stage/CTE status

### For Your Boss (Supervisor):
1. Open dashboard (default is Supervisor View)
2. See key metrics at a glance
3. Check pipeline status
4. Review upcoming deadlines
5. Use filters to drill down

**They never need to click Admin Mode!**

---

## 🔧 Daily Workflow

### Morning Check:
```
1. Open dashboard (auto-loads latest data)
2. Check "At Risk" KPI (red flag courses)
3. Review deadline timeline (next 2 weeks)
4. Switch to Admin Mode if updates needed
```

### Updating a Course:
```
1. Admin Mode → Find course
2. Click to expand
3. Click any field (stage, progress, deadline, etc.)
4. Edit → Click ✓
5. Done! (auto-saved)
```

### Weekly Review:
```
1. Export CSV for records
2. Review stage distribution
3. Update overdue deadlines
4. Add notes for pending actions
```

---

## 📁 File Structure

```
hqcdp-cloudflare/
├── index.html              # Main dashboard (single-file app)
├── README.md               # Full documentation
├── DEPLOYMENT-GUIDE.md     # Step-by-step Cloudflare setup
├── MIGRATION-HELPER.md     # Import existing data
└── QUICK-START.md          # This file!
```

**Single File = Easy Deployment!**

---

## 💾 Data Storage

- **Location**: Browser localStorage (saved on YOUR computer)
- **Backup**: Automatic backup before each save
- **Export**: CSV/JSON export anytime
- **Sync**: Cloudflare Pages serves the app, data stays local

**Important**: Each user's browser stores their own data. For shared data across users, you'll need to export/import JSON files.

---

## 🎯 Key Features

### Supervisor View:
✅ Executive KPI dashboard
✅ Pipeline status visualization
✅ Deadline timeline view
✅ Clean, minimal interface
✅ Mobile responsive

### Admin View:
✅ Inline editing (click to edit)
✅ Bulk add/edit/delete
✅ CSV & JSON export
✅ Private notes section
✅ Document tracking
✅ Auto-save & backup
✅ Search & filter
✅ Quick entry form

### Security:
✅ GitHub private repo
✅ Cloudflare Zero Trust access
✅ Email-based authentication
✅ HTTPS by default
✅ No server = no data breach

---

## 🆘 Common Questions

**Q: Can multiple people edit at once?**
A: Each user has their own data in their browser. To share updates, export JSON and import in other browsers.

**Q: What if I lose my data?**
A: Regular exports create backups! Export JSON weekly. System also auto-backs up before each save.

**Q: Can my boss edit data?**
A: Only if you share Admin Mode access. By default, Supervisor View is read-only.

**Q: How do I update the dashboard after deployment?**
A: Make changes locally → `git push` → Cloudflare auto-deploys!

**Q: What browsers work?**
A: Chrome, Firefox, Safari, Edge (all modern browsers)

**Q: Does it work on mobile?**
A: Yes! Fully responsive design.

---

## 📞 Next Steps

1. ✅ Deploy to Cloudflare Pages (5 min)
2. ✅ Add access protection (5 min)
3. ✅ Import your existing data (10 min)
4. ✅ Share URL with boss
5. ✅ Bookmark for daily use!

---

## 📚 More Help

- **Full docs**: See `README.md`
- **Deployment**: See `DEPLOYMENT-GUIDE.md`
- **Migration**: See `MIGRATION-HELPER.md`
- **Support**: Check browser console (F12) for errors

---

## ✨ Pro Tips

1. **Bookmark both views**:
   - Supervisor: `https://hqcdp-dashboard.pages.dev`
   - Admin: (same URL, just toggle mode)

2. **Export weekly**: Create backups every Friday

3. **Use filters**: Quickly find courses by stage/CTE status

4. **Mobile access**: Works great on tablets for meetings!

5. **Custom deadlines**: Click deadline field in Admin to change

6. **Bulk updates**: Export CSV → Edit in Excel → Re-import

---

**Ready to deploy?** Run these 3 commands:

```bash
cd /Users/angelawestmoreland/hqcdp-cloudflare
gh repo create hqcdp-dashboard --private --source=. --push
open https://dash.cloudflare.com
```

Then follow Cloudflare's "Connect to Git" wizard!

---

**Questions?** Check the other guides or open `index.html` locally to test first!
