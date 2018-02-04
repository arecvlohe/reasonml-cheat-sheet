# ReasonML Cheat Sheet

## List: list

Lists are created with square brackets:

```reason
[]
```
## Array: array

Arrays are created with square brackets and pipes:

```reason
[||]
```

## String: string

Strings are created with double quotes:

```reason
"Yeah, to you it's Thanksgiving; to me it's Thursday"
```

Strings are concatenated with double plus signs:

```reason
"Yo Adrian, " ++ "I did it!"
```

Strings interpolation comes with help from BuckleScript:

```reason
let rocky = "Rocky"
let mickey = "Mickey"

let scene = {j|$mickey makes $rocky chase chickens to get in shape.|j}
```
