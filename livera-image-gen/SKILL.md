---
name: livera-image-gen
description: >
  Generate brand-aligned image assets for Livera using nano-banana-2 CLI.
  Trigger whenever the user wants to create a Livera visual — ad creatives,
  product mockups, social posts, UGC-style photos, transparent assets, or
  any image generation request mentioning Livera. Also trigger on
  "generate a Livera asset", "create a Livera ad", "make a Livera image",
  "product mockup for Livera", "UGC visual for Livera", "social post for Livera",
  or /livera-image. This skill is Livera-only — do not use for Happy Aging
  or Goldie assets.
---

# Livera Image Generator

Generate on-brand Livera assets using the `nano-banana` CLI (Gemini image generation).
Every prompt you build must encode Livera's brand DNA. Never ask the user to supply
brand details — you already know them.

## Prerequisites

The nano-banana CLI must be installed and linked (`bun link` from `~/tools/nano-banana-2`).
A Gemini API key must be set in `~/.nano-banana/.env`.
Reference images live in `~/livera-assets/refs/`.
All output goes to `~/livera-assets/output/`.

If any prerequisite is missing, tell the user what to run and stop.

## LIVERA BRAND DNA — bake into every prompt

### Colors
- **Primary:** Bright Orange `#FF6B09` — dominant in every asset
- **Light Blue:** `#BAE2FC` — secondary, backgrounds/gradients
- **Lilac:** `#E2C5E6` — accent, sparingly
- **Beige:** `#EBE9E4` — neutral base, gradient target
- **White:** `#FFFFFF` — logo and text on orange

### Signature visual elements
- Orange-to-Beige gradient backgrounds (the brand "filter")
- Orange-to-LightBlue gradient (hero brand pattern)
- Blurred abstract orange/blue/lilac washes for backgrounds
- Concentric orange circles on beige ("how it works" pattern)
- Bold rounded lowercase "livera" wordmark — white on orange, or orange on beige

### Photography style
- Natural light, warm tones, blue sky backgrounds
- Hand holding gummy or product in palm
- Close-up macro of deep red gummy texture
- Oranges/citrus alongside product
- Real women 25-45, lifestyle-forward, not clinical

### Aesthetic: "Bright Expert"
Lemme's energy meets Dose's ingredient credibility.
Bold, warm, sunny, optimistic. Never clinical. Never luxury theater.
Think: orange California morning, not green pharmacy.

### NEVER generate
- Dark/moody/gothic aesthetics
- Clinical white lab photography
- Overly luxury/gold/prestige vibes
- Generic stock photo energy
- Anything that looks like a pharmaceutical ad

### Brand prompt boilerplate
Always weave into prompts (don't paste verbatim — integrate naturally):

> Livera brand aesthetic: bright orange #FF6B09 primary color, warm and energizing,
> "Bright Expert" personality (Lemme energy meets Dose credibility),
> never clinical or pharmaceutical, premium DTC wellness,
> beige #EBE9E4 and light blue #BAE2FC as secondary colors,
> target audience health-conscious women 25-45.

---

## WORKFLOW

### Step 1: Identify the asset type

Map the user's request to one of these categories:

| Category | Description |
|---|---|
| **Ad creative** | Meta/TikTok paid ads — competitor comparison, symptom hook, UGC-style |
| **Product mockup** | Hero product shot, ingredient macro, citrus lifestyle flat lay |
| **Social post** | IG feed, TikTok thumbnail, Stories |
| **Transparent asset** | Gummy cluster or brand element with no background (for video/Remotion) |

If unclear, ask one question to disambiguate.

### Step 2: Select dimensions and model

**Dimensions:**

| Use case | Flags |
|---|---|
| Meta feed ad | `-s 2K -a 4:5` |
| Meta story / TikTok | `-s 2K -a 9:16` |
| IG square | `-s 2K -a 1:1` |
| Widescreen / hero banner | `-s 2K -a 16:9` |

**Model selection:**

| Use case | Flag |
|---|---|
| Rapid ideation / testing | *(default — flash)* |
| UGC-style (want imperfect) | *(default — flash)* |
| Hero ad creative | `--model pro` |
| Final production asset | `--model pro` |

Default to flash unless the user asks for hero/final quality or you judge the asset
warrants pro fidelity.

### Step 3: Build the prompt

Write the prompt as natural creative direction (not keyword soup). Always include:

1. **Livera brand colors** — at minimum `#FF6B09` orange, plus relevant secondaries
2. **Aesthetic direction** — "Bright Expert", DTC wellness, warm/energizing
3. **Specific subject** — what's in the frame, with materials and textures
4. **Lighting/mood** — warm natural light, California morning, etc.
5. **Anti-patterns** — never clinical, never pharmaceutical, never dark/moody
6. **Text** — if any text appears, put exact words in quotation marks

### Step 4: Build and run the command

Construct the `nano-banana` command:

```bash
nano-banana "YOUR PROMPT HERE" \
  -r ~/livera-assets/refs/product.jpg \
  -s 2K -a ASPECT_RATIO \
  -o OUTPUT_NAME \
  -d ~/livera-assets/output
```

Rules:
- Always include `-r ~/livera-assets/refs/product.jpg` if the file exists
- Always output to `-d ~/livera-assets/output`
- Use descriptive `-o` names: `livera-{type}-{descriptor}` (e.g., `livera-ad-competitor-swap`)
- Add `-t` flag for transparent assets
- Add `--model pro` for hero/production assets
- Check that `~/livera-assets/refs/` exists and has files before adding `-r` flags

Before running, show the user the full command and prompt. Then execute it.

### Step 5: Present the result

After the command completes:
1. Tell the user where the file was saved
2. Open the image so they can see it: `open PATH_TO_IMAGE`
3. Offer refinements: "Want to adjust anything — copy, composition, colors, model?"

---

## ASSET TYPE PLAYBOOK

Use these as starting templates. Adapt and improve based on the user's specific request.

### Ad Creatives

**Competitor comparison (Swap That / For This)**
Split-frame: LEFT shows competitor in muted/clinical tones, RIGHT shows Livera
product on warm orange gradient with benefit copy. Orange #FF6B09 dominant on right.

**Symptom hook**
Bold orange background, large white text with the symptom/hook, product shot centered,
stat badge at bottom. DTC energy, not clinical.

**UGC-style lifestyle**
Authentic candid photo: woman 25-35 holding gummies, natural light, blue sky,
California morning vibe. Not overly posed.

### Product Mockups

**Hero product shot**
Product floating on orange-to-beige gradient, dramatic soft lighting, gummies
spilling artfully beside bag.

**Ingredient macro**
Extreme close-up of deep red gummies, translucent jewel-like quality, raking light,
orange gradient background.

**Citrus lifestyle flat lay**
Overhead: product surrounded by halved oranges, ginger, milk thistle, dandelion greens.
Warm beige surface, natural light.

### Social Posts

**IG feed**
Abstract orange/blue gradient wash background, product sharp in foreground,
white bold text overlay. Editorial DTC aesthetic.

**TikTok thumbnail**
Bright orange background, large expressive white text (hook/thread style),
product in corner. Energetic, playful, not clinical.

### Transparent Assets

Add `-t` flag. Describe subject cleanly with "no background" in prompt.
Good for gummy clusters, brand pattern elements, product cutouts.

---

## PROVEN AD FRAMES

Livera's ad angles — reference these when building ad prompts:

1. **Competitor comparison:** "Swap That / For This" (e.g., Dose liquid shot vs Livera gummy — exploit Livera's lower friction format)
2. **Symptom connection:** Lead with pain (bloating, crashes, brain fog) then product as solution
3. **Routine/habit:** "One gummy. Every morning. That's the whole routine."
4. **Price-first / deal frame:** Lead with `~~$X~~ $89` or "90 days · $89" — Dose's highest-volume pattern; generate a clean product image for text to be overlaid in Canva
5. **Value-stack receipt:** Itemized order summary showing bundled value → $89 total. Hook: "How is this real?" Generate clean white background for this format.
6. **Ingredient flat lay:** Product surrounded by oranges, milk thistle, dandelion greens on beige surface — Dose uses this heavily; Livera's ingredient story is untold

## DOSE COMPETITIVE INTELLIGENCE (reference for all ad prompts)

Dose ($90/mo liquid shot, direct competitor) runs 10+ creative frames on the same offer. Key patterns to reference and counter:
- They always use strikethrough anchoring (`~~$90~~ $63`) — Livera should use `~~$X~~ $89`
- Their benefit bullets never change: "Clinically-backed · 2oz daily shot · Tastes like orange juice · Boosts energy"
- Livera's counter-angle: gummies = zero prep, no refrigeration, lower daily friction vs liquid shot
- Full intel: `/Users/lucas/Documents/Obsidian/Livera/wiki/marketing/instagram/competitor-intel/dose.md`

---

## COST REFERENCE

| Asset type | Size | Approx cost |
|---|---|---|
| Quick concept | 512 flash | ~$0.045 |
| Standard social | 1K flash | ~$0.067 |
| Production-ready | 2K flash | ~$0.101 |
| Hero / campaign | 2K pro | ~$0.201 |
| Ultra-high res | 4K pro | ~$0.302 |

Check spend anytime: `nano-banana --costs`

---

## IMPORTANT RULES

- This skill is **Livera-only**. Never use for Happy Aging or Goldie.
- Keep output organized in `~/livera-assets/output/`.
- Always verify refs directory has files before adding `-r` flags.
- When the user's request is vague, default to the closest playbook template and iterate.
- For batch generation (multiple assets), run commands sequentially and present each result.
- **NEVER render readable text in generated images.** AI hallucoinates supplement labels, USDA badges, ingredient lists, flavor names, and headline copy. Always include in prompts: *"Do not render any readable text. Leave the top third clean for copy added in post. Product label should face slightly away or be softly out of focus."* All text overlays (headline, CTA, price, dates) are added in Canva or Figma after generation. Confirmed failure mode: World Liver Day ad April 2026 — label text corrupted, wrong gummy count, garbled badge.
