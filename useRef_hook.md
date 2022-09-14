# **useRef Hook:**

`useRef` is a react hook that is basically used to store the reference of something like DOM element.
`useRef` takes a single argument as initial value and return an object with a single property called `.current`.

The `current` holds the reference of whatever we tell to hold to.

### **Using useRef hook Steps:**

1. First import the Hook:

```javascript
import { useRef } from 'react';
```

2. Then declare and initialized:

```javascript
const inputELment = useRef(null);
```

useRef doesn't notify when its content changes. Mutating the `.current` property doesn't cause a re-render.

## **Example 1: Focused on Input Field:**

```javascript
import React, { useRef } from 'react';
import './App.css';

function App() {
  const inputFocus = useRef(null);
  function clickHandle() {
    // console.log(inputFocus.current);
    inputFocus.current.focus();
  }
  return (
    <div className='App'>
      <>
        <input type='text' ref={inputFocus} />
        <button onClick={clickHandle}>Focused Button</button>
      </>
    </div>
  );
}

export default App;
```

## **Example 2: How many time our component is re-render:**

```javascript
import React, { useState, useRef } from 'react';
import { useEffect } from 'react';
import './App.css';

function App() {
  const [counter, setCounter] = useState(0);
  const renderCount = useRef(1);

  useEffect(() => {
    renderCount.current = renderCount.current + 1;
  });
  return (
    <div className='App'>
      <h3>{counter}</h3>
      <button onClick={() => setCounter(counter + 1)}>Increment</button>
      <button onClick={() => setCounter(counter - 1)}>Decrement</button>
      <h3>This component is render {renderCount.current} </h3>
    </div>
  );
}

export default App;
```

**Conculsion:**
We use useRef hook for:

- Store and update mutable information without triggering re-renders.
- Imperatively access DOM nodes via refs to perform some functions.
- Store reference of the element.

# **forwardRef:**

Ref forwarding is a technique for automatically pass `ref` through a component to one of its children.

forwardRef takes a callback function which have two arguments, one is _props_ and the other one is _ref_ that we want to forward.

Now we have four simple steps to use forwardRef concept:

1. First we create a `React ref` by calling `React.createRef` and assign to a variable.
2. We pass through our ref down `<Input ref={inputRef} />` to the child component by specifying it as a JSX attribute.
3. React pass the `ref` to the `(props,ref)=>` function inside forwardRef as a second argument.
4. We forward this `ref` argument down to `<button ref={ref}>` by specifying it as a JSX attributes.

And when ref is attached, `ref.current` will point to the `<button>` DOM node.

**Example:**

`App.js File`

```javascript
import React from 'react';
import './App.css';
import { Button } from './Components/Button';

function App() {
  return (
    <div className='App'>
      <Button />
    </div>
  );
}

export default App;
```

`Button.js File`

```javascript
import React, { useRef } from 'react';
import { createRef } from 'react';
import { Input } from './Input';

export const Button = () => {
  const inputRef = createRef();
  function handleClick() {
    inputRef.current.focus();
  }
  return (
    <div>
      <Input ref={inputRef} />
      <button onClick={handleClick}>Focused</button>
    </div>
  );
};
```

`Input.js File`

```javascript
import React from 'react';
import { forwardRef } from 'react';

export const Input = forwardRef((props, ref) => {
  return (
    <div>
      <input type='text' ref={ref} />
    </div>
  );
});
```
