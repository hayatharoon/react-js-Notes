# **Persist Redux State with redux-persist:**

When we refresh page in a web-app, the state always resets to the initial value. With the Redux-persist library, developers can save the Redux store in presistent storage, for example: Local storage. Therefore even after refreshing the browser, the site will still be preserved.

## **Step 1:**

Install the package:

```
npm i redux-presist
```

## **Step 2:**

Add the required imports to the redux store:

```javascript
//store.js
import { persistStore, persistReducer } from 'redux-persist';
import storage from 'redux-persist/lib/storage';
```

## **Step 3:**

We need to create the persist object config that will be stored to the storage:

```javascript
const persistConfig = {
  key: 'country-app',
  storage,
};
```

The Key specifices the ID of the persist object and the storage determines the type of the storage being used:

## **Step 4:**

Create the persist Reducer:

```javascript
const persistedReducer = persistReducer(persistConfig, reducer);
const store = createStore(persistedReducer, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__());
const persistor = persistStore(store);

export default store;
export { persistor };
```

## **Step 5:**

Import `persistor` and `persistGate` in the `index.js` file.

```javascript
//index.js
import { persistor } from './store/store';
import { PersistGate } from 'redux-persist/integration/react';
```

and wrap the store with `persistGate` and pass the `persistor` as a argument.

```javascript
root.render(
  <BrowserRouter>
    <Provider store={store}>
      <PersistGate persistor={persistor}>
        <App />
      </PersistGate>
    </Provider>
  </BrowserRouter>
);
```
