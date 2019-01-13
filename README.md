## Building a Complete CRUD App with React Hooks / Replacing Redux

I will build a TODO app using React Hooks and Replacing Redux.

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
