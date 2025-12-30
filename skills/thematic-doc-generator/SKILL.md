---
name: thematic-doc-generator
description: Generate comprehensive, publication-quality technical manuals with thematic storytelling using multi-agent orchestration. Use when user asks for themed documentation, narrative technical guides, memorable training materials, or mentions themes like Victorian, steampunk, art deco, medieval, pharmaceutical, or nautical combined with technical topics. Triggers on phrases like "create themed manual", "documentation with storytelling", or requests to make docs "more engaging" or "memorable".
---

# Thematic Technical Documentation Generator

Generate publication-quality technical manuals with thematic storytelling through multi-agent orchestration.

## What This Does

Transform dry technical topics into engaging, memorable documentation using:
- **Multi-agent parallel workflow** - 2 research + N chapter + 3 assembly agents
- **Thematic metaphor system** - Complex concepts through memorable metaphors
- **AI-generated illustrations** - Custom artwork matching theme (optional)
- **Professional structure** - Front matter, back matter, index, glossary
- **Publication pipeline** - Pandoc conversion to PDF/HTML/EPUB (optional)
- **Quality enforcement** - 95%+ QA score requirement

## When to Use

Invoke this skill when creating:
- Technical documentation with personality and narrative
- Training materials that need to be memorable
- Onboarding guides that people actually want to read
- Comprehensive technical manuals (20K+ words)
- Documentation that stands out from typical reference material

## How It Works

### 6-Phase Workflow

**Phase 1: Adversarial Research** (15-30 min)
- Theme Designer proposes visual identity and metaphor system
- Structure Architect critiques and designs chapter structure
- Output: `THEME_PROPOSAL.md` (synthesized specification)

**Phase 2: Parallel Content Generation** (30-60 min)
- N chapter agents write simultaneously (self-contained chapters)
- Each follows theme proposal for consistency
- Output: `CHAPTER_01_*.md` through `CHAPTER_N_*.md`

**Phase 3: Publisher Assembly** (15-30 min)
- Synthesizes chapters into complete manual
- Adds front matter (foreword, TOC, usage guide)
- Adds back matter (appendices, index, glossary, statistics)
- Output: `THE_COMPLETE_MANUAL.md`

**Phase 4: Illustration Generation** (15-30 min, optional)
- Generates 6-8 AI images via fal-text-to-image
- Maintains theme consistency
- Gracefully skips if fal unavailable
- Output: `images/*.png` + integration guides

**Phase 5: Builder Pipeline** (5-10 min, optional)
- Generates theme-specific CSS from THEME_PROPOSAL.md
- Converts MD → PDF/HTML/EPUB using pandoc
- Optional: GitHub Pages deployment, Algolia search
- Output: `build/` directory with all formats

**Phase 6: QA Validation** (15-30 min)
- Validates structural integrity, theme consistency, technical accuracy
- Scores deliverables (minimum 95% required)
- Output: `QA_FINAL_REPORT.md` with APPROVED/REJECTED

## Parameters

Configure the manual generation:

```yaml
topic: "Technical subject to document"
theme: "Thematic metaphor/aesthetic (Victorian, pharmaceutical, etc.)"
audience: "beginner|intermediate|expert practitioners"
chapters: 8  # 5-12 range
word_count: {min: 2500, max: 3500}  # per chapter
include_illustrations: true  # requires fal-text-to-image
illustration_count: 6  # 3-10 range
character_guide: true  # mascot narrator
output_dir: "./docs/manual"
output_format: ["markdown", "pdf", "html", "epub"]
```

## Examples

### Example 1: Nyquil Cat ComfyUI Manual

```
topic: "ComfyUI for absolute beginners"
theme: "Nyquil Cat - drowsy pharmaceutical guide"
chapters: 8
include_illustrations: true
```

**Output:** 47,633 words, 8 chapters, 8 AI illustrations, 97.8% QA score

### Example 2: Victorian Railway Git Manual

```
topic: "git worktree safety and best practices"
theme: "Victorian railway operations (1880s-1920s)"
chapters: 6
character: "Cornelius Worktree, Chief Railway Dispatcher"
```

**Output:** 30,606 words, 6 chapters, 6 illustrations, 98.4% QA score

## Execution

When user requests themed manual:

1. **Validate fal availability** (if illustrations requested)
2. **Create output directory**
3. **Launch Phase 1** - Theme Designer + Structure Architect agents in parallel
4. **Launch Phase 2** - N chapter agents in parallel
5. **Launch Phase 3** - Publisher agent (sequential)
6. **Launch Phase 4** - Illustrator agent (optional, sequential)
7. **Launch Phase 5** - Builder agent (optional, sequential)
8. **Launch Phase 6** - QA agent (sequential, final gate)

**Do NOT skip illustrator phase** - validate fal availability, prompt user if missing.

**Total execution:** ~90-180 minutes for 8-chapter manual with illustrations.

## Quality Standards

Enforce these minimums:
- Overall QA score ≥ 95%
- Zero critical issues
- Zero major issues
- Theme consistency: 100%
- Technical accuracy: 100%
- Chapter delivery: ≥75%

## Output Structure

```
output_dir/
├── THEME_PROPOSAL.md
├── THE_COMPLETE_MANUAL.md (primary deliverable)
├── CHAPTER_01_*.md through CHAPTER_N_*.md
├── images/
│   ├── 01_cover_hero.png
│   ├── 02_character_portrait.png
│   └── ...
├── build/ (if builder enabled)
│   ├── THE_COMPLETE_MANUAL.pdf
│   ├── THE_COMPLETE_MANUAL.html
│   ├── THE_COMPLETE_MANUAL.epub
│   └── manual.css
├── IMAGE_INTEGRATION_GUIDE.md
├── ILLUSTRATION_SUMMARY.md
├── QA_FINAL_REPORT.md
└── MANUAL_README.md
```

## Dependencies

**Required:**
- Task tool (for agent orchestration)
- Read, Write, Edit tools
- Glob, Grep tools

**Optional:**
- fal-text-to-image skill (for illustrations)
- pandoc (for PDF/HTML/EPUB)
- xelatex or pdflatex (for PDF generation)

## Agent References

All agent prompts in `prompts/` directory:
- `research-theme.md` - Theme Designer
- `research-structure.md` - Structure Architect
- `chapter-orchestrator.md` - Chapter Writer (generic)
- `publisher.md` - Publisher
- `illustrator.md` - Illustrator
- `builder.md` - Builder (pandoc pipeline)
- `qa-orchestrator.md` - QA Validator

## Templates

Pandoc templates in `templates/` directory:
- `dummies-book.css` - Theme-specific CSS generation template
- `pandoc-template.tex` - LaTeX template for PDF
- `metadata-template.yaml` - Pandoc metadata
- `cover-template.svg` - Editable cover design

## Success Criteria

Manual is complete when:
- QA score ≥ 95%
- All planned chapters delivered (or gracefully noted as missing)
- Theme consistency validated
- Technical accuracy verified
- No placeholders or TODOs
- Ready for publication or distribution
