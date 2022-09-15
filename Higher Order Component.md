# **Higher Order Component:**

A higher order component(HOC) is an advanced technique in React for reusing component logic.

**Higher Order Component is a function that takes a component is an argument and return a new enhanced component.**

```javascript
const enhancedComponent = higherOrderComponent(wrappedComponent);
```

- Whereas a component transforms props into UI, a higher order component transforms a component into another component.
- HOCs are common in third-party libraries, such Redux `connect` and Relay's `createFragmentContainer`.

## **Example:**

Let's see we have movies list database and we want to display the movies in the card-form. But leter we also want to display the same movie list in the tabular form.

So, if we are writing separate code for fetching the movies data in both component. That should be break our DRY(Don't repeat yourself) principle.

The solution is write `Higher Order Component` which contain the data fetching logic and take the other component and enhanced them.

`App.js File`

```javascript
import './App.css';
import MoviesList from './Components/MoviesList';

function App() {
  return (
    <div className='App'>
      <MoviesList />
    </div>
  );
}

export default App;
```

`WithFetch.js File`

```javascript
import React from 'react';

// * Create a HOC that take a component as a argument.
const withFetch = (WrappedComponent) => {
  // * Create a class component that enhanced the wrappedComponent.
  class WithFetch extends React.Component {
    constructor(props) {
      super(props);

      this.state = {
        movies: [],
      };
    }
    componentDidMount() {
      //? Fetch the movie data by making an API call
      fetch('http://json-faker.onrender.com/movies')
        .then((response) => response.json)
        .then((data) => {
          this.setState({ movies: data.movies });
        });
    }
    render() {
      return <>{this.state.movies.length > 0 && <WrappedComponent movies={this.state.movies} />}</>;
    }
  }

  // * It is an optional to set the display name of the component but useful when comes to debug the HOC
  WithFetch.displayName = `WithFetch(${WithFetch.name})`;

  //* Finally return the component.
  return WithFetch;
};

export default withFetch;
```

`MovieList.js File`

```javascript
import React from 'react';
import WithFetch from './WithFetch';

const MoviesList = (props) => {
  const { movies } = props;
  return (
    <div className='movie-container'>
      <h2>Movie List with HOC</h2>
      {
        <ul>
          {movies.map((movies, index) => (
            <li key={index}>
              <img src={movies.poster} alt={movies.title} />
              <span>{movies.title}</span>
            </li>
          ))}
        </ul>
      }
    </div>
  );
};

// ! Important step in HOC. Wrapped the component with HOC.
export default WithFetch(MoviesList);

//* Same logic with two lines:
//? const HOC = WithFetch(MoviesList);
//? export default HOC
```

## **Don't Mutate the Original Component. Use Composition:**

If we mutate the Original component then there is a few problem with this:

- The input component cannot be reused separately from the enhanced component.
- More crucially, if you apply another HOC to `EnhancedComponent` that also mutates `componentDidUpdate`, the first HOC’s functionality will be overridden!. This HOC also won’t work with function components, which do not have lifecycle methods.

## **Wrap the Display Name for Easy Debugging**
The container components created by HOCs show up in the React Developer Tools like any other component. To ease debugging, choose a display name that communicates that it’s the result of a HOC.

The most common technique is to wrap the display name of the wrapped component.

```javascript
// * It is an optional to set the display name of the component but useful when comes to debug the HOC
  WithFetch.displayName = `WithFetch(${WithFetch.name})`;

```
