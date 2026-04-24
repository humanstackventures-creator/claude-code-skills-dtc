---
name: canva-deck
description: >
  Create, edit, and manage presentation decks on Canva. Trigger whenever the
  user wants to create a deck, presentation, slides, or pitch deck on Canva.
  Also trigger on "make me a deck", "create a presentation", "build slides",
  "canva presentation", "pitch deck", or any request to generate, edit, or
  export a Canva presentation. When in doubt about whether the user wants a
  Canva deck, trigger.
argument-hint: <topic or instruction>
allowed-tools:
  - mcp__claude_ai_Canva__request-outline-review
  - mcp__claude_ai_Canva__generate-design-structured
  - mcp__claude_ai_Canva__generate-design
  - mcp__claude_ai_Canva__create-design-from-candidate
  - mcp__claude_ai_Canva__start-editing-transaction
  - mcp__claude_ai_Canva__perform-editing-operations
  - mcp__claude_ai_Canva__commit-editing-transaction
  - mcp__claude_ai_Canva__cancel-editing-transaction
  - mcp__claude_ai_Canva__get-design
  - mcp__claude_ai_Canva__get-design-content
  - mcp__claude_ai_Canva__get-design-pages
  - mcp__claude_ai_Canva__search-designs
  - mcp__claude_ai_Canva__export-design
  - mcp__claude_ai_Canva__get-export-formats
  - mcp__claude_ai_Canva__list-brand-kits
  - mcp__claude_ai_Canva__upload-asset-from-url
  - mcp__claude_ai_Canva__get-assets
  - mcp__claude_ai_Canva__comment-on-design
  - mcp__claude_ai_Canva__list-comments
  - mcp__claude_ai_Canva__get-design-thumbnail
  - mcp__claude_ai_Canva__get-presenter-notes
  - mcp__claude_ai_Canva__resize-design
  - Read
  - Glob
  - Grep
  - Bash
---

# Canva Deck Builder

Create, edit, and manage presentation decks on Canva directly from Claude Code.

## Quick Start

When the user asks to create a deck, follow this workflow:

1. **Gather requirements** — topic, audience, style, length, and whether they
   want to use a brand kit.
2. **Generate the outline** — use `request-outline-review` so the user can
   review and refine the slide structure before generation.
3. **Generate the design** — once the outline is approved, call
   `generate-design-structured` with the finalized outline.
4. **Create the design** — use `create-design-from-candidate` to convert the
   chosen candidate into an editable Canva design.
5. **Edit if needed** — use the editing transaction tools to refine content.
6. **Export if requested** — export to PDF, PPTX, PNG, etc.

---

## Workflows

### 1. Create a New Deck

This is the primary workflow. Always use the structured presentation flow.

**Step 1 — Clarify the brief.**
Ask the user (if not already provided):
- **Topic**: What is the deck about?
- **Audience**: Who will see it? (e.g., investors, team, students)
- **Style**: Visual preference? (minimalist, playful, elegant, geometric, etc.)
- **Length**: short (1-5 slides), balanced (5-15), or comprehensive (15+)
- **Brand kit**: Do they want to use their Canva brand kit?

If the user gives a topic and enough context, skip unnecessary questions and
proceed. Bias toward action over interrogation.

**Step 2 — Build the outline.**
Craft a slide-by-slide outline based on the brief. Each slide needs:
- A clear **title**
- A **description** with the key content for that slide

Then call `request-outline-review` with:
- `topic`: high-level topic (max 150 chars)
- `pages`: array of `{title, description}` objects
- `audience`, `style`, `length` as appropriate
- `brand_kit_id` / `brand_kit_name` if the user selected one

The user will review and may request changes. If they do, update the outline
and call `request-outline-review` again. Repeat until approved.

**Step 3 — Generate the presentation.**
Once approved, call `generate-design-structured` with:
- `design_type`: "presentation"
- `topic`, `audience`, `style`, `length`
- `presentation_outlines`: the approved slide outlines
- `brand_kit_id` if applicable

**Step 4 — Finalize.**
Call `create-design-from-candidate` with the `job_id` and chosen `candidate_id`
to create the editable design. Share the Canva link with the user.

### 2. Edit an Existing Deck

When the user wants to modify an existing presentation:

1. **Find the design** — use `search-designs` or ask for the design ID/URL.
2. **Start editing** — call `start-editing-transaction` with the design ID.
   Always show the page thumbnails to the user.
3. **Make changes** — call `perform-editing-operations` with the edits:
   - `replace_text` or `find_and_replace_text` for text changes
   - `update_fill` or `insert_fill` for images/videos
   - `format_text` for styling (color, alignment, font size, etc.)
   - `delete_element` to remove content
   - `position_element` / `resize_element` for layout adjustments
4. **Show preview** — always show the updated thumbnails to the user.
5. **Save** — call `commit-editing-transaction` after getting user approval.
   Never commit without asking first.

### 3. Review / Read a Deck

- Use `get-design` for metadata (title, owner, dates, page count).
- Use `get-design-content` for text content.
- Use `get-design-pages` for page thumbnails.
- Use `get-presenter-notes` for speaker notes.

### 4. Export a Deck

1. Call `get-export-formats` first to check supported formats.
2. Call `export-design` with the desired format:
   - **PPTX** — for PowerPoint compatibility
   - **PDF** — for sharing / printing
   - **PNG/JPG** — for individual slide images
   - **MP4** — for video presentations
3. Share the download URL with the user.

### 5. Brand Kit Integration

If the user wants on-brand designs:
1. Call `list-brand-kits` to show available kits.
2. Let the user pick one.
3. Pass the `brand_kit_id` to generation tools.

---

## Important Rules

- **Always use `request-outline-review` first** for new presentations. Never
  skip the outline review step — it lets the user shape the deck before
  generation.
- **Never call `generate-design-structured` without an approved outline.** If
  the user asks for changes to the outline, call `request-outline-review` again.
- **Always show thumbnails** after starting an editing transaction and after
  making edits.
- **Always ask before committing edits.** Changes are drafts until committed
  and will be lost if not saved.
- **Batch editing operations** across pages in a single
  `perform-editing-operations` call when possible.
- **Design IDs** start with "D" and are exactly 11 characters. Extract them
  from Canva URLs when the user provides links.
- **For shortlinks** (canva.link/...), use `resolve-shortlink` first.
- When the user provides a **local file** they want in the deck, tell them to
  upload it to a public URL first, or use `upload-asset-from-url` if they have
  a public link.

---

## Example Interactions

**User:** "Make me a pitch deck for my AI startup"
→ Ask about audience, style preference, key points to cover. Build outline,
  call `request-outline-review`, iterate, generate.

**User:** "Create a 5-slide deck about Q1 results"
→ Build a short outline (title, highlights, revenue, key wins, next steps),
  call `request-outline-review`, generate.

**User:** "Edit my deck — change the title on slide 1"
→ Ask for design ID/URL, `start-editing-transaction`, `find_and_replace_text`
  or `replace_text`, show preview, `commit-editing-transaction`.

**User:** "Export my deck as PDF"
→ Ask for design ID, `get-export-formats`, `export-design` with PDF format,
  share download link.
