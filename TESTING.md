# Testing Guide: Dummies Book Creative Team Plugin

## Quick Verification

### 1. Installation Test

```bash
# Verify plugin structure
tree -L 2 ~/.claude/plugins/dummies-book-creative-team

# Expected output:
# ├── agents/ (7 files)
# ├── skills/thematic-doc-generator/
# ├── plugin.json
# ├── README.md
# └── .gitignore
```

### 2. Skill Triggering Test

Launch Claude Code and try these trigger phrases:

**Test A: Explicit theme request**
```
Create a themed manual about Docker using a Victorian steamship aesthetic
```
**Expected:** Skill loads, asks for parameters (chapters, audience, etc.)

**Test B: Generic documentation request**
```
Generate documentation with storytelling for my React hooks API
```
**Expected:** Skill loads and offers theme proposals

**Test C: Thematic keyword**
```
I need a pharmaceutical-themed guide to Kubernetes
```
**Expected:** Skill loads immediately with pharmaceutical theme

### 3. Agent Workflow Test

Start a simple manual to verify agent orchestration:

```
topic: "Python virtual environments"
theme: "Art Deco hotel management"
chapters: 3
include_illustrations: false
output_format: ["markdown"]
```

**Verify each phase completes:**
- ✅ Phase 1: THEME_PROPOSAL.md created
- ✅ Phase 2: CHAPTER_01_*.md, CHAPTER_02_*.md, CHAPTER_03_*.md created
- ✅ Phase 3: THE_COMPLETE_MANUAL.md assembled
- ✅ Phase 4: Skipped (illustrations disabled)
- ✅ Phase 5: Skipped (no PDF/HTML requested)
- ✅ Phase 6: QA_FINAL_REPORT.md with score ≥95%

### 4. Illustrator Agent Test (Optional)

**Prerequisites:**
- fal-text-to-image skill installed
- `.claude/dummies-book-creative-team.local.md` with FAL_KEY

**Test:**
```
topic: "Git basics"
theme: "Nautical navigation"
chapters: 2
include_illustrations: true
illustration_count: 4
```

**Verify:**
- ✅ images/01_cover_hero.png created
- ✅ images/02_character_portrait.png created
- ✅ IMAGE_INTEGRATION_GUIDE.md created
- ✅ All images ≥1024px resolution

**Graceful degradation test:**
Remove fal-text-to-image and verify plugin continues without failing.

### 5. Builder Pipeline Test (Optional)

**Prerequisites:**
- pandoc installed
- xelatex installed (for PDF)

**Test:**
```
output_format: ["markdown", "pdf", "html"]
```

**Verify:**
- ✅ build/manual.css generated with theme colors
- ✅ build/THE_COMPLETE_MANUAL.pdf created
- ✅ build/THE_COMPLETE_MANUAL.html created
- ✅ BUILD_REPORT.md documents all commands

### 6. Quality Validation Test

**Test QA rejection:**
Create a manual with deliberately missing chapters:
```
chapters: 5
# But delete CHAPTER_03_*.md before publisher runs
```

**Expected:** QA score <90%, REJECTED status in report

**Test QA approval:**
Complete normal manual generation.

**Expected:** QA score ≥95%, APPROVED status

## Common Issues

### Issue: Skill doesn't trigger
**Solution:** Check trigger phrases. Use explicit "create themed manual" or mention theme keywords.

### Issue: Illustrator skipped unexpectedly
**Solution:** Verify fal-text-to-image installed:
```bash
cc --list-skills | grep fal-text-to-image
```

### Issue: Pandoc conversion fails
**Solution:** Check pandoc installation:
```bash
pandoc --version
which xelatex
```

### Issue: QA fails with low score
**Solution:** Check QA_FINAL_REPORT.md for specific issues. Common causes:
- Missing chapters
- Broken theme consistency
- Placeholder text (TODO, FIXME)

## Performance Benchmarks

**Typical execution times:**
- 3-chapter manual, no illustrations: ~45-60 minutes
- 6-chapter manual with illustrations: ~90-120 minutes
- 8-chapter manual with full pipeline: ~120-180 minutes

**Expected output sizes:**
- Manual: 20K-50K words
- Images: 15-30 MB total (6-8 images)
- PDF: 2-5 MB
- Full output directory: 50-100 MB

## Success Criteria

Plugin is working correctly when:
- ✅ Skill triggers on theme-related phrases
- ✅ All 6 phases execute in sequence
- ✅ QA score ≥95% for complete manuals
- ✅ Illustrator gracefully skips if fal unavailable
- ✅ Builder creates all requested formats
- ✅ No crashes or unhandled errors

## Next Steps

After verification:
1. Create your first real manual
2. Experiment with different themes
3. Test optional features (GitHub Pages, Algolia)
4. Provide feedback at github.com/anthropics/claude-code/issues
