
# Inference

## Utility Types

TypeScript provides several [utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html#awaitedtype) to facilitate common type transformations. These utilities are available globally.

These are useful for inference and extracting types.

## Keyof

If you are wanting to extract they keys of an object or a type you need to make use of the `keyof` keyword.

```ts
type TestingFrameworks = {
	a: string;
	b: string;
	c: string;
};

type TestingFramework = keyof TestingFrameworks;
// a | b | c
```

The above takes our *TestFrameworks* type which has keys `a,b,c` and then using `keyof` we then create a union of those keys.

If we wanted to get the keys of a object we first need to create a type using `typeof` and then use the `keyof`:

```ts
const testingFrameworks = {
	a: 'a';
	b: 'b';
	c: 'c';
};

type TestingFramework = keyof typeof testingFrameworks;
// a | b | c
```

# Unions and Indexing

## Types of Unions

There are 2 types of unions:

### Union

```ts
type B = "a" | "b" | "c" 
```

### Discriminated Union

```ts
type A =
  | {
      type: "a"
      a: string
    }
  | {
      type: "b"
      b: string
    }
  | {
      type: "c"
      c: string
    }
```

The difference is that in a *discriminated union* there is a common denominator. In the above case that denominator is the `type` key. 

This means when doing checks on results of the type `A`:

```ts
const getUnion = (result: A) => { } 
```

If we then had to go and do `result.type == 'a'`  it would then have some nice auto completion and type checking which when inside an if statement would allow for `result.a`.

## Extract a type from Union

https://www.typescriptlang.org/docs/handbook/utility-types.html#extracttype-union

```ts
type Fruit = "apple" | "banana" | "orange" 
type BananaAndOrange = Extract<Fruit, "banana" | "orange"> 
// banana | orange
```

This can also be used on discriminated unions when searching for object keys.