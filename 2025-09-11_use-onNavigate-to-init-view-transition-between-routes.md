---
date: 2025-09-11
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# use onNavigate to init view transition between routes

`onNavigate` is a function that fires whenever a navigation happens in a SvelteKit app. We can use it in our root layout because it always exists even when we navigate between routes.

```svelte
<!-- src/routes/+layout.svelte -->
<script>
	import Header from './Header.svelte';
    import { onNavigate } from '$app/navigation'; // `onNavigate` fires when any navigation happens
	import './styles.css';

  // You can use this to trigger view transitions, but let's check what parameters we get first
  onNavigate((nav) => {
    console.log(nav);
  });
</script>

<div class="app">
	<Header />

	<main>
		<slot />
	</main>
</div>
```


Let's see what `nav` contains when we navigate from home page to a single image page:


```
{
    "from": {
        "params": {},
        "route": {
            "id": "/"
        },
        "url": "http://localhost:5173/"
    },
    "to": {
        "params": {
            "id": "3"
        },
        "route": {
            "id": "/image/[id]"
        },
        "url": "http://localhost:5173/image/3"
    },
    "willUnload": false,
    "type": "link",
    "complete": {} <-- this is a promise that resolves when new page is loaded to the DOM, so you can snapshot the new DOM for the new state of view transition
}
```

We can also return a promise in the `onNavigate` callback to delay the navigation to the new page. This is useful if we want to ensure the old state of the DOM is captured before the new page is loaded.

```svelte
<script>
	import Header from './Header.svelte';
  import { onNavigate } from '$app/navigation';
	import './styles.css';

  onNavigate((nav) => {
    // we can return a promise here, the page will be suspended until it resolves
    return new Promise((resolve) => {
      setTimeout(() => {
        resolve();
      }, 500);
      // ^^^ so the page will wait for 500ms before starting the transition
    });
  });
</script>
```

Now whenever we navigate between routes, there will be a 500ms delay before the new page is loaded.

After understanding all these, we can start to use `document.startViewTransition` to create a view transition between routes.

```svelte
<script>
import Header from './Header.svelte';
import { onNavigate } from '$app/navigation';
import './styles.css';

  onNavigate((nav) => {
    if (!document.startViewTransition) return; // check if the browser supports View Transitions API, if not, just simply navigate without transition
    return new Promise((resolve) => { // because we should ensure the `document.startViewTransition` can get old state snapshot before the new page is loaded
      document.startViewTransition(async () => { // this callback is called before the new page is loaded, so it can capture the old state
        resolve(); // resolve the promise to let the navigation continue to load the new page
        await nav.complete; // wait for the new page to be loaded to the DOM here, so `document.startViewTransition` can capture the new state
      });
    });
  });
</script>
```

Now the `document.startViewTransition` can capture both old and new state of the DOM, so the view transition can be animated between routes!

Currently, the snapshot of the view transition includes the entire page. We can isolate the elements using CSS `view-transition-name`, so that we can only animate specific elements.

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

        <!-- assume we want to isolate the title for root view transitions -->
		<div class="title">
			<h1>Echoes of What Was</h1>
		</div>

	</div>
</header>

<style>
     ...
     /* isolate the title element by giving it a view-transition-name */
     header .title {
       view-transition-name: header-title;
     }
     ...
</style>
```
