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

You can often see implementations that use more than one implementation, depending on the use case. With react, the implementation is quite simple, we just need to be careful with some limitations, soon you’re gonna see what they are.
