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

## **Design Redux Store:**

### **Redux State vs Local State**

| Store Global data in redux | Store all data in redux |
| -------------------------- | :---------------------: |
| Esay to Implement          |   Unifies data access   |
|                            |      Cacheability       |
|                            |     Easier to debug     |
|                            |   More testable code    |

**Exception: `Form State`**

- Temprory values
- Too many dispatches
- Harder to debugging

> The more state we put in the store. The more we can get out of redux.

### **Structuring a Redux Store Best Practices:**

Two type of structure:

- Array of object.
- Using Object.

**Object provides fast lookUps, but they don't preserve order**.

**and Array is slow but its provide order.**

> So, If we need for fast lookups, use object.and If we need ordered data, use an array.

If we have multiple slices in the store:

**For Example:**

- bug: [ ]
- project: [ ]
- tags: [ ]

> Then it a good practice to put these slices under the parent slice called `entities`.

> And the another top level slice called `auth` which include the data about the current user such as username, ID, authentication.

> We have another top level slice called `UI` and this is where we can store state specific to certain page or component.

```javascript
{
  entities: {...},
  auth: {userId: 1, name: "Faisal"},
  UI: {
    bugs: {query: "", sortBy: ""}
  }
}
```

### **Combining Reducers:**

Combining reducer multiple reducer, So we can create a store with multiple slices.

`Create a reducer.js File:`

```javascript
import {combineReducers} from redux;
import bugsReducer from './bugs';
import projectReducer from './projects';

3xport default combineReducers({
  bugs: bugsReducer,
  projects: projectReducer,
})
```

Then goto: `store folder`:

```javascript
import reducer from 'reducer';
```

> So by combining these reducer, we are essentially creating a hierarchy of reducer function. At the top, we have the root reducer. and this is the reducer that our store talk to.

![alt text](image/../images/storeCombining.PNG)

If we dispatch an `action` our store passes that action to the root reducer. This root reducer will then pass that action to each child reducer.

### Normalization:

Another important principle to designa `Redux Store` is `Normalzation`. Normalization is basically means that we should not duplicate data in our store.

## **What is Middleware?**

When we dispatching action, you're essentially sending them through a single entry point.

![alt text](image/../images/middleware_1.PNG)

Now on the other side of the entry point, we have our reducer. So, when we dispatch an action, it goes through this pipeline, and reaches it then reaches it to the root reducer.
The root reducer will then pass this action to his child reducers.

Now, in this pipeline, we add function that would get executed an action is dispatched. This is what we call middleware because it sit in the middle.

`So, middleware is the piece if code that gets executed after an action is dispatched, and before it reaches to the root reducer.`

![middleware](images/../images/middleware_2.PNG)

**Middleware:**

- Calling APIs
- Error Reporting
- Analytics
- Authorization

### **How to create middleware function:**

- creating `middleware` folder in the `store` folder.
- create a middleware function inside `middleware` folder: such as `logger.js`.

This fucntion having three parameters:

- **action**: Action is the action that was dispatched.
- **next**: Next reference to the next middleware funciton. If this is the only middleware fucntion we have, next is going to be the reducer that is going to handle this action

```javascript
// curried version of the fucntion...
const logger = store => next => action {
  console.log("Store", store)
  console.log("Next", next)
  console.log("Action", action)
  next(action); //if we don't call next() the action is not going to be processed furthur.
}
export default logger;
```

Now in the configureStore File, the function take the configuration parameter. So in the object, we're specifying our reducer. and now, we can also specify our middleware functions

```javascript
import { configureStore } from '@reduxjs/toolkit';
import logger from '/middleware/logger';

export default function () {
  return configureStore({
    reducer,
    middleware: [logger],
  });
}
```

### **Middleware when not using redux-toolkit:**

```javascript
import {createStore, applyMiddleware} from 'redux';
import  reducer from './store/reducer';

const store = createStore(reducer,applyMiddleware(logger))
```