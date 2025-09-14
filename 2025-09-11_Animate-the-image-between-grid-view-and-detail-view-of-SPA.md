---
date: 2025-09-11
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# Animate the image between grid view and detail view of SPA

We want to implement a smooth transition animation when clicking on an image in grid view to enter detail view. This can be achieved by giving the images on two different pages the same `view-transition-name`.

```svelte
<!-- src/routes/+page.svelte -->
<script>
	import data from '$lib/data';
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<div class="grid-outer">
	<div class="grid">
		{#each data as item (item.id)}
			<div class="grid-item">
				<img src={item.thumbnail} alt={item.title} style:view-transition-name="image-{item.id}" />
                                                      <!-- ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ give each image of grid view a unique name -->
				<h3><a href="image/{item.id}">{item.title}</a></h3>
			</div>
		{/each}
	</div>
</div>
```

```svelte
<!-- src/routes/image/[id]/+page.svelte -->
<script>
	import data from '$lib/data';
	import cameraIcon from '$lib/images/camera-svgrepo-com.svg';
	import dateIcon from '$lib/images/date-range-svgrepo-com.svg';
	import { page } from '$app/stores';
	$: item = data.find((i) => `${i.id}` === $page.params.id);
</script>

<svelte:head>
	<title>{item?.title}</title>
</svelte:head>

<div class="item">
	<img src={item?.image} alt={item?.title} style:view-transition-name="image-{item?.id}" />
                                        <!-- ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ give the image of detail view the same unique name as in grid view -->
	<h2>{item?.title}</h2>
	<div class="info"> ... </div>
	<div class="description"> ... </div>
</div>
```

Because the img elements in the two different pages above are actually completely different DOM elements, there may be some differences in styles (in this case, aspect-ratio), so we write the following global css to directly avoid the fade-in/out effect during the animation process, allowing the new image to cover the old image from the beginning.

```css
/* src/routes/styles.css */
...
::view-transition-new(image-3), ::view-transition-old(image-3) {
/*                          ^                               ^ because wildcards are not supported yet, we use a specific example here */
  animation: none;
  mix-blend-mode: normal;
  height: 100%;
  overflow: hidden;
}
::view-transition-new(image-3) {
  object-fit: cover;
}
```

