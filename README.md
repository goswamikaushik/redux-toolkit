# 🔥 **Redux Overview**

Redux is a state management library for JavaScript applications, primarily used with **React**. It provides a **centralized store** for managing the application state in a **predictable** manner, making state changes more consistent and easier to debug.

Redux follows a **unidirectional** data flow, where state updates are handled through **actions** and **reducers**. This structure simplifies state management, improves scalability, and enhances the developer experience through powerful debugging tools.

---

## 🚀 **What is Unidirectional Data Flow?**

> The term unidirectional refers to something that moves or operates in a single direction.
> 

In the context of software architecture and state management, it describes a data flow pattern where data moves in a **one-way direction** without reversing or creating circular dependencies.

### ✅ **Unidirectional Data Flow in Redux:**

1. **Action** – A user or system event triggers an action.
2. **Dispatcher** – The action is dispatched to a central store.
3. **Reducer** – The reducer processes the action and updates the state.
4. **View Update** – The updated state triggers a re-render of the UI.

---

## 😎 **Why Redux is Powerful:**

### ✅ **Predictable:**

- Redux follows a strict pattern where state updates are handled through **pure functions** called `reducers`.
- Pure functions ensure that:
    - Given the same input, they **always return the same output**.
    - No **side effects** (e.g., API calls or state mutations) occur inside reducers.
- This predictability makes debugging and testing much easier.

---

### ✅ **Maintainable:**

- Redux enforces a clear structure:
    - **Actions** – Define what happens.
    - **Reducers** – Define how the state changes.
    - **Store** – Holds the state.
- Each part has a defined role, improving **scalability** and **code readability**.
- The consistent structure makes the codebase easier to scale and maintain.

---

### ✅ **Global State Management:**

- Redux manages state in a **centralized store**.
- Useful for apps requiring global state, such as:
    - Authentication status
    - User profile
    - Theme settings
- State can be accessed and updated from multiple components.

---

### ✅ **Cross-Environment Support:**

- Redux is **environment-agnostic** — it works consistently across:
    - **Web apps** with React
    - **Mobile apps** with React Native
    - **Server-side rendering** (SSR) with Next.js

> Agnostic – Doesn’t depend on any specific environment or platform.
> 

---

### ✅ **Testing:**

- Redux relies on **pure functions** for state updates, making testing simple.
- Reducers are easy to test:
    - They take `state` and `action` as input.
    - Return predictable output.

---

## 💻 **Code Example:**

Here’s a complete Redux setup with **actions**, **reducers**, **store**, and **middleware**:

```tsx
tsx
CopyEdit
import { createStore, applyMiddleware, Middleware } from 'redux';

// ✅ Define Action Types
enum CounterActionType {
  INCREMENT = 'INCREMENT',
  DECREMENT = 'DECREMENT'
}

// ✅ Define Action Interfaces
type CounterAction =
  | { type: CounterActionType.INCREMENT }
  | { type: CounterActionType.DECREMENT };

// ✅ Action Creators
const increment = (): CounterAction => ({ type: CounterActionType.INCREMENT });
const decrement = (): CounterAction => ({ type: CounterActionType.DECREMENT });

// ✅ Reducer
const initialState: number = 0;

const counterReducer = (state = initialState, action: CounterAction): number => {
  switch (action.type) {
    case CounterActionType.INCREMENT:
      return state + 1;
    case CounterActionType.DECREMENT:
      return state - 1;
    default:
      return state;
  }
};

// ✅ Middleware (Logger)
const loggerMiddleware: Middleware = (store) => (next) => (action) => {
  console.log('Dispatching:', action);
  const result = next(action);
  console.log('Next State:', store.getState());
  return result;
};

// ✅ Create Store
const store = createStore(
  counterReducer,
  applyMiddleware(loggerMiddleware)
);

// ✅ Subscribe to Store Updates
store.subscribe(() => {
  console.log('State updated:', store.getState());
});

// ✅ Dispatch Actions
store.dispatch(increment()); // State updated: 1
store.dispatch(increment()); // State updated: 2
store.dispatch(decrement()); // State updated: 1

// ✅ Test Cases
test('increments the counter', () => {
  expect(counterReducer(0, { type: CounterActionType.INCREMENT })).toBe(1);
});

test('decrements the counter', () => {
  expect(counterReducer(1, { type: CounterActionType.DECREMENT })).toBe(0);
});

```

# 🚀 **Redux Toolkit Overview**

The **Redux Toolkit (RTK)** is the official, recommended way to write Redux logic. It was created to address the most common pain points developers face when working with Redux:

1. **Complex store configuration**
2. **Too many dependencies**
3. **Excessive boilerplate code**

Redux Toolkit simplifies the Redux setup process and provides powerful utilities that help reduce boilerplate and improve development efficiency.

---

## 💡 **Why Redux Toolkit?**

### ✅ **1. Complex Store Configuration**

- Setting up a Redux store manually involves:
    - Creating reducers
    - Combining reducers
    - Applying middleware
    - Configuring dev tools
- **Redux Toolkit** abstracts these complexities with the `configureStore()` utility, making it easy to set up a store with sensible defaults.

---

### ✅ **2. Too Many Dependencies**

- To use Redux effectively, you previously needed to install multiple packages:
    - `redux`
    - `redux-thunk`
    - `redux-devtools-extension`
    - `redux-logger`
- **Redux Toolkit** combines these into a single package — reducing dependency clutter and simplifying maintenance.

---

### ✅ **3. Boilerplate Code**

- Traditional Redux requires a lot of boilerplate code:
    - Writing action types
    - Creating action creators
    - Writing switch-case reducers
- **Redux Toolkit** reduces boilerplate with:
    - `createSlice()` – Automatically generates action creators and reducers
    - `createAsyncThunk()` – Handles async logic without boilerplate
    - `configureStore()` – Simplifies store setup

## 🔥 **Powerful Data Fetching with RTK Query**

Redux Toolkit includes a powerful data-fetching and caching solution called **RTK Query**.

### **What is RTK Query?**

- Built on top of Redux Toolkit
- Automates state management for server data
- Handles caching, invalidation, and background fetching
- Reduces the need to write complex data-fetching logic

### ✅ **RTK Query Provides:**

✔ Automatic caching and re-fetching

✔ Background updates

✔ Data invalidation and synchronization

✔ Normalized state handling

✔ Code reduction by managing side effects internally

---

## 💻 **Example: Basic Redux Toolkit Setup**

Here’s an example of a **Redux Toolkit** setup using `createSlice` and `configureStore`:

```tsx
tsx
CopyEdit
import { configureStore, createSlice, PayloadAction } from '@reduxjs/toolkit';

// ✅ Create Slice
const counterSlice = createSlice({
  name: 'counter',
  initialState: 0,
  reducers: {
    increment: (state) => state + 1,
    decrement: (state) => state - 1,
    addAmount: (state, action: PayloadAction<number>) => state + action.payload,
  },
});

// ✅ Export Actions
export const { increment, decrement, addAmount } = counterSlice.actions;

// ✅ Configure Store
const store = configureStore({
  reducer: {
    counter: counterSlice.reducer,
  },
});

// ✅ Usage Example
store.dispatch(increment());
console.log(store.getState()); // Output: 1

store.dispatch(addAmount(5));
console.log(store.getState()); // Output: 6

// ✅ TypeScript Setup
export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;

```

---

## 💻 **Example: RTK Query Setup**

Here’s how you can use **RTK Query** to manage data fetching:

```tsx
tsx
CopyEdit
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

// ✅ Create API Slice
const pokemonApi = createApi({
  reducerPath: 'pokemonApi',
  baseQuery: fetchBaseQuery({ baseUrl: 'https://pokeapi.co/api/v2/' }),
  endpoints: (builder) => ({
    getPokemonByName: builder.query({
      query: (name) => `pokemon/${name}`,
    }),
  }),
});

// ✅ Export Hooks
export const { useGetPokemonByNameQuery } = pokemonApi;

// ✅ Add to Store
const store = configureStore({
  reducer: {
    [pokemonApi.reducerPath]: pokemonApi.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(pokemonApi.middleware),
});

// ✅ Usage in Component
const Pokemon = ({ name }: { name: string }) => {
  const { data, error, isLoading } = useGetPokemonByNameQuery(name);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error!</div>;

  return <div>{data.name}</div>;
};

```

---

## 🧪 **Testing Reducers with Redux Toolkit**

Testing is straightforward since `createSlice` produces pure reducers.

```tsx
tsx
CopyEdit
import { counterSlice } from './store';

test('increment reducer works correctly', () => {
  expect(counterSlice.reducer(0, counterSlice.actions.increment())).toBe(1);
});

test('addAmount reducer works correctly', () => {
  expect(counterSlice.reducer(0, counterSlice.actions.addAmount(5))).toBe(5);
});

```

---

## ✅ **Summary:**

| Feature | Description |
| --- | --- |
| **Simplified Setup** | `configureStore()` reduces manual configuration. |
| **Reduced Boilerplate** | `createSlice()` automatically generates actions and reducers. |
| **Consolidated Dependencies** | Combines Redux, Thunk, and DevTools into a single package. |
| **RTK Query** | Built-in data fetching and caching solution. |
| **TypeScript Support** | Strong TypeScript support for better type inference and safety. |

---

## 🔥 **Why Use Redux Toolkit?**

✅ Less code, more functionality

✅ Easier state management

✅ Built-in data fetching and caching

✅ Consistent developer experience