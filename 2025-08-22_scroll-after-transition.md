# scroll after transition

```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  function expandImage(item) { ... }

  // ...

  grid.addEventListener("click", async (e) => {
    // ...

    document.startViewTransition(() => {
      expandImage(item);
    });
  });
  
  // ...

});
```

When the `expandImage` function above triggers a view transformation, if the selected grid item is towards the back, it may be moved out of the viewport after the transformation. Therefore, we want to scroll to this item after the transformation is complete.

We can first conduct experiments on the browser, inspect the item, then `store as global variable`, and then try using the `scrollIntoView` method of the variable on the console to see if we can actually trigger the scrolling effect.


```
temp1.scrollIntoView()  // scrolls to the item
temp1.scrollIntoView({ behavior: 'smooth' })   // scrolls to the item with smooth scrolling
```

If the above works, we can then use the `scrollIntoView` method in our code after the transition is complete.


```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  grid.addEventListener("click", async (e) => {
    const item = e.target.closest(".grid-item");
    
    // ...

    const transition = document.startViewTransition(() => {
//  ^^^^^^^^^^^^^^^^^^^
      expandImage(item);
    });

    await transition.updateCallbackDone; // Wait for the transition CALLBACK to complete
                                         // or we will get the old state scroll position

    // Scroll to the item
    item.scrollIntoView({
      behavior: "smooth",
      block: "nearest"
    })
  });
  
  // ...
});
```



```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  grid.addEventListener("click", async (e) => {
    const item = e.target.closest(".grid-item");
    
    // ...

    const transition = document.startViewTransition(() => {
      expandImage(item);
    });

    await transition.finished; // You can also wait for the transition to finish

    item.scrollIntoView({
      behavior: "smooth",
      block: "nearest"
    })
  });
  
  // ...
});
```


`updateCallbackDone` will scroll faster, while `finished` will wait until the entire transition animation is finished before scrolling. You can choose to use according to your needs.


