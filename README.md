# redux-create-react-app-easy-guide
This guide assumes create react app has been succesfully done, so follow this guide before getting into this one: https://github.com/marijndegen/styled-create-react-app-easy-debug-guide

1. yarn add @reduxjs/toolkit react-redux
2. Create a directory src/redux
3. cd redux
4. Create a file store.ts and fill it with this content:
```
import { combineReducers, configureStore } from "@reduxjs/toolkit";
import type { PreloadedState } from "@reduxjs/toolkit";

import mySlice from "./mySlice";
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";

// Create the root reducer separately so we can extract the RootState type
const rootReducer = combineReducers({
  mySlice,
});

export const setupStore = (preloadedState?: PreloadedState<RootState>) => {
  return configureStore({
    reducer: rootReducer,
    preloadedState,
  });
};

export type RootState = ReturnType<typeof rootReducer>;
export type AppStore = ReturnType<typeof setupStore>;
export type AppDispatch = AppStore["dispatch"];
export const useAppDispatch: () => AppDispatch = useDispatch;
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

5. Create a file mySlice.ts with the following content:
```
import { createSlice } from "@reduxjs/toolkit";

interface CounterState {
  value: number;
}

const initialState = { value: 0 };

const counterSlice = createSlice({
  name: "counter",
  initialState, // Delete this comment when tested. This used to be a cast but that is not desired here: initialState: initialState as CounterState,
  reducers: {
    increment: (state: CounterState) => {
      state.value += 1;
    },
  },
});

export const { increment } = counterSlice.actions;

export default counterSlice.reducer;

```
6. In index.tsx, import setupStore, Provider and create the store:
```
import { Provider } from "react-redux";
import { setupStore } from "./redux/store";
const store = setupStore();
```
7. In index.tsx, wrap the provider to provide the store (replace `<App />` with the following):
```
<Provider store={store}>
  <App />
</Provider>
```
8. Replace the content of app.tsx with:
```
import React from 'react';
import { increment } from "./redux/mySlice";
import { useAppDispatch, useAppSelector } from "./redux/store";

function App() {
  const counter = useAppSelector((state) => state.mySlice.value);
  const dispatch = useAppDispatch();

  return (
    <>
      <button onClick={() => dispatch(increment())}>
        Click me to increment, {counter}
      </button>
    </>
  );
}

export default App;
```
