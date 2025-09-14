# Example of Naming View Transition in Cross Documents View Transition

```js
// script.js
window.addEventListener('pageswap', async (e) => {
  if (!e.viewTransition) return;
  console.log('pageswap')
  console.log(e.viewTransition)
  console.log(e.activation)
})

// we can do something when the new page is first rendered
window.addEventListener('pagereveal', async (e) => {
  if (!e.viewTransition) return;
  console.log('pagereveal')
  console.log(e.viewTransition)
  console.log(navigation.activation)
})
```


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
```

## Image view transition from grid to photo detail view

### naming the image in the grid view

In the grid view, there are many images, we must find the image that the user clicked on, and then give it a unique view-transition-name.

```js
// create a function to get photo id from url (.../photo/1.html => 1)
function getPhotoId(_url) {
  const url = new URL(_url);
  const arr = url.pathname.split('/');
  return arr.includes('photo') && arr[arr.length - 1].split('.')[0];
  //     ^^^^^^^^^^^^^^^^^^^^^^^^ so if there is no 'photo' in the path, return false
  //                              we can determine if it is in the photo detail view or grid view by this
}

// implement the view transition from grid to photo detail view by giving the image a unique view-transition-name
// we should give the view-transition-name to the image of the grid when we navigate to the photo detail view
// in the pageswap event, because at this time, the thumbnail image is in the *old state*
window.addEventListener('pageswap', async (e) => {
  if (!e.viewTransition) return;
  const photoId = getPhotoId(e.activation.entry.url);
  if (!photoId) return; // if not in photo detail view, return
  // to find the img item which is clicked and give it a unique view-transition-name `image`
  const grid = document.querySelector('.grid');
  const item = grid.children[photoId - 1];
  item.querySelector('img').style.viewTransitionName = 'image';
  // wait for the view transition to finish, then remove the view-transition-name for safety
  await e.viewTransition.finished;
  item.querySelector('img').style.viewTransitionName = 'none';
})

window.addEventListener('pagereveal', async (e) => {
  if (!e.viewTransition) return;
})
```

### naming the image in the photo detail view

Because in the photo detail view, there is only one image and no issue of duplicate names, so it can be directly named in the CSS.

```css
...
.wrapper.single .main .item > img {
  view-transition-name: image;
}
...

```

## Make transition smoother by adding some styles to view-transition-new and view-transition-old

```css
::view-transition-old(image),
::view-transition-new(image) {
  animation: none;
  mix-blend-mode: normal;
  overflow: hidden;
  height: 100%;
}
::view-transition-old(image) {
  object-fit: contain;
}
::view-transition-new(image) {
  object-fit: cover;
}
```

There is no need to explain too much here, as this part has already been explained many times before.

## Image view transition from photo detail view back to grid view

### naming the image in the grid view when navigating back to the grid view

```js
function getPhotoId(_url) { ... }

window.addEventListener('pageswap', async (e) => { ... })

// we should give the view-transition-name to the image of the grid when we navigate back to the grid view
// in the pagereveal event, because at this time, the thumbnail image is in the *new state*
window.addEventListener('pagereveal', async (e) => {
  if (!e.viewTransition) return;
  const fromUrl = navigation.activation.from.url;
  const currentUrl = navigation.activation.entry.url;
  if (!getPhotoId(fromUrl) || getPhotoId(currentUrl)) return;
  const grid = document.querySelector('.grid');
  const item = grid.children[getPhotoId(fromUrl) - 1];
  item.querySelector('img').style.viewTransitionName = 'image';
  await e.viewTransition.ready;
  item.querySelector('img').style.viewTransitionName = 'none';
})
```

### naming the image in the photo detail view when navigating back to the grid view

We already named it in the CSS.


