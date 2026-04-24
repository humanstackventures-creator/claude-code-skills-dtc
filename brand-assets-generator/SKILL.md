---
name: brand-assets-generator
description: Generate professional, studio-quality product photoshoot images using Gemini 2.5 Flash (Nano Banana 2) image generation. Trigger this skill whenever the user uploads a product photo and wants to generate brand assets, product shots, ad creatives, or photoshoot variations. Also trigger when the user says "generate a shot", "create a product photo", "make a brand asset", "shoot this product", "give me 3 concepts", or any request involving visual asset generation from an uploaded product image. This is the primary skill for all visual content production across any brand. When in doubt, trigger it.
---

# Brand Assets Generator

Generates 1–3 professional product photoshoot images using **Gemini 2.5 Flash Image** (aka Nano Banana 2) — the same quality bar as Runway-grade editorial and studio photography, but fully controllable via prompt.

---

## How This Skill Works

1. **Receive** the uploaded product image + optional brief from the user
2. **Ask 2 clarifying questions** (product name/what it is + shot mood/vibe)
3. **Propose 1–3 shot concepts** for the user to choose from
4. **Generate** the selected concept(s) using Nano Banana 2 via the Gemini API
5. **Accept tweaks** — the user can iterate on lighting, background, mood, angle, etc.

---

## Step 1 — Intake

When triggered, check what the user has provided:

- ✅ Product photo uploaded → proceed
- ✅ Text description of product → proceed (describe product in the generation prompt)
- ❌ Neither → ask: *"Can you upload a photo of the product, or describe it for me?"*

Then ask exactly **2 questions** (no more):

**Q1 — Product identity:**
> "What's this product called and what does it do? (e.g. 'Livera — liver detox gummy supplement')"

**Q2 — Shot mood/vibe:**
> "What's the vibe you're going for? (e.g. clean studio, lifestyle scene, editorial/bold, or describe it)"

If the user has already provided this info in their message, skip the relevant question.

---

## Step 2 — Propose Shot Concepts

Based on the intake, generate **1–3 shot concepts** as brief descriptions. Format like this:

```
Here are 3 shot concepts for [Product Name]:

**Concept A — [Name]**
[2-sentence description of the scene, mood, lighting, background, key visual elements]

**Concept B — [Name]**
[2-sentence description]

**Concept C — [Name]** *(optional, only if you have a strong third idea)*
[2-sentence description]

Which concept(s) should I generate? You can also ask me to tweak any of them before generating.
```

**Concept variety rules:**
- Always include at least 1 clean/studio shot and 1 lifestyle or editorial shot
- Vary the mood meaningfully — don't propose 3 similar shots
- Match the brand's implied positioning (premium vs approachable vs clinical, etc.)

---

## Step 3 — Generate with Nano Banana 2

Once the user selects a concept, build the Gemini image generation prompt and call the API.

### API Call

```javascript
// Use Gemini 2.5 Flash image generation (Nano Banana 2)
const response = await fetch("https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash-exp-image-generation:generateContent", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "x-goog-api-key": process.env.GEMINI_API_KEY
  },
  body: JSON.stringify({
    contents: [{
      parts: [
        {
          inline_data: {
            mime_type: "image/jpeg", // or image/png
            data: "<base64_encoded_product_image>"
          }
        },
        {
          text: "<YOUR_GENERATION_PROMPT>"
        }
      ]
    }],
    generationConfig: {
      responseModalities: ["image", "text"],
      temperature: 1.0
    }
  })
});
```

> **Note:** If `GEMINI_API_KEY` is not set in the environment, prompt the user: *"Can you add your Gemini API key as `GEMINI_API_KEY` in your environment?"*

---

## Prompt Architecture

Build every generation prompt using this structure:

```
[SHOT TYPE] — [PRODUCT DESCRIPTION]

SCENE: [Background, environment, surface, props]
LIGHTING: [Type, direction, quality, color temperature]
CAMERA: [Angle, distance, depth of field]
MOOD: [Overall atmosphere and feeling]
PRODUCT: [Exact placement, orientation, visibility requirements]
STYLE: [Rendering style — photorealistic / editorial / 3D / etc.]
OUTPUT: Photorealistic product photography, high resolution, professional studio quality. No text overlays. Product label must be fully visible and legible.
```

---

## Shot Type Reference Library

Use these as building blocks — mix and adapt freely:

### Clean Studio
```
Shot type: Hero studio shot on seamless background
Scene: Pure white or off-white seamless backdrop, minimal negative space
Lighting: Soft box key light at 45°, fill light opposite, subtle rim light from behind
Camera: Eye-level or slight high angle, tight crop with product filling 60% of frame
Mood: Clinical precision, premium, trustworthy
Style: Hyperrealistic product photography, sharp focus throughout
```

### Lifestyle / In-Context
```
Shot type: Lifestyle product placement
Scene: [Relevant environment — kitchen counter, bathroom shelf, wooden table, etc.], natural daylight from window
Lighting: Golden hour natural light, warm tones, soft shadows
Camera: Slightly overhead or 3/4 angle, product in foreground with blurred background (f/2.8 equivalent)
Mood: Aspirational daily ritual, warm, inviting
Style: Editorial lifestyle photography, cinematic color grading
```

### Editorial / High-Concept
```
Shot type: Editorial brand shot
Scene: Bold color backdrop, sculptural props or abstract elements
Lighting: Dramatic directional lighting, strong shadows, high contrast
Camera: Unexpected angle (macro, extreme overhead, worm's eye)
Mood: Bold, premium, fashion-forward, stops the scroll
Style: High-fashion editorial, vibrant, high contrast
```

### Glass / Translucent Material
```
Shot type: Material transformation — glass retexture
Retexture the product with the following aesthetic:
Material: translucent glass with iridescent effects
Surface: smooth, polished with subtle reflections and refractive effects
Lighting: studio HDRI, angled top-left key with ambient fill, accent colors [blue, green, purple], reflections + refractions + bloom
Background: pure black with vignette
Post: chromatic aberration, glow, high contrast, sharp details
Style: photorealistic 3D render
```

### 3D Pop-Out / Hero Composite
```
Shot type: 3D perspective hero composite
Scene: Product appears to break out of its packaging or a framed surface, strong forced perspective
Lighting: Soft directional studio light with subtle drop shadows
Mood: Dynamic, energetic, premium DTC brand energy
Style: Hyperrealistic compositing, shallow depth of field on background
```

---

## Iteration Protocol

After generating, always end with:

> *"Want me to adjust anything? I can tweak the lighting, background, angle, mood, or generate a different concept."*

Accept natural language tweaks and rebuild the prompt accordingly. Common tweaks:

| User says | What to change in prompt |
|-----------|--------------------------|
| "more premium" | Add: `ultra-luxury finish, aspirational lighting, soft shadows, rich colors` |
| "cleaner" | Simplify background, increase negative space, remove props |
| "more lifestyle" | Add environment, context, human-scale props, warmer light |
| "darker/moodier" | Low-key lighting, deep shadows, richer color grade |
| "show the label better" | Add: `product label fully visible and legible, frontal orientation` |
| "make it pop more" | Increase contrast, add rim lighting, bolder background |

---

## Quality Rules (Non-Negotiable)

- Product label must always be **fully visible and readable** unless user says otherwise
- Never add **text overlays** (the user handles copy separately)
- Product must be the **clear hero** of the image — never obscured or too small
- Output must be **photorealistic** by default unless user requests stylized
- If Gemini distorts the product shape significantly, flag it: *"The model altered the product shape — want me to try again with tighter constraints?"*

---

## Example Prompt (Livera Liver Detox Gummies)

**Concept: Wellness Ritual — Warm Lifestyle**

```
Hero lifestyle shot of a amber glass jar of Livera liver detox gummies, label fully visible.

SCENE: Natural marble countertop, morning light from left window, small sprig of milk thistle herb beside the jar, one gummy resting next to it
LIGHTING: Soft golden morning light, warm 5500K, gentle fill from right, subtle shadow beneath jar
CAMERA: 3/4 angle, slightly elevated, product fills 50% of frame, background softly blurred
MOOD: Healthy daily ritual, clean, trustworthy, premium wellness
PRODUCT: Jar upright, label facing camera, cap on, one loose gummy visible beside it
STYLE: Photorealistic editorial product photography, clean color grading, high resolution
OUTPUT: Professional brand photography, no text overlays, label fully legible
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| API key missing | Ask user to set `GEMINI_API_KEY` |
| Product distorted | Add: `preserve exact product shape, dimensions, and label design` |
| Background too busy | Simplify scene description, reduce props |
| Label not visible | Add: `product oriented directly facing camera, label fully legible` |
| Output too generic | Add more specific mood, lighting direction, and prop details |
