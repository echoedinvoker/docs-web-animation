---
date: 2025-09-11
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# Animate the image between grid view and detail view of SPA (mobile)

## Prevent the image view transition from taking effect on small screens

When switching between grid view and detail view, the images will have separate transition animations, which look great on large screens but not so good on small screens. We can use CSS media queries to disable the separate image transition animations on small screens and revert to the default fade-in and fade-out transitions.

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
                <!--                                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ -->
				<h3><a href="image/{item.id}">{item.title}</a></h3>
			</div>
		{/each}
	</div>
</div>

<style>
  ...
  /* Disable the separate image transition on small screens */
  @media (max-width: 640px) {
    .grid-item > img {
      view-transition-name: none !important;
    }
  }
</style>
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
    <!--                                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ -->
    ...
</div>

<style>
  ...

  /* Disable the separate image transition on small screens */
  @media (max-width: 640px) {
    .item > img {
      view-transition-name: none !important;
    }
  }
</style>
```


Above, we use @media query to disable the independent transition animation of images on small screens, so that the images will transition back to the root view transition animation in the default crossfade manner.


## Add sliding animation on small screens

We can add a sliding animation to the root view transition on small screens to enhance the user experience.

```css
/* src/routes/styles.css */
...
@keyframes slide-to-left {
  to {
    transform: translateX(-100%);
  }
}

@keyframes slide-from-right {
  from {
    transform: translateX(100%);
  }
}

@media (max-width: 640px) {
  ::view-transition-new(root) {
    animation: 250ms ease-out slide-from-right;
  }
  ::view-transition-old(root) {
    animation: 250ms ease-out slide-to-left;
  }
}
```


## Prevent the header arrow from sliding

Currently on the small screen, when we navigate from the grid view to the single image view, the image enters the screen with a sliding animation. However, the arrow in the title also has a sliding animation, which doesn't look quite right. Therefore, we need to separate the header from the root view transition so that it does not have a sliding animation but instead crossfades.

```svelte
<!-- src/routes/Header.svelte -->
<script> ... </script>

<header class="header" class:is-single={isSingleImagePage} class:is-home={isHomePage}>
	<div class="header-inner">
		{#if isSingleImagePage}
			<a class="grid-view-button" href="/">
				<span class="visually-hidden">Back to Home Page</span>
				<img src={backIcon} alt="" />
			</a>
		{/if}

		<div class="title">
			<h1>Echoes of What Was</h1>
		</div>
	</div>
</header>

<style>
        .header {
            background-color: #0c0d12;
            position: sticky;
            top: 0;
            z-index: 100;
            view-transition-name: header; /* speparate header from root view transition to prevent the arrow sliding */
        }

      .header .title {
        view-transition-name: header-title; /* we already have a view transition on the header title, it will be further separated from the header view transition */
      }
      ...
</style>
```


## Animate the sliding direction based on navigation direction

When on a small screen now, whether we navigate from grid view to single image view, or from single image view back to sliding animation, it slides to the left. This doesn't look quite right, so we need the animation to slide to the right when navigating back to grid view.

We must first determine the direction of navigation, and then decide which type of animation to apply based on the direction of navigation.

```svelte
<!-- src/routes/+layout.svelte -->
<script>
  ...

  onNavigate((nav) => {
    const isBack = nav.from.url.pathname.startsWith('/image') && nav.to.url.pathname === '/'; // determine if it's a navigation back to the grid view from the detail view
    if (!document.startViewTransition) return;
    return new Promise((resolve) => {
      if (isBack) document.documentElement.classList.add('back'); // add a class to indicate it's a back navigation
      const transition = document.startViewTransition(async () => {
//    ^^^^^^^^^^^^^^^^ for waiting the view transition to complete
        resolve();
        await nav.complete;
      });
      await transition.finished; // wait for the transition to finish
      document.documentElement.classList.remove('back'); // remove the class after the transition is done
    });
  });
</script>

<div class="app">
	<Header />

	<main>
		<slot />
	</main>
</div>
...

```


```css
/* src/routes/styles.css */
...
@keyframes slide-to-left {
  to {
    transform: translateX(-100%);
  }
}

/* add the slide-to-right animation for back navigation */
@keyframes slide-to-right {
  to {
    transform: translateX(100%);
  }
}

/* add the slide-from-left animation for back navigation */
@keyframes slide-from-left {
  from {
    transform: translateX(-100%);
  }
}

@keyframes slide-from-right {
  from {
    transform: translateX(100%);
  }
}

@media (max-width: 640px) {
  ::view-transition-new(root) {
    animation: 250ms ease-out slide-from-right;
  }
  ::view-transition-old(root) {
    animation: 250ms ease-out slide-to-left;
  }

  /* apply opposite sliding animations for back navigation */
  html.back::view-transition-new(root) {
/*^^^^^^^^^ use the html `back` class we added in +layout.svelte to target the back navigation */
    animation: 250ms ease-out slide-from-left;
  }
  html.back::view-transition-old(root) {
/*^^^^^^^^^ use the html `back` class we added in +layout.svelte to target the back navigation */
    animation: 250ms ease-out slide-to-right;
  }
}
```

## Respect user's prefers-reduced-motion setting

Some users may have motion sensitivity and prefer to reduce motion on the web. We can respect their preference by disabling all view transition animations if they have enabled the `prefers-reduced-motion` setting in their system.


```css
/* src/routes/styles.css */
...
@media (prefers-reduced-motion) {
  ::view-transition-group(*),
  ::view-transition-new(*),
  ::view-transition-old(*) {
    animation: none !important;
  }
}
```
