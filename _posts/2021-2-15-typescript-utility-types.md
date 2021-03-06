---
layout: post
title:  "Partial Utility Type in Typescript!"
---

# Partial Utility Type

Typescript has several utility types that make type transformations easier. In this blog post, I will describe the **Partial<Type>**:
* What it is.
* An example of how to achieve what it does without using it.
* An example the same thing as above, using the Partial type.

**What:** It is a generic utility type that takes a given Type and returns a new Type that represents all subsets of the original Type, but with all properties set to optional.

Assuming you have a `Type` defined that represents a `Todo` list with non-optional properties:
```typescript
type Todo = {
    title: string,
    completed: boolean,
    description: string
}
```

Using the above `Todo`, you can now create a new `Todo` object:
```typescript
let dailyTodo: Todo = {
  title: "Go for a run",
  completed: true,
  description: "Run for 10km"
};
```

The above makes all properties mandatory. So if you do not use any of the properties defined in the `Todo` type when creating the new object above. You will get a compilation error, similar to:
```
TS2739: Type '{ title: string; }' is missing the following properties from type 'Todo': completed, description
```

However, you may have a valid reason not to have values provided for the properties (e.g. there are default values). In such cases, you can make the properties optional by either:

**Manually setting the properties to Optional using `?`**
```typescript
type Todo = {
    title?: string,
    completed?: boolean,
    description?: string
};
```

**Or by Using the Partial Utility Type**
```typescript
let simpleDailyTodo: Partial<Todo> = {
  title: "Go for a run"
};

 // Prints "Go for a run, undefined, undefined"
console.log(`${simpleDailyTodo.title}, ${simpleDailyTodo.completed}, ${simpleDailyTodo.description}`);
```

By using the `Partial<Type>` you don't have to change the properties of the original type. Under the hood, its implementation looks like the following:

```typescript
// for each key (property) in the Type T, make the property optional and make the Type same as the original (T[K])
type Partial<T> = {
    [K in keyof T]? : T[K]
};
```
