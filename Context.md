# **Context:**

Consider a React app that have alot of Components. And there is a situation to pass data into the lowest component in the tree. So, we have to pass down the data to the child component and then child component pass that data into has child component and so on and then finally the data reached to the desired component.

In this case the upper component does not need the data but still he receives that props to pass through them to be able to pass into has child component.

This is not the way pass the data to the component. We need solution that directly pass the data into the desirable component. So the **Context** come into the picture.

### **Context:**

Context provides a way to pass data through the component tree without having to pass props down manually at every level.

### **When to use Context:**

Context is design to share data that can be considered "global" for the tree of React components:

- The current authenticated user
- Theme
- Perferred language.

### **There are three steps to create Context:**

1. Create the context
2. Provide a context value.
3. Consume the context value.

**Example:**
In this example I take Home, Main, Navbar and Footer Component which are nesting inside and the lower Component is `Footer` component. So, I pass the data from the `Home` component to the `Footer` Component.

`App.js File`

```javascript
import './App.css';
import Home from './Components/Home';

function App() {
  return (
    <div className='App'>
      <Home />
    </div>
  );
}

export default App;
```

`Home.js File`

```javascript
import React, { createContext } from 'react';
import Main from './Main';

export const username = createContext();

function Home() {
  return (
    <div>
      <username.Provider value='Atrilogic'>
        <Main />
      </username.Provider>
    </div>
  );
}

export default Home;
```

`Main.js File`

```javascript
import React from 'react';
import Navbar from './Navbar';
function Main() {
  return (
    <div>
      <Navbar />
    </div>
  );
}

export default Main;
```

`Navbar.js File`

```javascript
import React from 'react';
import Footer from './Footer';

function Navbar() {
  return (
    <div>
      <Footer />
    </div>
  );
}

export default Navbar;
```

`Footer.js File`

```javascript
import React from 'react';
import { username } from './Home';

function Footer() {
  return (
    <username.Consumer>
      {(username) => {
        return <h2>The username is {username}</h2>;
      }}
    </username.Consumer>
  );
}

export default Footer;
```

### Step 1: Create the context

```javascript
export const username = createContext();
```

### Step 2: Provide a context value:

```javascript
<username.Provider value='Atrilogic'>
  <Main />
</username.Provider>
```

### Step 3: Consume the context value:

```javascript
import { username } from './Home';

<username.Consumer>
  {(username) => {
    return <h2>The username is {username}</h2>;
  }}
</username.Consumer>;
```
