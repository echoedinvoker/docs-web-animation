---
date: 2025-09-02
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# Using view transition with ReactJS

```tsx
import { useState } from "react";
import { flushSync } from "react-dom";
import "./App.css";

export default function App() {
  const [todos, setTodos] = useState([
    { id: 1, title: "Todo 1", done: false },
    { id: 2, title: "Todo 2", done: false },
    { id: 3, title: "Todo 3", done: false },
  ]);

  function updateTodo(id, value) {
    const newTodos = todos.map((todo) => {
      if (todo.id === id) {
        return { ...todo, done: value };
      }
      return todo;
    });
    setTodos(newTodos);
  }
  return (
    <main>
      <div className="wrapper">
        <div className="inner">
          <h2>To Do</h2>
          <ul>
            {todos
              .filter((t) => !t.done)
              .map((todo) => (
                <li key={todo.id}>
                  <span>{todo.title}</span>{" "}
                  <button
                    onClick={() => {
                      updateTodo(todo.id, true);
                    }}
                  >
                    Done
                  </button>
                </li>
              ))}
          </ul>
        </div>
        <div className="inner">
          <h2>Done</h2>
          <ul>
            {todos
              .filter((t) => t.done)
              .map((todo) => (
                <li key={todo.id}>
                  <span>{todo.title}</span>{" "}
                  <button
                    onClick={() => {
                      updateTodo(todo.id, false);
                    }}
                  >
                    Undo
                  </button>
                </li>
              ))}
          </ul>
        </div>
      </div>
    </main>
  );
}
```

Above is a todo list app in ReactJS. In ReactJS, we do not directly update the DOM. Instead, when we give a new value to the state, React will update the DOM for us behind the scene.

So we can only use `document.startViewTransition` to wrap the part that updates the state, but because ReactJS updates the DOM asynchronously, we must use `flushSync` to ensure that the DOM is immediately updated.

```jsx
// ...

export default function App() {
  // ...

  function updateTodo(id, value) {

    // ...

    document.startViewTransition(() => { // wrap the method to change state by `document.startViewTransition` even React updating DOM behind the scene
      flushSync(() => { // because React update DOM asynchronously, we need to use `flushSync` to ensure the DOM update immediately
        setTodos(newTodos);
      });
    });
  }
  return (
    ...
                <li
                  key={todo.id}
                  style={{ viewTransitionName: `todo-${todo.id}` }} <!-- add unique view transition name to each todo item -->
                >
                  <span>{todo.title}</span>{" "}
                  <button
                    onClick={() => {
                      updateTodo(todo.id, true);
                    }}
                  >
                    Done
                  </button>
                </li>
              ))}
          </ul>
        </div>
        <div className="inner">
          <h2>Done</h2>
          <ul>
            {todos
              .filter((t) => t.done)
              .map((todo) => (
                <li
                  key={todo.id}
                  style={{ viewTransitionName: `todo-${todo.id}` }} <!-- add unique view transition name to each todo item -->
                >
                  <span>{todo.title}</span>{" "}
                  <button
                    onClick={() => {
                      updateTodo(todo.id, false);
                    }}
                  >
                    Undo
                  </button>
                </li>
              ))}
          </ul>
        </div>
      </div>
    </main>
  );
}
```
