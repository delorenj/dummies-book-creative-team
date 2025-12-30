# fal-text-to-image Integration Protocol

Integration protocol for the Illustrator agent to generate thematic AI artwork.

---

## Pre-flight Check

Before illustration phase, verify:

```bash
# Check skill exists
test -d /home/delorenj/.claude/skills/fal-text-to-image/

# Check API key is set
test -n "$FAL_KEY"
```

If either check fails, skip illustration phase and proceed text-only.

---

## Invocation Protocol

### Method 1: Skill Invocation (Recommended)

```bash
# Via Claude Code skill system
Use skill: fal-text-to-image
  prompt: "{FULL_PROMPT}"
  model: "{MODEL_SELECTION}"
  output: "{OUTPUT_PATH}"
```

### Method 2: Direct API (Fallback)

```python
import fal_client
import os

result = fal_client.subscribe(
    "fal-ai/{MODEL_ENDPOINT}",
    arguments={
        "prompt": prompt_text,
        "image_size": "1024x1024",
        "guidance_scale": 7.5,
        "num_inference_steps": 50
    }
)

image_url = result["images"][0]["url"]
```

---

## Prompt Template

Use this structure for consistent themed images:

```
{SUBJECT_DESCRIPTION},
{CHARACTER_OR_FOCAL_POINT},
authentic {ERA_PERIOD} {CULTURAL_CONTEXT} period details,
{VISUAL_STYLE} style,
{COLOR_PALETTE_DESCRIPTION}, {HEX_COLORS},
{MATERIAL_ELEMENTS},
{LIGHTING_DESCRIPTION}, atmospheric,
professional, highly detailed, {QUALITY_MODIFIERS},
{ACCURACY_REQUIREMENTS},
{COMPOSITION_FRAMING}
```

### Template Variables

| Variable | Purpose | Example |
|----------|---------|---------|
| `SUBJECT_DESCRIPTION` | What the image depicts | "Victorian railway signal tower interior" |
| `ERA_PERIOD` | Time period | "1880s-1920s British" |
| `VISUAL_STYLE` | Artistic style | "vintage photography" |
| `COLOR_PALETTE_DESCRIPTION` | Colors from theme | "deep railway green, brass gold" |
| `HEX_COLORS` | Exact hex codes | "#2C5530, #B8860B" |
| `QUALITY_MODIFIERS` | Quality indicators | "cinematic, 8K resolution" |

---

## Standard Image Set

Generate these 6 images minimum:

| # | Type | Dimensions | Model | Purpose |
|---|------|-----------|-------|---------|
| 1 | Cover/Hero | 2752x1536 | flux-pro/v1.1-ultra | Cover art |
| 2 | Character Portrait | 1024x1024 | flux-pro/v1.1-ultra | Narrator visual |
| 3 | Technical Diagram | 1024x1820 | recraft/v3 | Architecture |
| 4 | Concept Visualization | 1920x1080 | flux-pro/v1.1-ultra | Key metaphor |
| 5 | Safety/Warning Poster | 1024x1820 | recraft/v3 | Best practices |
| 6 | Decorative Element | 2752x1536 | recraft/v3 | Chapter dividers |

Optional (for 8-image set):
- 7: Detailed Scene / Action Shot
- 8: Thematic Background / Texture

---

## Model Selection

| Model | Best For | Notes |
|-------|----------|-------|
| `flux-pro/v1.1-ultra` | Photorealistic scenes, characters | Highest quality |
| `recraft/v3/text-to-image` | Technical diagrams, posters with text | Better typography |
| `flux-2` | Quick generation, fallback | Faster, lower quality |

---

## Error Handling

```
IF generation fails:
├─ "FAL_KEY not set"
│  └─ Exit with error: "Configure FAL_KEY"
├─ "Rate limit exceeded"
│  └─ Wait 60s, retry (max 2 attempts)
│     If still fails: Skip image, continue
├─ "Timeout"
│  └─ Retry with flux-2 (faster model)
└─ "Model not found"
   └─ Fallback to flux-2
```

### Recovery Strategy

- Per-image timeout: 300 seconds
- Retry logic: Up to 2 retries
- Minimum required: 5/6 images
- Never block manual on image failures

---

## Fallback Strategy (No Images)

If fal-text-to-image unavailable:

1. Skip illustration phase entirely
2. Generate manual without images
3. Add "Visual assets pending" notes
4. Document in QA report
5. Publisher proceeds text-only
6. Quality score reduced by 10 points

---

## Output Documentation

Each image must have:

1. **Filename**: `images/{NN}_{type}_{subject}.png`
2. **Prompt used**: Exact prompt text
3. **Model**: Which model generated it
4. **Alt text**: 80+ characters for accessibility
5. **Regeneration instructions**: Complete command

### Integration in Manual

```markdown
![{Description}](images/01_cover_hero.png)
*Alt: {Accessibility description}*
```

---

## Success Criteria

- [ ] All 6 priority images generated
- [ ] Theme consistency verified
- [ ] Zero anachronisms detected
- [ ] Alt text for all images
- [ ] Integration guide complete
- [ ] Total storage < 50 MB
