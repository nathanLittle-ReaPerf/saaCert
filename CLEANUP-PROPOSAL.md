# Repository Cleanup Proposal

**Date:** December 8, 2025
**Current Status:** 60+ files, many duplicates and outdated materials
**Goal:** Clean, organized structure for 28-day study sprint

---

## 🗑️ Files to DELETE (Safe to Remove)

### 1. HTML Duplicates (12 files) - Just exports of .md files
```
✗ Compute Services Quick Reference.pdf
✗ Day-2-Database-Deep-Dive.html
✗ Day-3-VPC-Networking-Deep-Dive.html
✗ Day-4-Cheat-Sheet-S3-Security-Replication.html
✗ Day-7-Updated-Weaknesses.html
✗ Day-7-Weakness-Destroyer-Quiz.html
✗ Day-7-Week-1-Deep-Dive-Review.html
✗ Load-Balancer-Cheat-Sheet-ALB-NLB-GLB.html
✗ Quick-Reference-Compute.html
✗ Quick-Reference-Databases.html
✗ Quick-Reference-Networking.html
✗ Recovery-Schedule-Week-1-Foundation-Repair.html
✗ SAA-C03-Study-Schedule.html
✗ Weak-Areas-Cheat-Sheet.html
✗ aws-storage-comparison.html
```
**Reason:** You have the .md versions, HTML just clutter

### 2. Outdated Study Schedule (1 file)
```
✗ SAA-C03-Study-Schedule.md (references old Dec 17 exam date)
```
**Reason:** Superseded by Revised-Study-Schedule-Dec-5-Jan-5.md

### 3. Old Summary/Status Files (2 files)
```
✗ NEW-MATERIALS-SUMMARY.md (from Nov 21, outdated)
✗ Dec-5-Status-Report.md (point-in-time snapshot, no longer relevant)
```
**Reason:** Historical snapshots that served their purpose

**Total to delete: 18 files (~400 KB freed)**

---

## 📁 Files to CONSOLIDATE

### Daily Progress Files (Currently 15+ files scattered)
**Consolidate into:** `Progress-Tracker.md`

**Current mess:**
- Day-1-Recovery-Session-Summary.md
- Day-2-Session-Summary.md
- Day-8-Foundation-Quiz-Failure-Analysis.md
- Day-8-Weakness-Recovery-Quiz.md
- Dec-7-Session-Summary.md
- etc.

**Proposed:** Single progress tracker with sections per day

### Weakness Tracking (Currently 3 files)
**Consolidate into:** `Weakness-Tracker.md`

**Current files:**
- AWS-SAA-Weaknesses.md
- Weak-Areas-Cheat-Sheet.md
- Day-7-Updated-Weaknesses.md

**Proposed:** Single living document that updates as weaknesses are addressed

### Quiz Files (Currently scattered in Day-X files)
**Keep separate but organize:**
- Days-1-3-Comprehensive-Quiz.md
- Day-2-Quiz-Auto-Scaling-Load-Balancing.md
- Day-6-Catchup-Quiz-Days-1-5-Review.md
- Day-6-Weakness-Focused-Quiz.md
- Day-7-Weakness-Destroyer-Quiz.md
- Dec-5-Recovery-Quiz-20Q.md
- Dec-7-Comprehensive-Quiz-20Q.md

**Proposed:** Rename with consistent pattern: `Quiz-Week1-Day2-AutoScaling.md`

---

## 📂 Proposed New Structure

```
saaCert/
│
├── 📘 STUDY MATERIALS (Reference - don't modify during study)
│   ├── Quick-Reference-Compute.md
│   ├── Quick-Reference-Storage.md
│   ├── Quick-Reference-Networking.md
│   ├── Quick-Reference-Databases.md
│   ├── Quick-Reference-Security-IAM.md
│   ├── Quick-Reference-Monitoring-DR-Other.md
│   ├── Quick-Reference-Analytics.md
│   ├── Quick-Reference-Migration.md
│   ├── Exam-Strategy-Tips.md
│   ├── Serverless-Architecture-Patterns.md
│   └── aws-storage-comparison.md
│
├── 📝 PRACTICE & QUIZZES
│   ├── Practice-Scenarios.md
│   ├── Practice-Scenarios-Additional.md
│   ├── Advanced-Practice-Scenarios-Hard-Mode.md
│   ├── Quiz-Week1-Day2-AutoScaling.md
│   ├── Quiz-Week1-Day6-Catchup.md
│   ├── Quiz-Week1-Day7-Weaknesses.md
│   ├── Quiz-Week1-Comprehensive.md
│   └── Quiz-Dec7-Comprehensive.md
│
├── 📊 PROGRESS TRACKING (Living documents)
│   ├── Progress-Tracker.md (consolidated daily summaries)
│   ├── Weakness-Tracker.md (ongoing weakness monitoring)
│   └── Week-1-Flashcards-Print-Template.md
│
├── 📅 PLANNING
│   ├── Revised-Study-Schedule-Dec-5-Jan-5.md (current schedule)
│   └── CLAUDE.md (instructions for Claude Code)
│
└── 🗄️ ARCHIVE (completed/reference only)
    ├── Recovery-Schedule-Week-1-Foundation-Repair.md
    ├── Day-2-Catchup-Auto-Scaling-Load-Balancing.md (specific deep dives)
    ├── Day-2-Database-Deep-Dive.md
    ├── Day-3-VPC-Networking-Deep-Dive.md
    ├── Day-4-Cheat-Sheet-S3-Security-Replication.md
    ├── Load-Balancer-Cheat-Sheet-ALB-NLB-GLB.md
    └── Redis-ElastiCache-Exam-Guide.md
```

---

## ✅ Benefits of This Structure

1. **Easy to find materials:** "Need to review VPC? → STUDY MATERIALS → Quick-Reference-Networking.md"
2. **Track progress:** Single file shows your journey, not scattered across 15 files
3. **Less clutter:** From 60+ files to ~25 organized files
4. **Clear purpose:** Each file has a clear role (study vs practice vs tracking)

---

## 🎯 Proposed Consolidations

### 1. Progress-Tracker.md (Consolidate 15 daily files)
```markdown
# AWS SAA-C03 Study Progress Tracker
**Exam Date:** January 5, 2026

## Week 1: Foundation
### Day 1 - EC2 & Compute (Nov 21)
- Initial quiz: 5/20 (25%) ❌
- Recovery: 16/20 (80%) ✅
- Key learnings: [...]

### Day 2 - Auto Scaling & Load Balancing (Nov 22)
- Quiz: 8/10 (80%) ✅
- Deep dive created: Day-2-Database-Deep-Dive.md
- Key learnings: [...]

[...continue for all days...]
```

### 2. Weakness-Tracker.md (Consolidate 3 files)
```markdown
# Weakness Tracking - Living Document

## Current Active Weaknesses
| Topic | Status | Last Quiz | Target |
|-------|--------|-----------|--------|
| S3 Storage Classes | 🟡 Improving | 75% | 90% |
| Aurora Multi-Master | 🔴 Need Review | 0% | 80% |

## Resolved Weaknesses (Archived)
| Topic | Date Resolved | Final Score |
|-------|--------------|-------------|
| VPC NACLs | Dec 8 | 100% ✅ |
| Auto Scaling Policies | Dec 8 | 100% ✅ |

## Weakness History
### S3 Storage Classes
- Dec 7: 45% → Identified as critical weakness
- Dec 8: 75% → Improving, still need polish on Glacier vs Standard-IA
```

---

## 🚀 Execution Plan

**Option A: Aggressive Cleanup (Recommended)**
- Delete all 18 duplicate/outdated files
- Consolidate 15 daily files → Progress-Tracker.md
- Consolidate 3 weakness files → Weakness-Tracker.md
- Rename quiz files with consistent pattern
- **Result:** 60 files → 25 files, organized structure

**Option B: Conservative Cleanup**
- Delete only HTML duplicates (12 files)
- Keep daily files separate but organized
- Keep weakness files separate
- **Result:** 60 files → 45 files, still cluttered

**Option C: Archive Only**
- Move old files to /archive/ subdirectory
- Don't delete anything
- **Result:** 60 files → 60 files (just hidden in folder)

---

## 🤔 Questions Before We Proceed

1. **Are you okay deleting HTML/PDF duplicates?** (You have the .md versions)
2. **Do you want consolidated progress tracking** or prefer separate day files?
3. **Should we create subdirectories** (STUDY MATERIALS, QUIZZES, etc.) or keep flat structure?
4. **Any files you specifically want to keep** that I'm proposing to delete?

---

## Next Steps

Once you approve the plan:
1. I'll delete the duplicates/outdated files
2. Create consolidated Progress-Tracker.md
3. Create consolidated Weakness-Tracker.md
4. Rename quiz files consistently
5. Update CLAUDE.md with new structure

**Estimated time:** 10-15 minutes
**Risk:** Low (we're deleting duplicates, not unique content)
