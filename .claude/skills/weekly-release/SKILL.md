---
name: weekly-release
description: Generate Release Notes
allowed-tools: Bash, Read, Write
---

# Linguana Weekly Release Notes Generator

Generate comprehensive weekly release notes for Linguana repositories, organized by committer.

## Repositories to Include

The following repositories in `/Users/odedshafran/vocaloca/` are included by default:
- **cms** - Content Management System
- **catalog** - Catalog service
- **orchestration** - Orchestration workflows
- **getFreelancer** - Freelancer management

Additional repositories can be added by the user when invoking the skill.

## Instructions

### Step 1: Collect Git Commits

For each repository, run the following command to get commits from the past 7 days:

```bash
cd /Users/odedshafran/vocaloca/<repo> && git fetch origin && git log --all --since="7 days ago" --pretty=format:'%H|%an|%ae|%ad|%s' --date=short 2>/dev/null
```

### Step 2: Parse and Categorize Commits

Parse each commit and categorize based on commit message patterns:

**Features** (badge-feature):
- Messages starting with `feat:`, `feature:`, `add:`, `new:`
- Messages containing "implement", "add", "create", "introduce"

**Bug Fixes** (badge-fix):
- Messages starting with `fix:`, `bugfix:`, `hotfix:`
- Messages containing "fix", "resolve", "correct", "repair"

**Performance** (badge-perf):
- Messages starting with `perf:`, `optimize:`, `performance:`
- Messages containing "optimize", "performance", "speed", "improve"

**Other**:
- `refactor:`, `chore:`, `docs:`, `style:`, `test:`, `ci:`, `build:`

### Step 3: Group by Author

Group all commits by author name, normalizing similar names:
- Combine variations like "Oded Shafran" and "odedshafran"
- Use the most formal/complete name variation found

For each author, track:
- Total commit count
- Features count
- Fixes count
- Performance count
- List of repositories contributed to

### Step 4: Generate HTML Report

Create an HTML file at `/Users/odedshafran/vocaloca/release_notes/Linguana_Team_Accomplishments_<DATE>.html`

Use the following structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Linguana Team Accomplishments - <DATE></title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; line-height: 1.5; color: #333; max-width: 900px; margin: 0 auto; padding: 30px 20px; font-size: 12px; }
        .header { text-align: center; margin-bottom: 25px; border-bottom: 3px solid #7c3aed; padding-bottom: 15px; }
        .header h1 { color: #5b21b6; font-size: 24px; margin-bottom: 5px; }
        .header .date { color: #6b7280; font-size: 13px; }
        .summary-table { width: 100%; border-collapse: collapse; margin: 20px 0; font-size: 11px; }
        .summary-table th, .summary-table td { padding: 8px 10px; text-align: left; border: 1px solid #e5e7eb; }
        .summary-table th { background: #5b21b6; color: white; font-weight: 600; }
        .summary-table tr:nth-child(even) { background: #f9fafb; }
        .summary-table .number { text-align: center; font-weight: 600; }
        .summary-table .highlight { background: #f3e8ff; }
        .employee-section { margin-bottom: 20px; page-break-inside: avoid; }
        .employee-header { background: linear-gradient(135deg, #7c3aed 0%, #5b21b6 100%); color: white; padding: 12px 15px; border-radius: 8px 8px 0 0; display: flex; justify-content: space-between; align-items: center; }
        .employee-header h2 { font-size: 16px; font-weight: 600; }
        .employee-stats { display: flex; gap: 15px; font-size: 11px; }
        .employee-stats span { background: rgba(255,255,255,0.2); padding: 3px 8px; border-radius: 4px; }
        .employee-content { border: 1px solid #e5e7eb; border-top: none; border-radius: 0 0 8px 8px; padding: 15px; background: #fafafa; }
        .category { margin-bottom: 12px; }
        .category h3 { font-size: 12px; color: #5b21b6; margin-bottom: 6px; padding-bottom: 3px; border-bottom: 1px solid #e9d5ff; }
        .category ul { margin-left: 15px; }
        .category li { margin-bottom: 4px; font-size: 11px; }
        .repos { font-size: 10px; color: #6b7280; margin-top: 8px; }
        .repos strong { color: #5b21b6; }
        .footer { text-align: center; margin-top: 25px; padding-top: 15px; border-top: 1px solid #e5e7eb; color: #6b7280; font-size: 10px; }
        .badge { display: inline-block; padding: 2px 6px; border-radius: 3px; font-size: 9px; font-weight: 600; margin-left: 5px; }
        .badge-feature { background: #d1fae5; color: #047857; }
        .badge-fix { background: #fee2e2; color: #b91c1c; }
        .badge-perf { background: #dbeafe; color: #1d4ed8; }
        .badge-ai { background: linear-gradient(135deg, #0891b2 0%, #0e7490 100%); }
    </style>
</head>
<body>
    <div class="header">
        <h1>Team Accomplishments Report</h1>
        <div class="date">Week of <START_DATE> - <END_DATE></div>
    </div>

    <h2 style="color: #5b21b6; font-size: 14px; margin-bottom: 10px;">Team Summary</h2>
    <table class="summary-table">
        <tr>
            <th>Team Member</th>
            <th style="text-align: center;">Commits</th>
            <th style="text-align: center;">Features</th>
            <th style="text-align: center;">Fixes</th>
            <th>Repositories</th>
        </tr>
        <!-- Generate rows for each team member, sorted by commit count descending -->
        <!-- Use class="highlight" for the top contributor -->
    </table>

    <!-- For each team member, create an employee-section -->
    <div class="employee-section">
        <div class="employee-header">
            <!-- Use badge-ai style for "Cyrus AI" commits -->
            <h2>Employee Name</h2>
            <div class="employee-stats">
                <span>X commits</span>
                <span>Y repos</span>
            </div>
        </div>
        <div class="employee-content">
            <div class="category">
                <h3>Features Implemented <span class="badge badge-feature">COUNT</span></h3>
                <ul>
                    <li>Feature description (cleaned up from commit message)</li>
                </ul>
            </div>
            <div class="category">
                <h3>Bug Fixes & Improvements <span class="badge badge-fix">COUNT</span></h3>
                <ul>
                    <li>Fix description</li>
                </ul>
            </div>
            <div class="category">
                <h3>Performance <span class="badge badge-perf">COUNT</span></h3>
                <ul>
                    <li>Performance improvement description</li>
                </ul>
            </div>
            <div class="repos"><strong>Repositories:</strong> RepoName (count), RepoName2 (count)</div>
        </div>
    </div>

    <div class="footer">
        Generated by Cyrus AI Agent | Linguana Platform | Week of <START_DATE> - <END_DATE>
    </div>
</body>
</html>
```

### Step 5: Clean Up Commit Messages

When displaying commit messages in the report:
1. Remove conventional commit prefixes (feat:, fix:, etc.)
2. Capitalize first letter
3. Remove trailing periods
4. Truncate very long messages (keep first sentence or ~100 chars)
5. Group similar commits when possible

### Step 6: Output Location

Save the generated file to:
```
/Users/odedshafran/vocaloca/release_notes/Linguana_Team_Accomplishments_<YYYY-MM-DD>.html
```

Where `<YYYY-MM-DD>` is today's date.

## Usage Examples

- `/weekly-release` - Generate release notes for the past 7 days from default repos
- `/weekly-release --since "2025-01-01"` - Generate from a specific date
- `/weekly-release --repos cms,catalog,orchestration,getFreelancer,linguana-asr` - Include additional repos

## Notes

- Always fetch latest changes from origin before collecting commits
- Handle repositories that may not exist or have no recent commits gracefully
- Sort team members by total commit count (highest first)
- Use the Cyrus AI badge style for any commits from "Cyrus" or containing "Claude" in author
