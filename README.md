# Redux Intro - Skapa Todolist

I denna uppgift ska du öva på grunderna i **Redux**.  
Målet är att förstå de tre viktigaste delarna:

1. **Store** – centraliserad state
2. **Actions** – beskriv vad som ska hända
3. **Reducer** – bestäm hur state förändras

Vi använder `react-redux` för att koppla Redux med React-komponenter.

> **Tid:** ~1–2 timmar  
> **Mål:** kunna skapa en store, dispatcha actions och läsa state i komponenter.

---

## Steg 1 – Skapa projekt

```bash
npm create vite@latest redux-intro -- --template react
cd redux-intro
npm install
```

Installera Redux och react-redux:

```bash
npm install redux react-redux
```

---

## Steg 2 – Projektstruktur

Skapa en mapp `src/store` med följande filer:

- `store.js` – själva store
- `actions.js` – definierar actions
- `reducer.js` – reducerfunktion

---

## Steg 3 – Reducer

En reducer är en funktion som uppdaterar state. Som input får den en action: En sträng med dess namn (type) och data som den behöver (payload), tillsammans med state (i det här fallet en todo-lista). En action kan vara till exempel:
`{type: "ADD_TODO", payload: "städa"}`.
Med hjälp av detta uppdateras todo-listan med en ny todo.

Skapa `src/store/reducer.js`:

```js
// Start-state
const initialState = {
  todos: [],
};

// Reducer – tar state och action, returnerar nytt state
export function reducer(state = initialState, action) {
  switch (action.type) {
    case "ADD_TODO":
      return {
        ...state,
        todos: [...state.todos, { id: Date.now(), text: action.payload }],
      };
    case "REMOVE_TODO":
      return {
        ...state,
        todos: state.todos.filter((todo) => todo.id !== action.payload),
      };
    default:
      return state;
  }
}
```

---

## Steg 4 – Actions

Skapa `src/store/actions.js`:

```js
export const addTodo = (text) => ({
  type: "ADD_TODO",
  payload: text,
});

export const removeTodo = (id) => ({
  type: "REMOVE_TODO",
  payload: id,
});
```

---

## Steg 5 – Store

Skapa `src/store/store.js`:

```js
import { createStore } from "redux";
import { reducer } from "./reducer";

export const store = createStore(reducer);
```

---

## Steg 6 – Provider i React

I `src/main.jsx`:

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";
import { Provider } from "react-redux";
import { store } from "./store/store";

ReactDOM.createRoot(document.getElementById("root")).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

---

## Steg 7 – App-komponent

Redigera `src/App.jsx`:

```jsx
import { useSelector, useDispatch } from "react-redux";
import { addTodo, removeTodo } from "./store/actions";

function App() {
  const todos = useSelector((state) => state.todos);
  const dispatch = useDispatch();

  function handleAdd(e) {
    e.preventDefault();
    const text = e.target.elements.todo.value.trim();
    if (!text) return;
    dispatch(addTodo(text));
    e.target.reset();
  }

  return (
    <div style={{ padding: "2rem" }}>
      <h1>Redux Intro – Todo List</h1>
      <form onSubmit={handleAdd}>
        <input name="todo" placeholder="Lägg till todo" />
        <button type="submit">Add</button>
      </form>

      <ul>
        {todos.map((t) => (
          <li key={t.id}>
            {t.text}{" "}
            <button onClick={() => dispatch(removeTodo(t.id))}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

---

## Steg 8 – Testa

```bash
npm run dev
```

1. Lägg till några todos.
2. Ta bort en todo med “Remove”.

---

## Steg 9

- Lägg till en action för att markera en todo som “klar”.
- Lägg till en counter i state som räknar antal todos.
- Visa antalet todos i UI.

---

## Success ✅

När du är klar kan du:

- Skapa en **store** med `createStore`
- Definiera **actions** och **reducers**
- Använda `useSelector` för att läsa state
- Använda `useDispatch` för att skicka actions

Detta är grunden till Redux!
