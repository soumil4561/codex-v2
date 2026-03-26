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

### Named result parameters
- Return parameters can be named and treated as regular variables.
- Named parameters are initialized to their type's zero value at the start of the function.
- If a `return` statement has no arguments (naked return), the current values of the named parameters are returned.
- Names are optional but serve as documentation, clarifying the purpose of each returned value.
- Using named results can make code shorter and more readable.
```go
func ReadFull(r Reader, buf []byte) (n int, err error) {
    for len(buf) > 0 && err == nil {
        var nr int
        nr, err = r.Read(buf)
        n += nr
        buf = buf[nr:]
    }
    return
}
```

### Defer
Go's `defer` statement schedules a function call (the _deferred_ function) to be run immediately before the function executing the `defer` returns. It's an unusual but effective way to deal with situations such as resources that must be released regardless of which path a function takes to return. The canonical examples are unlocking a mutex or closing a file.
```go
// Contents returns the file's contents as a string.
func Contents(filename string) (string, error) {
    f, err := os.Open(filename)
    if err != nil {
        return "", err
    }
    defer f.Close()  // f.Close will run when we're finished.

    var result []byte
    buf := make([]byte, 100)
    for {
        n, err := f.Read(buf[0:])
        result = append(result, buf[0:n]...) // append is discussed later.
        if err != nil {
            if err == io.EOF {
                break
            }
            return "", err  // f will be closed if we return here.
        }
    }
    return string(result), nil // f will be closed if we return here.
}
```


## Data

### Allocation with new
- **Allocation Primitives:** Go uses two built-in functions for allocation: `new` and `make`.
- **The `new` Function:**
    - Allocates memory for a type `T` but does not initialize it.
    - Only "zeros" the memory (sets it to the type's zero value).
    - Returns the address of the allocated storage (a pointer of type `*T`).
- **Design Philosophy:** Data structures should be designed so their zero value is immediately usable without further initialization.
- **Usage:** This allows users to create an instance with `new` and begin using it immediately.
```go
type SyncedBuffer struct {
    lock    sync.Mutex
    buffer  bytes.Buffer
}
//Values of type `SyncedBuffer` are also ready to use immediately upon allocation or just declaration. In the next snippet, both `p` and `v` will work correctly without further arrangement.
p := new(SyncedBuffer)  // type *SyncedBuffer
var v SyncedBuffer      // type  SyncedBuffer
```

### Constructors and composite literals
We can simplify constructor initialisation with composite literal (Sometimes the zero value isn't good enough):
```go
f := File{fd, name, nil, 0}
```

It's perfectly OK to return the address of a local variable; the storage associated with the variable survives after the function returns. In fact, taking the address of a composite literal allocates a fresh instance each time it is evaluated
```go
//The fields of a composite literal are laid out in order and must all be present. However, by labeling the elements explicitly as field:value pairs, the initializers can appear in any order
 return &File{fd: fd, name: name} //the missing ones left as their respective zero values
```

>As a limiting case, if a composite literal contains no fields at all, it creates a zero value for the type. The expressions `new(File)` and `&File{}` are equivalent.

Composite literals can also be created for arrays, slices, and maps, with the field labels being indices or map keys as appropriate. In these examples, the initialisations work regardless of the values of `Enone`, `Eio`, and `Einval`, as long as they are distinct.
```go
a := [...]string   {Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
s := []string      {Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
m := map[int]string{Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
```

### Allocation with make
- **Scope:** `make` is used exclusively for **slices, maps, and channels**.
- **Initialization:** Unlike `new` (which only zeros memory), `make` **initializes** the internal data structure and prepares the value for immediate use.
- **Return Type:** It returns an **initialized value** of type `T`, not a pointer (`*T`).
- **Underlying Mechanism:** Slices, maps, and channels are descriptors that require internal setup (e.g., pointers to underlying arrays, length, and capacity) to function.
- **Comparison for Slices:**
    - `make([]int, 10, 100)`: Allocates a 100-int array and returns a slice structure (length 10, capacity 100) pointing to it.
    - `new([]int)`: Returns a pointer to a `nil` slice structure; this is rarely useful.
- **Pointers:** Because `make` returns a value, you must use `new` or the address-of operator (`&`) if an explicit pointer is required.

### Arrays
Arrays are useful when planning the detailed layout of memory and sometimes can help avoid allocation, but primarily they are a building block for slices. There are major differences between the ways arrays work in Go and C. In Go,

- Arrays are values. Assigning one array to another copies all the elements.
- In particular, if you pass an array to a function, it will receive a _copy_ of the array, not a pointer to it.
- The size of an array is part of its type. The types `[10]int` and `[20]int` are distinct.

The value property can be useful but also expensive; if you want C-like behavior and efficiency, you can pass a pointer to the array.

```go
func Sum(a *[3]float64) (sum float64) {
    for _, v := range *a {
        sum += v
    }
    return
}
array := [...]float64{7.0, 8.5, 9.1}
x := Sum(&array)  // Note the explicit address-of operator
```

But even this style isn't idiomatic Go. Use slices instead.

