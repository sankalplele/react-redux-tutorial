# Redux

A tool for state management in JS apps.

## react-redux

Specific for ReactJS, acts as a bridge between core Redux and ReactJs

## redux-toolkit

ease of setting up redux. Built-in middlewares, Slicing.

## store

Where the data is stored globally

## reducers

To update data in store

## useSelector and useDispatch

to get and dispatch data

## Installation

```
npm i @reduxjs/toolkit
npm i react-redux
```

Then we go on to adding `store.js` in `/src/app` directory.

```
import { configureStore } from "@reduxjs/toolkit";

export const store = configureStore({});
```

Now, we will create Reducers in `src/features` in the form of Slices example, `todoSlice` using `createSlice()` which comes from reduxToolkit

> The thing about redux is here we create slices like we created contexts in ContextAPI, and each slice has its reducers which facilitate working with the data, i.e. the functionality like addTodo() etc. The thing which is different here is that in ContextAPI we only gave the reference of the functions but in redux we give the function definition there and then.

> While defining the reducers, we have to use the parameters, `state` and `action`. `state` gives us access to the current state which is a result of updations done in the `initialState` specified earlier and `action` gives us things like `action.payload.id` which help us know why this function is called or from where it is called and so on.

So, unlike in ContextAPI where we had to do spreading of arrays etc. here we get `state.todos` and we can simply do whatever we want with it.

So here is the `todoSlice.js` code :

```
import { createSlice, nanoid } from "@reduxjs/toolkit";

const initialState = {
  todos: [{ id: 1, text: "Hello World" }],
};

export const todoSlice = createSlice({
  name: "todo",
  initialState,
  reducers: {
    addTodo: (state, action) => {
      const todo = {
        id: nanoid(),
        text: action.payload.text,
      };
      state.todos.push(todo);
    },
    removeTodo: (state, action) => {
      state.todos = state.todos.filter((todo) => {
        return todo.id !== action.payload;
      });
    },
  },
});

// since we will be using and calling reducers individually we need to export them individually
export const { addTodo, removeTodo } = todoSlice.actions;

// to tell the store that we have these reducers we need to export these reducers
export default todoSlice.reducers;

```

And similarly here we have the updated `store.js` file.

```
import { configureStore } from "@reduxjs/toolkit";
import todoReducer from "../features/todo/todoSlice.js";

export const store = configureStore({
  reducer: todoReducer,
});
```
