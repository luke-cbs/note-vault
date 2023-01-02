
# Inference

### Useful Links

- [Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)
- [Literal Inference and as const](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-inference)

## Utility Types

Typescript provides several [utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html#awaitedtype) to facilitate common type transformations. These utilities are available globally.

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

This can also be used on discriminated unions when searching for object keys. Similar to the above you can use the `Exclude` to do the opposite.

## Extract the Discriminator from a Discriminated Union

To extract the discriminator you first must ensure each discriminated object has the same property otherwise it will not work:

```ts
export type Event =
  | {
      type: "click"
      event: MouseEvent
    }
  | {
      type: "focus"
      event: FocusEvent
    }
  | {
      event: KeyboardEvent
    }
    
type EventType = Event["event"]
// MouseEvent | FocusEvent | KeyboardEvent
```

## Create a union from an objects values

If you have an object and are wanting to create a union of its prop values you can use the following syntax:

```ts
export const programModeEnumMap = {
  GROUP: "group",
  ANNOUNCEMENT: "announcement",
  ONE_ON_ONE: "1on1",
  SELF_DIRECTED: "selfDirected",
  PLANNED_ONE_ON_ONE: "planned1on1",
  PLANNED_SELF_DIRECTED: "plannedSelfDirected",
} as const;

export type IndividualProgram = typeof programModeEnumMap[
  | "ONE_ON_ONE"
  | "SELF_DIRECTED"
  | "PLANNED_ONE_ON_ONE"
  | "PLANNED_SELF_DIRECTED"
]
// 1on1 | selfDirected | planned1on1 | plannedSelfDirected
```

You can also exclude using this method:

```ts
export type IndividualProgram = typeof programModeEnumMap[
	Exclude<
	  keyof typeof programModeEnumMap,
	  "GROUP" | "ANNOUNCEMENT"
	>
]
```

This would also work with other types of Utilities.

## Get All of an Objects values as a Union

```ts
export const programModeEnumMap = {
  GROUP: "group",
  ANNOUNCEMENT: "announcement",
  ONE_ON_ONE: "1on1",
  SELF_DIRECTED: "selfDirected",
  PLANNED_ONE_ON_ONE: "planned1on1",
  PLANNED_SELF_DIRECTED: "plannedSelfDirected",
} as const;

type ProgramModeEnum =
  typeof programModeEnumMap[keyof typeof programModeEnumMap]
// All of the above RHS as a union
```

## Create Unions out of Array Values

To create a union from an array you must first make it `as const` which you can then make use of utility functions or get all of them by passing `[number]` which essentially means for each index in the array to add onto the union:

```ts
const fruits = ["apple", "banana", "orange"] as const;

type AppleOrBanana = Extract<typeof fruits[number], "apple" | "banana">;
type AppleOrBananaAlt = typeof fruits[0 | 1];
type Fruit = typeof fruits[number];
```