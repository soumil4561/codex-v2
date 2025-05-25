---
Created: 2025-03-02
tags:
  - typescript
---
Typescript is a development tool that transpiles itself back to Javascript with baked in type safety. 

`tsc is the executor for Typescript project`

### Why typescript?
Ideally, we could have a tool that helps us find the `TypeError` bugs _before_ our code runs. That’s what a static type-checker like TypeScript does.
*Static types systems describe the shapes and behaviors of what our values will be when we run our programs.*
A type-checker like TypeScript uses that information and tells us when things might be going off the rails

### The types
#### Primitives: Number, boolean and string
- `string` represents string values like `"Hello, world"`
- `number` is for numbers like `42`. JavaScript does not have a special runtime value for integers, so there’s no equivalent to `int` or `float` - everything is simply `number`
- `boolean` is for the two values `true` and `false`

In most cases, though, defining types isn’t needed. Wherever possible, TypeScript tries to automatically _infer_ the types in your code. For example, the type of a variable is inferred based on the type of its initializer:
```ts
// No type annotation needed -- 'myName' inferred as type 'string'
let myName = "Alice";
```

#### Arrays
To specify the type of an array like `[1, 2, 3]`, you can use the syntax `number[]` You may also see this written as `Array<number>`, which means the same thing. More in [[Generics]]

#### Any
TypeScript also has a special type, `any`, that you can use whenever you don’t want a particular value to cause typechecking errors. (**Avoid with the highest priority***)

When a value is of type `any`, you can access any properties of it (which will in turn be of type `any`), call it like a function, assign it to (or from) a value of any type, or pretty much anything else that’s syntactically legal

```ts
let obj: any = { x: 0 };
// None of the following lines of code will throw compiler errors.
// Using `any` disables all further type checking, and it is assumed
// you know the environment better than TypeScript.
obj.foo();
obj();
obj.bar = 100;
obj = "hello";
const n: number = obj;
```

>`noImplicitAny` is the ts.config option for disabling the usage of any type`

#### Functions
##### Parameter type annotations
When you declare a function, you can add type annotations after each parameter to declare what types of parameters the function accepts. 

```ts
function greet(name: string) {  
	console.log("Hello, " + name.toUpperCase() + "!!");
}
```

##### Return Type Annotations
You can also add return type annotations. Return type annotations appear after the parameter list:
```ts
function getFavoriteNumber(): number {  
	return 26;
}
```

Much like variable type annotations, you usually don’t need a return type annotation because TypeScript will infer the function’s return type based on its `return` statements.

- If you want to annotate the return type of a function which returns a promise, you should use the `Promise` type as `:Promise<number>`

##### Anonymous Functions
Anonymous functions are a little bit different from function declarations. When a function appears in a place where TypeScript can determine how it’s going to be called, the parameters of that function are automatically given types.

```ts
const names = ["Alice", "Bob", "Eve"]; 
// Contextual typing for function - parameter s inferred to have type string
names.forEach(function (s) {  
	console.log(s.toUpperCase());
}); 
// Contextual typing also applies to arrow functionsnames.forEach((s) => {  console.log(s.toUpperCase());});
```

This process is called _contextual typing_ because the _context_ that the function occurred within informs what type it should have.

#### Object Types
To define an object type, we simply list its properties and their types.

```ts
// The parameter's type annotation is an object type
function printCoord(pt: { x: number; y: number }) {  
	console.log("The coordinate's x value is " + pt.x);  
	console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

You can use `,` or `;` to separate the properties, and the last separator is optional either way.

The type part of each property is also optional. If you don’t specify a type, it will be assumed to be `any`.
##### Optional Properties
Object types can also specify that some or all of their properties are _optional_. To do this, add a `?` after the property name:
```ts
function printName(obj: { first: string; last?: string }) {  
// ...
}
// Both OK
printName({ first: "Bob" });
printName({ first: "Alice", last: "Alisson" });
```

In JavaScript, if you access a property that doesn’t exist, you’ll get the value `undefined` rather than a runtime error. Because of this, when you _read_ from an optional property, you’ll have to check for `undefined` before using it.

```ts
function printName(obj: { first: string; last?: string }) {    
// Error - might crash if 'obj.last' wasn't provided!    
	console.log(obj.last.toUpperCase());  
	// 'obj.last' is possibly 'undefined'.

	if (obj.last !== undefined) {    // OK    
	  console.log(obj.last.toUpperCase());  
	}   
	
	// A safe alternative using modern JavaScript syntax:  
	console.log(obj.last?.toUpperCase());}
```

#### Type Aliases
A _type alias_ is a _name_ for any _type_. The syntax for a type alias is:

```ts
type Point = {  
	x: number;  
	y: number;
};

function printCoord(pt: Point) {  
	console.log("The coordinate's x value is " + pt.x);  
	console.log("The coordinate's y value is " + pt.y);
} 

printCoord({ x: 100, y: 100 });
```

#### Union Type
A union type is a type formed from two or more other types, representing values that may be _any one_ of those types. We refer to each of these types as the union’s _members_.

You can actually use a type alias to give a name to any type at all, not just an object type. For example, a type alias can name a union type:
```ts
type ID = number | string;
```

>[!Tip]
>The separator of the union members is allowed before the first element, so you could also write this:

```ts
function printTextOrNumberOrBool(  
	textOrNumberOrBool:    
		| string    
		| number    
		| boolean
	) {  
		console.log(textOrNumberOrBool);
	}
```

##### Narrowing
```ts
function printId(id: number | string) {  
	console.log(id.toUpperCase());
		//Property 'toUpperCase' does not exist on type 'string | number'.
		  //Property 'toUpperCase' does not exist on type 'number'.
  }
```
Here, we can see a problem with union. The solution is to _narrow_ the union with code, the same as you would in JavaScript without type annotations. _Narrowing_ occurs when TypeScript can deduce a more specific type for a value based on the structure of the code.

```ts
function printId(id: number | string) {  
	if (typeof id === "string") {    // In this branch, id is of type 'string' 
		console.log(id.toUpperCase());  
	} else {    // Here, id is of type 'number'    
	console.log(id);  
	}
}
```

##### Using union types for array definition
```ts
// this will throw error
let arr: string[] | number[] = [1,2,"3"] 

//this is how to do it
let arr: (string | number)[] = ["Hello", 1, "World", 2];
```

##### Literal Assignments in Union Types

Literal assignments are when literals are used to define types. for example:
```ts
	let pi: 3.14 = 3.14 // cant even have the value 3.145
```

This can be useful in scenarios where you want to only have certain values to be in a variable. for example:
```ts
let status: "SUCCESS" | "FAILURE"
```
Now status can only take values `SUCCESS` and `FAILURE`



### Certain terms

- `readonly` : if defined before a property in a type, it stops further manipulation post object instantiation. for eg:
```ts
type objType2 = {
  readonly _id: number;
  name: string;
};
```

- `?`: The ? tells TypeScript that the property is optional. if accessed, it may be undefined.
- **create new type by extending existing type**: 
```ts
type objType3 = objType2 & {
  age: number;
};
```

