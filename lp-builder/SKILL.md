---
name: lp-builder
description: >
  Build, edit, and manage landing pages using Google Stitch (primary) or Framer
  MCP. Trigger whenever the user wants to create a landing page, build a web
  page, edit an existing page, add sections or components, manage CMS content,
  create or update code components, adjust styles (colors, typography), or do
  anything related to their site. Also trigger on /lp-builder, "build me a
  landing page", "update the site", "add a hero section", "change the colors",
  "create a new page", or any mention of Framer, Stitch, landing page builder,
  or LP. When in doubt, trigger.
---

# Landing Page Builder

Build and manage landing pages for CPG supplement brands using Google Stitch
(primary) or Framer MCP (secondary).

## Getting Started — Always Do This First

1. **Ask the user which design tool to use** — Stitch (default) or Framer.
2. **If Stitch:** Confirm the Stitch MCP server is connected. The API key
   should be configured in Claude Code's MCP settings. If not connected, ask
   the user for their Stitch API key and add it.
3. **If Framer:** Ask the user for their Framer MCP connection URL (the
   `mcp.unframer.co` link with their project ID and secret). Do NOT skip this
   step — the MCP connection may change between sessions or projects.
4. **Load the project state** — call the appropriate tool to understand what
   already exists before making changes.
5. **Ask clarifying questions** if the user's request is ambiguous (e.g.,
   "which page should I edit?" or "do you want a new page or a new section?").

---

## Design Tools

### Google Stitch (Primary)

Stitch is an AI-powered UI design tool that generates high-fidelity web UIs
from text prompts and exports clean HTML/CSS. Use the `mcp__stitch` tools.

#### Stitch MCP Tools

| Task | Tool |
|---|---|
| Generate screen from text | `generate_screen_from_text(projectId, text)` |
| Edit existing screens | `edit_screens(projectId, screenIds[], text)` |
| Create a new project | `create_project(name, description?)` |
| List all projects | `list_projects` |
| Get project details | `get_project(projectId)` |
| List screens in a project | `list_screens(projectId)` |
| Get a single screen | `get_screen(projectId, screenId)` |
| Download screen code | `download_code(projectId, screenId)` |
| Download screenshot | `download_screenshot(projectId, screenId)` |

#### Stitch Text-to-Design Workflow

For each LP section, follow this 6-step process:

**Step 1 — Enhance the prompt.** Never send a raw/vague prompt to Stitch.
Transform every prompt through the enhancement pipeline:
- Start with the section's purpose and the copy you've written.
- Add **layout specifics**: grid structure, alignment, spacing direction.
- Add **visual specifics**: color values (hex), font families, weight, size.
- Add **component specifics**: button styles, card treatments, icon placement.
- Add **responsive intent**: how the layout should adapt on mobile.

**Step 2 — Generate the screen.**
Call `generate_screen_from_text(projectId, enhancedPrompt)`.

**Step 3 — Review the output.**
Call `get_screen(projectId, screenId)` and `download_screenshot` to inspect.

**Step 4 — Iterate if needed.**
Use `edit_screens(projectId, [screenId], editInstructions)` to refine.
Be specific: "Move the CTA button below the subheadline, increase font size
to 18px, change background to #0c3d3d."

**Step 5 — Download the code.**
Call `download_code(projectId, screenId)` to get the HTML/CSS.

**Step 6 — Assemble.** Combine all section outputs into the full LP.

#### Stitch Prompt Enhancement Rules

**Always translate vague terms to professional UI language:**

| Vague | Enhanced |
|---|---|
| "clean" | "generous whitespace, 80px section padding, single-column with max-width 1200px" |
| "modern" | "geometric sans-serif (Inter/Plus Jakarta Sans), 4px border-radius, subtle shadows" |
| "premium" | "dark background (#1a1a2e), gold accent (#d4a853), high-contrast typography" |
| "minimal" | "monochromatic palette, single typeface, no borders, content-driven hierarchy" |
| "bold" | "oversized display type (72px+), high-contrast color blocks, full-bleed imagery" |
| "warm" | "cream/beige base (#faf5ef), terracotta accent (#c4703f), soft rounded corners (12px)" |
| "trustworthy" | "navy/dark teal base, white cards, badge icons, structured grid, serif headings" |

**Prompt structure template for each section:**

```
[SECTION PURPOSE]: [What this section does in the LP flow]

LAYOUT:
- [Grid/flex structure, columns, alignment]
- [Spacing: section padding, element gaps]
- [Max-width, container behavior]

CONTENT (exact copy):
- Headline: "[exact headline text]"
- Subheadline: "[exact subheadline text]"
- Body: "[exact body copy]"
- CTA: "[exact button text]"
- [Any additional content elements]

VISUAL DESIGN:
- Background: [color hex or treatment]
- Typography: [font family, sizes, weights for each element]
- Colors: [primary, secondary, accent with hex values]
- [Card/container treatments, shadows, borders]

COMPONENTS:
- [Buttons: style, size, hover state direction]
- [Icons: style (outline/filled), size, placement]
- [Images: aspect ratio, treatment, placeholder direction]

RESPONSIVE:
- [How this section stacks/reflows on mobile]
- [Font size adjustments, spacing changes]
```

#### Stitch Design Quality Standards

When generating screens, enforce these standards:

**Anti-patterns to avoid (banned in prompts):**
- Generic stock-photo-looking layouts
- Centered-everything with no visual hierarchy
- Rainbow/multi-color palettes (stick to 2-3 colors max)
- Decorative elements that don't serve information architecture
- Default blue (#0066ff) or default Bootstrap-looking components
- Evenly-spaced identical cards with no visual variation
- Generic "Lorem ipsum" or placeholder text — always use real copy

**Quality signals to include:**
- Intentional whitespace — not just "add spacing" but specific values
- Typography hierarchy — clear distinction between heading levels
- One dominant visual element per section (product shot, stat, headline)
- Consistent border-radius across all components
- Color used semantically (accent for CTAs, muted for supporting text)
- Micro-interactions described (hover states, transitions)

#### Stitch Edit Workflow

When refining an existing screen:

1. **Get the current state**: `get_screen(projectId, screenId)` + screenshot.
2. **Identify specific changes**: List exact modifications (position, size,
   color, content, spacing).
3. **Call `edit_screens`** with precise instructions. Reference elements by
   their visible content or position ("the main headline", "the green CTA
   button", "the third benefit card").
4. **Verify**: Download screenshot to confirm changes landed correctly.
5. **Iterate**: Repeat steps 2-4 as needed. Stitch handles incremental edits
   well — don't regenerate the whole screen for small fixes.

#### Generating a DESIGN.md for Multi-Screen Projects

For projects with multiple screens (full LP builds), synthesize a design system
document after generating the first 2-3 screens:

1. Call `list_screens(projectId)` and download screenshots of existing screens.
2. Extract the emerging design system: colors, typography, spacing, component
   patterns, border treatments.
3. Document it and use it as context in subsequent screen prompts to maintain
   visual consistency across all sections.

This prevents drift between sections — each new screen prompt should reference
the established design system.

### Framer MCP (Secondary)

Use the `mcp__mcp-unframer-co` tools for all Framer operations. These connect
directly to the user's Framer project.

#### Reading the Project

| Task | Tool |
|---|---|
| Get full project structure | `getProjectXml` |
| Inspect a specific node | `getNodeXml(nodeId)` |
| See what's selected in canvas | `getSelectedNodesXml` |
| Get published site URLs | `getProjectWebsiteUrl` |
| Get component insert info | `getComponentInsertUrlAndTypes(id)` |

#### Editing Pages & Nodes

| Task | Tool |
|---|---|
| Update/create/reorder nodes | `updateXmlForNode(nodeId, xml)` |
| Duplicate a node | `duplicateNode(nodeId)` |
| Delete a node | `deleteNode(nodeId)` |
| Create a new page | `createPage(name, type)` |
| Focus canvas on a node | `zoomIntoView(nodeId)` |

**Key rules for `updateXmlForNode`:**
- Nodes **without** a `nodeId` attribute in the XML are created as new nodes.
- Nodes **with** a `nodeId` are updated in place.
- Use this to reorder or reparent nodes by changing their position in the XML tree.

#### Styles & Fonts

| Task | Tool |
|---|---|
| Create/update color style | `manageColorStyle(type, stylePath, properties)` |
| Create/update text style | `manageTextStyle(type, stylePath, properties)` |
| Search available fonts | `searchFonts(query)` |

**Style paths** start with `/` (e.g., `/primary`, `/heading-1`). Reference them
in XML via `color="/primary"` or the text style path.

#### Code Components

| Task | Tool |
|---|---|
| Create a .tsx code file | `createCodeFile(name, content)` |
| Update a code file | `updateCodeFile(codeFileId, content)` |
| Read a code file | `readCodeFile(codeFileId)` |

#### CMS

| Task | Tool |
|---|---|
| List collections & fields | `getCMSCollections` |
| Fetch items (with search) | `getCMSItems(collectionId, filter, skip, limit)` |
| Create a collection | `createCMSCollection(name, fields)` |
| Create/update an item | `upsertCMSItem(collectionId, fieldData, ...)` |
| Delete an item | `deleteCMSItem(collectionId, itemId)` |

**Always call `getCMSCollections` first** before any CMS write operation.

#### Export

| Task | Tool |
|---|---|
| Export as React components | `exportReactComponents` |

---

## Business Context

The user operates **consumer packaged goods (CPG) supplement brands**. Key context:

- **Multiple brands** — each project may be a different brand with its own voice,
  colors, and identity. Always derive brand voice from the references the user
  provides for that specific project.
- **Single product per LP** — each landing page sells one product (not bundles
  or collections).
- **The user will provide per-project:** brand references (URLs/screenshots),
  product details, color palette, brand voice cues, and institutional context.
  Study the references carefully to extract tone, layout patterns, and visual
  language, then create a merged brand voice from them.

## Image Generation

When product shots, lifestyle imagery, or icons are needed, use the **Nano Banana**
image generation models (via the `image-prompt` skill or Google AI Studio).
Ask the user if you need an API key. Generate prompts optimized for supplement
product photography and lifestyle contexts.

## Per-Project Kickoff Checklist

Every time a new LP project starts, collect the following from the user:

1. **Design tool** — Stitch (default) or Framer? Confirm MCP is connected.
2. **Brand references** — URLs or screenshots of brands they like for this project
3. **Product details** — name, type, key ingredients, certifications
4. **Color palette** — primary, secondary, accent colors
5. **Brand voice direction** — or let you derive it from the references
6. **Institutional context** — brand story, mission, certifications for the footer
7. **Pricing structure** — subscription vs. one-time, pack sizes, prices
8. **Any reference folders** the user has added to the project

## Landing Page Sections — Required Order

Build every LP with these sections in this exact order. The user will provide
product-specific copy direction, but you are responsible for **writing the hooks,
headlines, subheadlines, body copy, and CTAs** for every section. Craft
persuasive, benefit-driven copywriting tailored to the supplement's target
audience and the brand voice derived from references.

### 1. Hero Section

- **Big product shot** as the primary visual — prominent, clean, high-quality.
- Optionally include lifestyle imagery alongside or behind the product shot.
- Strong headline — can be short & punchy or benefit-driven depending on brand.
- Subheadline that reinforces the main promise.
- **CTA button** — always present. Typically "Shop Now" or "Try It" (ask user
  if unclear).
- **Sticky header/nav** at the top of the page.

### 2. Problem Section

- Frame the problem the target customer faces — pain points, frustrations,
  unmet needs.
- Approach depends on brand voice (stats/data, emotional storytelling, or
  relatable scenarios — study the references).
- **Use icons** to visually represent each problem point.
- Typically 3-4 problem points.
- The tone should create empathy and urgency without being fear-based.

### 3. Solution / Product Section

- Position the product as the answer to the problems above.
- **Show ingredients** — key active ingredients with brief explanations.
- **Show certifications** — FDA, GMP, third-party tested, organic, etc.
  (whatever applies).
- **3-4 key benefits** highlighted clearly.
- Clean layout — ingredient callouts, benefit icons, product imagery.

### 4. Offering Section

- **Pricing lives here.** Structure as subscription vs. one-time purchase.
- Typical options: 1-month supply (one-time) vs. 3-month subscription
  (discounted).
- Make the subscription option visually prominent as the recommended choice.
- Include trust signals (money-back guarantee, free shipping, etc. — ask user).

### 5. Comparison Section

- **Us vs. Them** table or visual comparison.
- Compare the product against generic/competitor alternatives.
- Highlight differentiators: quality, ingredients, certifications, price value.
- Keep it factual but persuasive.

### 6. FAQ Section

- **~5 questions** focused on handling objections.
- Cover: shipping, returns/refunds, "does it actually work?", ingredients/safety,
  subscription management.
- Write clear, confident answers that build trust.
- Accordion-style layout.

### 7. Testimonials Section

- **Generate realistic customer reviews** (the user will replace with real ones
  later or confirm these).
- Include **star ratings** (4-5 stars).
- Mix of short and medium-length reviews.
- Focus reviews on results, taste/experience, and value.
- 3-6 testimonials.

### 8. Institutional / Footer Section

- Brand story, mission, values — **user will provide context per project.**
- Trust badges and certifications.
- **Include a final CTA** before the actual footer (e.g., "Ready to transform
  your health? Shop Now").
- Footer: navigation links, legal (privacy policy, terms), social media icons,
  contact info.

## Copywriting Guidelines

You are responsible for writing ALL copy across every section. This is a core
part of the skill — not just layout and design.

- **Study the brand references** the user provides to extract tone, vocabulary,
  and messaging patterns. Create a merged voice from multiple references.
- **Lead with benefits, not features.** "Wake up energized" > "Contains 500mg
  magnesium."
- **Write hooks that stop the scroll.** Headlines should provoke curiosity,
  address a pain point, or make a bold promise.
- **Keep paragraphs short.** 1-2 sentences max for body copy on LPs.
- **CTAs should be action-oriented and specific.** "Start Sleeping Better" >
  "Learn More."
- **Use social proof language** in copy — "Join 10,000+ customers", "Rated #1",
  etc.
- After building, present the full copy to the user for review before moving to
  the next section.

## Animations

When the user asks for animations, use patterns from the **21st.dev** component
ecosystem. The stack is **Framer Motion (Motion) + Tailwind CSS + React**.
Browse https://21st.dev/community/components for reference implementations.

### Animation Library

The primary animation library is **Framer Motion** (now called **Motion**).
Import from `motion/react` or `framer-motion`. Key APIs:

| API | Use Case |
|---|---|
| `motion.div` / `motion.span` | Animated wrapper for any element |
| `useInView` | Trigger animation when element enters viewport |
| `useScroll` + `useMotionValueEvent` | Scroll-linked animations |
| `AnimatePresence` | Enter/exit animations (testimonials, modals) |
| `variants` | Define hidden/visible state objects |
| `stagger()` | Sequential animation of children |
| `whileHover` / `whileTap` | Gesture-triggered animations |
| `layout` | Automatic layout transitions |

### Recommended Animations by LP Section

#### Hero Section
- **Blur Fade** (`magicui/blur-fade`) — Headline and subheadline fade in with
  blur(6px→0) + opacity + Y-offset. Stagger headline then subheadline then CTA.
- **Hero Highlight** — Animated underline/highlight on key phrase in headline.
- **Word Rotate** (`magicui/word-rotate`) — Cycle benefit words in headline
  ("Better Sleep / More Energy / Sharper Focus").
- **Lamp Effect** (`aceternity/lamp-effect`) — Dramatic spotlight on product shot.
- **Background effects** — Aurora, gradient animation, or particles behind hero.
  Keep subtle — don't compete with the product shot.
- **Shimmer Button** for CTA — CSS conic gradient + `shimmer-slide` keyframe.

#### Problem Section
- **Staggered reveal** — Each problem point fades in sequentially on scroll
  using `useInView` + `stagger(0.15)`.
- **Number Ticker** (`magicui/number-ticker`) — Animate stats (e.g., "87% of
  adults have nutrient gaps") counting up on viewport entry.
- **Icon entrance** — Icons scale or fade in slightly before their text.

#### Solution / Product Section
- **Sticky Scroll Reveal** (`aceternity/sticky-scroll-reveal`) — Left panel
  scrolls through benefits, right panel shows corresponding product imagery.
  Uses `useScroll` + `useMotionValueEvent`.
- **Blur Fade stagger** — Ingredients/benefits reveal one by one on scroll.
- **3D Card Effect** — Product card with subtle perspective tilt on hover.

#### Offering Section
- **Glow Effect** (`ibelick/glow-effect`) — Glowing border on the recommended
  subscription card. Modes: rotate, pulse, breathe.
- **Hover Border Gradient** — Animated gradient border on pricing cards on hover.
- **Focus Cards** — Non-selected pricing option dims when other is hovered.
- **Animated Number** (`motion-primitives/animated-number`) — Price transitions
  when switching between subscription/one-time.

#### Comparison Section
- **In View reveal** — Table rows/comparison points animate in on scroll entry.
- **Checkmark/X animations** — Check icons pop in with spring, X icons fade in
  muted.

#### FAQ Section
- **Accordion with AnimatePresence** — Smooth height animation on expand/collapse
  using `layout` prop + `AnimatePresence`.
- **Blur Fade** on initial viewport entry for the question list.

#### Testimonials Section
- **Animated Testimonials** (`aceternity/animated-testimonials`) — Carousel with
  staggered word blur-reveal (0.02s per word), AnimatePresence for enter/exit.
- **Infinite Moving Cards** / **Marquee** (`magicui/marquee`) — Continuous
  horizontal scroll for testimonial cards.
- **Star rating animation** — Stars fill in sequentially with stagger.

#### Institutional / Footer Section
- **Blur Fade** — Final CTA and brand story fade in on scroll.
- **Trust badge entrance** — Certification badges scale in with spring easing.

### Animation Principles

- **Subtlety over spectacle.** Animations should enhance trust and polish, not
  distract from the product. Supplement LPs sell credibility — don't make it
  feel like a tech demo.
- **Viewport-triggered by default.** Most animations should fire on scroll into
  view (`useInView`), not on page load.
- **Stagger children, don't animate parents.** Animate individual elements
  (words, cards, icons) sequentially, not entire sections at once.
- **Keep durations short.** 0.3–0.6s for reveals, 0.15–0.3s for hover effects.
  Longer only for background ambience (aurora, particles).
- **Ease out, not linear.** Use `ease: "easeOut"` or spring physics for natural
  feel. Never linear for UI elements.
- **One hero animation per section.** Each section gets one signature effect
  (e.g., sticky scroll reveal in Solution). Don't stack multiple complex
  animations in the same section.
- **Respect reduced motion.** Always check `prefers-reduced-motion` and provide
  a static fallback.
- **Mobile performance.** Disable particle/shader effects on mobile. Keep
  scroll-linked animations lightweight.

### Source Libraries on 21st.dev

| Library | Strength | URL |
|---|---|---|
| **Aceternity UI** | Backgrounds, 3D effects, scroll parallax, hero sections | ui.aceternity.com |
| **Magic UI** | Text effects, number animations, social proof, polish | magicui.design |
| **Motion Primitives** | Text morphing, cursor effects, animation primitives | motion-primitives.com |
| **Animate UI** | Full animated component distribution | animateui.com |

Browse components: `https://21st.dev/community/components`
Filter by category: append `/s/hero`, `/s/text-animation`, `/s/scroll-animation`,
`/s/background`, `/s/hover-effects`, etc.

## Design Principles

- **Fresh, clean aesthetic** — supplements should look premium and trustworthy.
- **Reuse existing styles** — check project styles before creating new ones.
- **Consistent spacing** — use the project's existing spacing patterns.
- **Mobile-responsive** — structure layouts with stacks and relative sizing.
- **Keep it simple** — clean and well-structured beats over-engineered.
- **Sticky header** — always include a sticky nav/header.
- **No countdown timers** — do not add urgency/scarcity elements.

### Before Creating Anything New

1. Check if a similar component or style already exists in the project.
2. If creating a new color, add it as a project color style so it's reusable.
3. If creating a new text style, add it as a project text style.
4. Prefer project-level styles over inline values.

## Error Handling

- If a tool call fails, read the error message carefully — errors are usually
  descriptive.
- For Framer: if `updateXmlForNode` fails, the XML is likely malformed. Check
  node IDs and XML structure.
- If a style or node doesn't exist, refresh your understanding of the project
  state.
- For Stitch:
  - If `generate_screen_from_text` produces unexpected results, **don't
    regenerate** — use `edit_screens` with specific corrections first.
  - If the layout is fundamentally wrong, regenerate with a more structured
    prompt (add explicit grid/flex instructions).
  - If colors or typography are off, edit rather than regenerate — Stitch
    handles targeted style edits well.
  - If the MCP connection fails, verify the API key is set correctly in
    Claude Code's MCP settings (`claude mcp list` to check).
