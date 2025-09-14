---
date: 2025-09-11
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# prepare sveltekit project for demo view transition on SPA

```bash
❯ tree -I node_modules/
# .
# ├── package.json
# ├── package-lock.json
# ├── README.md
# ├── src
# │   ├── app.d.ts
# │   ├── app.html
# │   ├── lib
# │   │   ├── data.ts  // mock data for demo
# │   │   └── images
# │   │       ├── back-svgrepo-com.svg
# │   │       ├── camera-svgrepo-com.svg
# │   │       ├── date-range-svgrepo-com.svg
# │   │       ├── img-1-lg.jpg
# │   │       ├── img-1-th.jpg
# │   │       ...
# │   └── routes
# │       ├── Header.svelte  // header component used in layout, and show different content based on page
# │       ├── image
# │       │   └── [id]
# │       │       ├── +page.svelte  // image detail page
# │       │       └── +page.ts
# │       ├── +layout.svelte
# │       ├── +page.svelte // home page, show a grid of images
# │       ├── +page.ts
# │       └── styles.css
# ├── static
# │   └── favicon.png
# ├── svelte.config.js
# ├── tsconfig.json
# └── vite.config.ts
```

As shown above, the home page ('/') will display a grid of images. Clicking on any image will route to the image detail page ('/image/[id]'). Additionally, the Header component in the layout will display different content based on the page. Currently, there are no view transition effects.

This topic only focuses on preparing the SvelteKit project for demonstrating view transitions on a Single Page Application (SPA). The actual implementation of view transitions will be covered in a separate topic.

[Blog of Svelte View Transition](https://svelte.dev/blog/view-transitions)

