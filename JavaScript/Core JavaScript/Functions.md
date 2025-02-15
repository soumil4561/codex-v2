### Defining:

	1. Normal way: function name (params)
	2. Function Expressions: Can be made anonymously or naming(do if u need to reference the function back again). Useful when u need to pass a function as an argument to some other function.
```javascript
const square = function (number) {
	return number * number;
};
		
const factorial = function fac(n) {
	return n < 2 ? 1 : n * fac(n - 1);
};
```

**Function Hoisting** 
JavaScript interpreter hoists the entire function declaration to the top of the current scope. Only works for declarations not function expressions. 

### Nested Functions
You may nest a function within another function. The nested (inner) function is private to its containing (outer) function. 
A closure is an expression (most commonly, a function) that can have free variables together with an environment that binds those variables
The inner function contains the scope of the outer function.

```javascript
function outside(x) {
  function inside(y) {
    return x + y;
  }
  return inside;
}

const fnInside = outside(3); // Think of it like: give me a function that adds 3 to whatever you give it
console.log(fnInside(5)); // 8
console.log(outside(3)(5)); // 8
```

**Preservation of Variables**
`x` is preserved when `inside` is returned. A closure must preserve the arguments and variables in all scopes it references. 

**Name Conflicts**
When two arguments or variables in the scopes of a closure have the same name, there is a _name conflict_. More nested scopes take precedence. So, the innermost scope takes the highest precedence, while the outermost scope takes the lowest.

```javascript
function outside() {
  const x = 5;
  function inside(x) {
    return x * 2;
  }
  return inside;
}

console.log(outside()(10)); // 20 (instead of 10)
```

### Closures
A closure is created when the inner function is somehow made available to any scope outside the outer function.
But what's the point of closures:
```javascript
const createPet = function (name) {
  let sex;

  const pet = {
    // setName(newName) is equivalent to setName: function (newName)
    // in this context
    setName(newName) {
      name = newName;
    },

    getName() {
      return name;
    },

    getSex() {
      return sex;
    },

    setSex(newSex) {
      if (
        typeof newSex === "string" &&
        (newSex.toLowerCase() === "male" || newSex.toLowerCase() === "female")
      ) {
        sex = newSex;
      }
    },
  };

  return pet;
};

const pet = createPet("Vivie");
console.log(pet.getName()); // Vivie

pet.setName("Oliver");
pet.setSex("male");
console.log(pet.getSex()); // male
console.log(pet.getName()); // Oliver
```

Sort of reminds the OOP class creation along with methods. 

**Arguments Object**
The arguments of a function are maintained in an array-like object. Using the `arguments` object, you can call a function with more arguments than it is formally declared to accept. JavaScript is flexible with its arguments. 

### Arrow Function
An arrow function expression has a smaller syntax compared to the function expression and does not have its own [`this`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this), [`arguments`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments), [`super`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super), or [`new.target`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target). Arrow functions are always anonymous.
```javascript
const a3 = a.map((s) => s.length);
```

Sort of like lambda functions.
