---
name: slides-authoring
description: Read, view, create, edit, reorganize, and visually verify presentations on Slides.com using the Slides MCP tools. Use when a user asks to inspect or understand a presentation, build a presentation, revise a deck, add or rewrite slides, change slide order, or inspect the rendered result.
---

# Slides Authoring

Use the Slides MCP server as the source of truth for deck content and rendered output.

## Authoring workflow

1. Determine whether the user wants a new deck or changes to an existing one. If an existing deck is not clearly identified, use `search_decks` or `list_decks` to resolve it.
2. For an existing deck, call `get_deck` for deck-level context and `list_slides` for the compact outline. Follow pagination when the whole narrative matters.
3. Fetch only the slides needed for the task with `get_slides`, in batches of at most 10 IDs. Avoid requesting complete deck HTML unless the task truly requires it.
4. Plan the narrative and visual changes before writing. Preserve the deck's existing theme, tone, dimensions, and markup conventions unless the user asks to change them.
5. Use `create_deck` for a new presentation, `update_slide` for focused edits, and `add_slide`, `move_slides`, or `remove_slides` for structural changes. Make the smallest coherent set of mutations.
6. After a meaningful batch of visual edits, call `get_slide_screenshot` for the affected slides. Check clipping, overlap, hierarchy, contrast, spacing, and consistency; repair visible problems before finishing. Screenshot calls are limited, so do not use one after every trivial change.
7. Use `view_deck` for a final whole-deck review when available, then give the user the deck link and a concise summary of what changed.

## Slide markup

- Supply one complete leaf `<section>...</section>` when adding or replacing slide HTML. Let Slides assign the new slide ID.
- When editing an existing slide, retain its root slide ID and preserve block IDs for content that remains conceptually the same.
- Never insert `<style>` or `<script>` elements.
- Never use absolute positioning.
- Reuse patterns from fetched slides instead of inventing unsupported components or attributes.
- Keep text concise enough for the canvas. Split overloaded material across slides rather than shrinking it until it is hard to read.
- Put presentation-only guidance in speaker notes, not visibly on the slide.

## Editing discipline

- Treat `list_slides` as the outline and `get_slides` as the detailed read path.
- Do not remove or reorder existing slides unless the request calls for it.
- Re-read a slide after a complex mutation if its returned content is insufficient to verify the exact saved state.
- Prefer a small number of intentional writes over repeated speculative rewrites.
- If a screenshot exposes a problem that cannot be fixed confidently, describe the issue rather than claiming the deck is finished.
