# March 4th, 2019

## TypeScript: Differentiating Types

[Type Guards](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-guards-and-differentiating-types)

if you have a parameter you cannot feed a check for a certain type into a variable and have the compiler recognize that if the check is true it has to be of one type or another.

For Example:

```typescript
interface Thing {
    total: number
    percentage: number;
}

function getNumber(thing: Thing | number): { basic: number} {
    const isNumber = typeof thing === 'number';
    const isSingular = isNumber ? thing === 1 : thing.total === 1;
    return { basic: isNumber ? thing : thing.percentage }
}
```

In this code snippet `.total` and `.percentage` would throw errors because typescript would see that those properties don't exist on the `number` type even though we did an explicit check for them.

However, If we moved the check inline it would pass.

For Example:

```typescript
interface Thing {
    total: number
    percentage: number;
}

function getNumber(thing: Thing | number): { basic: number} {
    const isSingular = typeof thing === 'number' ? thing === 1 : thing.total === 1;
    return { basic: typeof thing === 'number' ? thing : thing.percentage }
}
```

In this snippet everything type checks because the compiler is able to recognize that thing has to be a number when the `typeof` check is true. But this is suboptimal because I have to rewrite the check everytime `thing` is referenced. To fix this typescript has a feature called type guards. The way type guards work is that you can take in a parameter to a function and as the functions return type you would cast it to whatever type you expect it to be.

Consider: 

```typescript
interface Thing {
    total: number
    percentage: number;
}


function isNumber(x: any): x is number {
    return typeof x === "number";
}

function stuff(a: Thing | number) {
    const isSingular = isNumber(a) ? a === 1 : a.total === 1;
    return { basic: isNumber(a) ? a : a.percentage }
} 
```

This would typecheck properly. But there is a big problem with casting it in this way. Imagine for a moment that you made a mistake and wrote `typeof x === "string"`. Like so:

```typescript
interface Thing {
    total: number
    percentage: number;
}


function isNumber(x: any): x is number {
    return typeof x === "string";
}

function stuff(a: Thing | number) {
    const isSingular = isNumber(a) ? a === 1 : a.total === 1;
    return { basic: isNumber(a) ? a : a.percentage }
}
```

Here we can tell that the return type clearly cant be a number because we checked that it was a string. Yet when we call isNumber we do not get any warnings that something is amiss. Instead typescript assues that everything is a number even though the code says otherwise. This is a poor approach.

## Todo on Gigg

1. Getting a 502 on `/exchange-token` once mongo stuff is pulled in.
2. need to get long lived token in `Instagram.js`.
3. get facebook userId
4. pass long lived token and userId to `Instagram constructor` as class properties.
5. use token and userId in calls to graph api
