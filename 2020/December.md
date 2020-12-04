# December 2nd, 2020

## Platform Routing

Ops maintains a mapping of route urls to app urls so that when a request is made to the domain `app.pluralsight.com` that request can be forwarded to the right place to handle the request.

For Example:

```json
{
  "app.pluralsight.com/priorities": "app.priorities.com"
}
```

client request -> `app.pluralsight.com/priorities` -> priorities express server -> priorities SPA. 

`#routing`

# December 4th, 2020

## F# Fundamentals

Syntax built on top of Ocaml with support for the .net platform.

### Benefits of F#

* Concise and declarative
* Reduced accidental complexity

### Functions

uses lambdas:

```
let double x = 2 * x
```

#### Function Application

```
let square x = x * x
// val square : x:int -> int

square 3
// val it : int = 9
```

F# is left associative in

```
let square x = x * x

square 3 + 1
```

will be interpreted as `(square 3) + 1` which will be reduced to `9 + 1 = 10`

#### Currying and Partial Application

currying is supported. Refers to the ability to supply only a portion of the arguments required for a function and if you do you will get a function back expecting any additionally required arguments for the function. If this is done to a function the function is known to be partially applied.

#### Prefix and Infix Functions

a standard function as we are use to it is a prefix function an infix function is where the function is called in the middle of both its arguments.

```
// prefix
add 2 3

// infix
2 add 3
```

infix functions can be called with a different syntax where they are written first similarly to a prefix function like so

```
let add2 = (+) 2
// val add2 : (int -> int)

add2 5
// val it : int = 7
```

#### Lambdas

used extensively

#### Recursion

must use special keyword to let the F# compiler know that something is recursive

```
let rec length = function
    | [] -> 0
    | x::xs -> 1 + length xs
```

#### Piping

* simplifies chains of calls making them easier and more logical to read.
* can help the compiler with type inference
* supports backwards and forwards piping aka `<|` `|>`

##### Double and Triple Piping

Used to pipe multiple numbers to a function

```
(1,2) ||> (+)
(1,2,3) |||> (+)
```

#### Function Composition

```
// Forward Composition
>> : ('a -> 'b) -> ('b -> 'c) -> 'a -> 'c

// Backward Composition
<< : ('a -> 'b) -> ('c -> 'b) -> 'c -> 'b
```

### Data

#### Primitive Types

| Type      | Literal Syntax |
|-----------|----------------|
|int        |314             |
|float      |3.14 or 1.      |
|byte       |255uy           |
|decimal    |3.14M           |
|BigInteger |99999999999999l |
|char       |'B' or '\u0042' |
|string     |"Foo"           |
|bool       |true or false   |

`#fsharp`

