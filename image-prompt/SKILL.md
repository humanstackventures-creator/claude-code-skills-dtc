---
name: image-prompt
description: >
  Craft optimized prompts for AI image generation in Google AI Studio, especially
  Nano Banana 2 (gemini-3.1-flash-image-preview) and Nano Banana Pro. Trigger
  whenever the user wants help describing, writing, or improving a prompt for
  AI image generation — including vague requests like "help me make a good image,"
  "I want to generate AI art," "write a description of this scene," "how do I
  prompt for X," or any mention of Nano Banana, Gemini image generation, Google
  AI Studio images, or AI image creation. Also trigger on /image-prompt. When in
  doubt, trigger — users rarely phrase these requests precisely.
---

# Nano Banana Prompt Generator

Help users craft high-quality prompts for Google AI Studio's Nano Banana image
models. The core job is translating a vague idea into clear creative direction —
the same way you'd brief a human photographer or illustrator.

## Choosing a Model

Both models live in Google AI Studio and use the same prompting approach.

| | **Nano Banana 2** | **Nano Banana Pro** |
|---|---|---|
| **Model ID** | `gemini-3.1-flash-image-preview` | `gemini-3-pro-image-preview` |
| **Speed** | Fast, optimized for high volume | Slower, highest fidelity |
| **Best for** | Iteration, everyday use, API workflows | Maximum detail, precise real-world rendering |
| **Resolution** | Up to 4K (specify explicitly) | Up to 4K |
| **Reference images** | Up to 14 | Up to 14 (6 with high fidelity) |
| **Free tier** | Yes (via Google AI Studio) | Paid only |
| **Conversational editing** | Yes | Yes |

**Default to Nano Banana 2** for almost everything — it's faster, free-tier eligible,
and handles complex prompts well. Recommend Pro only when the user explicitly needs
maximum visual fidelity (architectural renders, premium product photography, hero
imagery for marketing materials).

## Core Principles

Both models understand intent, physics, and composition. They reward **clear creative
direction** over keyword lists.

**Write like you're briefing an artist, not filing a search query.**

- BAD: `dog, park, sunset, 4k, realistic, cinematic`
- GOOD: `A golden retriever bounding through a sun-dappled park at golden hour, low angle, shallow depth of field`

**Specificity matters more than length.** One precise detail outperforms five vague ones.
- Instead of "a woman" → "a sophisticated elderly woman in a vintage Chanel-style tweed suit"
- Include materials: "matte finish," "brushed steel," "weathered leather," "soft velvet"

**Context about purpose improves the output.** "A hero image for a premium coffee brand's
website" tells the model to infer professional lighting, composition, and polish. Include
it when you have it.

**Edit, don't re-roll.** When an image is mostly right, request specific conversational
changes rather than starting from scratch.

## Workflow

### Step 1: Understand the Vision

Ask the user what they want. If the description is thin, identify the 2–3 highest-leverage
gaps and ask about those — not all of them at once.

**Priority order for what to ask first:**

1. **Subject** — what's the actual focus? (if unknown, ask this first)
2. **Purpose/use case** — where will this be used? (unlocks composition, tone, and style)
3. **Mood/atmosphere** — what should it feel like?
4. **Lighting** — golden hour, dramatic side-light, soft window light, neon, etc.
5. **Style** — photorealistic, cinematic, watercolor, editorial, retro, anime, etc.
6. **Composition** — close-up, wide shot, bird's eye, rule of thirds, etc.
7. **Resolution/aspect ratio** — 16:9, 9:16, 1:1, 4K, etc.
8. **Text** — any words to render in the image? (always put in quotation marks)

If the user gives you a lot upfront, skip the questions and build the prompt — then
offer to refine.

### Step 2: Build the Prompt

Construct naturally flowing descriptive language — not a template, not a list.
Think of the best example prompts below and write toward that quality.

**Techniques that reliably improve output:**

- **Camera language**: "wide establishing shot," "tight close-up," "over-the-shoulder," "Dutch angle," "shallow depth of field"
- **Lighting specifics**: "Rembrandt lighting," "backlit with rim light," "soft window light from the left," "dramatic chiaroscuro"
- **Material and texture**: "brushed aluminum," "hand-knit wool," "cracked leather," "translucent frosted glass"
- **Color direction**: "muted earth tones," "high-contrast complementary colors," "monochromatic blue palette"
- **Text rendering**: Put exact text in quotation marks — Nano Banana has strong text legibility. Specify style if needed: "bold sans-serif," "handwritten script," "retro neon sign"
- **Resolution**: Explicitly state when it matters — Nano Banana 2 supports up to 4K

### Step 3: Present the Prompt

Output the prompt in a clearly marked block:

```
PROMPT:
─────────────────────────────────────────────
[The generated prompt]
─────────────────────────────────────────────
```

Only explain your choices if the user seems new to image generation or if there's
something non-obvious worth flagging. Don't default to a rationale block — most
users just want to copy the prompt and go.

### Step 4: Offer to Refine

After the prompt, invite iteration:

> "Want to adjust anything — style, mood, composition, lighting?"

Keep iterating until the user is satisfied. Small targeted edits beat full rewrites.

## Special Capabilities

Surface these when relevant:

**Conversational editing (both models)**
After generating an image in AI Studio, the user can type natural-language edits and
the model adjusts physics, lighting, and context automatically:
- "Change the sunny day to a rainy night"
- "Remove the person in the background, add a potted plant"
- "Make the text neon blue instead of white"

**Reference images**
Both models support up to 14 reference images for style and character consistency.
- "Use the uploaded images as a strict style reference"
- "Keep the character from Image 1 but place them in the setting from Image 2"
- "Keep facial features exactly the same as the reference"

**Dimensional translation**
- Hand-drawn sketches → photorealistic renders
- Floor plans → 3D room visualizations
- Wireframes → high-fidelity UI mockups

**Structural control**
Users can upload layout sketches to control where objects and text appear — useful
for design work, posters, and UI mockups.

**Image search grounding (Nano Banana 2)**
Nano Banana 2 supports Google Image Search grounding, enabling the model to pull
real-world visual reference when generating images.

## Anti-Patterns to Avoid

Watch for these and fix them when building or reviewing prompts:

- **Tag soup** — disconnected keywords instead of sentences → rewrite as natural description
- **Vague subjects** — "a person," "a building" → add specific details, materials, context
- **No lighting or mood** — these dramatically affect output → always include
- **Missing purpose** — omitting use case when it's known → include it
- **Contradictory details** — over-stuffed prompts that fight each other → cut and focus

## Example Prompts

Aim for this level of specificity and naturalness.

**Product photography:**
> A flat lay of artisanal coffee beans spilling from a matte black ceramic cup onto a weathered oak table, soft directional window light from the upper left, warm earth tones with deep shadows, shot from directly above, styled for a premium coffee brand's Instagram feed.

**Portrait:**
> A cinematic medium close-up of a jazz musician mid-performance, eyes closed, sweat glistening under warm amber stage lighting, shallow depth of field with bokeh from string lights in the background, shot on what looks like 35mm film with natural grain.

**Text-heavy design:**
> A vintage-style concert poster with the text "MIDNIGHT REVERIE" in bold art deco typography at the top, a silhouette of a saxophone player against a deep indigo night sky with a full moon, "Live at The Blue Note — March 15, 2026" in smaller elegant serif type at the bottom, gold and navy color palette.

**Fantasy / illustration:**
> A lush watercolor illustration of a hidden forest library, towering bookshelves made from living trees with glowing mushrooms as reading lamps, a cozy armchair draped in moss-green velvet, shafts of golden sunlight filtering through the canopy above, whimsical and enchanting atmosphere.
