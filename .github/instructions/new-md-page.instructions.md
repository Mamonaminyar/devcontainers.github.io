---
applyTo: '**/*.md'
---

## Guidance for new markdown specification pages

### Layout
- Any new page should have information like the following at the top, where the title, shortTitle, and index are updated appropriately:
```
---
layout: implementors
title:  "Dev Container Templates distribution and discovery"
shortTitle: "Templates distribution"
author: Microsoft
index: 8
---
```
- The layout comes from the _layouts folder in the repo and will always be `layout: implementors` for pages of this type.

### Content
- Add an emoji key before any paragraphs or tables using emojis 
- Make all headings clickable links