## Building a Complete CRUD App with React Hooks / Replacing Redux

I will build a TODO app using React Hooks and Replacing Redux.
url: https://hooks-todo.netlify.com/

# Includes

- Axios
- UUID
- react@next
- react-dom@next
- Tailwind CSS
- React Context

## Avoiding Props Drilling with React Context and the useContext Hook

IN our index file we can warp our app with a Provider then export that provider so we can access it in our app component.

```javascript
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import * as serviceWorker from "./serviceWorker";

export const UserContext = React.createContext(); // Export UserContext

const username = "Dan"; // Value to export

ReactDOM.render(
  <UserContext.Provider value={username}>
    <App />
  </UserContext.Provider>,
  document.getElementById("root")
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: http://bit.ly/CRA-PWA
serviceWorker.unregister();
```

Then in our app component we bring in `useContext` from hooks and `UserContext`.

```javascript
import React, { useContext } from "react";
import { UserContext } from "./index";

export default function App() {
  const value = useContext(UserContext);

  return (
    <div>
      <h1>Hello, {value}</h1>
    </div>
  );
}
```

## Replacing Redux with the useReducer Hook

Lets build a counter to display how to implement `useReducer`.

```javascript
import React, { useContext, useReducer } from "react";
// import { UserContext } from "./index";

// Initial State
const initialState = {
  count: 0
};

// Reducer Function
function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return {
        count: state.count + 1
      };
    case "decrement":
      return {
        count: state.count - 1
      };
    case "reset":
      return initialState;
    default:
      return initialState;
  }
}

export default function App() {
  const [state, dispatch] = useReducer(reducer, initialState); // Get updated state
  // const value = useContext(UserContext);

  return (
    <div>
      Count: {state.count}
      <button
        onClick={() => dispatch({ type: "increment" })}
        className="border p-1"
      >
        Increment
      </button>
      <button
        onClick={() => dispatch({ type: "decrement" })}
        className="border p-1"
      >
        Decrement
      </button>
      <button
        onClick={() => dispatch({ type: "reset" })}
        className="border p-1"
      >
        Reset
      </button>
    </div>
  );
}
```

## Combining useContext and useReducer to Make Initial App Stat

#### Steps

- Remove App component
- Recreate App component inside the index file.
- Create context.js
- Create reducer.js
- Create components folder with TodoList.js

  `index.js`

```javascript
import React, { useContext, useReducer } from "react";
import ReactDOM from "react-dom";
import * as serviceWorker from "./serviceWorker";
import TodosContext from "./context";
import todosReducer from "./reducer";
import TodoList from "./components/TodoList";

const App = () => {
  const initialState = useContext(TodosContext);
  const [state, dispatch] = useReducer(todosReducer, initialState);

  return (
    <TodosContext.Provider value={{ state, dispatch }}>
      <TodoList />
    </TodosContext.Provider>
  );
};

ReactDOM.render(<App />, document.getElementById("root"));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: http://bit.ly/CRA-PWA
serviceWorker.unregister();
```

`context.js`

```javascript
import React from "react";

const TodosContext = React.createContext({
  todos: [
    { id: 1, text: "Eat breakfast", complete: false },
    { id: 2, text: "Do laundry", complete: false },
    { id: 3, text: "Finish project", complete: true }
  ]
});

export default TodosContext;
```

`reducer.js`

```javascript
export default function reducer(state, action) {
  switch (action.type) {
    default:
      return state;
  }
}
```

`TodoList.js`

```javascript
import React, { useContext } from "react";
import TodosContext from "../context";

export default function TodoList() {
  const { state } = useContext(TodosContext);

  return (
    <div>
      <ul>
        {state.todos.map(todo => (
          <li key={todo.id}>
            <span>{todo.text}</span>
          </li>
        ))}
      </ul>
    </div>
  );
}
```
