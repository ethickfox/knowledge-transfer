#### Basic Concepts
##### What is TypeScript and why use it? Differences between TypeScript and JavaScript.
   TypeScript is an open-source programming language developed and maintained by Microsoft. It is a **superset of JavaScript** that adds optional static typing and other features to the language. Any valid JavaScript code is also valid TypeScript code, but TypeScript introduces strong typing, interfaces, and compile-time checks that make code more robust and maintainable

| Feature                    | **JavaScript**                                         | **TypeScript**                                                       |
| -------------------------- | ------------------------------------------------------ | -------------------------------------------------------------------- |
| **Typing**                 | Dynamic typing. No type checking at compile time.      | Static typing. Type checking at compile time.                        |
| **Type Annotations**       | Does not support type annotations.                     | Supports type annotations (`number`, `string`, `boolean`, etc.).     |
| **Compiler**               | Interpreted directly by JavaScript engines (e.g., V8). | Needs to be compiled to JavaScript using the TypeScript compiler.    |
| **Interfaces**             | No built-in support for interfaces.                    | Supports defining interfaces to enforce the shape of an object.      |
| **Enums**                  | Does not have enums.                                   | Supports enums for defining a set of named constants.                |
| **Tooling Support**        | Limited tooling support in IDEs.                       | Strong IDE support (type-checking, auto-completion, refactoring).    |
| **ES6+ Features**          | Supports ES6+ syntax based on the environment.         | Supports all modern ES6+ features and beyond.                        |
| **Compile-Time Errors**    | No compile-time error checking.                        | Compile-time error checking helps prevent runtime bugs.              |
| **Optional Properties**    | No concept of optional properties.                     | Supports optional properties (`propertyName?: type`).                |
| **Backward Compatibility** | Dependent on the environment’s JavaScript version.     | Compiles to ES3, ES5, or ES6, ensuring broad compatibility.          |
| **OOP Features**           | Limited support for Object-Oriented Programming (OOP). | Enhanced OOP features such as access modifiers and abstract classes. |
| **Type Inference**         | No type inference.                                     | TypeScript can infer the type of a variable or return value.         |
``` TypeScript
function add(a: number, b: number): number {
  return a + b;
}
	
let myVariable: string = "hello";
myVariable = 42; // Error: Type 'number' is not assignable to type 'string'.

interface User { 
	name: string; 
	age?: number; // Optional property 
	readonly id: string; // Read-only property 
}
```

TypeScript compiles down to plain JavaScript, which means that browsers or environments (like Node.js) that only support JavaScript can still execute it. The `.ts` files are transpiled into `.js` files using the TypeScript compiler (`tsc`).]

##### Setting up a TypeScript environment.
1. `npm install --save-dev typescript`
2. Create a `tsconfig.json`
	1. Create a `tsconfig.json` file in the root of your project: `npx tsc --init`
	2. Prepare basic configuration
		``` JSON
				{
				  "compilerOptions": {
				    "target": "ES6",          // Target ECMAScript version.
				    "module": "commonjs",     // Module system for Node.js compatibility.
				    "strict": true,           // Enable all strict type-checking options.
				    "esModuleInterop": true,  // Support for importing CommonJS modules.
				    "skipLibCheck": true,     // Skip checking library files.
				    "forceConsistentCasingInFileNames": true,// Enforces consistent naming.
				    "outDir": "./dist",       // Output directory for compiled files.
				    "rootDir": "./src"        // Source directory for TypeScript files.
				  },
				  "include": ["src/**/*.ts"], // Include all ts files in the src directory.
				  "exclude": ["node_modules", "**/*.test.ts"] // Exclude modules.
				}
		```

3. Compile typescript - `npx tsc`
4. Run compiled code `node dist/index.js`
5. Add a Build Script to `package.json`
   ``` JSON
   "scripts": {
     "build": "tsc",
	  "start": "node dist/index.js"
	}
   ```
7. Optional: Setting Up TypeScript with a Bundler
	1. `npm install --save-dev webpack webpack-cli ts-loader`
	2. Create a `webpack.config.js` 
	   ``` JS
		   const path = require('path');
			
			module.exports = {
			  entry: './src/index.ts',
			  module: {
			    rules: [
			      {
			        test: /\.tsx?$/,
			        use: 'ts-loader',
			        exclude: /node_modules/,
			      },
			    ],
			  },
			  resolve: {
			    extensions: ['.tsx', '.ts', '.js'],
			  },
			  output: {
			    filename: 'bundle.js',
			    path: path.resolve(__dirname, 'dist'),
			  },
			};

		```
8. Optional: Using with react: `npm install --save-dev @types/react @types/react-dom`
   `@types` is a prefix commonly used in the TypeScript ecosystem to refer to type definition packages. These type definitions provide TypeScript with information about the types and structures of existing JavaScript libraries, enabling better type-checking, auto-completion, and IntelliSense features when using those libraries.
9. Optional: Using with node: `npm install --save-dev @types/node`
10. Optional: Integrate eslint
	1. - `npm install --save-dev eslint prettier eslint-plugin-prettier eslint-config-prettier`
	2. Configure
	   ``` JSON
		   {
			  "parser": "@typescript-eslint/parser",
			  "extends": [
			    "eslint:recommended",
			    "plugin:@typescript-eslint/recommended",
			    "plugin:prettier/recommended"
			  ],
			  "rules": {
			    "@typescript-eslint/no-unused-vars": "warn"
			  }
			}
		```

##### Type Annotations and Type Inference
1. Basic Types: `string`, `number`, `boolean`, `any`, `unknown`, `void`, etc.
	1. `number` - Represents both integer and floating-point numbers. 
	2. `string` - Used for textual data. Strings in TypeScript are defined with single quotes (`'`), double quotes (`"`), or backticks (`` ` ``).
	3. `boolean` - Represents `true` or `false` values.
	4. `Array` - Represents a list of elements. You can declare an array using two different syntaxes:
		- **Generic array type:** `Array<type>`
		- **Square bracket type:** `type[]`
	5.  Tuple - A tuple allows you to specify the exact number and types of elements in an array.
		``` Js
		let person: [string, number];
		 person = ['Alice', 25]; // Correct 
		 // Error: Type 'number' is not assignable to type 'string'
		 // person = [25, 'Alice']; 
		```
	6. `enum` - Enums allow you to define a set of named constants. They can be numeric or string-based.
		``` JS
			enum Direction {
			  Up = 1,
			  Down,
			  Left,
			  Right,
			}
			
			let move: Direction = Direction.Up; // 1
			
			enum Colors { 
				Red = 'RED', 
				Green = 'GREEN', 
				Blue = 'BLUE', 
			}
			
			let colorName: Colors = Colors.Green; // 'GREEN'
	    ```
	7.  `any` - The `any` type allows a variable to hold any type of value. It’s used when you don’t want to perform type checking or when migrating old JavaScript code to TypeScript.
	8. `unknown` is similar to `any`, but it requires you to perform a type check before using it. This makes it safer than `any`.
		```JS
		let value: unknown = 'Hello, world!';
		value = 42; // No error
		
		// Type guard required before using `unknown` as a specific type
		if (typeof value === 'string') {
		  console.log(value.toUpperCase()); // Valid because `value` is now of type `string`
		}
		```
	9. `void` - Typically used to indicate that a function does not return a value.
	10. `null` is a type with only the `null` value. Represents the intentional absence of any object value.
	11. `undefined` is a type with only the `undefined` value. Represents a variable that has been declared but has not yet been assigned a value. It can also mean that a function does not return a value.
	12. `never` - Represents the type of values that never occur. This is used for functions that never return (e.g., they throw an error or have an infinite loop).
	13. `object` - Represents non-primitive types
	14. Union types allow a variable to hold multiple types. You define them using the pipe (`|`) symbol.
	    ``` JS
	    let result: string | number; 
	    result = 'Success'; 
	    result = 42;
		```
	15. Literal types specify exact values that a variable can hold, such as specific strings, numbers, or boolean values.
	    ``` JS
	    let direction: 'up' | 'down';
		direction = 'up'; // Valid
		// Error: Type '"left"' is not assignable to type '"up" | "down"'.
		// direction = 'left';
		```
	16. Type Assertions
	    ```JS
	    let someValue: unknown = 'This is a string'; 
	    let strLength: number = (<string>someValue).length; // Using angle bracket syntax 
	    let strLength2: number = (someValue as string).length; // Using `as` syntax
		```
	17. `bigint` - `igint` is used for very large integers that exceed the safe integer range of `number`
	18. `symbol` - is a unique and immutable data type that is often used as an object property identifier.
		```JS
		const sym: symbol = Symbol('unique');
		```
##### Type Inference vs. Explicit Typing.

2. Union and Intersection Types.
3. Type Aliases and Interfaces.
4. Literal Types and Enums.
#### Master Functions and Advanced Type Features
1. Function Types and Call Signatures.
2. Optional Parameters and Default Values.
3. Overloads and Function Signatures.
4. Generics: Writing reusable and type-safe code.
5. `keyof` and `typeof` operators.
6. Conditional Types and Mapped Types.
#### Explore TypeScript Classes and Object-Oriented Programming
1. Classes and Interfaces.
2. Access Modifiers (`public`, `private`, `protected`).
3. Abstract Classes and Inheritance.
4. Static Members.
5. Decorators (advanced topic).
#### Typescript for react
React TypeScript Cheat sheet
https://react-typescript-cheatsheet.netlify.app/docs/basic/setup

#### Explore TypeScript for Backend Development
1. Setting up TypeScript with Node.js.
2. Using TypeScript with Express or NestJS.
3. Type-safe database access (e.g., with TypeORM or Prisma).

#### Advanced TypeScript Features
1. Type Guards and Type Narrowing.
2. Advanced Generics.
3. Conditional and Mapped Types.
4. Utility Types (`Partial`, `Pick`, `Omit`, etc.).
5. TypeScript with Deno (alternative runtime).
6. 
https://www.freecodecamp.org/news/typescript-for-beginners-guide/
#### Books 
1. Effective TypeScript
