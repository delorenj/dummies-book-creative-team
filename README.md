# Old School Dummies Book Creative Team

Multi-agent workflow for generating publication-quality thematic technical manuals with AI illustrations and pandoc publishing pipeline.

## What It Does

Transforms dry technical topics into engaging, memorable documentation through:
- **Adversarial research** - Theme Designer vs Structure Architect debate visual identity
- **Parallel content generation** - N chapter agents write simultaneously
- **AI illustration** - Generates 6-8 thematic images (requires fal-text-to-image)
- **Publication assembly** - Professional front/back matter, index, glossary
- **Pandoc pipeline** - Convert to PDF/HTML/EPUB with theme-specific CSS
- **QA validation** - 95%+ quality score enforcement

## Installation

```bash
# Clone or copy to plugins directory
cp -r dummies-book-creative-team ~/.claude/plugins/

# Optional: Install fal-text-to-image for AI illustrations
# (Plugin works without it, just skips illustrations)
```

## Prerequisites

**Required:**
- Claude Code CLI
- Basic: markdown support

**Optional:**
- `fal-text-to-image` skill (for AI illustrations)
- `pandoc` (for PDF/HTML/EPUB conversion)
- `xelatex` or `pdflatex` (for PDF generation)

## Usage

```
Create a technical manual with theme:

topic: "ComfyUI for beginners"
theme: "Nyquil Cat - drowsy pharmaceutical guide"
chapters: 8
include_illustrations: true
output_format: ["markdown", "pdf", "html"]
```

Triggers:
- "create themed manual"
- "generate documentation with storytelling"
- Mentions themes: Victorian, steampunk, art deco, pharmaceutical, etc.

## Configuration

**Optional:** Create `.claude/dummies-book-creative-team.local.md` for fal.ai API key:

```markdown
# Dummies Book Creative Team Configuration

## fal.ai API Key (for illustrations)

FAL_KEY=your-api-key-here
```

## Workflow Phases

1. **Research** (15-30 min) - Adversarial theme design
2. **Content** (30-60 min) - Parallel chapter generation
3. **Assembly** (15-30 min) - Publisher synthesizes manual
4. **Illustration** (15-30 min) - AI image generation (optional)
5. **Building** (5-10 min) - Pandoc conversion (optional)
6. **QA** (15-30 min) - Quality validation

## Output

**Files Created:**
- `THE_COMPLETE_MANUAL.md` - Full manual (30K-50K words)
- `images/*.png` - AI illustrations (if enabled)
- `build/*.pdf|html|epub` - Converted formats (if enabled)
- `QA_FINAL_REPORT.md` - Quality metrics
- Individual chapter files for reference

## Examples

See `examples/` directory for:
- `comfyui-nyquil-cat/` - Pharmaceutical theme (47K words, 8 chapters, 8 images)
- `git-worktree-victorian/` - Railway theme (30K words, 6 chapters, 6 images)

## License

Public domain (Unlicense). Use freely.

## Credits

Created by Jarad DeLorenzo using multi-agent orchestration patterns.
