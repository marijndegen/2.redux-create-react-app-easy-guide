# redux-create-react-app-easy-guide
This guide assumes create react app has been succesfully done, so follow this guide before getting into this one: https://github.com/marijndegen/styled-create-react-app-easy-debug-guide

1. yarn add @reduxjs/toolkit react-redux
2. Create a directory src/redux
3. cd redux
4. Create a file store.ts and fill it with this content:
```
import { configureStore, combineReducers } from "@reduxjs/toolkit";
import mySlice from "./mySlice";

const rootReducer = combineReducers({
  mySlice,
  // Your other reducers here
});

export const store = configureStore({
  reducer: rootReducer,
});

export type AppDispatch = typeof store.dispatch;
export type RootState = ReturnType<typeof store.getState>;
```

4. Create a file mySlice.ts with the following content:
```
import { createSlice } from "@reduxjs/toolkit";

interface CounterState {
  value: number;
}

const initialState = { value: 0 };

const counterSlice = createSlice({
  name: "counter",
  initialState: initialState as CounterState,
  reducers: {
    increment: (state: CounterState) => {
      state.value += 1;
    },
  },
});

export const { increment } = counterSlice.actions;

export default counterSlice.reducer;

```
5. In index.tsx, import store and Provider:
```
import { Provider } from "react-redux";
import { store } from "./redux/store";
```
6. In index.tsx, wrap the provider to provide the store (replace `<App />` with the following):
```
<Provider store={store}>
  <App />
</Provider>
```
7. Replace the content of app.tsx with:
```
import { useDispatch, useSelector } from "react-redux";
import { increment } from "./redux/mySlice";
import { RootState } from "./redux/store";

function App() {
  const counter = useSelector((state: RootState) => state.mySlice.value);
  const dispatch = useDispatch();

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
