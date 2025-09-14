# cross documents view transition

Previously, we introduced the way to implement view transition for a single page. Therefore, we all used `document.startViewTransition` to trigger view transition. But if we want to use view transition in a cross-document context, how can we do it? Actually, it's very simple. We just need to use `@view-transition` in CSS, and the browser will automatically handle the cross-document view transition for us.

```bash
❯ tree
# .
# └── view-transitions-document
#     ├── assets
#     ├── index.html
#     ├── photo
#     │   ├── 1.html  // not only index.html, but also other html files
#     │   ├── 2.html
#     │   ├── 3.html
#     │   ├── 4.html
#     │   ├── 5.html
#     │   ├── 6.html
#     │   ├── 7.html
#     │   └── 8.html
#     ├── script.js
#     └── styles.css
```


```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head> ... </head>
  <body>
    <div class="header">
      <div class="header-inner">
        <div class="title">
          <h1>Echoes of What Was</h1>
        </div>
      </div>
    </div>
    <div class="wrapper">
      <div class="grid">
        <div class="grid-item" tabindex="0">
          <img src="./assets/img-1-th.jpg" alt="" />
          <h3><a href="photo/1.html">Forgotten Dreams</a></h3> <!-- link to other html files, so just simply load another document, not update DOM -->
        </div>
        <div class="grid-item" tabindex="0">
          <img src="./assets/img-2-th.jpg" alt="" />
          <h3><a href="photo/2.html">The Weight of the World</a></h3>
        </div>
        ...
      </div>
    </div>
  </body>
</html>
```

```css
...
@view-transition { /* we can simply use @view-transition to enable cross-document view transition, without using document.startViewTransition */
  navigation: auto;
}
```

This cross-document view transition will add the pseudo element `::view-transition-...` to the root element of the new page, instead of the old page.

## Add custom animation for cross-document view transition

We can also add custom animations for the pseudo element `::view-transition-...`.


```css
@keyframes slideOld {
  to {
    transform: translateX(-30px);
    opacity: 0;
  }
}
@keyframes slideNew {
  from {
    transform: translateX(30px);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

::view-transition-old(root) {
  animation: 0.3s both ease-in-out slideOld;
}
::view-transition-new(root) {
  animation: 0.3s both ease-in-out slideNew;
}
```


## Get ViewTransition object in cross-document view transition
```js

// do something before transition (old state)

const transition = document.startViewTransition(() => {
  // do something when new state
  // update DOM
});

await transition.finished; // use promise of transition

// do something after transition finished

```

Previously, we used `document.startViewTransition` to trigger view transition, but in a cross-document context, we simply use `@view-transition` in CSS to enable view transition, without using `document.startViewTransition`. In this case, if we want to do something when in old state, new state, or when the transition is finished, how can we achieve that? We can use the `pageswap` and `pagereveal` events.

```js
// script.js
// we can do something when the old page is about to be unloaded
window.addEventListener('pageswap', async (e) => {
  if (!e.viewTransition) return; // check if ViewTransition is supported (not all browsers support it)
  console.log('pageswap')
  console.log(e.viewTransition) // get ViewTransition object
  console.log(e.activation) // get NavigationActivation object, which contains info about navigation (where we came from, where we go, type of navigation)
})

// we can do something when the new page is first rendered
window.addEventListener('pagereveal', async (e) => {
  if (!e.viewTransition) return;
  console.log('pagereveal')
  console.log(e.viewTransition)
  console.log(navigation.activation) // get NavigationActivation object but from navigation object instead of event
})
```

Now you can switch to the browser and check the output of the console (remember to open `preserve log` in settings, because the document will reload).

```
(index):90 Live reload enabled.
script.js:3 pageswap
script.js:4 ViewTransition {finished: Promise, ready: Promise, updateCallbackDone: Promise, types: ViewTransitionTypeSet}
script.js:5 NavigationActivation {entry: NavigationHistoryEntry, from: NavigationHistoryEntry, navigationType: 'push'}
Navigated to http://127.0.0.1:8080/photo/1.html
1.html:86 Live reload enabled.
script.js:10 pagereveal
script.js:11 ViewTransition {finished: Promise, ready: Promise, updateCallbackDone: Promise, types: ViewTransitionTypeSet}
script.js:12 NavigationActivation {entry: NavigationHistoryEntry, from: NavigationHistoryEntry, navigationType: 'push'}

