# Keywords

# Framworks

## Vite vs Next.js vs webpack


# React 

## Components

## Hooks
https://stackoverflow.com/questions/58252454/react-hooks-using-usestate-vs-just-variables

## props
    Way of passing data from parent components to child.
    using {} to pass argument
    ![alt text](image.png)

## Immutability vs shallow-copy vs deep copy
Immutability: won't change the value of the original object/array.

When doing shallow-copy:
The primitive type value will not be altered.
But object is copied with reference and change the element in the object may lead to changes in the original object/array too.

* Objects will reflect change in the original place from where they were shallowly copied because they are stored as references (to their address in the Heap).

* Primitive data types will NOT reflect change in the original place because they are directly stored in the callstack (in Execution Contexts).

e.g.
```javascript
let animals = ['ant', 'bison', 'camel', [1, 2]];

let t = animals.slice();

t[0] = 'aaa';    // string (primitive datatype)
t[t.length-1][0] = 0;    // array (object)

console.log(t);
console.log(animals);
```
https://stackoverflow.com/questions/47738344/does-javascript-slice-method-return-a-shallow-copy

## StrictMode 

## TypeScript vs JSX vs JS

## Render
* Q: When is the html re-rendered by React
* Q: Generally, what decides the latency of a webpage, What contributes a big portion of it? Like rendering?

### Rendering list in react
When a list is re-rendered, React takes each list item’s key and searches the previous list’s items for a matching key. If the current list has a key that didn’t exist before, React creates a component. If the current list is missing a key that existed in the previous list, React destroys the previous component. If two keys match, the corresponding component is moved.

# CSS

- .board-row:after : specifies the rules for the element after the current element (board-row)

# JS
```typescript
parameter? : type
```
is equal to
```typescript
parameter: type | undefined
```
