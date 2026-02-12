# HQCDP Dashboard - 2025-2026 Academic Year

A powerful, dual-mode dashboard for tracking High-Quality Course Development Program (HQCDP) progress for the **2025-2026 academic year**. Features a simplified supervisor view for leadership and a comprehensive admin interface for course management.

**Current Academic Year:** 2025-2026
**Last Updated:** 2026-02-12

## 🎯 Features

### Supervisor View (Boss-Facing)
- **Executive KPI Cards**: Total courses, CTE count, certified courses, at-risk courses, average progress
- **Pipeline Status**: Visual stage distribution with progress bars
- **Upcoming Deadlines**: Timeline view of courses by due date with overdue indicators
- **Clean Filters**: Search, stage filter, CTE/Non-CTE filter
- **One-Click Access**: No complex controls, just essential metrics

### Admin View (Your Backend)
- **Full CRUD Operations**: Add, edit, delete courses with ease
- **Inline Editing**: Click any field to edit directly - no complex forms
- **Quick Entry Form**: Add new courses fast with smart defaults
- **Bulk Export**: CSV and JSON export for data backup/analysis
- **Private Notes**: Keep internal notes separate from public information
- **Real-Time Updates**: All changes save automatically to localStorage
- **Automatic Backups**: Previous version stored before each save

### Data Management
- **JSON Backend**: Easy to understand, human-readable data format
- **Auto-Save**: Changes persist automatically to localStorage
- **Import/Export**: CSV and JSON export for spreadsheet analysis
- **Backup System**: Automatic backup before each save operation
- **No Server Required**: Runs entirely in browser, perfect for Cloudflare Pages

## 🚀 Quick Start

### Local Testing
1. Open `index.html` in any modern web browser
2. Toggle between Supervisor and Admin views using the header buttons
3. Data persists in browser localStorage automatically

### Cloudflare Pages Deployment

#### Step 1: Initialize Git Repository (if not already)
```bash
cd hqcdp-cloudflare
git init
git add .
git commit -m "Initial HQCDP dashboard deployment"
```

#### Step 2: Create GitHub Private Repository
```bash
# Create new private repo (keep your data private!)
gh repo create hqcdp-dashboard --private --source=. --push
```

#### Step 3: Deploy to Cloudflare Pages

1. **Login to Cloudflare Dashboard**: https://dash.cloudflare.com
2. **Navigate to Pages**: Workers & Pages → Create Application → Pages → Connect to Git
3. **Connect GitHub Account**: Authorize Cloudflare to access your private repo
4. **Select Repository**: Choose `hqcdp-dashboard` (private)
5. **Configure Build**:
   - Project name: `hqcdp-dashboard`
   - Production branch: `main`
   - Build command: (leave empty)
   - Build output directory: `/`
   - Root directory: `/`
6. **Deploy**: Click "Save and Deploy"

Your dashboard will be live at: `https://hqcdp-dashboard.pages.dev`

#### Step 4: Add Access Protection (Recommended)

**Option A: Cloudflare Access (Zero Trust)**
1. Go to Zero Trust → Access → Applications
2. Create new application:
   - Name: HQCDP Dashboard
   - Domain: `hqcdp-dashboard.pages.dev`
3. Add policy:
   - Policy name: Authorized Users
   - Action: Allow
   - Include: Email addresses (add your boss's email, your email)
4. Save

Now visitors need to authenticate before viewing!

**Option B: Password Protection (Simple)**
1. In Pages → Settings → Environment Variables
2. Add: `ACCESS_PASSWORD` = `your-secret-password`
3. Modify index.html to check password (optional enhancement)

## 📊 Data Structure

Each course contains:
```json
{
  "id": "BUS-115",
  "title": "Business Law I",
  "instructor": "Dr. Lewis",
  "instructorEmail": "lewis@ftcc.edu",
  "department": "Business",
  "courseRep": "Jane Smith",
  "complexity": "Medium",
  "cte": true,
  "stage": 2,
  "microstage": 2.3,
  "progress": 35,
  "totalModules": 12,
  "modulesCompleted": 4,
  "deadline": "2025-12-15",
  "lastUpdated": "2025-01-13",
  "notes": "Private admin notes",
  "publicNotes": "Notes visible to everyone",
  "documents": [
    { "name": "Course Map", "url": "", "status": "Received" },
    { "name": "Syllabus", "url": "", "status": "Pending" },
    { "name": "HIDOC Sheet", "url": "", "status": "Received" }
  ]
}
```

## 🎨 Stage Definitions

The HQCDP program uses a **6-stage pyramid model** to track course development progress. Like a pyramid, each stage builds on the previous one, with the ultimate goal of reaching the apex: Certification.

### Stage Pyramid (ROYGBIV Color Scheme)

```
                    ▲ Stage 6
                   ╱ ╲  VIOLET
                  ╱   ╲ (Certified)
                 ╱─────╲
                ╱ Stage 5 ╲
               ╱   BLUE     ╲
              ╱ (Post-Review)╲
             ╱───────────────╲
            ╱   Stage 4        ╲
           ╱     GREEN          ╲
          ╱ (Internal Review)    ╲
         ╱───────────────────────╲
        ╱      Stage 3             ╲
       ╱       YELLOW               ╲
      ╱  (Course Rep Review)         ╲
     ╱─────────────────────────────╲
    ╱         Stage 2                ╲
   ╱          ORANGE                  ╲
  ╱     (Active Development)           ╲
 ╱───────────────────────────────────╲
╱            Stage 1                   ╲
             RED
          (Planning)
```

### Color Key

| Stage | Name | Description | Color (ROYGBIV) |
|-------|------|-------------|-----------------|
| 1 | Planning | Initial planning and course design | 🔴 Red |
| 2 | Active Development | Building course content and materials | 🟠 Orange |
| 3 | Course Rep Review | Content complete, under Course Rep review | 🟡 Yellow |
| 4 | Internal Review | Under internal QA review | 🟢 Green |
| 5 | Post-Review | Final refinements after review | 🔵 Blue |
| 6 | Certified | Complete and certified for delivery | 🟣 Violet |

## 💾 Data Management

### Exporting Data
- **CSV Export**: For spreadsheet analysis (Excel, Google Sheets)
- **JSON Export**: For backup or migration to other systems
- Click export buttons in header (both modes)

### Importing Data from CSV
The dashboard starts with sample data. To import your existing CSV:

1. Switch to Admin Mode
2. Manually add courses using the "Add Course" form
3. Or: Export current data, edit JSON file, re-import via browser console:
   ```javascript
   // In browser console:
   localStorage.setItem('hqcdp_courses_v2', JSON.stringify({
     courses: YOUR_COURSE_ARRAY,
     lastUpdated: new Date().toISOString(),
     version: '2.0'
   }));
   location.reload();
   ```

### Backup & Restore
- **Automatic Backup**: System backs up before each save
- **Manual Backup**: Export JSON regularly
- **Restore**: Import JSON file via browser console

## 🔧 Customization

### Adding Custom Fields
Edit the `index.html` file and add fields to:
1. `DataManager.getDefaultData()` - default structure
2. `AddCourseForm` - form fields
3. `AdminCourseCard` - display fields

### Changing Colors/Branding
- Edit `STAGE_DEFINITIONS` for stage colors
- Modify Tailwind classes in components
- Update gradient in header: `.gradient-bg { background: ... }`

### Adding More Document Types
In each course object, add to `documents` array:
```javascript
documents: [
  { name: "Custom Doc", url: "", status: "Pending" }
]
```

## 📱 Mobile Responsive

Dashboard is fully responsive and works on:
- Desktop browsers (Chrome, Firefox, Safari, Edge)
- Tablets (iPad, Android tablets)
- Mobile phones (iOS, Android)

## 🔒 Security Notes

- **Private GitHub Repo**: Keeps your data private
- **Cloudflare Access**: Requires authentication before viewing
- **No Server**: Data stored in browser localStorage (not on server)
- **HTTPS**: Cloudflare Pages uses HTTPS by default
- **Private Notes**: Only visible in Admin Mode

## 🆘 Troubleshooting

### Data Not Saving
- Check browser localStorage is enabled
- Try different browser
- Export JSON backup regularly

### Cloudflare Deployment Issues
- Ensure repository is connected
- Check build logs in Cloudflare dashboard
- Verify `index.html` is in repository root

### Access Protection Not Working
- Check Zero Trust application settings
- Verify email addresses are correct
- Clear browser cache and try again

## 📈 Future Enhancements

Potential additions:
- [ ] Document upload/attachment system
- [ ] Gantt chart timeline view
- [ ] Email notifications for deadlines
- [ ] Multi-user collaboration
- [ ] Activity log/audit trail
- [ ] Advanced analytics dashboard
- [ ] PDF report generation
- [ ] Integration with Canvas/LMS

## 📞 Support

For issues or questions:
1. Check this README
2. Review browser console for errors
3. Export data backup before making changes
4. Test in incognito/private mode to rule out extensions

---

## 📦 Archive

Historical documentation and data files have been moved to the `/archive` directory to maintain focus on the current 2025-2026 academic year.

---

**Version**: 2.1
**Academic Year**: 2025-2026
**Last Updated**: February 12, 2026
**License**: Private Use
