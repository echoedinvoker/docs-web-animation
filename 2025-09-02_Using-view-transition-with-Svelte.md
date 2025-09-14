# Using view transition with Svelte

```tsx
<script>
	let todos = [
    { id: 1, title: "Todo 1", done: false },
    { id: 2, title: "Todo 2", done: false },
    { id: 3, title: "Todo 3", done: false },
  ];
	$: done = todos.filter(t => t.done);
	$: todo = todos.filter(t => !t.done);

	 function updateTodo(id, value) {
        const newTodos = todos.map((todo) => {
          if (todo.id === id) {
            return { ...todo, done: value };
          }
          return todo;
        });
		todos = newTodos; // svelte don't use setter to update state, just simply assign new value to state
     }
</script>

 <main>
	<div class="wrapper">
		<div class="inner" >
			<h2>To Do</h2>
			<ul>
				 {#each todo as item, index}
					 <li>
						<span>{item.title}</span>{" "}
						<button on:click={() => updateTodo(item.id, true)}>
							Done
						</button>
					</li>
				{/each} 
			</ul>
		</div>
		<div class="inner" >
			<h2>Done</h2>
			<ul>
				{#each done as item, index}
					 <li>
						<span>{item.title}</span>{" "}
						<button on:click={() => updateTodo(item.id, false)}>
							Undo
						</button>
					</li>
				{/each} 
			</ul>
		</div>
	</div>
</main>
```

The above is an example of a todo app in Svelte. One key difference compared to ReactJS is that in Svelte, new values are assigned directly to the state instead of using a setter.

We follow the same approach as ReactJS, using `document.startViewTransition` to wrap the part where the state value is updated.

Another point is that there is no `flushSync` in Svelte, but there is `tick`, which will be resolved when all DOM updates are completed. We can await it in the callback of `document.startViewTransition` to ensure that all DOM updates are completed before the callback ends.

```tsx
<script>
	import { tick } from 'svelte';  // import tick

    // ...

	 function updateTodo(id, value) {
        const newTodos = todos.map((todo) => {
          if (todo.id === id) {
            return { ...todo, done: value };
          }
          return todo;
        });
		document.startViewTransition(async () => {  // wrap the updating state code with `document.startViewTransition`
			todos = newTodos;
			await tick(); // wait all DOM updated
		})
  }
</script>

 <main>
	<div class="wrapper">
		<div class="inner" >
			<h2>To Do</h2>
			<ul>
				 {#each todo as item, index}
					 <li style:view-transition-name="item-{item.id}">
                     <!--^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ add unique view transition name to each todo item -->
						<span>{item.title}</span>{" "}
						<button on:click={() => updateTodo(item.id, true)}>
							Done
						</button>
					</li>
				{/each} 
			</ul>
		</div>
		<div class="inner" >
			<h2>Done</h2>
			<ul>
				{#each done as item, index}
					 <li style:view-transition-name="item-{item.id}">
                     <!--^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ add unique view transition name to each todo item -->
						<span>{item.title}</span>{" "}
						<button on:click={() => updateTodo(item.id, false)}>
							Undo
						</button>
					</li>
				{/each} 
			</ul>
		</div>
	</div>
</main>
```
