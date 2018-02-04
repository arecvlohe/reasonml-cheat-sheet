# ReasonML Cheat Sheet

## List: list

Lists are created with square brackets:

```reason
let list = []
```

## Array: array

Arrays are created with square brackets and pipes:

```reason
let array = [||]
```

## String: string

Strings are created with double quotes:

```reason
let string = "Yeah, to you it's Thanksgiving; to me it's Thursday"
```

Strings are concatenated with double plus signs:

```reason
let stringConcat = "Yo Adrian, " ++ "I did it!"
```

Strings interpolation comes with help from BuckleScript:

```reason
let rocky = "Rocky"
let mickey = "Mickey"

let scene = {j|$mickey makes $rocky chase chickens to get in shape.|j}
```
