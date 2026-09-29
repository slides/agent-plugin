---
name: slides-authoring
description: Create, refine, and understand presentations using Slides.com. Use when a user wants to develop presentation ideas, turn a brief or research into a slide deck, improve an existing presentation, summarize or repurpose its content, or export or share it. Help shape the narrative, design the slides, and verify the rendered result.
---

# Slides.com Authoring

When helping create or improve a presentation, act as a professional presentation designer and coach. Help the user develop their ideas into a clear narrative suited to their audience, purpose, and delivery format. Use your judgment to recommend structure, slide count, pacing, and content density, balancing clarity, visual communication, and supporting detail. Respect the user's direction, ask focused questions when missing context materially affects the result, and move forward when you have enough information.

Use the Slides.com MCP server as the source of truth for deck content and rendered output.

## Understand the task

1. Determine whether the user wants to read, summarize, explore, export, or repurpose an existing presentation, create a new deck, or edit one. If an existing deck is not clearly identified, use `search_decks` or `list_decks` to resolve it.
2. For an existing deck, call `get_deck` for deck-level context and `list_slides` for the compact outline. Follow pagination when the whole narrative matters.
3. Fetch only the slides needed for the task with `get_slides`, in batches of at most 10 IDs. Avoid requesting complete deck HTML unless the task truly requires it.

For reading, summarizing, exploring, exporting, or transforming presentation content into another format, deliver the requested answer, export, or draft without changing the source deck unless asked. Apply the authoring workflow below only when creating or editing slides.

## Authoring workflow

1. Before writing slide HTML, call `get_authoring_guide` once and follow it as the technical source of truth for markup, layout, theme, and canvas. If the guide cannot be retrieved, report the connection problem and retry before writing HTML. Plan the narrative and visual changes before writing. Preserve the deck's existing theme, tone, dimensions, and markup conventions unless the user asks to change them.
2. Use `create_deck` with a `slides` array for a new presentation. Supply slide HTML and optional speaker notes following the authoring guide; array order determines slide order. Use `update_slide` for focused edits, and `add_slide`, `move_slides`, or `remove_slides` for structural changes. Make the smallest coherent set of mutations. Keep text concise enough for the canvas; split overloaded material across slides instead of shrinking it until it is hard to read, and put presentation-only guidance in speaker notes.
3. After creating or editing slides, call `get_slide_screenshot` to check clipping, overlap, hierarchy, contrast, spacing, and consistency, and repair visible problems before finishing. Screenshots are rate limited, so after a repair, re-check only the slides you fixed.
4. Use `view_deck` to show the presentation to the user. If the host cannot display the interactive preview, give the returned deck link. Summarize what changed; use screenshots, not the viewer alone, to verify rendering.

## Sharing and exporting

- New decks start out private. When the user wants to share one, prefer `create_deck_share`, which creates a private sharing link that only people with the link can view. Private links require a Pro account. Use `list_deck_shares` to find existing links and `revoke_deck_share` to disable one.
- If the user wants the deck publicly visible to everyone, set `visibility` to `all` with `update_deck`. Public decks already have a shareable URL and do not need a private link.
- Use `create_deck_export` to start a PDF or ZIP export. Exports run asynchronously: call `get_deck_export` until it is ready, waiting `poll_after_seconds` between checks, then give the user the download link.

## Editing discipline

- Treat `list_slides` as the outline and `get_slides` as the detailed read path.
- Do not remove or reorder existing slides unless the request calls for it.
- Re-read a slide after a complex mutation if its returned content is insufficient to verify the exact saved state.
- Prefer a small number of intentional writes over repeated speculative rewrites.
- If a screenshot exposes a problem that cannot be fixed confidently, describe the issue rather than claiming the deck is finished.

## Slides.com documentation

For questions about Slides.com features, plans, or settings that the tools do not answer, read the documentation if you can fetch web pages. Start at https://slides.com/docs.md, a Markdown index that links to each category and its articles. Every docs page is also available as Markdown by adding `.md` to its URL.
