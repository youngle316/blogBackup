# Learn Redux Toolkit By Todo List App

# Learn Redux Toolkit By Todo List App

> [Redux Toolkit](https://redux-toolkit.js.org/) is the official, opinionated, batteries-include toolset for efficient Redux development

# Introduction

Redux is a state management for frontend lib, like React. It is complex and not easier to use and understand. So the Redux Toolkit is out. Less code, less api, but more powerful. In this tutorial, I do not introduce what is redux, I will explain how to use it in a little app. I prefer to use Redux Toolkit in your project. I also show how to use typescript with redux.

# The UI

The UI such like this below. Not focus on css, very simple.

- click the "play" can change it to done
- click the doing or done button can change the status to filter the todo list

![https://cdn.hashnode.com/res/hashnode/image/upload/v1664461656834/ifbq-99c6.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664461656834/ifbq-99c6.png)

# Create Project

Now we should use [Create React App](https://create-react-app.dev/) tool to init our project.

```shell
npx create-react-app todoList --template typescript
```

## Create the static page

- create components
- change `App.tsx`

![https://cdn.hashnode.com/res/hashnode/image/upload/v1664463923956/F1vw1Oqtb.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664463923956/F1vw1Oqtb.png)

```typescript
import TodoList from "./components/TodoList/TodoList";
import Filter from "./components/Filter/Filter";
import SearchPanel from "./components/SearchPanel/SearchPanel";
import "./App.css";

function App() {
  return (
    <div className="main">
      <SearchPanel />
      <div className="todoList">
        <TodoList />
      </div>
      <Filter />
    </div>
  );
}

export default App;
```

# Use Redux Toolkit

## Install Redux Toolkit

Also need to install `react-redux`

```bash
npm install @reduxjs/toolkit react-redux
```

## Create Store

Store is where the state is stored globally

### Create store file

![https://cdn.hashnode.com/res/hashnode/image/upload/v1664508278813/b7Cmf8nAu.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664508278813/b7Cmf8nAu.png)

### config store

use [configureStore](https://redux-toolkit.js.org/api/configureStore) api to create store. Now we create an empty store

```typescript
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {},
})

export type RootState = ReturnType<typeof store.getState>
export type AppDispatch = typeof store.dispatch
```

### Provide redux store to react

change `index.tsx` file

```typescript
import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";
import { store } from "./app/store";
import { Provider } from "react-redux";

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

## Create Redux State Slice

Add two slice file named `filter` and `todoList`

![https://cdn.hashnode.com/res/hashnode/image/upload/v1664519479417/ZlAz8WeGG.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664519479417/ZlAz8WeGG.png)

`/src/features/filter/filterSlice.ts`

```typescript
import { createSlice, PayloadAction } from "@reduxjs/toolkit";

export type StatusType = 'all' | 'done' | 'doing'

interface FilterProps {
  status: StatusType
}

const initialState: FilterProps = {
  status: 'all'
}

export const filterSlice = createSlice({
  name: 'filter',
  initialState,
  reducers: {
    changeStatus: (state, action: PayloadAction<StatusType>) => {
      state.status = action.payload
    }
  },
})

export const { changeStatus } = filterSlice.actions

export default filterSlice.reducer;
```

`/src/features/todoList/todoListSlice.ts`

```typescript
import { createSlice, PayloadAction } from '@reduxjs/toolkit'

interface TodoListProps {
  allTodo: TodoItemProps[]
}

export interface TodoItemProps {
  id: string
  name: string
  isComplete: boolean
}

const initialState: TodoListProps = {
  allTodo: [{id: '0', name: 'play', isComplete: false}]
};

export const todoListSlice = createSlice({
  name: 'todoList',
  initialState,
  reducers: {
    addItem: (state, action: PayloadAction<string>) => {
      const item: TodoItemProps = {
        id: (Math.random() * 1000000).toFixed(0),
        name: action.payload,
        isComplete: false,
      }
      state.allTodo.push(item);
    },
    changeItemStatus: (state, action: PayloadAction<string>) => {
      const item = state.allTodo.find((item) => item.id === action.payload)
      if (item) {
        item.isComplete = !item.isComplete;
      }
    },
  }
})

export const { addItem, changeItemStatus } = todoListSlice.actions;

export default todoListSlice.reducer;
```

Create a slice require a string name identify slice, need a initialState, and one or more reducer functions to define how the state can be updated

then export the action creators and reducer for the whole slice

## Add Slice Reducers to the Store

Just import all reducers and add to the store

```typescript
import { configureStore } from '@reduxjs/toolkit'
import todoListReducer from '../features/todoList/todoListSlice'
import filterReducer from '../features/filter/filterSlice'

export const store = configureStore({
  reducer: {
++    todoList: todoListReducer,
++    filter: filterReducer,
  },
})

export type RootState = ReturnType<typeof store.getState>
export type AppDispatch = typeof store.dispatch
```

# Use The State

## Use State

We can use `useSelector` to get the state

```typescript
import { useSelector } from "react-redux";
import { RootState } from "../../app/store";

const filter = useSelector((state: RootState) => state.filter.status);
```

## Change State

```typescript
import { useDispatch } from "react-redux";
import { changeStatus } from "../../features/filter/filterSlice";

const dispatch = useDispatch();

dispatch(changeStatus(status));
```

For now we can get state and can change state, the todoList slice is same, you can clone this [github repository](https://github.com/youngle316/todoapp) to get all code

# Final Output


![redux_toolkit.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1664522182870/aSo5JQX40.gif align="left")

# Conclusion

When you need to use Redux in your project, I prefer Redux Toolkit than regular redux way. This way less code, less api, easier to understand. And it contain redux-thunk, I will update this part.

That’s all, Thank you.