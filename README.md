# SUPREME
Well, these are just notes...


Topics:
- JavaScript
- [TypeScript](#typescript)
    - [Generics](#generics)
- ReScript
- Functional Programming
- Object-oriented Programming


# TypeScript

## Generics

Generics allow us to reuse code that can work with a variety of types

Example: 

Say we have a function that returns the length of a given array:

```typescript
let getLength = (arr: Array<string>) => arr.length;
```

This function can only operate on arrays and only array of strings.
What if we could reuse the same function to get the length of a custom data-structure or a string for say. In this case we would need to write another function for each different data-structure, and that's huh lame.

With generics, we can make the `getLength` function also work with strings or any data-structure as long as they have a `length` property.

```typescript
let getLength = <T extends {length: number}>(data: T) => data.length;

getLength("Hello, world!!"); // TS will infer the type;
getLength<Array<string>>(["Hello", "world!!"]); // 
```

`T` here is a type parameter, just like function parameter but for types, and you can have as many as you want. Whichever is the type of the parameter we pass to the function `getLength`, `T` will hold that information.

Let's push things a bit further, let's create a `reduce` function and make it generic:

```typescript
let reduce = (array: any, callback: any, initialValue: any) => array.reduce(callback, initialValue);
```
Well, the function is already generic and I'm using `any` to make it so, but if we use like this, we lose all typing information and validation. So, lemme fix it.

```typescript
let reduce = <T, I>(
    array: T[], 
    callback: (acc: I, value: T) => I, 
    initialValue: I
) => array.reduce(callback, initialValue);

let sum = reduce([2, 3, 4], (acc, value) => acc + value, 0);
sum // result here has type of number

let hello = reduce(['h', 'e', 'l', 'l', 'o'], (acc, value) => acc + value, '');
hello // hello here has type of string
```

Here I have two type variables `T`and `I` and I could name them whatever I want, they'll hold the type information of my function parameter and could explicitly tell what they are or just let the compiler infer them.


We can also make generic `interfaces` and `classes`, here's how:

```typescript
interface GIdentity {
    <Value>(value: Value): Value
}

let identity = <Input>(a: Input): Input => a;
let identity2: GIdentity = identity;

identity2<string>("3");
```

```typescript
interface GIdentity<Value> {
    (value: Value): Value
}

let identity = <Input>(a: Input): Input => a;
let identity2: GIdentity<number> = identity;

identity2(3);
```

```typescript
class DataBase<T> {
    value: T;
    constructor(value: T) {
        this.value = value
    }
    get(): T {
        return this.value;
    }
}

const db = new XCoDataBasensole("2");
console.log(db.get());
```

