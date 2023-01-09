
# Inference

### Useful Links

- [Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)
- [Literal Inference and as const](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-inference)
- https://millsp.github.io/ts-toolbelt/index.html
- https://github.com/reduxjs/redux/blob/master/src/types/reducers.ts

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

## Create an Object from a Union

This allows us to create a type by combining our string literals and using the `Record` utility you can then tell it to take each key and create a property of type string: 

```ts
type TemplateLiteralKey = `${"admin" | "user" | "post" | "comment"}${
  | "Id"
  | "Name"
  | "Email"}`
  
type ObjectOfKeys = Record<TemplateLiteralKey, string>
```

Outputs: 
```ts
type ObjectOfKeys = {
    adminId: string;
    adminName: string;
    adminEmail: string;
    userId: string;
    userName: string;
    userEmail: string;
    postId: string;
    postName: string;
    postEmail: string;
    commentId: string;
    commentName: string;
    commentEmail: string;
}
```



# Template Literals

## String Patterns

With the below code what we are doing here is saying I want a string that starts with a `/` and the type being passed must be a string:

```ts
type Route = `/${string}` 

const goToRoute = (route: Route) => {}

goToRoute("/users/1")

// ts-expect-error
goToRoute("users/1")
```

We can then start to combine pattern matching with utility functions like Extract:

```ts
type Routes = '/users' | '/users/:id' | '/posts' | '/posts/:id';

type DynamicRoutes = Extract<Routes, `${string}:${string}`>;
// '/users/:id' | '/posts/:id'
```

This says match anything with a string before the `:` and any string after it as well.

## Combining Inference with String Literals and Records

In the below example we are creating a new object with inference and string literals.

First taking an object and extracting the types out of it and then appending `_alt_payment_method` onto it, this creates a union of these new strings.
```ts
const TypePurchases = {
	firstSelfHostLicensePurchase: 'first_purchase',
	renewalSelfHost: 'renewal_self',
	monthlySubscription: 'monthly_subscription',
	annualSubscription: 'annual_subscription',
} as const;

type MetadataGatherWireTransferKeys = `${ValueOf<typeof TypePurchases>}_alt_payment_method`
```

This generates the below:
```ts
// Generated union
type MetadataGatherWireTransferKeys =
  | "first_purchase_alt_payment_method"
  | "refewal_self_alt_payment_method"
  | "monthly_subscription_alt_payment_method"
  | "annual_subscription_alt_payment_method"
```

We can then create a new record with specified types from our newly created union:
```ts
type CustomerMetadataGatherWireTransfer = Partial<
  Record<MetadataGatherWireTransferKeys, string>
>
```

This will create the following:
```ts
type CustomerMetadataGatherWireTransfer = {
  first_purchase_alt_payment_method?: string | undefined
  refewal_self_alt_payment_method?: string | undefined
  monthly_subscription_alt_payment_method?: string | undefined
  annual_subscription_alt_payment_method?: string | undefined
}
```

# Type Helpers

## Using Constraints to Limit Type Parameters

If you want your generic value to be constrained to a type (which can be important when you are wanting to working with things like string template), you can use the `extends` keyword to then define the type that the generic can be a part of.

```ts
type AddRoutePrefix<TRoute extends string> = `/${TRoute}`;
```

## Function Constraints

There are cases where you may have a function and want to extract the types out of that function such as its return and parameters:

```ts
type GetParametersAndReturnType<T extends (...args: any) => any> = {}
```

Something like the above mimics utilities such as Parameters or Return Type. What we are concerned about is not so much what is passed in, the `extends` aspect is just essentially saying we want to constrain this to a function with any type of args and return and then we can infer these values later on.

## Not Null or Undefined

Typescript does constructional comparisons when checking if things are assignable to each other. You can therefore do the following:

```ts
export type Maybe<T extends {}> = T | null | undefined;
```

This will now check do constructional comparisons on anything other than null and undefined as these do not meet the same makeup of an object. Where strings and the like do. Everything else besides those are objects.

## Non Empty Array

A way to validate that there are at minimum amount of a certain type is by the following:

```ts
type NonEmptyArray<T> = [T, ...Array<T>]; 
```

What this is saying that `NonEmptyArray` expects a type `T` and that it is going to be an array. However the first index of that array we put `T` to say that it must exist. Then we use `...Array<T>` which means that any more elements after is fine they just must be of type T.

```ts
type NonEmptyArray<T> = [T, T, ...Array<T>]; 
```

Doing something like this now means we must then have a min of 2 and so on.