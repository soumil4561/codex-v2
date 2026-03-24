---
Created: 2026-03-24
tags:
  - go
---
## Naming
The visibility of a name outside a package is determined by whether its first character is upper case.
### Package Naming
- **Style:** Use **lowercase, single-word names** only.
- **Avoid:** Do not use underscores (`snake_case`) or `mixedCaps` (camelCase).
- **Brevity:** Keep names short, as users will type them frequently.
- **Collisions:** Don't obsess over unique names. If a conflict occurs, the importer can rename the package locally.
#### **Exported Names & Redundancy**
- **Contextual Naming:** Leverage the package name to avoid repetition in exported types or functions.
    - _Example:_ Use `bufio.Reader`, not `bufio.BufReader`.
- **Namespace Protection:** Exported names (like `Reader`) won't conflict with other packages because they are always prefixed by their package name (e.g., `io.Reader` vs. `bufio.Reader`).
### **Constructor Patterns**

- **The `New` Convention:** If a package exports a single primary type, name the constructor `New` rather than `NewType`.
    - _Example:_ In the `ring` package, the constructor for a `Ring` should be `ring.New`, not `ring.NewRing`.
- **Structural Logic:** Let the package hierarchy do the descriptive work for you.

### Getters
- Go does not provide automatic support for getters and setters.
- Manual getters/setters are appropriate, but the "Get" prefix is not idiomatic for getters.
- **Getters:** Use the capitalized (exported) version of the field name (e.g., field `owner` becomes method `Owner`).
- **Setters:** Use the "Set" prefix (e.g., `SetOwner`).
- **Discrimination:** Capitalization differentiates the exported method from the unexported field.
### MixedCaps
Finally, the convention in Go is to use `MixedCaps` or `mixedCaps` rather than underscores to write multiword names.

## **The Semicolon Rule**
If the last token before a newline is an identifier (which includes words like `int` and `float64`), a basic literal such as a number or string constant, or one of the tokens
```go
break continue fallthrough return ++ -- ) }
```

the lexer always inserts a semicolon after the token. This could be summarized as, “if the newline comes after a token that could end a statement, insert a semicolon”.
A semicolon can also be omitted immediately before a closing brace, so a statement such as
```go
go func() { for { dst <- <-src } }()
```
needs no semicolons. *Idiomatic Go programs have semicolons only in places such as `for` loop clauses, to separate the initializer, condition, and continuation elements.*

## Control Structs
- **Loops:**
    - No `do` or `while` loops.
    - Only a generalized `for` loop is used for all looping needs.
- **Conditional Statements:**
    - `if` and `switch` accept an optional initialization statement (similar to `for`).
    - `switch` is more flexible than its C counterpart.
- **Control Flow:** `break` and `continue` statements can take an optional label to specify which structure to exit or restart.
- **Unique Structures:**
    - **Type switch:** Used for identifying types.
    - **select:** A multi-way communications multiplexer for concurrency.
- **Syntax Rules:**
    - Parentheses `()` are not used for conditions.
    - Braces `{}` are mandatory for all statement bodies.

### If
```go
if x > 0 {
    return y
}
```

>*Mandatory braces encourage writing simple `if` statements on multiple lines. It's good style to do so anyway, especially when the body contains a control statement such as a `return` or `break`.*

Since `if` and `switch` accept an initialization statement, it's common to see one used to set up a local variable.

```go
if err := file.Chmod(0664); err != nil {
    log.Print(err)
    return err
}
```

### For
```go
// Like a C for
for init; condition; post { }

// Like a C while
for condition { }

// Like a C for(;;)
for { }
```

Short declarations make it easy to declare the index variable right in the loop.
```go
sum := 0
for i := 0; i < 10; i++ {
    sum += i
}
```

If you're looping over an array, slice, string, or map, or reading from a channel, a `range` clause can manage the loop.
```go
for key, value := range oldMap {
    newMap[key] = value
}
```

If you only need the first item in the range (the key or index), drop the second:
```go
for key := range m {
    if key.expired() {
        delete(m, key)
    }
}
```

If you only need the second item in the range (the value), use the _blank identifier_, an underscore, to discard the first:
```go
sum := 0
for _, value := range array {
    sum += value
}
```

Finally, Go has no comma operator and `++` and `--` are statements not expressions. Thus if you want to run multiple variables in a `for` you should use parallel assignment (although that precludes `++` and `--`).
```go
// Reverse a
for i, j := 0, len(a)-1; i < j; i, j = i+1, j-1 {
    a[i], a[j] = a[j], a[i]
}
```

### Switch
Go's `switch` is more general than C's. The expressions need not be constants or even integers, the cases are evaluated top to bottom until a match is found, and if the `switch` has no expression it switches on `true`. It's therefore possible—and idiomatic—to write an `if`-`else`-`if`-`else` chain as a `switch`.

a `switch` can be written **without an expression**:
```go
switch {  // --> this changes itself to switch true {
case x > 10:  
    fmt.Println("x is greater than 10")  
case x > 5:  
    fmt.Println("x is greater than 5")  
default:  
    fmt.Println("x is 5 or less")  
}
```

So Go is effectively doing this:
```go
switch true {  
case (x > 10):  
case (x > 5):  
}
```

Cases can be presented in comma-separated lists:
```go
func shouldEscape(c byte) bool {
    switch c {
    case ' ', '?', '&', '=', '#', '+', '%':
        return true
    }
    return false
}
```

### Type Switch
A switch can also be used to discover the dynamic type of an interface variable. Such a _type switch_ uses the syntax of a type assertion with the keyword `type` inside the parentheses. If the switch declares a variable in the expression, the variable will have the corresponding type in each clause. It's also idiomatic to reuse the name in such cases, in effect declaring a new variable with the same name but a different type in each case.

```go
var t interface{}
t = functionOfSomeType()
switch t := t.(type) {
default:
    fmt.Printf("unexpected type %T\n", t)     // %T prints whatever type t has
case bool:
    fmt.Printf("boolean %t\n", t)             // t has type bool
case int:
    fmt.Printf("integer %d\n", t)             // t has type int
case *bool:
    fmt.Printf("pointer to boolean %t\n", *t) // t has type *bool
case *int:
    fmt.Printf("pointer to integer %d\n", *t) // t has type *int
}
```

## Functions
- Functions and methods can return multiple values.
- Improves upon clumsy C idioms:
    - **In-band error returns:** Eliminates the need for special return values like `-1` for EOF.
    - **Argument modification:** Eliminates the need to pass addresses to modify arguments for "returning" data.