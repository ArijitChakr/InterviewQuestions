# Question 1: What is TypeScript, and why would you choose it over JavaScript?

## Answer:

TypeScript is a statically typed superset of JavaScript that compiles to plain JavaScript. It introduces optional type annotations, interfaces, generics, and other features that improve code quality and maintainability.

### How It Works:

- Static Typing: Developers can declare types for variables, function parameters, and return values, which allows for early detection of errors during development.

- Compilation: TypeScript is transpiled into JavaScript, ensuring compatibility with all browsers and JavaScript environments.

- Tooling & Developer Experience: Enhanced IDE support, refactoring tools, and autocomplete features provide a more robust development workflow.

### Why It’s Important:

TypeScript helps catch errors early, improves code clarity, and scales better for large applications. It enables teams to work more effectively by enforcing contracts through types, reducing runtime errors and maintenance overhead.

# Question 2: What are the differences between interfaces and type aliases in TypeScript?

## Answer:

Interfaces and type aliases both allow you to define custom types, but they have different capabilities and use cases.

### How It Works:

- Interfaces:

  - Define the shape of an object or a contract that classes can implement.

  - Support declaration merging, meaning you can extend an interface by redeclaring it.

- Type Aliases:

  - Can create aliases for any type, including primitives, unions, tuples, and intersections.

  - Do not support declaration merging.

### Why It’s Important:

Choosing between interfaces and type aliases depends on the specific scenario: interfaces are ideal for defining contracts in object-oriented designs, while type aliases offer flexibility for complex type compositions and advanced type manipulations.

# Question 3: How do generics work in TypeScript, and why are they useful?

### Answer:

Generics provide a way to create reusable components that can work with a variety of types while still enforcing type safety.

## How It Works:

- Generic Functions and Classes:

  You can define functions, classes, or interfaces with placeholder types that are specified when the function is called or the class is instantiated.

- Example:

```
function identity<T>(value: T): T {
return value;
}

const numberValue = identity<number>(42);
const stringValue = identity<string>('Hello TypeScript');
```

- Constraints:

  You can constrain generic types using the extends keyword to ensure they have certain properties or methods.

### Why It’s Important:

Generics increase code reusability and flexibility while preserving type safety, making it easier to build robust libraries and APIs that work with multiple data types.

# Question 4: What are advanced types in TypeScript, and can you explain union and intersection types?

## Answer:

Advanced types in TypeScript allow you to combine, manipulate, and refine types for complex scenarios.

## How It Works:

- Union Types:

  Allow a variable to be one of several types using the | operator.

- Example:

```
function format(input: string | number): string {
return typeof input === 'number' ? input.toFixed(2) : input;
}
```

- Intersection Types:

  Combine multiple types into one, requiring the type to satisfy all specified types using the & operator.

- Example:

```
interface BusinessPartner {
name: string;
credit: number;
}

interface Identity {
id: number;
name: string;
}

type Employee = BusinessPartner & Identity;
const employee: Employee = { id: 1, name: 'Alice', credit: 200 };
```

### Why It’s Important:

These advanced types enable precise type definitions, reducing errors and ensuring that complex data structures behave as expected. They are critical for building robust APIs and complex systems.

# Question 5: What are decorators in TypeScript, and in what scenarios would you use them?

## Answer:

Decorators are a special kind of declaration that can be attached to classes, methods, properties, or parameters to modify their behavior at design time.

### How It Works:

- Syntax & Usage:

  - Decorators are functions prefixed with the @ symbol and are applied directly above the class or class member.

- Example for a class decorator:

```
function sealed(constructor: Function) {
Object.seal(constructor);
Object.seal(constructor.prototype);
}

@sealed
class MyClass {
// ...
}
```

- Types of Decorators:

  - Class, method, accessor, property, and parameter decorators.

### Why It’s Important:

Decorators provide a declarative way to modify or enhance classes and members without changing their underlying code. They are often used in frameworks (like Angular) for dependency injection, logging, and metadata management.

# Question 6: How does TypeScript handle module resolution, and what are some common module systems supported by TypeScript?

## Answer:

TypeScript supports multiple module systems and provides flexible module resolution strategies to integrate with various project setups.

### How It Works:

- Module Systems:

  - Supports CommonJS (used in Node.js), AMD, UMD, SystemJS, and ES Modules.

- Module Resolution:

  - TypeScript uses strategies like "node" and "classic" module resolution.

  - The "node" resolution mimics Node.js’s module lookup logic, searching in node_modules and handling package.json configurations.

  - Configuration is managed in the tsconfig.json file with options such as "module" and "moduleResolution".

### Why It’s Important:

Understanding module resolution ensures that your TypeScript project integrates seamlessly with existing libraries and build systems, reducing configuration issues and improving maintainability.

# Question 7: What are conditional types in TypeScript, and how do they work?

## Answer:

Conditional types allow you to create types that depend on conditions expressed in terms of other types. They use the syntax `T extends U ? X : Y` to choose between two types based on a condition.

### How It Works:

- Syntax Example:

```
type IsString<T> = T extends string ? 'yes' : 'no';
type A = IsString<string>;  // 'yes'
type B = IsString<number>;  // 'no'
```

- Distributive Behavior:

  When used with union types, conditional types distribute over each member automatically.

```
type ToArray<T> = T extends any ? T[] : never;
type Example = ToArray<string | number>; // string[] | number[]
```

### Why It’s Important:

Conditional types enable powerful type transformations and help create flexible, reusable type utilities that can adapt based on input types. They’re essential for building robust type-level logic in large codebases.

# Question 8: How do utility types like Partial, Pick, and Omit work in TypeScript, and why are they useful?

## Answer:

Utility types are built-in generic types provided by TypeScript that help transform existing types without writing extra code.

### How It Works:

- `Partial<T>`: Makes all properties in type T optional.

```
interface Person {
  name: string;
  age: number;
}
type PartialPerson = Partial<Person>; // { name?: string; age?: number; }
```

- `Pick<T, K>`: Creates a new type by picking a set of properties K from type T.

```
type PersonName = Pick<Person, 'name'>; // { name: string; }
```

- `Omit<T, K>`: Constructs a type by removing properties K from type T.

```
type PersonWithoutAge = Omit<Person, 'age'>; // { name: string; }
```

### Why It’s Important:

Utility types streamline type manipulation, making it easier to adapt existing interfaces for various scenarios without duplicating code. They are particularly valuable in large projects where slight variations of data shapes are needed.

# Question 9: What is type inference in TypeScript, and how does it affect coding in practice?

## Answer:

Type inference is the ability of the TypeScript compiler to automatically deduce types of expressions and variables when explicit type annotations are not provided.

### How It Works:

Example:

```
let message = "Hello, TypeScript!"; // inferred as string
const count = 42; // inferred as number
```

- Function Inference:

  - When you define a function without explicit return types, TypeScript infers the return type from the function body.

```
function add(a: number, b: number) {
return a + b; // inferred as number
}
```

### Why It’s Important:

Type inference reduces the amount of boilerplate code, enhances developer productivity, and maintains type safety. It allows you to write cleaner code while still benefiting from TypeScript’s strong type system.

# Question 10: Can you explain declaration merging in TypeScript and provide an example?

## Answer:

Declaration merging occurs when the TypeScript compiler merges two or more separate declarations with the same name into a single definition.

## How It Works:

- Interfaces:

  - Multiple interface declarations with the same name are merged automatically.

```
interface User {
name: string;
}

interface User {
age: number;
}

// Merged type:
const user: User = { name: 'Alice', age: 30 };
```

- Modules:

  - Similarly, module augmentation allows you to add properties to existing modules.

### Why It’s Important:

Declaration merging facilitates extending existing types and libraries without modifying their source code. This is especially useful for adding custom properties or methods to external modules.

# Question 11: What are type guards in TypeScript, and how do they help ensure type safety?

## Answer:

Type guards are techniques used to narrow down types within a conditional block, allowing TypeScript to infer more specific types and ensuring safer operations on variables.

### How It Works:

- Using typeof and instanceof:

```
function example(input: string | number) {
if (typeof input === 'string') {
// TypeScript knows input is a string here
console.log(input.toUpperCase());
} else {
// Here, input is a number
console.log(input.toFixed(2));
}
}
```

- Custom Type Guards:

  - Functions that return a type predicate.

```
interface Cat { meow(): void; }
interface Dog { bark(): void; }

function isCat(animal: Cat | Dog): animal is Cat {
return (animal as Cat).meow !== undefined;
}

function speak(animal: Cat | Dog) {
if (isCat(animal)) {
animal.meow();
} else {
animal.bark();
}
}
```

### Why It’s Important:

Type guards help prevent runtime errors by allowing you to safely perform operations on narrowed types. They are essential for working with union types and complex data structures.

# Question 12: How do you handle null and undefined values in TypeScript?

## Answer:

TypeScript provides strict null checks and various type operators to manage null and undefined values effectively.

### How It Works:

- Strict Null Checks:

  - Enabling the strictNullChecks option ensures that null and undefined are not assignable to other types unless explicitly specified.

- Optional Chaining (?.):

  - Allows safe access of deeply nested properties.

```
const user = { address: { city: 'New York' } };
const city = user.address?.city; // Returns 'New York' or undefined if address is null/undefined
```

- Nullish Coalescing (??):

  - Provides a default value when encountering null or undefined.

```
const name = user.name ?? 'Anonymous';
```

### Why It’s Important:

Proper handling of null and undefined increases application robustness by preventing common runtime errors and improving code clarity regarding optional values.

# Question 13: What are some best practices for organizing a large TypeScript project?

## Answer:

Organizing a large TypeScript project requires clear architecture, consistent coding standards, and effective use of TypeScript’s features.

### How It Works:

- Modular Structure:

  - Break the project into modules or services, each with clear responsibilities.

  - Consistent Naming and Folder Structure:

  - Follow naming conventions and organize files logically (e.g., separate folders for components, services, models, and utilities).

- Strict Compiler Options:

  - Enable strict mode (strict: true) to catch potential errors early.

  - Use of Interfaces and Types:

  - Define clear contracts for modules and components.

- Automated Linting and Testing:

  - Integrate tools like ESLint and Jest to enforce code quality and maintainability.

### Why It’s Important:

A well-organized project structure improves code readability, scalability, and collaboration, making it easier to manage complexity as the project grows.

# Question 14: How do you extend global types or add custom type declarations in TypeScript?

## Answer:

Extending global types and adding custom type declarations allow you to augment existing libraries or declare types for third-party modules that lack TypeScript definitions.

### How It Works:

- Declaration Files (.d.ts):

  - Create custom declaration files to define types for modules or global variables.

```
// globals.d.ts
declare global {
interface Window {
myCustomMethod: () => void;
}
}
export {};
```

- Module Augmentation:

  - Use module augmentation to extend existing modules.

```
// In a declaration file for an external module:
declare module 'express' {
export interface Request {
user?: { id: number; name: string };
}
}
```

### Why It’s Important:

This capability ensures that your codebase remains type-safe even when using untyped libraries or extending functionality, thereby improving integration and reducing runtime errors.

# Question 15: What is the difference between nominal and structural typing, and how does TypeScript approach type compatibility?

## Answer:

Nominal typing means type compatibility is based on explicit declarations (names), whereas structural typing (which TypeScript uses) means type compatibility is determined by the actual shape or structure of the types.

## How It Works:

- Structural Typing:

  - In TypeScript, if two types have the same structure, they are considered compatible—even if they have different names.

```
interface Point { x: number; y: number; }
interface Coordinate { x: number; y: number; }

const p: Point = { x: 10, y: 20 };
const c: Coordinate = p; // Valid because the structures match.
```

- Nominal Typing (Not Native to TypeScript):

  - Some languages require explicit declarations for type equivalence, which is not how TypeScript operates by default.

### Why It’s Important:

Understanding TypeScript’s structural typing model helps prevent unexpected type mismatches and improves interoperability between different parts of your code. It also highlights the flexibility and potential pitfalls when comparing types solely by their structure.
