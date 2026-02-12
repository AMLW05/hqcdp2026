# Data Migration Helper

## Importing Your Existing HQCDP Data

This guide helps you migrate data from your existing CSV tracker to the new dashboard.

---

## Quick CSV to Dashboard Import

### Step 1: Open Your Existing CSV
You have: `/Users/angelawestmoreland/Documents/HQCDP_Mock_Tracker.csv`

Current format:
```
Course ID, Course Name, CTE Status, HQCDP Stage, Instructor, Owner, Deadline, ...
BUS-101, Introduction to Business, Yes, 3, Dr. Adams, Shared, 2025-09-15, ...
```

### Step 2: Convert to Dashboard Format

Open the dashboard in your browser, press **F12** to open console, and paste this converter:

```javascript
// CSV Converter Function
function convertCSVtoDashboard(csvText) {
  const lines = csvText.trim().split('\n');
  const headers = lines[0].split(',').map(h => h.trim());

  const courses = [];

  for (let i = 1; i < lines.length; i++) {
    const values = lines[i].split(',').map(v => v.trim());
    if (!values[0]) continue; // Skip empty lines

    const course = {
      id: values[0],
      title: values[1],
      cte: values[2].toLowerCase() === 'yes',
      stage: parseInt(values[3]) || 0,
      instructor: values[4] || 'TBD',
      instructorEmail: '', // Add manually later
      owner: values[5] || 'TBD',
      deadline: values[6] || new Date(Date.now() + 180 * 24 * 60 * 60 * 1000).toISOString().split('T')[0],
      department: '', // Add manually
      courseRep: '', // Add manually
      complexity: 'Medium',
      microstage: parseFloat(values[3]) || 0,
      progress: parseInt(values[9]) || 0,
      totalModules: 0, // Add manually
      modulesCompleted: 0,
      notes: values[11] || '',
      publicNotes: '',
      lastUpdated: new Date().toISOString().split('T')[0],
      documents: [
        { name: 'Course Map', url: '', status: 'Pending' },
        { name: 'Syllabus', url: '', status: 'Pending' },
        { name: 'HIDOC Sheet', url: '', status: values[7] || 'Pending' }
      ]
    };

    courses.push(course);
  }

  return courses;
}

// USAGE:
// 1. Copy your CSV data
// 2. Paste it as the csvData variable below
// 3. Run this code

const csvData = `
Course ID,Course Name,CTE Status,HQCDP Stage,Instructor,Owner,Deadline,Homework Sheet Status,Tool Route,% Complete,CTE Hours Logged,Notes
BUS-101,Introduction to Business,Yes,3,Dr. Adams,Shared,2025-09-15,Submitted,"Goalify, Mapify, Cognify",45,8,Waiting on Stage 4 planning sheet
ENG-102,English Composition II,No,2,Prof. Brown,Instructor,2025-09-05,Not Sent,"Goalify, Mapify",20,0,Needs HIDOC homework
GRD-154,Vector Imaging Software,Yes,4,Dr. Green,You,2025-09-20,Reviewed,"Realiza, Cognify, Rubrica",70,12,Ready for QA next week
`;

const convertedCourses = convertCSVtoDashboard(csvData);

// Save to dashboard
localStorage.setItem('hqcdp_courses_v2', JSON.stringify({
  courses: convertedCourses,
  lastUpdated: new Date().toISOString(),
  version: '2.0'
}));

console.log(`✅ Imported ${convertedCourses.length} courses!`);
console.log('Reloading page...');
location.reload();
```

### Step 3: Manual Cleanup (After Import)

1. Switch to **Admin Mode**
2. For each course, click to expand and add:
   - Instructor email
   - Department
   - Course representative
   - Total modules
   - Complexity level
3. Review progress percentages
4. Add private notes as needed

---

## Importing from GitHub HQCDP Repo

If you want to import from `/Documents/GitHub/HQCDP/courses-data.json`:

```javascript
// This imports from your existing JSON structure
async function importFromGitHubJSON() {
  // You'll need to copy the JSON content manually since it's local
  // Or you can read from the file if uploaded

  const oldData = {
    courses: [
      // Copy content from /Documents/GitHub/HQCDP/courses-data.json
      // Paste here
    ]
  };

  // Convert to new format
  const convertedCourses = oldData.courses.map(course => ({
    id: course.id,
    title: course.title,
    instructor: course.instructor,
    instructorEmail: '', // Add manually
    department: course.department || '',
    courseRep: course.courseRep || '',
    complexity: course.complexity || 'Medium',
    cte: course.cte || false,
    stage: course.stage || 0,
    microstage: course.microstage || 0,
    progress: course.progress || 0,
    totalModules: course.totalModules || 0,
    modulesCompleted: course.modulesCompleted || 0,
    deadline: course.deadline || new Date(Date.now() + 180 * 24 * 60 * 60 * 1000).toISOString().split('T')[0],
    lastUpdated: course.lastUpdated || new Date().toISOString().split('T')[0],
    notes: course.notes || '',
    publicNotes: course.publicNotes || '',
    documents: course.planningDocuments ?
      course.planningDocuments.map(doc => ({
        name: doc.name,
        url: doc.url || '',
        status: 'Received'
      })) : [
        { name: 'Course Map', url: '', status: 'Pending' },
        { name: 'Syllabus', url: '', status: 'Pending' },
        { name: 'HIDOC Sheet', url: '', status: 'Pending' }
      ]
  }));

  // Save
  localStorage.setItem('hqcdp_courses_v2', JSON.stringify({
    courses: convertedCourses,
    lastUpdated: new Date().toISOString(),
    version: '2.0'
  }));

  console.log(`✅ Imported ${convertedCourses.length} courses from GitHub repo!`);
  location.reload();
}

// Run it
importFromGitHubJSON();
```

---

## Stage Mapping Reference

Your old data → New dashboard:

| Old System | New Stage | Stage Name |
|------------|-----------|------------|
| Not tracked | 0 | Not Started |
| Planning | 1 | Planning |
| Stage 2 | 2 | Foundation |
| Stage 3 | 3 | Active Build |
| Stage 4 | 4 | Internal Review |
| Stage 5 | 5 | Refinement |
| Stage 6 | 6 | Quality Assurance |
| Complete/Certified | 7 | Certified |
| On Hold | H | On Hold |
| Recert | R | Recertification |

---

## Document Status Mapping

If you have document tracking in your CSV:

| CSV Value | Dashboard Status |
|-----------|------------------|
| Submitted, Received, Reviewed, Yes | Received |
| Not Sent, Pending, No | Pending |
| N/A, Not Applicable | N/A |

---

## Quick Data Validation Script

After importing, run this to check for issues:

```javascript
// Data validation
function validateDashboardData() {
  const data = JSON.parse(localStorage.getItem('hqcdp_courses_v2'));
  const courses = data.courses;

  console.log('=== DATA VALIDATION REPORT ===');
  console.log(`Total courses: ${courses.length}`);

  const issues = [];

  courses.forEach((course, idx) => {
    // Check required fields
    if (!course.id) issues.push(`Course ${idx}: Missing ID`);
    if (!course.title) issues.push(`${course.id}: Missing title`);
    if (!course.instructor) issues.push(`${course.id}: Missing instructor`);
    if (!course.deadline) issues.push(`${course.id}: Missing deadline`);

    // Check data types
    if (typeof course.stage !== 'number' && course.stage !== 'H' && course.stage !== 'R') {
      issues.push(`${course.id}: Invalid stage: ${course.stage}`);
    }

    if (course.progress < 0 || course.progress > 100) {
      issues.push(`${course.id}: Invalid progress: ${course.progress}%`);
    }
  });

  if (issues.length === 0) {
    console.log('✅ All courses validated successfully!');
  } else {
    console.log(`⚠️ Found ${issues.length} issues:`);
    issues.forEach(issue => console.log(`  - ${issue}`));
  }

  // Summary stats
  const cteCount = courses.filter(c => c.cte).length;
  const certified = courses.filter(c => c.stage === 7).length;
  const active = courses.filter(c => [2,3,4,5].includes(c.stage)).length;

  console.log('\n=== SUMMARY ===');
  console.log(`CTE Courses: ${cteCount}`);
  console.log(`Certified: ${certified}`);
  console.log(`Active Development: ${active}`);
  console.log(`Average Progress: ${Math.round(courses.reduce((sum, c) => sum + c.progress, 0) / courses.length)}%`);
}

// Run validation
validateDashboardData();
```

---

## Backup Your Old Data First!

Before migrating, backup your existing data:

```bash
# Backup CSV
cp /Users/angelawestmoreland/Documents/HQCDP_Mock_Tracker.csv \
   /Users/angelawestmoreland/Documents/HQCDP_Mock_Tracker_BACKUP_$(date +%Y%m%d).csv

# Backup GitHub data
cp /Users/angelawestmoreland/Documents/GitHub/HQCDP/courses-data.json \
   /Users/angelawestmoreland/Documents/GitHub/HQCDP/courses-data_BACKUP_$(date +%Y%m%d).json
```

---

## Step-by-Step Migration Checklist

- [ ] Backup existing CSV and JSON files
- [ ] Open new dashboard in browser
- [ ] Open browser console (F12)
- [ ] Copy CSV content or JSON content
- [ ] Paste appropriate converter script
- [ ] Run import function
- [ ] Validate data with validation script
- [ ] Review courses in Admin Mode
- [ ] Add missing information (emails, departments)
- [ ] Export new JSON as backup
- [ ] Share dashboard URL with boss

---

## Testing Migration

After import, verify:

1. **Supervisor View**:
   - Total course count matches
   - CTE count is correct
   - Stage distribution looks right
   - Deadlines show up in timeline

2. **Admin View**:
   - All courses listed
   - Can edit any course
   - Notes are preserved
   - Export works

3. **Data Integrity**:
   - Run validation script
   - Check for missing fields
   - Verify progress percentages
   - Confirm dates are valid

---

## Need Help?

Common issues:

**"Imported but courses not showing"**
- Check browser console for errors
- Verify localStorage isn't full
- Try clearing cache and re-importing

**"Some courses missing after import"**
- Check CSV format (no extra commas)
- Verify course IDs are unique
- Look for empty rows in source data

**"Progress percentages wrong"**
- Update manually in Admin Mode
- Or adjust converter script to calculate from modules

---

**Migration Date**: _________________
**Courses Migrated**: _________________
**Issues Found**: _________________
**Resolution**: _________________
