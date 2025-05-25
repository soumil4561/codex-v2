
### Packages
Every Go program is made up of packages. Programs start running in package `main`. By convention, the package name is the same as the last element of the import path.

### Exported names
In Go, a name is exported if it begins with a capital letter. For example, `Pizza` is an exported name, as is `Pi`, which is exported from the `math` package.
When importing a package, you can refer only to its exported names. Any "non-exported" names are not accessible from outside the package.

### Functions
```go
func add(x int, y int) int {
	return x + y
}
```

*Notice that the type comes after the variable name.*

This can be shortened when two or more consecutive named function parameters share a type, you can omit the type from all but the last.

```go
func add(x, y int) int {
	return x + y
}
```

### Multiple Results
A function can return any number of results.
```go
func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```

**What `:=` Means in Go**
It declares **and** initializes variables in one shot. It does **three things at once**:

1. **Calls** the `swap()` function with `"hello"` and `"world"` as inputs.
2. **Returns two values** from that function.
3. **Creates** two new variables `a` and `b` (if they don’t already exist) and assigns the returned values to them.
### Variables
The `var` statement declares a list of variables; as in function argument lists, the type is last.
A `var` statement can be at package or function level.
`var i int`

A var declaration can include initializers, one per variable.

If an initializer is present, the type can be omitted; ***the variable will take the type of the initializer.***
`var c, python, java = true, false, "no!"`

### Short Variable Declaration
Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type.

### Go Basic Types
```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```
