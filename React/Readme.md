# Beginner-Level

## 1. What is JSX and why is it used?

### Answer:

JSX (JavaScript XML) is a syntax extension for JavaScript that allows you to write HTML‑like code within your JavaScript files. It’s most commonly used with React to describe the UI structure in a declarative, component‑based way.

#### How It Works:

- Syntax Extension: JSX looks like HTML but lives inside JavaScript. Under the hood, tools like Babel or the TypeScript compiler transform JSX into React.createElement calls.

- Element Creation:

```
const element = <h1>Hello, world!</h1>;
// Transpiles to:
const element = React.createElement('h1', null, 'Hello, world!');
```

- Component Composition: You can embed components and expressions directly:

```
function Greeting({ name }) {
  return <div>Hello, {name}!</div>;
}
```

- Compile‑Time Checks: During transpilation, JSX can catch typos in tag names or mismatched braces, surfacing errors before runtime.

#### Best Practices:

- Wrap Multi‑Line JSX: Use parentheses for readability:

```
return (
  <div>
    <Header />
    <Content />
  </div>
);
```

- Keep Logic Out of JSX: Move complex logic into helper functions or hooks to keep your markup clean.

- Use Meaningful Component Names: Start component names with a capital letter so React treats them as custom elements.

- Key Lists Properly: When rendering arrays, always provide a unique key prop to each item to help React’s diffing algorithm.

#### Benefits:

- Declarative UI: JSX lets you describe what the UI should look like, not how to update the DOM.

- Improved Readability: Mixing markup with logic in one place makes components more self‑contained and easier to understand.

- Tooling Support: IDEs and linters can provide autocomplete, syntax highlighting, and error checking for JSX.

- Performance Optimizations: Transpiled createElement calls enable React to build an efficient virtual DOM and batch updates.

## 2. How do you create a functional component vs. a class component?

### Answer:

React offers two primary ways to define components: functional components (as plain JavaScript functions) and class components (as ES6 classes extending React.Component). Both render UI via JSX but differ in syntax, lifecycle management, and how they handle state.

#### How It Works:

- Functional Component:

  - Definition: A JavaScript function that returns JSX.

    Syntax:

```
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
// Or with arrow function
const Greeting = ({ name }) => <h1>Hello, {name}!</h1>;
```

        State & Side Effects: Use Hooks (useState, useEffect, etc.) to manage state and lifecycle logic.

- Class Component:

  - Definition: An ES6 class that extends React.Component (or React.PureComponent) and implements a render() method.

    Syntax:

```
class Greeting extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  componentDidMount() {
    // lifecycle logic
  }

  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

        State & Lifecycle: State is held in this.state, updated via this.setState(), and lifecycle methods (componentDidMount, componentDidUpdate, etc.) manage side effects.

#### Best Practices:

- Prefer Functional Components: They’re more concise, avoid this binding, and work seamlessly with Hooks.

- Keep Components Small & Focused: Whether function or class, aim for single responsibility.

- Use Hooks for Logic Reuse: Extract stateful logic into custom Hooks rather than HOCs or render props.

- Type & Prop Validation: Leverage TypeScript interfaces or PropTypes to enforce prop shapes.

- Minimize Class Components: Only use class components in legacy codebases or when you need error boundaries (until React 16.6+ supports hooks-based error boundaries).

#### Benefits:

- Functional Components:

  - Simplicity & Readability: Less boilerplate, no constructor or this.

  - Performance: Potentially faster startup and smaller bundle sizes.

  - Hooks Ecosystem: Enables reusable stateful logic and cleaner side‑effect management.

- Class Components:

  - Lifecycle Clarity: Explicit methods for each phase (mount, update, unmount).

  - Legacy Support: Necessary in older React codebases and for features like error boundaries prior to Hooks support.

## 3. What are props in React? How do you pass data from parent to child?

### Answer:

Props (short for “properties”) are the mechanism by which React components receive external data and configuration. They form a unidirectional data flow from parent to child, making components reusable and predictable.

#### How It Works:

- Definition & Immutability:

  - Props are plain JavaScript objects passed to a component.

  - Inside the child, props are read‑only; you should never modify them.

- Passing Data:

  - JSX Attributes: In the parent’s render/return, you supply props as attributes on the child component:

    ```
    <UserCard name="Alice" age={30} onClick={handleClick} />
    ```

  - Accessing in Functional Components: The child function receives a props object (often destructured):
    ```
        function UserCard({ name, age, onClick }) {
            return <div onClick={onClick}>{name} is {age} years old.</div>;
        }
    ```
  - Accessing in Class Components: Available on this.props:
    ```
    class UserCard extends React.Component {
      render() {
        const { name, age, onClick } = this.props;
        return <div onClick={onClick}>{name} is {age} years old.</div>;
      }
    }
    ```

- Unidirectional Flow:

  - Data flows down: parents → children.

  - Children communicate back via callback props (e.g., onClick, onChange).

#### Best Practices:

- Destructure Props: Improves readability and avoids repeated props. prefixes.

- Prop Validation: Use TypeScript interfaces or PropTypes to document expected prop shapes and catch errors early.

- Default Values: Provide defaultProps or default parameter values to handle missing props gracefully.

- Avoid Prop Drilling: If you find yourself passing the same prop through many layers, consider Context or state management libraries.

- Keep Props Minimal: Pass only what the child needs—don’t overload with unnecessary data.

#### Benefits:

- Reusability: Components become configurable and reusable across different contexts.

- Predictability: Immutable inputs and unidirectional flow simplify reasoning about UI state.

- Separation of Concerns: Parents handle data/state, while children focus on presentation and behavior.

- Testability: With clear inputs (props), components are easier to unit‑test by supplying mock props.

## 4. What is state in React? How do you initialize and update it?

### Answer:

State is a built‑in React object used to hold data that may change over the lifetime of a component. When state updates, React re‑renders the component to reflect the new data.

#### How It Works:

- Functional Components (Hooks):

  - Initialization: Use the useState hook to declare state variables.

    ```
    import { useState } from 'react';

    function Counter() {
      const [count, setCount] = useState(0); // count is state, 0 is initial value
      // ...
    }
    ```

  - Updating: Call the updater function (setCount) with a new value or a function receiving the previous state:

    ```
    // Simple update
    setCount(count + 1);

    // Functional update (when new state depends on previous)
    setCount(prev => prev + 1);
    ```

- Class Components:

  - Initialization: Define this.state in the constructor.
    ```
    class Counter extends React.Component {
      constructor(props) {
        super(props);
        this.state = { count: 0 };
        }
        // ...
    }
    ```
  - Updating: Use this.setState() to merge new state.

    ```
    this.setState({ count: this.state.count + 1 });

    // Or functional form
    this.setState(prevState => ({ count: prevState.count + 1 }));
    ```

#### Re‑Rendering:

Any call to setState (class) or the updater from useState (functional) schedules a re‑render of that component and its children.

#### Best Practices:

- Keep State Minimal: Only store what’s necessary; derive other values on render.

- Avoid Direct Mutation: Never modify state objects/arrays directly—always replace with new copies.

- Use Functional Updates: When new state depends on previous, pass a function to the updater to avoid stale closures.

- Co-locate State: Keep state in the closest common ancestor of components that need it; lift state up sparingly.

- Split State: For complex objects, consider multiple useState calls or useReducer for clearer updates.

#### Benefits:

- Reactivity: State-driven updates ensure UI stays in sync with data.

- Encapsulation: Each component manages its own state, promoting modular design.

- Predictability: State transitions via explicit updater functions make data flow easier to trace and debug.

- Flexibility: Hooks like useState and useReducer enable powerful patterns for managing both simple and complex state.

## 5. Explain the component lifecycle methods in class components (e.g., componentDidMount, componentDidUpdate, componentWillUnmount).

### Answer:

In React class components, lifecycle methods are special functions that are invoked at different stages of a component’s existence—mounting, updating, and unmounting. They allow you to hook into these stages to perform side effects like data fetching, DOM manipulation, cleanup, etc.

#### How It Works:

1. Mounting Phase (when the component is added to the DOM):

   - constructor()

     Initializes state and binds methods.

     Avoid side effects here.

   - render()

     Returns JSX and is called on every render.

   - componentDidMount()

     Called once after the component is rendered and mounted.

     - Used for:
       Fetching data, Subscribing to events,DOM operations

     ```
     componentDidMount() {
       fetchData().then(data => this.setState({ data }));
     }
     ```

2. Updating Phase (when props or state change):

- shouldComponentUpdate(nextProps, nextState) (optional)

- Returns true or false to control re-rendering.

- Useful for performance optimization.

  - componentDidUpdate(prevProps, prevState)

    Called after re-render due to state/prop change.

- Used for:

  - Triggering side effects on update

  - Comparing previous and current props/state
    ```
    componentDidUpdate(prevProps) {
      if (prevProps.id !== this.props.id) {
        this.fetchNewData();
      }
    }
    ```

3. Unmounting Phase (when the component is removed from the DOM):

   - componentWillUnmount()

     Cleanup before the component is destroyed.

   - Used for:
     Clearing timers, Unsubscribing from events or listeners, Canceling API calls
     ```
     componentWillUnmount() {
       clearInterval(this.intervalId);
     }
     ```

#### Best Practices:

- Avoid Side Effects in render(): Keep it pure—no API calls, timers, or DOM manipulations.

- Always Clean Up in componentWillUnmount(): Prevent memory leaks and side effects.

- Use shouldComponentUpdate for Optimization: Only when performance becomes an issue—prefer React.PureComponent if comparing shallow props/state is enough.

- Compare Props in componentDidUpdate: Prevent infinite loops by checking if relevant data has changed before triggering updates.

#### Benefits:

- Granular Control: Lifecycle methods give you explicit hooks for managing side effects, updates, and cleanup.

- Predictability: Knowing the exact sequence of lifecycle methods makes debugging easier.

- Powerful Patterns: Enable advanced behaviors like memoization, subscriptions, and deferred rendering.

## 6. What’s the difference between a controlled and an uncontrolled component?

### Answer:

Controlled and uncontrolled components differ in how form input elements are handled in React. Controlled components are React-driven—state is maintained by React, whereas uncontrolled components rely on the DOM to manage their own state.

#### How It Works:

1. Controlled Components:

   - Definition: Input elements where the value is controlled by React state.

   - React is the source of truth for the input value.

   - Syntax Example:

     ```
     function Form() {
       const [name, setName] = useState("");

       return (
         <input
           type="text"
           value={name}
           onChange={(e) => setName(e.target.value)}
         />
       );
     }
     ```

   - Flow:

     User types → onChange updates state → state updates value prop → input updates

2. Uncontrolled Components:

   - Definition: Input elements that manage their own state internally in the DOM.

   - Accessed via ref to get current values when needed.

   - Syntax Example:

     ```
     function Form() {
       const inputRef = useRef(null);

       function handleSubmit() {
         console.log(inputRef.current.value);
       }

       return <input type="text" ref={inputRef} />;
     }
     ```

   - Flow:

     User types → input value changes (DOM only) → read value via ref when needed

#### Best Practices:

- Prefer Controlled Components when:

  - You need to validate, manipulate, or reset input dynamically.

  - You're building complex forms (e.g., using Formik or React Hook Form).

- Use Uncontrolled Components for:

  - Simple forms where you don’t need real-time updates.

  - Integrating with non-React code or libraries.

#### Benefits:

- Controlled:
  Predictable and testable form logic

  Easier validation and error handling

  Enables features like disabling submit until all fields are valid

- Uncontrolled:
  Less boilerplate

  Slightly better performance in very large forms

  Closer to traditional DOM behavior

## 7. How do you handle events in React (e.g., onClick, onChange)?

### Answer:

In React, handling events is similar to handling DOM events in vanilla JavaScript, but with a few key differences: events are named in camelCase, and you pass a function reference, not a string. React wraps native events in a cross-browser wrapper called SyntheticEvent for consistency.

#### How It Works:

1. Event Handling Syntax:

   - Functional Component:

     ```
     function Button() {
       const handleClick = () => {
         console.log("Button clicked!");
       };

       return <button onClick={handleClick}>Click Me</button>;
     }
     ```

   - Class Component:

     ```
     class Button extends React.Component {
       handleClick = () => {
         console.log("Button clicked!");
       };

       render() {
         return <button onClick={this.handleClick}>Click Me</button>;
       }
     }
     ```

2. Common Event Types:
   | Event Type | Usage Example |
   | ---------- | ------------- |
   | onClick | `<button onClick={handleClick} />` |
   | onChange | `<input onChange={handleChange} />` |
   | onSubmit | `<form onSubmit={handleSubmit} />` |
   | onMouseEnter | `<div onMouseEnter={handleHover} />` |
   | onKeyDown | `<input onKeyDown={handleKeyPress} />` |
3. Event Object (SyntheticEvent):

   - React normalizes the browser event into a SyntheticEvent to ensure consistency across browsers.

   - You can access event properties like event.target, event.preventDefault(), and event.stopPropagation().
     ```
     function handleChange(e) {
     console.log(e.target.value); // current input value
     }
     ```

#### Best Practices:

- Use arrow functions or bind handlers: In class components, use arrow functions or bind in the constructor to maintain this context.

- Avoid inline functions for performance-critical code: Inline functions can cause unnecessary re-renders in deeply nested or high-frequency components.

- Prevent default behavior where necessary: Use e.preventDefault() in forms to stop default submission.

#### Benefits:

- Declarative and clean: Event handlers are easy to read and colocated with JSX.

- Cross-browser consistency: SyntheticEvent ensures predictable behavior.

- Tight integration with state: Easily update React state inside event handlers, enabling reactive UI updates.

## 8. How do you conditionally render elements in React?

### Answer:

In React, conditional rendering means dynamically showing or hiding elements based on some condition, such as state, props, or logic. It's a fundamental pattern for building interactive UIs and is done using standard JavaScript control flow inside JSX.

#### How It Works:

React uses JavaScript expressions to conditionally render content. You can use:

1. Ternary Operator: `{isLoggedIn ? <LogoutButton /> : <LoginButton />}`

2. Logical AND (&&): `{isAdmin && <AdminPanel />}` Only renders `<AdminPanel />` if isAdmin is truthy.

3. if/else Statements (outside JSX):
   ```
   function Greeting({ isLoggedIn }) {
     if (isLoggedIn) {
       return <h1>Welcome back!</h1>;
     } else {
       return <h1>Please sign in.</h1>;
     }
   }
   ```
4. Conditional Rendering with Functions or Components:
   ```
   function renderGreeting(user) {
     return user ? <h1>Hello, {user.name}!</h1> : <h1>Welcome, Guest!</h1>;
   }
   ```

#### Best Practices:

- Keep logic clean: Avoid deeply nested ternaries; extract into helper functions or components when logic gets complex.

- Use fragments (<> </>) to group multiple conditionally-rendered elements without adding extra DOM nodes.

- Return null to render nothing: `if (!shouldShow) return null;`

#### Benefits:

- Dynamic UI: Lets your app respond to user actions, permissions, or async data.

- Reusability: Encourages splitting logic into smaller, condition-aware components.

- Declarative: Keeps your UI logic clean and readable within JSX.

#### Summary:

Conditional rendering in React uses standard JavaScript expressions like ternaries, &&, or if/else inside or outside JSX. It enables dynamic interfaces that reflect user state, permissions, or data conditions—essential for building interactive applications.

## 9. What is the key prop, and why is it important when rendering lists?

### Answer:

The key prop is a special attribute used by React when rendering lists of elements. It helps React identify which items have changed, been added, or removed, improving rendering performance and ensuring UI consistency.

#### How It Works:

- When rendering lists with .map(), you provide a unique key to each element:

```
const items = ['apple', 'banana', 'cherry'];

return (
  <ul>
    {items.map((item, index) => (
      <li key={item}>{item}</li>
    ))}
  </ul>
);
```

#### Why It Matters:

- React uses the key to track each item across re-renders.

- Without a unique key, React may:

- Reuse DOM elements incorrectly

- Cause visual glitches or unexpected behavior (especially with inputs)

- Helps React optimize rendering by minimizing DOM changes.

#### Best Practices:

- Use unique, stable identifiers (e.g., database IDs or UUIDs).
  `items.map(item => <li key={item.id}>{item.name}</li>);`
- Avoid using array index as key, unless:

  - The list is static (doesn’t change)

  - No unique ID is available

  - `items.map((item, index) => <li key={index}>{item}</li>);`

- Don’t use random values or Math.random()—keys must stay consistent across renders.

#### Benefits:

- Performance: Enables efficient diffing and minimal DOM manipulation.

- Predictability: Prevents UI bugs during reordering or stateful list rendering.

- Cleaner code: Enforces clarity when mapping over arrays.

#### Summary:

The key prop tells React how to uniquely identify items in a list. It’s critical for list rendering performance and avoiding UI issues. Always use stable, unique values as keys—not array indexes, unless absolutely necessary.

## 10. How do you lift state up, and why would you do it?

### Answer:

Lifting state up is the pattern of moving shared state to the closest common ancestor of components that need to read or update it. This creates a single source of truth, ensuring consistency across related components.

#### How It Works:

- Identify Shared State:

  - Determine which pieces of data are needed by multiple sibling or cousin components.

  - Move State to Parent:

    Declare the state in the nearest common ancestor using useState (functional) or this.state (class).

  - Example (functional):
    ```
    function Parent() {
      const [value, setValue] = useState('');
      return (
        <>
          <ChildA value={value} onChange={setValue} />
          <ChildB value={value} />
        </>
      );
    }
    ```

- Pass via Props & Callbacks:

  - Downward: Pass the state value as a prop to children that need to read it (ChildB).

  - Upward: Pass the state setter or a callback to children that need to update it (ChildA), which calls onChange(newValue) to notify the parent.

#### Best Practices:

- Minimal State: Only lift the state that truly needs to be shared—keep other state local.

- Clear Prop Contracts: Name props clearly (value / onChange) to indicate controlled inputs.

- Use Callbacks: When updating based on previous state, use functional updates (setValue(prev => …)) to avoid stale values.

- Consider Context: If you find yourself drilling props through many levels, use React Context or a state management library instead of excessive lifting.

#### Benefits:

- Single Source of Truth: Ensures all components derive from the same state, preventing UI inconsistencies.

- Predictable Data Flow: With unidirectional (parent→child) flow and callbacks for updates, it’s easier to trace how data changes.

- Reusability & Testability: Stateless children become pure and easier to test since they rely solely on props.

- Simplified Synchronization: Shared inputs, forms, or controls stay in sync without duplicated logic.

# Intermediate-Level

## 1. What are React Hooks? Name the most common built‑in hooks and their use cases.

### Answer:

React Hooks are special functions introduced in React 16.8 that let you “hook into” React state and lifecycle features from functional components. They replace many use cases for class components, enabling you to write cleaner, more reusable logic.

#### How It Works:

- useState:

  - Declares state in a functional component.

  - Returns a state value and a setter function: `const [count, setCount] = useState(0);`

- useEffect:

  - Runs side effects (data fetching, subscriptions, DOM mutations) after render.

  - You can control when it runs via its dependency array:
    ```
    useEffect(() => {
      fetchData();
    }, [query]);
    ```

- useContext:

  - Consumes a React Context value without a Consumer component.
  - `const theme = useContext(ThemeContext);`

- useReducer:

  - An alternative to useState for complex state logic or multiple sub-values.

  - `const [state, dispatch] = useReducer(reducer, initialState);`

- useMemo:

  - Memoizes expensive computations, re‑computing only when dependencies change.

  - `const sorted = useMemo(() => sortList(list), [list]);`

- useCallback:

  - Memoizes callback functions to prevent unnecessary re‑creations (useful for props equality).

  - `const handleClick = useCallback(() => { /* ... */ }, [deps]);`

- useRef:

  - Provides a mutable ref object for storing DOM refs or any mutable value that persists across renders.

  - `const inputRef = useRef();`

- useLayoutEffect:

  - Like useEffect, but fires synchronously after DOM mutations, useful for measuring layouts.

#### Best Practices:

- Follow the Rules of Hooks:

  - Only call Hooks at the top level and from React function components or custom Hooks.

  - Specify Dependencies Accurately:

  - Always list every value used inside useEffect, useMemo, and useCallback in their dependency arrays to avoid stale closures.

- Extract Custom Hooks:

  - Encapsulate reusable stateful logic into custom Hooks (e.g., useForm, useFetch) to keep components clean.

- Avoid Over‑Optimization:

  - Only use useMemo/useCallback when there’s a measurable performance benefit; they add cognitive overhead otherwise.

#### Benefits:

- Simplified Components:

  Eliminate boilerplate from class components (constructor, this, lifecycle methods).

- Reusability:

  Share stateful logic across components via custom Hooks without changing component hierarchy.

- Better Readability:

  Group related logic (state, effects) together, making code more intuitive.

- Enhanced Testing:

  Functional components with Hooks are easier to unit‑test since they’re plain functions.

## 2. How does the useEffect hook work? How do you control when it runs?

### Answer:

The useEffect hook lets you perform side effects—such as data fetching, subscriptions, or manually changing the DOM—from within functional components. It combines the behaviors of several class‑based lifecycle methods into a single API.

#### How It Works:

- Execution Timing:

  - After every completed render, React runs the effect’s callback, then applies any DOM updates.

  - If your effect returns a cleanup function, React runs it before the next effect or when the component unmounts.

- Dependency Array:

  - No array: `useEffect(() => { … });`

  - Runs after every render (mount + every update).

  - Empty array: `useEffect(() => { … }, []);`

  - Runs once after the initial mount (like componentDidMount).

  - With dependencies: `useEffect(() => { … }, [depA, depB]);`

  - Runs after mount and whenever any dependency (depA or depB) changes (like componentDidUpdate for those props/state).

- Cleanup Function:
  ```
  useEffect(() => {
    const id = setInterval(tick, 1000);
    return () => clearInterval(id); // runs before unmount or next effect
  }, []);
  ```

#### Best Practices:

- Declare All Dependencies: Always list every variable used inside the effect in its dependency array to avoid stale closures.

- Split Effects by Concern: Use multiple useEffect calls for unrelated side effects (e.g., one for data fetching, another for subscriptions).

- Cleanup Subscriptions: Return a cleanup function to unsubscribe or cancel timers to prevent memory leaks.

- Avoid Heavy Work in Effects: For CPU‑intensive tasks, consider useMemo/useCallback or offloading to Web Workers.

- Guard Against Unnecessary Runs: When calling external APIs, check that inputs have actually changed before re‑fetching.

#### Benefits:

- Unified Lifecycle: Consolidates mount, update, and unmount logic into a single, declarative API.

- Readability: Co‑locates side‑effect code with related state logic, improving component clarity.

- Cleanup Control: Built‑in cleanup mechanism ensures resources (timers, subscriptions) are released properly.

- Functional Components: Enables full-featured components without needing class syntax or lifecycle methods.

## 3. What’s the difference between useMemo and useCallback?

### Answer:

Both useMemo and useCallback are React performance hooks that help you avoid unnecessary work by memoizing values or functions. However, they serve distinct purposes:

#### How It Works:

- useMemo

  - Purpose: Memoizes the result of a computation.

  - Signature: `const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);`

  - Behavior: React calls the factory function on mount and whenever any dependency (a or b) changes. Otherwise, it returns the previously cached value.

- useCallback

  - Purpose: Memoizes the function itself, returning a stable function reference.

  - Signature:

    ```
    const memoizedCallback = useCallback(() => {
      doSomething(a, b);
    }, [a, b]);
    ```

  - Behavior: React recreates the callback only when its dependencies change. Otherwise, it returns the same function instance.

#### Best Practices:

- Use Only When Necessary:

  Apply these hooks when you have expensive computations (useMemo) or need to prevent child re‑renders due to changing function props (useCallback).

- Correct Dependencies:

  Always list all variables used inside the factory or callback in the dependency array to avoid stale values or infinite loops.

- Avoid Over‑Optimization:

  Memoization itself has a cost; don’t wrap every value or function in these hooks unless you’ve measured a performance issue.

- Choose Appropriately:

  useMemo for caching computed values.

  useCallback for caching event handlers or functions passed to child components or other hooks.

#### Benefits:

- Performance Gains:

  - useMemo skips expensive recalculations when inputs haven’t changed.

  - useCallback avoids recreating functions on every render, which can prevent unnecessary renders of memoized child components.

- Stable References:

  - Ensures that objects or functions passed as props remain the same across renders, enabling deeper optimizations in React.memo or dependency‑based hooks.

- Cleaner Code:

  - Encapsulates optimization logic in a declarative way, keeping render logic focused on JSX structure rather than manual caching.

## 4. Explain the Context API and when you’d use it instead of prop drilling.

### Answer:

The React Context API provides a way to share values (such as state, themes, or user info) globally across the component tree without having to pass props manually through every level—commonly known as prop drilling.

#### How It Works:

- Create a Context: `const ThemeContext = React.createContext();`

- Provide a Value: Wrap part of your component tree with the Provider to supply a value:
  ```
  <ThemeContext.Provider value="dark">
    <App />
  </ThemeContext.Provider>
  ```
- Consume the Context:

  Inside a component, use the useContext hook (or Context.Consumer in class components):

  ```
  const theme = useContext(ThemeContext);
  ```

#### When to Use Context API:

- ✅ Use it when:

  - You have global or app-wide data: e.g., authentication status, theme, locale, or user preferences.

  - You need to avoid prop drilling through many layers of components.

  - State or behavior needs to be shared across many unrelated components.

- ❌ Avoid it when:

  - The data is only used by a few components in a local scope.

  - You’re trying to manage complex state logic (consider using a state manager like Redux, Zustand, or Jotai for that).

#### Best Practices:

- Split contexts by concern to avoid unnecessary re-renders.

- Use memoized values with useMemo when passing objects/functions in value to prevent context consumers from re-rendering unnecessarily.

- Combine with custom hooks for cleaner access patterns:

  ```
  const useTheme = () => useContext(ThemeContext);
  ```

#### Benefits:

- Avoids prop drilling, leading to cleaner and more maintainable code.

- Reactive: Components consuming the context re-render when the context value changes.

- Composable: Easily combine with other hooks and state management techniques.

- Lightweight: Native to React—no external libraries needed.

## 5. What are Higher‑Order Components (HOCs)? How do they differ from render props?

### Answer:

A Higher-Order Component (HOC) is a design pattern in React for reusing component logic. It’s a function that takes a component and returns a new component, enhancing it with additional capabilities like state, props, or side effects.

#### How It Works:

```
const withLogger = (WrappedComponent) => {
  return function EnhancedComponent(props) {
    useEffect(() => {
      console.log('Component mounted:', WrappedComponent.name);
    }, []);
    return <WrappedComponent {...props} />;
  };
};
```

- withLogger(MyComponent) returns a new component that wraps MyComponent.

- It can inject props, add side effects, or modify behavior.

- HOCs are pure functions and should not modify the original component.

- Common Use Cases:

  - Access control (e.g., withAuth)

  - Logging and analytics

  - Code reuse (e.g., form handling, pagination logic)

  - Connecting to Redux (connect() is an HOC)

- How HOCs Differ from Render Props:
  | Concept | Higher-Order Component (HOC) | Render Props |
  | --- | --- | --- |
  | Structure | Function that returns a new component | Component that uses a function as a child |
  | Logic Sharing | Wraps a component | Passes logic to a child function |
  | Example | `withAuth(Component)` | `<Auth>{(user) => <Dashboard user={user} />}</Auth>` |
  | Code Readability | Cleaner for deeply shared logic | More verbose and nesting-heavy |
  | Performance | May re-render less frequently | May cause more frequent re-renders if not memoized properly |
  | Composability | Easily composed using functions | Composed using component nesting

#### Best Practices:

- Name HOCs with the with prefix (withLogger, withAuth) for clarity.

- Don’t mutate the original component.

- Copy over static methods using hoist-non-react-statics.

- Avoid excessive nesting of HOCs—it can reduce readability.

#### Benefits:

- Logic Reuse: Share cross-cutting concerns across components.

- Composable: Multiple HOCs can be chained or composed together.

- Encapsulation: Keeps shared logic separated from UI rendering logic.

- Type Safety (with TypeScript): Can be strongly typed for safer prop injection.

HOCs are a powerful way to abstract and reuse logic across React components by wrapping and enhancing them. They differ from render props in that they return an enhanced component rather than rendering via a function, making them more concise and composable in many cases.

## 6. How do you handle error boundaries in React?

### Answer:

An error boundary in React is a special component that catches JavaScript errors in the component tree below it during rendering, lifecycle methods, and constructors, and displays a fallback UI instead of crashing the whole app.

#### How It Works:

- Error boundaries must be class components that implement one or both of the following lifecycle methods:

  - `static getDerivedStateFromError(error)`

  - `componentDidCatch(error, errorInfo)`

- Basic Example:

  ```
  class ErrorBoundary extends React.Component {
  constructor(props) {
  super(props);
  this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
  return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
  console.error("Error caught by boundary:", error, errorInfo);
  }

  render() {
  if (this.state.hasError) {
  return <h1>Something went wrong.</h1>;
  }
  return this.props.children;
  }
  }
  ```

- Usage:
  ```
  <ErrorBoundary>
    <MyComponent />
  </ErrorBoundary>
  ```
- What Error Boundaries Catch (and Don’t Catch):

  - They catch: Render errors, Lifecycle method errors, Constructor errors of child components

  - They don't catch: Event handler errors (handle manually in a try/catch block), Asynchronous code (e.g., inside setTimeout or fetch), Server-side rendering errors

#### Best Practices:

- Wrap major UI sections individually to isolate errors (e.g., sidebar, feed, dashboard).

- Provide a custom fallback UI with guidance or recovery options.

- Log errors to a monitoring service in componentDidCatch.

- Combine with React’s ErrorBoundary component in future versions once functional component support is added.

#### Benefits:

- Improved stability: Prevents a single crashing component from bringing down the entire app.

- User-friendly fallback: Shows a graceful UI when something goes wrong.

- Debuggable: Captures detailed error info and stack traces for developers.

- Resilient production app: Greatly improves robustness for real-world usage.

Error boundaries are essential for robust React apps. They allow you to catch and handle runtime errors in the component tree gracefully—displaying fallback UIs and logging issues—without breaking the entire UI. They're implemented using class components, but upcoming React updates may support hooks-based boundaries in the future.

## 7. What is React Router, and how do you implement nested routes?

### Answer:

React Router is a standard routing library for React applications that enables client-side navigation. It allows you to define and manage routes, switch views without full-page reloads, and build single-page applications (SPAs) with multiple views.

React Router is essential for building multi-page experiences within single-page React apps. It supports nested routing via `<Route>` components and `<Outlet>`, allowing developers to organize and render views hierarchically for better modularity and user experience.

#### How It Works:

- React Router uses the HTML5 History API to manipulate the browser’s URL and render components based on the route.

- It provides declarative components like `<Routes>`, `<Route>`, `<Link>`, `<Navigate>`, and `useNavigate()` to manage navigation and URL-based rendering.

- Basic Setup:

  ```
  import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

  <Router>
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
    </Routes>
  </Router>
  ```

- Nested Routes (How To Implement):

  - Nested routes allow rendering child components inside a parent route. Useful for layouts, dashboards, or sections within a page.

    1. Define Parent and Child Routes:
       ```
       <Routes>
       <Route path="/dashboard" element={<Dashboard />}>
       <Route path="analytics" element={<Analytics />} />
       <Route path="settings" element={<Settings />} />
       </Route>
       </Routes>
       ```
    2. Render `<Outlet />` in the Parent Component:

       ```
        import { Outlet } from 'react-router-dom';

        function Dashboard() {
          return (
                    <div>
                        <h2>Dashboard</h2>
                        <Outlet /> {/_ Renders nested route component _/}
                    </div>
                );
        }
       ```

- When the route is /dashboard/analytics, it renders Dashboard → Analytics.

#### Best Practices:

- Use `<Outlet />` to render child routes inside a layout or wrapper component.

- Use `useParams()` to access dynamic route parameters.

- Group routes in separate files for large apps (e.g., authRoutes, adminRoutes).

- Use `<Navigate />` for redirects, and protect routes using wrappers like PrivateRoute.

#### Benefits:

- Client-side navigation without page reloads.

- Nested and dynamic routing support (e.g., /user/:id).

- Route protection and lazy loading support.

- Clear URL-driven UI logic with deep linking.

## 8. How would you implement code‑splitting and lazy loading in a React app?

### Answer:

Code-splitting and lazy loading are performance optimization techniques in React that allow you to load only the code needed for a specific part of your app, rather than the entire bundle at once. This improves initial load time, reduces bundle size, and enhances user experience—especially for large applications.

#### How It Works:

- React supports code-splitting out of the box using:

- `React.lazy()` – for dynamic import of components

- Suspense – to render a fallback UI while the component is loading

#### Basic Implementation:

- Step 1: Use React.lazy() to Import a Component

  ```
  import React, { Suspense } from 'react';

  const LazyDashboard = React.lazy(() => import('./Dashboard'));
  ```

- Step 2: Wrap It in <Suspense> with a Fallback UI
  ```
  <Suspense fallback={<div>Loading...</div>}>
    <LazyDashboard />
  </Suspense>
  ```
- Example: Lazy Load Routes with React Router

  ```
  import { BrowserRouter, Routes, Route } from 'react-router-dom';

  const Home = React.lazy(() => import('./pages/Home'));
  const Profile = React.lazy(() => import('./pages/Profile'));

  function App() {
    return (
      <BrowserRouter>
        <Suspense fallback={<div>Loading...</div>}>
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/profile" element={<Profile />} />
          </Routes>
        </Suspense>
      </BrowserRouter>
    );
  }
  ```

#### Best Practices:

- Group by route or feature to split the code in a meaningful way.

- Always provide a fallback UI with <Suspense> to improve user experience.

- Lazy load third-party libraries or charts that are not critical on first load.

- Combine with dynamic imports and bundlers like Webpack or Vite for optimal results.

#### Advanced Enhancements:

- Use React.SuspenseList (experimental) for coordinating multiple lazy-loaded components.

- Combine with preloading strategies using import(/_ webpackPrefetch: true _/ './Component') to fetch in the background.

- Use libraries like @loadable/component for SSR-friendly lazy loading.

#### Benefits:

- Faster initial load by splitting your app into smaller chunks.

- Reduced bundle size, especially for larger apps.

- On-demand loading ensures users download only what they need.

- Improves perceived performance by displaying loading states gracefully.

## 9. Describe how you would manage global state (e.g., Redux, MobX, Zustand, React Context + Reducer).

### Answer:

Global state management is essential when data or UI state needs to be shared across multiple components that aren't directly related. Several tools can be used in React to manage global state, each with its own strengths depending on complexity, scalability, and developer preferences.

#### How It Works:

Global state tools allow you to store and update shared state outside of individual components, and trigger re-renders when the state changes. You typically access global state via hooks or context APIs and update it through defined actions or mutators.

1. Redux (Most Popular for Complex Apps)

   - Centralized state store using a reducer pattern and action dispatching.

   - Requires setup of store, reducers, and actions.

     ```
     // Example using Redux Toolkit
     import { configureStore, createSlice } from '@reduxjs/toolkit';

     const counterSlice = createSlice({
       name: 'counter',
       initialState: { value: 0 },
       reducers: {
         increment: (state) => { state.value += 1; },
       },
     });

     export const { increment } = counterSlice.actions;

     const store = configureStore({ reducer: { counter: counterSlice.reducer } });
     ```

   - Use with <Provider store={store}> and useSelector/useDispatch.

   - Best for: Large-scale apps, enterprise-level state, middleware (e.g., logging, async logic)

2. React Context + useReducer (Built-in & Lightweight)

   - Use React’s Context API to expose state globally.

   - Combine with useReducer to manage state transitions.

     ```
     const AppContext = createContext();

     const reducer = (state, action) => {
       switch (action.type) {
         case 'INCREMENT': return { count: state.count + 1 };
         default: return state;
       }
     };

     const AppProvider = ({ children }) => {
       const [state, dispatch] = useReducer(reducer, { count: 0 });
       return (
         <AppContext.Provider value={{ state, dispatch }}>
           {children}
         </AppContext.Provider>
       );
     };
     ```

   - Best for: Small to medium apps, avoiding external libraries

3. Zustand (Minimalist & Scalable)

   - A modern, hook-based state manager that’s easy to set up and use.

   - State is stored in a plain JavaScript store and accessed via custom hooks.

     ```
     import { create } from 'zustand';

     const useStore = create((set) => ({
       count: 0,
       increment: () => set((state) => ({ count: state.count + 1 })),
     }));

     // Usage in component
     const count = useStore((state) => state.count);
     ```

   - Best for: Fast, scalable, and minimal setups without boilerplate

4. MobX (Reactive, Observer-Based)

   - Uses observable state and automatic tracking of dependencies.

   - State updates propagate automatically to components that use them.

     ```
     import { makeAutoObservable } from 'mobx';

     class CounterStore {
       count = 0;
       constructor() {
         makeAutoObservable(this);
       }
       increment() {
         this.count += 1;
       }
     }
     ```

   - Use with observer() from mobx-react-lite to re-render components on change.

   - Best for: Apps with dynamic, reactive data flows and less boilerplate

- Comparison Table:
  | Tool | Boilerplate | Async Support | Performance | Ecosystem | Learning Curve |
  | --- | --- | --- | --- | --- | --- |
  | Redux | High | ✅ (Redux Thunk, Saga) | High | Massive | Moderate–High |
  | React Context + Reducer | Low | ❌ (manual) | Good | Native | support Low |
  | Zustand | Very Low | ✅ (built-in) | Excellent | Lightweight | Very Low |
  | MobX | Low | ✅ (reactive) | Excellent | Moderate | Moderate |

#### Best Practices:

- Keep global state minimal; prefer local state when possible.

- Split state logically (auth, UI, data, etc.) for modularity.

- Use selectors/memoization for performance in Redux/Zustand.

- Avoid prop drilling with Context or other global tools.

#### Benefits of Good Global State Management:

- Improves scalability and maintainability

- Avoids prop drilling

- Encourages predictable state transitions

- Easier debugging and testing

## 10. How do you test React components? Compare approaches using Jest, React Testing Library, and Enzyme.

### Answer:

Testing React components ensures your UI behaves as expected, guards against regressions, and documents component contracts. The three most common tools are Jest (the test runner/assertion library), React Testing Library (RTL), and Enzyme.

#### How It Works:

- Jest (Test Runner & Assertion Library)

  - Role: Executes test files, provides assertions (expect), spies, and built‑in mocking.

  - Usage:

    ```
    test('adds two numbers', () => {
      expect(1 + 2).toBe(3);
    });
    ```

  - Integration: Works seamlessly with Babel/TypeScript and has snapshot testing for serializing component output.

- React Testing Library (RTL)

  - Philosophy: Test components from the user’s perspective—interact with the DOM the way a user would.

  - API: Queries like getByText, getByRole, and fireEvent or userEvent.

  - Example:

    ```
    import { render, screen, fireEvent } from '@testing-library/react';
    import LoginForm from './LoginForm';

    test('submits username', () => {
      render(<LoginForm />);
      fireEvent.change(screen.getByLabelText(/username/i), { target: { value: 'Alice' } });
      fireEvent.click(screen.getByRole('button', { name: /submit/i }));
      expect(screen.getByText(/welcome, Alice/i)).toBeInTheDocument();
    });
    ```

- Enzyme

  - Philosophy: Inspect and manipulate component instances and internals via shallow, mount, and static rendering.

  - API: Methods like shallow(), mount(), .find(), .simulate().

  - Example:

    ```
    import { shallow } from 'enzyme';
    import Button from './Button';

    test('calls onClick handler', () => {
      const onClick = jest.fn();
      const wrapper = shallow(<Button onClick={onClick}>Click</Button>);
      wrapper.simulate('click');
      expect(onClick).toHaveBeenCalled();
    });
    ```

#### Comparison:

| Feature                                                         | Jest                           | React Testing Library               | Enzyme                                       |
| --------------------------------------------------------------- | ------------------------------ | ----------------------------------- | -------------------------------------------- |
| Primary Role                                                    | Test runner, assertions, mocks | DOM-centric                         | component testing Component instance testing |
| Testing Philosophy                                              | Any JS testing                 | “What the user sees”                | “What’s inside the component”                |
| Rendering Methods                                               | N/A                            | Full DOM via JSDOM                  | `shallow`, `mount`, `render`                 |
| API Style                                                       | `expect`, `jest.fn()`          | `screen.getBy…`, `fireEvent`        | `.find()`, `.simulate()`                     |
| Snapshot Testing Built‑in Via Jest snapshots Via Jest snapshots |
| Ease of Refactoring                                             | N/A                            | High (less implementation coupling) | Lower (tests tied to structure)              |
| Learning Curve                                                  | Low                            | Low                                 | Moderate                                     |

#### Best Practices:

- Favor RTL for UI Tests: Tests are more resilient to implementation changes and focus on user interactions.

- Use Jest for Unit Logic & Mocks: Leverage its powerful mocking and snapshot capabilities for pure functions and simple components.

- Limit Enzyme to Legacy Code: If you need to inspect internal state or lifecycle methods in older codebases, Enzyme can help—but avoid for new tests.

- Keep Tests Fast & Deterministic: Mock network calls, avoid real timers unless necessary, and clean up after each test (cleanup() is automatic in RTL).

- Write Accessible Queries: In RTL, use queries like getByRole or getByLabelText to encourage accessible markup.

#### Benefits:

- Confidence: Automated tests catch regressions before they reach production.

- Documentation: Tests serve as living examples of component usage and expected behavior.

- Refactor Safety: With a robust test suite, you can refactor internal implementations without breaking functionality.

- User‑Focused Quality: RTL tests ensure your app works from the user’s perspective, improving overall reliability and accessibility.

# Advanced Level

## 1. Explain the React reconciliation algorithm (diffing) and how keys affect it.

### Answer:

React’s reconciliation algorithm, also known as the diffing algorithm, is the process React uses to efficiently update the DOM by comparing the previous and current Virtual DOM. The goal is to minimize direct DOM manipulations by making the smallest possible set of changes.

#### How It Works (Reconciliation Process):

When state or props change, React re-renders the component and creates a new Virtual DOM tree. The reconciliation algorithm then compares this new tree with the previous one to identify what changed.

- React follows these rules to perform efficient diffing:

  - Element Type Comparison

  - If the element types are different (<div> → <span>), React destroys the old node and mounts a new one.

  - If types are the same, it updates the attributes and recursively diffs children.

  - Component Type Comparison

  - If the component class or function changes, React unmounts the old component and mounts the new one.

  - Recursive Diffing of Children

  - React compares child nodes one by one.

  - It assumes child order doesn't change, so it compares elements at the same index.

#### Role of Keys in Reconciliation:

- Keys are essential for tracking which items changed, were added, or removed when dealing with lists of elements.
  ```
  {items.map(item => (
    <li key={item.id}>{item.name}</li>
  ))}
  ```
- Why Keys Matter:

  - Without keys (or with index as key), React compares elements by index.

  - With proper keys, React can reorder items efficiently without re-rendering all of them.

  - Keys help React identify which elements were changed, improving performance and avoiding UI bugs like lost focus or animation glitches.

- Example:

  ```
  const list = [ 'A', 'B', 'C' ];
  // Re-render with ['B', 'C', 'A']

  // Without keys: React will assume everything changed by index.
  // With keys: React knows 'A' moved, and 'B', 'C' are the same.
  ```

- Best Practices for Keys:

  - Use stable, unique identifiers (e.g., database IDs).

  - Avoid using array indexes as keys, especially in dynamic lists.

  - Ensure keys are consistent between renders to prevent remounting.

- Optimization Outcome:

  - Prevents unnecessary DOM updates.

  - Preserves component state and behavior across renders.

  - Enables smoother UI updates, animations, and interactions.

## 2. What is Concurrent Mode and how does it improve UI responsiveness?

### Answer:

Concurrent Mode is an advanced feature in React that allows rendering to be interruptible, enabling React to work on multiple tasks simultaneously. It improves UI responsiveness by making rendering non-blocking, allowing React to prioritize more important updates (like animations or input) over less urgent ones (like rendering a long list).

#### How It Works:

In traditional React rendering (legacy mode), the render phase is synchronous and blocking—once React starts rendering, it must finish before responding to user input or other tasks.

- With Concurrent Mode:

  - React’s render phase becomes interruptible.

  - Rendering work is split into small units.

  - React can pause work, respond to high-priority events, and resume rendering later.

  - This is powered by the React scheduler, which manages task priority and time slicing.

- Example:
  Imagine you have a typing input next to a large component that fetches and displays lots of data. In legacy mode, updating that large component may block input typing.

- With Concurrent Mode:

  - React can pause rendering the large component

  - Process the user’s keystroke first

  - Then resume rendering the heavy component afterward

- Enabling Concurrent Features:

  - Automatic in React 18+ when using features like:

  - `createRoot()` from react-dom/client

  - `useTransition()`

  - Suspense for data fetching

  - `startTransition()`

  ```
  import { startTransition } from 'react';

  startTransition(() => {
  setSearchQuery(input); // Marked as low-priority
  });
  ```

- Key Features of Concurrent Mode:
  | Feature | Purpose |
  | --- | --- |
  | startTransition() | Marks updates as low-priority (e.g., search results) |
  | useDeferredValue() | Defers rendering based on a lower-priority value |
  | Suspense | Pauses rendering until async data is ready |
  | Time slicing | Splits rendering into chunks so React can yield to more urgent tasks|

#### Benefits:

- Improved UI responsiveness: Input and animation stay smooth even during heavy rendering.

- Prioritized updates: React can focus on urgent work first.

- Better user experience: Reduces jank, freezes, and lag in the UI.

- Graceful loading: Combines well with Suspense for smooth async loading flows.

## 3. How do Suspense and React.lazy work together for data fetching and code‑splitting?

### Answer:

React.lazy and Suspense are features in React that work together to enable code-splitting and graceful loading of components or data. They help improve performance and user experience by loading only what’s necessary, while still allowing the UI to show fallback content during loading.

#### How It Works

1. React.lazy – Dynamic Import for Components

   - Enables code-splitting at the component level using import().

   - Dynamically loads the component only when it is rendered.

     ```
     import React, { lazy, Suspense } from 'react';

     const Profile = lazy(() => import('./Profile'));

     function App() {
       return (
         <Suspense fallback={<div>Loading...</div>}>
           <Profile />
         </Suspense>
       );
     }
     ```

   - React.lazy only works with default exports.

2. Suspense – Handles the Loading State

   - Wraps lazy components and displays fallback UI while the component is being loaded.

   - Can wrap multiple lazy components or use nested Suspense for finer control.

#### Use Cases

| Feature                    | Purpose                           | Example                                 |
| -------------------------- | --------------------------------- | --------------------------------------- |
| Code-splitting             | Split bundles into smaller chunks | `React.lazy(() => import('./Chart'))`   |
| Loading fallback           | Show spinner or skeleton on load  | `<Suspense fallback={<Loader />} />`    |
| Async data (via libraries) | Data loading with Suspense        | react-query, Relay, React 19 (upcoming) |

#### Example: Code Splitting with React.lazy

    ```
    const HeavyComponent = lazy(() => import('./HeavyComponent'));

    <Suspense fallback={<div>Loading heavy component…</div>}>
    <HeavyComponent />
    </Suspense>
    ```

- React will:

  - Pause rendering HeavyComponent

  - Show fallback (Loading…)

  - Replace fallback with component once loaded

#### Data Fetching with Suspense (React 19 / Relay / Libraries)

Although Suspense was originally built for code-splitting, newer versions of React (v19+) and libraries like Relay, React Query, or SWR allow you to suspend rendering while data is being fetched.

- Example (React Query v5 + Suspense):

  ````
  const Posts = () => {
  const { data } = useQuery(['posts'], fetchPosts, { suspense: true });
  return <PostList posts={data} />;
  };

      <Suspense fallback={<div>Loading posts...</div>}>
      <Posts />
      </Suspense>
      ```
  ````

#### Benefits

- Performance optimization: Reduce initial bundle size.

- User experience: Show meaningful loading indicators or skeletons.

- Clean abstraction: Avoid deeply nested loading logic and spinners.

- Developer ergonomics: Simple syntax for lazy loading components or data.

#### Best Practices

- Use React.lazy for non-critical or rarely-used components (e.g., modals, dashboards).

- Wrap each lazy import in Suspense with meaningful fallbacks.

- Combine with routing (React Router supports lazy routes).

- For data fetching, rely on libraries that integrate with Suspense or wait for React 19’s native support.

## 4. Describe server‑side rendering (SSR) with frameworks like Next.js. What are the trade‑offs?

### Answer:

Server-side rendering (SSR) is a technique where HTML for a React application is generated on the server for every request, rather than in the browser. Frameworks like Next.js make SSR seamless by offering built-in support to pre-render pages on the server and send them fully formed to the client.

#### How SSR Works in Next.js:

- A user requests a page.

- The server runs the React component (usually in getServerSideProps()).

- It fetches the necessary data and generates the HTML.

- The HTML is sent to the browser, where React hydrates it—adds interactivity to the static content.

  ```
  // Example in Next.js using SSR
  export async function getServerSideProps(context) {
    const res = await fetch('https://api.example.com/posts');
    const data = await res.json();

    return { props: { posts: data } };
  }

  function Blog({ posts }) {
    return <PostList posts={posts} />;
  }
  ```

#### Benefits of SSR

| Benefit                      | Description                                                              |
| ---------------------------- | ------------------------------------------------------------------------ |
| SEO-Friendly                 | HTML is pre-rendered and crawlable by search engines.                    |
| Faster Time-to-First-Byte    | Users get a fully rendered page instantly (especially on slow networks). |
| Dynamic Data on Each Request | Great for pages that need fresh, per-request data (e.g., dashboards).    |

#### Trade-Offs and Limitations

| Trade-Off                   | Explanation                                                             |
| --------------------------- | ----------------------------------------------------------------------- |
| Slower Time-to-Interactive  | Initial HTML loads fast, but hydration delays interactivity.            |
| Server Load                 | Each request triggers full rendering logic, increasing server demand.   |
| Not Suitable for All Pages  | For static content or user-specific data, SSR may be overkill.          |
| More Complex Dev Experience | Handling SSR-specific logic (auth, redirects, cookies) adds complexity. |

#### When to Use SSR (in Next.js)

- Pages that must be indexed by search engines (e.g., blogs, product pages).

- Content that changes frequently or is personalized per user.

- Apps where Time to First Byte (TTFB) matters more than interactivity.

##### SSR Alternatives in Next.js

- Method Use Case
- Static Generation (getStaticProps) For content that doesn’t change often
- Client-Side Rendering For user-specific, non-SEO data
- Incremental Static Regeneration (ISR) Hybrid between static and SSR

#### Best Practices

- Use SSR selectively where SEO or dynamic data is critical.

- Cache server responses (using CDNs or headers) to reduce load.

- Avoid SSR for pages with high interactivity or frequent navigation.

## 5. How would you optimize performance in a large React application? (e.g., virtualization, memoization, windowing)

### Answer:

Optimizing performance in a large React application involves using a combination of techniques to reduce unnecessary renders, minimize bundle sizes, and enhance perceived and actual speed. Strategies like memoization, virtualization, and windowing play a key role in making complex UIs performant and responsive.

#### Key Optimization Techniques

1. Memoization with React.memo, useMemo, and useCallback

   - Prevents unnecessary re-renders by caching components or values.

   - Useful for pure components or expensive computations.

     ```
     const MemoizedComponent = React.memo(MyComponent);

     const memoizedValue = useMemo(() => computeExpensiveValue(input), [input]);

     const handleClick = useCallback(() => { /* action */ }, []);
     ```

2. Virtualization / Windowing (e.g., react-window, react-virtualized)

   - Renders only visible portions of large lists instead of the entire list.

   - Essential for improving performance when dealing with thousands of DOM elements.

     ```
     import { FixedSizeList as List } from 'react-window';

     <List height={500} itemCount={1000} itemSize={35} width={300}>
       {({ index, style }) => <div style={style}>Row {index}</div>}
     </List>
     ```

3. Code-Splitting & Lazy Loading

   - Code-splitting reduces the initial bundle size by splitting the code into smaller chunks.

   - `const LazyComponent = React.lazy(() => import('./HeavyComponent'));`

4. Avoid Anonymous Functions & Inline Objects

   - Avoid using anonymous functions or inline objects in event handlers and lifecycle methods.

   ```
   // BAD
   <MyComponent onClick={() => doSomething()} />

   // GOOD
   const handleClick = useCallback(() => doSomething(), []);
   <MyComponent onClick={handleClick} />
   ```

5. Debounce / Throttle Input Handlers

   - Avoid excessive updates by debouncing or throttling heavy operations like search or scroll.

   - `const debouncedSearch = useMemo(() => debounce(handleSearch, 300), []);`

6. Efficient Reconciliation

   - Use stable keys in lists to help React's diffing algorithm.

   - Avoid using array indexes as keys.

   - `{items.map(item => <Item key={item.id} {...item} />)}`

7. Use the Profiler
   - React DevTools' Profiler tab helps identify performance bottlenecks by tracking render times.

#### Best Practices Summary

| Technique                 | Use Case                                          |
| ------------------------- | ------------------------------------------------- |
| `React.memo`              | Prevent re-renders of functional components       |
| `useMemo`, `useCallback`  | Cache computed values and stable functions        |
| List virtualization       | Improve performance on large lists/tables         |
| Debouncing/throttling     | Optimize user-driven events like search or scroll |
| Lazy loading              | Reduce initial bundle size                        |
| Static asset optimization | Compress and cache images, fonts, and media       |

## 6. Explain how you’d build a custom hook. What rules must you follow?

### Answer:

A custom hook in React is a JavaScript function whose name starts with "use" and allows you to extract and reuse stateful logic across components. Custom hooks help keep code modular, readable, and DRY (Don't Repeat Yourself) by abstracting repeated logic into reusable functions.

#### How to Build a Custom Hook

- Let’s walk through an example: a useWindowWidth hook that tracks the browser’s window width.

  ```
  import { useState, useEffect } from 'react';

  function useWindowWidth() {
    const [width, setWidth] = useState(window.innerWidth);

    useEffect(() => {
      const handleResize = () => setWidth(window.innerWidth);
      window.addEventListener('resize', handleResize);

      return () => window.removeEventListener('resize', handleResize);
    }, []);

    return width;
  }
  ```

- You can now use this hook in any functional component:
  ```
  function MyComponent() {
    const width = useWindowWidth();
    return <p>Window width: {width}px</p>;
  }
  ```

#### Rules of Custom Hooks

To ensure hooks work as expected, you must follow the Rules of Hooks:

- Only call hooks at the top level

  - Don’t call hooks inside loops, conditions, or nested functions.

  - This ensures React can track the order of hook calls consistently.

- Only call hooks from React functions

  - Use hooks in React function components or other custom hooks only.

  - Avoid using them in regular JavaScript functions or class components.

- Prefix with use

  - Custom hook names must start with use (e.g., useFetch, useLocalStorage) to be recognized by React linting tools and the compiler.

#### When to Use Custom Hooks

- Custom hooks are ideal when you want to:

  - Share logic across components (e.g., timers, form state, media queries).

  - Encapsulate side effects like data fetching or event listeners.

  - Compose behavior from other hooks.

#### Best Practices

| Practice                       | Reason                                                                 |
| ------------------------------ | ---------------------------------------------------------------------- |
| Use meaningful names (useAuth) | Makes intent and usage clear                                           |
| Return values clearly          | Use arrays or objects to make return values predictable and documented |
| Separate logic from UI         | Keep hooks focused on logic; UI remains in components                  |
| Compose hooks                  | if needed Build on existing hooks to create new functionality          |

## 7. What patterns do you use for component composition (e.g., compound components, function as child)?

### Answer:

Component composition is one of the core strengths of React. It allows developers to build flexible and reusable UIs by composing components together in meaningful ways. I often use patterns like compound components, function as a child (render props), and context-based composition to maintain clean separation of concerns and improve flexibility.

#### Common Component Composition Patterns

1.  Compound Components

    - How it works:
      Multiple related components work together under a single parent component to manage shared state implicitly.

      ```
      function Tabs({ children }) {
        const [activeIndex, setActiveIndex] = useState(0);

        return (
          <TabsContext.Provider value={{ activeIndex, setActiveIndex }}>
            {children}
          </TabsContext.Provider>
        );
      }

      function TabList({ children }) {
        return <div>{children}</div>;
      }

      function Tab({ index, children }) {
        const { activeIndex, setActiveIndex } = useContext(TabsContext);
        return (
          <button
            className={index === activeIndex ? "active" : ""}
            onClick={() => setActiveIndex(index)}
          >
            {children}
          </button>
        );
      }

      function TabPanel({ index, children }) {
        const { activeIndex } = useContext(TabsContext);
        return activeIndex === index ? <div>{children}</div> : null;
      }
      ```

    - Why it's useful:
      - Encourages clean API and encapsulated logic without passing props deeply.

2.  Function as Child (Render Props)

    - How it works:

      - A component accepts a function as its child. This function receives state or logic and returns JSX.

      ```
      function MouseTracker({ children }) {
        const [position, setPosition] = useState({ x: 0, y: 0 });

        useEffect(() => {
          const handleMove = e => setPosition({ x: e.clientX, y: e.clientY });
          window.addEventListener('mousemove', handleMove);
          return () => window.removeEventListener('mousemove', handleMove);
        }, []);

        return children(position);
      }

      // Usage:
      <MouseTracker>
        {({ x, y }) => <p>The mouse position is {x}, {y}</p>}
      </MouseTracker>
      ```

    - Why it's useful:
      - Great for reusable logic with custom UI. Replaced in many cases by hooks.

3.  Context-Based Composition

    - How it works:

      - Use React Context to share state/functionality between multiple nested components without prop drilling.

      ```
      const ThemeContext = React.createContext();

      function ThemeProvider({ children }) {
        const [theme, setTheme] = useState("dark");
        return (
          <ThemeContext.Provider value={{ theme, setTheme }}>
            {children}
          </ThemeContext.Provider>
        );
      }

      function ThemedComponent() {
        const { theme } = useContext(ThemeContext);
        return <div className={theme}>I'm themed!</div>;
      }
      ```

    - Why it's useful:
      - Solves deeply nested prop problems; works well with compound components.

#### Best Practices

| Pattern                | When to Use                                                |
| ---------------------- | ---------------------------------------------------------- |
| Compound Components    | For grouped elements sharing internal state                |
| Function as Child      | When custom rendering logic is needed with shared behavior |
| Context API            | For global or shared state management                      |
| Hook + Component combo | For clean separation of logic and presentation             |

## 8. Describe how you’d integrate React with a GraphQL API (e.g., Apollo Client, Relay).

### Answer:

Integrating React with a GraphQL API allows you to fetch and manage data declaratively using queries and mutations. Tools like Apollo Client and Relay streamline this process by handling query execution, caching, and state management on the client side. I typically use Apollo Client due to its flexibility and strong community support.

#### How to Integrate React with GraphQL Using Apollo Client

1. Install Apollo Client and Dependencies

   `npm install @apollo/client graphql`

2. Set Up Apollo Client

   ```
   import { ApolloClient, InMemoryCache, ApolloProvider } from '@apollo/client';

   const client = new ApolloClient({
     uri: 'https://your-graphql-endpoint.com/graphql',
     cache: new InMemoryCache(),
   });

   function App() {
     return (
       <ApolloProvider client={client}>
         <YourAppComponents />
       </ApolloProvider>
     );
   }
   ```

3. Use GraphQL Queries in Components

   ```
   import { useQuery, gql } from '@apollo/client';

   const GET_USERS = gql`
     query GetUsers {
       users {
         id
         name
       }
     }
   `;

   function UserList() {
     const { loading, error, data } = useQuery(GET_USERS);

     if (loading) return <p>Loading...</p>;
     if (error) return <p>Error loading users.</p>;

     return (
       <ul>
         {data.users.map(user => (
           <li key={user.id}>{user.name}</li>
         ))}
       </ul>
     );
   }
   ```

4. Mutations with Apollo

   ```
   import { useMutation, gql } from '@apollo/client';

   const ADD_USER = gql`
     mutation AddUser($name: String!) {
       addUser(name: $name) {
         id
         name
       }
     }
   `;

   function AddUserForm() {
     const [addUser] = useMutation(ADD_USER);

     const handleSubmit = e => {
       e.preventDefault();
       const name = e.target.elements.name.value;
       addUser({ variables: { name } });
     };

     return (
       <form onSubmit={handleSubmit}>
         <input name="name" placeholder="Enter name" />
         <button type="submit">Add User</button>
       </form>
     );
   }
   ```

#### Features Provided by Apollo

| Feature                | Benefit                                |
| ---------------------- | -------------------------------------- |
| Query & Mutation APIs  | Declarative data fetching and updates  |
| In-memory cache        | Reduces duplicate network calls        |
| DevTools integration   | Debug and inspect cache/state          |
| Error & loading states | Built-in UI helpers for async handling |

#### Relay (Alternative)

- Relay is a powerful GraphQL client built by Meta, designed for highly-scalable apps:

  - Strongly typed and static-optimized.

  - Fragments-first architecture for co-locating data with components.

  - Requires a build step with a GraphQL compiler.

- Relay is ideal for large, performance-critical apps with strict type safety needs.

#### Best Practices

| Practice                   | Why It Helps                                                  |
| -------------------------- | ------------------------------------------------------------- |
| Use fragments              | for reusable queries DRY and consistent data shapes           |
| Normalize cache            | keys properly Ensures cache consistency and avoids duplicates |
| Handle loading & errors    | gracefully Improves UX                                        |
| Use devtools for debugging | Speeds up development and cache inspection                    |

## 9. How would you architect a micro‑frontend system using React? What challenges arise?

### Answer:

A micro-frontend architecture breaks a large frontend app into smaller, independently deployable modules (or “frontends”), each owned by different teams. Using React in a micro-frontend setup allows teams to develop, deploy, and scale parts of the UI independently. This is especially valuable in enterprise-scale apps.

#### How to Architect a Micro-Frontend System Using React

1. Choose an Integration Strategy
   | Strategy | Description |
   | Module Federation (Webpack 5) | Each app exposes/imports React components at runtime |
   | iframes | Legacy isolation; poor UX but strict separation |
   | JavaScript-based integration | Apps exposed globally and loaded dynamically |
   | Single-spa | Framework-agnostic orchestrator for multiple frontends |
2. Module Federation (Preferred for React)

   - Using Webpack 5 Module Federation, each team builds its app independently, and a host shell loads them dynamically.

   - Example webpack.config.js in Remote App:
     ```
     module.exports = {
       name: "products",
       exposes: {
         './ProductList': './src/ProductList',
       },
       shared: { react: { singleton: true }, 'react-dom': { singleton: true } },
     };
     ```
   - Host App:
     ```
     remotes: {
       products: "products@http://localhost:3001/remoteEntry.js",
     }
     ```
   - Then load components via:

     `const ProductList = React.lazy(() => import("products/ProductList"));`

3. Shared Dependencies

   - Use singleton configuration for React and ReactDOM to avoid version mismatches.

   - Use a monorepo or strict versioning policies to manage shared dependencies across teams.

4. Routing Integration

   - Use a central router (like React Router in host) or isolate routes per micro-app.

   - Share route state using a global state manager (like Redux or Zustand) or messaging events.

5. Deployment & CI/CD

   - Each micro-frontend is independently deployed (e.g., as a static asset bundle).

   - The shell app can fetch the latest version dynamically, enabling partial rollouts or hot-swapping.

#### Challenges in Micro-Frontends

| Challenge                        | Solution                                                   |
| -------------------------------- | ---------------------------------------------------------- |
| Version mismatches               | Use singleton/shared dependencies in Module Federation     |
| Global state coordination        | Use shared context or event bus (e.g., postMessage, Redux) |
| Styling conflicts                | Use CSS modules, Tailwind, or Shadow DOM for isolation     |
| Increased complexity             | Add orchestration tools, clear team contracts              |
| Performance & bundle duplication | Deduplicate using shared Webpack config or build analysis  |

#### Best Practices

| Practice                           | Why It Helps                                 |
| ---------------------------------- | -------------------------------------------- |
| Keep micro-apps truly independent  | Enables better scalability and team autonomy |
| Define clear integration contracts | Reduces coupling and unexpected changes      |
| Use lazy loading for performance   | Prevents huge initial payloads               |
| Central logging & monitoring       | Unified observability across micro-apps      |
