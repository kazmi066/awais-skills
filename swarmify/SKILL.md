---
name: swarmify
description: >-
  Adds a swarm of colored cursors that draw and type an existing page as the
  reader scrolls. Use when someone asks to swarmify a page, add cursor agents
  that build the UI, or wants the scroll-built multi-cursor effect on a site
  they already have.
---

# Swarmify

Put a swarm of cursors on a page that already exists. The cursors draw boxes and type the real UI as the reader scrolls. Empty corners of the window stay empty.

Copy the two files in this folder. Mark the content. Start it once.

## Files

- `swarm.css` hides marked elements until a cursor finishes them, and styles the cursors.
- `swarm.js` is the swarm. It has no dependencies.

```html
<link rel="stylesheet" href="/swarm.css" />
<script type="module">
  import { swarmify } from "/swarm.js";
  swarmify();
</script>
```

Put `class="swarm"` on `<html>` so the hide rules apply before the script runs.

```html
<script>
  setTimeout(function () {
    if (document.documentElement.dataset.swarmLive === "1") return;
    document.querySelectorAll("[data-swarm]").forEach(function (el) {
      el.setAttribute("data-built", "1");
      el.style.transform = "";
      el.removeAttribute("data-swarm-shift");
    });
  }, 12000);
</script>
<noscript>
  <style>
    html.swarm [data-swarm] { opacity: 1 !important; clip-path: none !important; transform: none !important; }
  </style>
</noscript>
```

`swarmify()` returns a stop function. Call it when the page unmounts.

```js
const stop = swarmify();
// later
stop();
```

Pass `{ scrollToTop: false }` to leave the scroll position alone. The default jumps to the top so the build starts there.

## Mark the UI

Add one attribute to each piece the cursors should place. Mark the elements that are already on the page.

| Attribute | Use it on | What the cursor does |
| --- | --- | --- |
| `data-swarm="frame"` | A bar or group: nav, form, footer, a list | Drags a box around it, then the block appears |
| `data-swarm="card"` | A repeated card or image | Same drag, on that card |
| `data-swarm="type"` | A heading, name, or paragraph | Types the text that is already in the element |

`data-swarm-shell` goes on a wrapper whose border should stay invisible until one of its children is built. The wrapper itself is not a job.

```html
<h2 data-swarm="type">Projects</h2>
<a data-swarm="card" href="/work">...</a>
<nav data-swarm="frame">...</nav>
```

Rules:

- Mark the content, not the window. No rulers, no corner ticks, no measurement chrome, no fixed edge decorations.
- Do not mark a parent and a child. A hidden parent hides the child, so the typing never shows.
- One job per visible block. A heading is `type`. The card under it is `card`. The section wrapper is unmarked.
- Leave the real text in the element. `type` reveals that text.

## Groups and headings

Marked elements that share a parent start a beat apart. Two of that group can be underway at once. The rest wait their turn. A faster scroll shortens the beat. A heading with no siblings, or a single card, starts as soon as it is in view.

A heading is typed about 50px to the side of its final spot, shifted a few pixels up or down. A short single-line label gets the same treatment. When the typing finishes, that cursor grabs the line and drags it into place.

`data-swarm-settle="off"` types a heading where it belongs. `data-swarm-settle="on"` forces the draft-and-drag on any `type` element, including a longer paragraph.

## React or Next

Load the CSS from the root layout and put `className="swarm"` on `<html>`. Start the swarm from one client component:

```jsx
"use client";
import { useEffect } from "react";
import { swarmify } from "./swarm.js";

export function Swarm() {
  useEffect(() => swarmify(), []);
  return null;
}
```

Render `<Swarm />` once, on the page that has the marks.

## What the swarm does

A ring of cursors sits over the content. After about a second, or as soon as the reader scrolls, they start on whatever is in view. Further down the page waits until the reader reaches it. Scrolling faster brings in more cursors and speeds the work. Spare cursors stay inside the content column. When the last mark is done they flock and follow the pointer, still inside that column.

If the reader prefers reduced motion, `swarmify()` shows the page and skips the cursors.

## Check

- The page is blank except the cursor ring, then the real UI appears.
- Boxes grow from a corner of the element. A heading shows up slightly to the side, then a cursor drags it home. Sibling cards start one after another. The empty corners of the window stay clear.
- Scrolling builds the next section. The sections above stay built.
- With reduced motion, or with JavaScript off, the page is readable immediately.
