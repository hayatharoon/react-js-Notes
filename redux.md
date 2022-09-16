# **Redux:**

Redux is an independent javascript architecture for dealing with the state in predictable way.

Redux is a centralized store for state for the entire apppication.

![alt text](image/../images/redux.png)

## **Four steps to create React App:**

1. **Design the Store:** What we want to keep in the store.
2. **Define the Action:** What are the action that user can perform in this application.
3. **Create Reducer:** Reducer is plain javascript logic which takes action and current state and decide what kind of action going to be perform.
4. **Set up the Store:** set up the store according to the application requirement.

## **Writing Clean Code with Redux:**

- First thing for writing clean code that moved your all `redux` code inside a separate folder.
  ![alt text](image/../images/redux_folder.PNG)
- Second thing we want to do for imporving structure of the project is to group file by `feature` folders.
  ![alt text](image/../images/redux_folder_two.PNG)
  It will break down our application into a bunch of small sub bounded contexts. Each sub domain should have its own action, actionTypes, reducers etc.

The common issue with redux is that everytime we want to support a new action, we have to touch three different files:

- Constant Action types
- Action creators
- Reducers

So, some developer prefer to merge these files into a single file and This is called `DUCKS PATTERN`.

## **Ducks Pattern:**

Duck is essentailly a bundle of actions type, action and reducers for the specific domain.

## **Redux ToolKit:**

### **Creating Store:**

```javascript
import { configureStore } from '@reduxjs/toolkit';

export default function () {
  return configureStore({ reducer });
}
```

### Creating Action:

```javascript
import { createAction } from '@reduxjs/toolkit';
```

```javascript
const action = createAction('Bug Fixing');
console.log(action);
```

We get a function called `Bug Fixing`.
and if we call this function:

```javascript
console.log(action());
```

Then this will return `action` object having two properties:

- type
- payload

And when we call argument to the `action`:

```javascript
console.log(action({ id: 1 }));
```

Then it will return an object with `type: "Bug Fixing"` and `payload: {id: 1}`

### Creating Reducer:

```javascript
import { createReducer } from '@reduxjs/toolkit';
```

`createReducer()` take two argument:

- initial State
- Object: which consist of key value pair. The key is `action` and `value` is a function which handle the actions.

```javascript
createReducer([], {
  bugAdded: (state, action) => {
    bugs.push({
      id: ++lastID,
      description: action.payload.description,
      resolved: false,
    });
  },
  bugResolved: (bugs, action) => {
    const index = bugs.findIndex((bug) => bug.id === action.payload.id);
    state[index].resolved = true;
  },
});
```

### Creating Slice:

Slice create `actions` and `reducer` for us actumatically:

```javascript
import { createSlice } from '@reduxjs/toolkit';
```

```javascript
const slice = createSlice({
  name: 'bug',
  initialState: [],
  reducers: {
    bugAdded: (state, action) => {
      bugs.push({
        id: ++lastID,
        description: action.payload.description,
        resolved: false,
      });
    },
    bugResolved: (bugs, action) => {
      const index = bugs.findIndex((bug) => bug.id === action.payload.id);
      state[index].resolved = true;
    },
  },
});
```
