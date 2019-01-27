# ReasonML Cheat Sheet

ReasonML is a functional, strongly typed, programming language. It looks like JavaScript and compiles to JavaScript/Node as well as other languages. It is a dialect of OCaml and is compiled using BuckleScript. Therefore, you can use OCaml and BuckleScript APIs when using Reason.

## Try it Out

If you want a more interactive example of ReasonML's language features, check out this link to the ReasonML REPL on their homepage: 👉 [Click here](http://bit.ly/reason-by-example) 👈. 

## Comments

```reason
/* This is a comment whether single or multi-line */
```

## Types

1. Types are inferred. This means you don't have to declare types but you can if you want.
1. Type coverage is 100%. Because types are inferred it means coverage is everywhere.
1. Type system is sound. Compile and forget.

Types: `list`, `array`, `string`, `char`, `bool`, `int`, `float`, `unit`, `option`, etc.

## Generics

Generics are created by putting a `'` in front of an arbitrary variable name. However, it is most common to see single letters used.

```reason
let length: array('a) => int;
```

Generics allow you to use a function in a more _generic_ way. In the case of `length`, it's a function which takes an array of any type, `'a`, and returns an `int`. In this instance, `'a` can be a `string`, an `int`, or a `record`. What it enforces is that the `array` contains only that type. It can either be `array(int)` or `array(string)` but not both.

## Phantom Types

*"A phantom type is a parametrised type whose parameters do not all appear on the right-hand side of its definition..."* - [Haskell Wiki, PhantomType](https://wiki.haskell.org/Phantom_type)

```reason
type formData('a) = string;
```

Here `formData` is a phantom type as the `'a` parameter only appears on the left side.

### References

- [Phantom Types in ReasonML](https://sketch.sh/s/nerP3hkOxX6sMVvVUtBIbs/) by Ali Sharif
- [Phantom Types in ReasonML](https://medium.com/reasontraining/phantom-types-in-reasonml-1a4cfc18d999) by Kennet Postigo

## Abstract Types

An abstract type is a type without a concrete definition.

```reason
module type Person = {
  type details;
  
  let person: details;
  let make: unit => details;
  let greet: details => string;
} 
```

From the example above, the module declares the `details` type but leaves it to the module to define its implementation.

```reason
module Person: Person = {
  type details = { name: string, age: int };
  
  let person = { name: "Adam", age: 31};
  let make = () => person;
  let greet = (details) => "Hello. My name is " ++ details.name ++ " and I am " ++ string_of_int(details.age) ++ " years old.";
}

let adam = Person.make();
let greet = Person.greet(adam);
```

### References

- [Abstract Types - Ocaml Documentation](https://ocaml.org/learn/tutorials/modules.html#Abstract-types)

## List

Lists are created with square brackets:

```reason
let list: list(int) = [1, 2]
```

### Spread

```reason
let list1 = [1, 2, 3];
let list2 = [0, ...list1];

Js.log(list2); /* [0, 1, 2, 3] */
```

_\* Can only use the spread operator once within a list_

### Rest

```reason
let list: list(int) = [1, 2, 3, 4];

let pm = (list): string => {
  switch(list) {
    | [] => "Empty"
    | [head, ...tail] => {j|I only want $head|j}
  }
};

Js.log(pm(list)); /* I only want 1 */
```

## Array

Arrays are created with square brackets and pipes:

```reason
let arr: array(string) = [|"a", "b"|]
```

## String

Strings are created with double quotes:

```reason
let str: string = "Yeah, to you it's Thanksgiving; to me it's Thursday"
```

Strings are concatenated with double plus signs:

```reason
let stringConcat: string = "Yo Adrian, " ++ "I did it!"
```

String interpolation comes with help from BuckleScript:

```reason
let rocky: string = "Rocky"
let mickey: string = "Mickey"

let scene: string = {j|$mickey makes $rocky chase chickens to get in shape.|j}
```

## Char

Characters are created using single quotes:

```reason
let char: char = 'a';

Js.log(char); /* 97 */
```

## Functions

### Named

```reason
let someFunction = () => "someFunction";
```

### Anonymous

```reason
let someFunction = () => {
  () => {
    "someAnonFunction"
  }
};

Js.log(someFunction()()); /* "someAnonFunction" */
```

### Labeled Arguments

```reason
let labeledArgs = (~arg1, ~arg2) => {
  arg1 ++ arg2
};

Js.log(labeledArgs(~arg1="Some ", ~arg2="labeled args.")); /* "Some labeled args." */
```

### Labeled As

```reason
let labeledArgs = (~arg1 as one, ~arg2 as two) => {
  one ++ two
};

Js.log(labeledArgs(~arg1="Some ", ~arg2="labeled args.")); /* "Some labeled args." */
```

### Default and Optional Arguments

```reason
let labeledArgs = (~arg1="Some ", ~arg2=?, ()) =>
  switch (arg2) {
  | Some(arg) => arg1 ++ arg
  | None => arg1 ++ "labeled args."
  };

let res = labeledArgs(~arg2=?Some("labeled args"), ());

Js.log(res); /* Some labeled args. */
```

### Partial Application

```reason
let labeledArgs = (~arg1 as one, ~arg2 as two) => {
  one ++ two
};

let firstArg = labeledArgs(~arg1="Some ");

let secondArg = firstArg(~arg2="labeled args.");

Js.log(secondArg); /* "Some labeled args." */
```

_\* Doesn't matter which order you pass the labeled argument_

## Destructuring

### List

```reason
let [a, b]: list(int) = [1, 2];
```

### Array

```reason
let [|a, b|]: array(int) = [|1 , 2|];
```

### Tuple

```reason
let (a, b): (int, int) = (1, 2);
```

## Pattern Matching

Use the `switch` to pattern match against values:

```reason
switch (value) {
  | 1 => "one"
  | _ => "Everything else"
};
```

### Literal

```reason
switch (1) {
  | 1 => "one"
  | _ => "Everything else"
};
```

### Option

```reason
switch (option) {
  | Some(option) => option
  | None => None
};
```

### Destructuring

```reason
let arr: array(int) = [|1, 2, 3|];

let pm = (arr: array(int)): int => {
  switch (arr) {
    | [|a, b, _|] => a + b
    | [|a, b|] => a
    | [|a|] => 0
    | [||] => -1
  };
};

Js.log(pm(arr)); /* 3 */
```

### Guards

```reason
let arr = [|1, 2, 3|];

let pm = (arr: array(int)): int =>
  switch arr {
  | [|a, b, _|] when a < 2 => a + b
  | [|a, _, _|] when a > 2 => a
  | [|a|] => 0
  | [||] => (-1)
  };

Js.log(pm(arr)); /* 3 */
```

## Types

### Literal

#### Record

```reason
type person = {
  firstName: string,
  lastName: string
};

let listOfPerson: list(person) = [{ firstName: "Rocky", lastName: "Balboa" }];
```

#### Closed Object

```reason
type obj = {
  .
  color: string,
};

let car: obj = {
  pub color = "Red"
};

Js.log(car#color); /* "Red" */
```

_\* Objects in Reason are like constructors in JS_

#### Open Object

```reason
type obj('a) = {
  ..
  color: string,
} as 'a;

let car: obj({. color: string, isRed: unit => bool }) = {
  pub color = "Red";
  pub isRed = () => this#getColor() == "Red";
  pri getColor = () => this#color;
};

Js.log(car#isRed()); /* true */
```

#### Tuple

```reason
type message: (int, string) = (200, "Success");
```

### Tagged Union

```reason
type person =
  | Rocky
  | Mickey
  | Adrian;

let pm = (who: person) =>
  switch who {
  | Rocky => "Rocky"
  | Mickey => "Mickey"
  | Adrian => "Adrian"
  };

Js.log(pm(Rocky)); /* "Rocky" */
```

## Operators

| Symbol | Meaning                             |
| ------ | ----------------------------------- |
| +      | integer addition                    |
| +.     | float addition                      |
| -      | integer subtraction                 |
| -.     | float subtraction                   |
| \*     | integer multiplication              |
| \*.    | float mulitplication                |
| /      | integer division                    |
| /.     | float division                      |
| \*\*   | exponentation                       |
| \|>    | pipe / reverse application operator |
| @@     | application operator                |
| ~+     | unary addition integer              |
| ~+.    | unary addition float                |
| ~-     | unary negation integer              |
| ~-.    | unary negation float                |
| ++     | string concatenation                |
| @      | list concatenation                  |

## Helpful BuckleScript/OCaml Functions

| Function                 | Meaning                                    |
| ------------------------ | ------------------------------------------ |
| Js.log                   | Logs value to console                      |
| Js.Float.isNaN           | Checks to see if value is `NaN`            |
| Js.Boolean.to_js_boolean | Convert OCaml/Reason boolean to JS boolean |
| string_of_int            | Convert integer into string                |
| string_of_float          | Convert float to string                    |
