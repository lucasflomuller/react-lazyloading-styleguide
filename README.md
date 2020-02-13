# React lazyloading styleguide

A styleguide to improve performance over react applications using the lazyloading technique

## CONCEPT: What is lazyloading

In general terms, lazy loading is a technique used to avoid instantiating an element - an object, a component etc. - until it’s truly necessary. It is one of the main techniques for improving the performance of a system and it is used both on front-end and back-end. The improvements can be significant in objects that have an expensive instantiation, as an example we can think of the instantiation of an object that requires a request to a database. It is worth saying that there are cases where even though the object can be quickly instantiated, lazy loading is an improvement that can still be discussed based on: Difficulty to implement, frequency of instantiation, pre-defined patterns etc. The opposite of lazy loading is eager loading.

##### Example
Good use case for lazy loading in C#: https://stackoverflow.com/questions/10559279/how-to-deal-with-costly-building-operations-using-memorycache/15894928#15894928



## IMPLEMENTATION

There are four common ways to implement a lazy loading:
- Lazy initialization; using null
- Virtual proxy; object with same interface
- Ghost; loaded in partial state
- Value holder; generic object handling lazy behavior

You can often see implementations that use more than one way, depending on the use case. With react, the implementation is quite simple, for this we use `react-lazyload` library.

Important, the use of lazyloading on images is where the performance boosts significantly, so try to think about that when implementing a component with too much of them.

### react-lazyload

`react-lazyload` is a library that helps lazyload components, images or any other component. It is very simple to use.
Basically, the library gives you a `Lazyload` component that you can wrap on your component. As the example bellow:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import LazyLoad from 'react-lazyload';
import MyComponent from './MyComponent';

const App = () => {
  return (
    <div className="list">
      <LazyLoad height={200}>
        <img src="tiger.jpg" /> /*
                                  Lazy loading images is supported out of box,
                                  no extra config needed, set `height` for better
                                  experience
                                 */
      </LazyLoad>
      <LazyLoad height={200} once >
                                /* Once this component is loaded, LazyLoad will
                                 not care about it anymore, set this to `true`
                                 if you're concerned about improving performance */
        <MyComponent />
      </LazyLoad>
      <LazyLoad height={200} offset={100}>
                              /* This component will be loaded when it's top
                                 edge is 100px from viewport. It's useful to
                                 make user ignorant about lazy load effect. */
        <MyComponent />
      </LazyLoad>
      <LazyLoad>
        <MyComponent />
      </LazyLoad>
    </div>
  );
};

ReactDOM.render(<App />, document.body);
```

It also gives you a decorator to lazyload you component by default:

```javascript
import { lazyload } from 'react-lazyload';

@lazyload({
  height: 200,
  once: true,
  offset: 100
})
class MyComponent extends React.Component {
  render() {
    return <div>this component is lazyloaded by default!</div>;
  }
}
```

Important notes: 
- If your lazyloaded component is inside an overflow you need to set the overflow property to true and also make sure that your component has a position property other than `static`.
- You can provide a placeholder to your component as a fallback whilst your component doesn't render.

#### Links
- [Official repo](https://github.com/twobin/react-lazyload)
- [Demos](https://twobin.github.io/react-lazyload/examples/#/placeholder?_k=j8fxc4)

### The official react way

As of React v16.6 you can use the built-in function `React.lazy` to code-split your components, together with the `Suspense` component you can have a loading state, this is also a way to lazy load your components, since they'll be loaded only after the user request it, but this time all of the lazy loaded components will require a request to a new chunk of code.

before:
```
import OtherComponent from './OtherComponent';
```

after:
```
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

Limitation: React.lazy and Suspense are not yet available for server-side rendering. If you want to do code-splitting in a server rendered app, it is recommended the use of Loadable Components. It has a nice guide for bundle splitting with server-side rendering.

You can also see a document about code-splitting on Jungle dev's WIKI.
