# ReasonML Cheat Sheet

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

_* Can only use the spread operator once within a list_

### Rest

```reason
let list = [1, 2, 3, 4];

let pm = (list) => {
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

Strings interpolation comes with help from BuckleScript:

```reason
let rocky: string = "Rocky"
let mickey: string = "Mickey"

let scene: string = {j|$mickey makes $rocky chase chickens to get in shape.|j}
```

## Destructuring

### List

```reason
let [a, b]: list(int) = [1, 2];
```

### Array

```reason
let [|a, b|]: list(int) = [1 , 2];
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
  | _ = "Everything else"
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
}

let listOfPerson: list(person) = [{ firstName: "Rocky", lastName: "Balboa" }];
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

| Symbol | Meaning |
|--------|---------|
| +  | integer addition |
| +. | float addition
| -  | integer subtraction |
| -. | float subtraction |
| *  | integer multiplication |
| *. | float mulitplication |
| /  | integer division |
| /. | float division |
| ** | exponentation |
| \|> | pipe / reverse application operator |
| @@ | application operator |
| ~+ | unary addition integer |
| ~+. | unary addition float |
| ~- | unary negation integer |
| ~-. | unary negation float |
| ++ | string concatenation |

## Helpful BuckleScript/OCaml Functions

| Function | Meaning |
|----------|---------|
| Js.log | Logs value to console |
| Js.Float.isNaN | Checks to see if value is `NaN` |
| Js.Boolean.to_js_boolean | Convert OCaml/Reason boolean to JS boolean |
| string_of_int | Convert integer into string |
| string_of_float | Convert float to string |
