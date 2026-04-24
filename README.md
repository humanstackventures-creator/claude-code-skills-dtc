# Claude Code Skills for DTC Supplement Brands

A collection of Claude Code skills built for running a DTC supplement brand — from ad creative to landing pages to strategic playbooks. Built by the team at [Livera](https://trylivera.com), a liver health supplement brand.

These skills turn Claude Code into a full-stack brand operator: creative director, strategist, copywriter, and design tool.

## Skills

### Brand & Creative

| Skill | What it does |
|---|---|
| **[livera-image-gen](./livera-image-gen/)** | Generate on-brand product shots, ad creatives, and social assets using Nano Banana (Gemini image gen). Includes brand DNA (colors, photography style, anti-patterns), asset type playbooks, and proven ad frames. |
| **[brand-assets-generator](./brand-assets-generator/)** | Studio-quality product photoshoot generation from an uploaded product photo. Proposes 1-3 shot concepts (clean studio, lifestyle, editorial, glass, 3D pop-out), then generates via Gemini API. Works for any brand. |
| **[image-prompt](./image-prompt/)** | Craft optimized prompts for AI image generation. Teaches Claude to brief like a creative director — camera language, lighting, materials, composition. Works with any Gemini/Nano Banana model. |

### Marketing & Growth

| Skill | What it does |
|---|---|
| **[livera-ugc](./livera-ugc/)** | UGC and community content strategy. 8 tactical levers for treating customers like influencers (community reveals, early testing, PR list prioritization, UGC challenges, early access, spotlights, events, high-touch gestures). Includes a full UGC creator brief template. |
| **[gruns-chad-mentor](./gruns-chad-mentor/)** | Strategic advisor channeling the Gruns playbook — the $1.2B CPG brand built in 32 months and acquired by Unilever. Applies the 8 pillars (daily habit, DTC proof, subscription, UGC, founder-led storytelling, retention, acquisition readiness, sequenced distribution) to your brand decisions. |

### Web & Design

| Skill | What it does |
|---|---|
| **[lp-builder](./lp-builder/)** | Build landing pages using Google Stitch or Framer MCP. Full prompt enhancement pipeline, 8-section LP structure (hero, problem, solution, offering, comparison, FAQ, testimonials, institutional), copywriting guidelines, animation library (Framer Motion + Tailwind), and design quality standards. |
| **[canva-deck](./canva-deck/)** | Create, edit, and export presentation decks on Canva via MCP. Outline-first workflow, brand kit integration, editing transactions, and export to PDF/PPTX/PNG. |

## How to Use

### 1. Install a skill

Copy the skill folder into your Claude Code skills directory:

```bash
# Copy a single skill
cp -r livera-ugc ~/.claude/skills/

# Or copy all skills
cp -r ./* ~/.claude/skills/
```

### 2. Configure in your project settings

Add the skill to your project's `.claude/settings.json`:

```json
{
  "skills": ["livera-ugc", "brand-assets-generator", "lp-builder"]
}
```

Or reference them in your user settings at `~/.claude/settings.json`.

### 3. Trigger the skill

Each skill has trigger phrases in its frontmatter. Examples:

- `/ugc` or "write me a UGC brief" → triggers `livera-ugc`
- `/livera-image` or "generate a Livera ad" → triggers `livera-image-gen`
- `/lp-builder` or "build me a landing page" → triggers `lp-builder`
- `/gruns-mentor` or "what would Gruns do?" → triggers `gruns-chad-mentor`
- `/image-prompt` or "help me write an image prompt" → triggers `image-prompt`
- "make me a deck" or "create a presentation" → triggers `canva-deck`
- "generate a product shot" or "create a brand asset" → triggers `brand-assets-generator`

### 4. Customize for your brand

The Livera-specific skills (`livera-image-gen`, `livera-ugc`, `gruns-chad-mentor`) contain brand-specific details. Fork them and replace with your own:

- **Colors** — swap hex codes in `livera-image-gen`
- **Product details** — update the UGC brief template in `livera-ugc`
- **Competitor intel** — update reference brands in `gruns-chad-mentor`

The general skills (`brand-assets-generator`, `image-prompt`, `lp-builder`, `canva-deck`) work for any brand out of the box.

## Prerequisites

Some skills require external tools:

| Skill | Requires |
|---|---|
| `livera-image-gen` | [Nano Banana CLI](https://github.com/anthropics/nano-banana) + Gemini API key |
| `brand-assets-generator` | Gemini API key (`GEMINI_API_KEY` env var) |
| `lp-builder` | Stitch MCP or Framer MCP connection |
| `canva-deck` | Canva MCP connection |

## File Structure

```
skills/
├── brand-assets-generator/
│   └── SKILL.md
├── canva-deck/
│   └── SKILL.md
├── gruns-chad-mentor/
│   └── SKILL.md
├── image-prompt/
│   └── SKILL.md
├── livera-image-gen/
│   └── SKILL.md
├── livera-ugc/
│   └── SKILL.md
└── lp-builder/
    ├── SKILL.md
    └── references/
        └── livera-brand-analysis.md
```

## About Livera

Livera is a liver health supplement brand — liver detox gummies built on the insight that liver supplements exist but nobody takes them consistently. Gummies solve the compliance problem the same way Gruns solved it for greens.

Built by [Lucas](https://github.com/humanstackventures-creator) as CEO/Creative Strategist.

## License

MIT — use these however you want. If you build something cool with them, let us know.
