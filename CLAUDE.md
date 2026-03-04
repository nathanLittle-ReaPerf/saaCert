# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this repository.

## Repository Purpose

Study materials for the AWS Certified Solutions Architect - Associate (SAA-C03) exam.

**Result:** Passed — 789/1000 on March 2, 2026.

This repo is organized as a public reference for others preparing for the SAA-C03 exam.

## Repository Structure

```
├── README.md              # Entry point — start here
├── quick-reference/       # Per-service cheat sheets (8 files)
├── practice/              # Exam-style scenario questions (3 files)
├── decision-trees/        # When-to-use-what trees (3 files)
├── cheat-sheets/          # Specialized guides and comparison tables
├── flashcards/            # Printable flashcard sets by topic
├── study-plan/            # 56-day study schedule
└── archive/               # Daily drill notes, progress and weakness tracking
```

## Common Tasks

Since this repo contains only markdown study materials, there's no build process.

**Converting markdown to other formats (if pandoc is installed):**
```bash
pandoc quick-reference/Quick-Reference-Compute.md -o Quick-Reference-Compute.pdf
```

**Viewing files:** Any markdown viewer, VS Code preview, or GitHub rendering works fine.

## Content Guidelines (if contributing or extending)

- Quick reference guides: use comparison tables, include "Exam Pattern Recognition" sections
- Practice scenarios: follow format (scenario → 4 options → explanation in `<details>` tags)
  - Always explain why the correct answer is right AND why wrong answers are wrong
  - Use difficulty levels: Easy, Medium, Hard
- Decision trees: focus on the "which service do I use?" question with clear branch conditions
- Keep flashcards concise — one concept per card
