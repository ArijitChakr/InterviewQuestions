# Beginner-Level

## What is JSX and why is it used?

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

## How do you create a functional component vs. a class component?

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

## What are props in React? How do you pass data from parent to child?

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

## What is state in React? How do you initialize and update it?

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

## Explain the component lifecycle methods in class components (e.g., componentDidMount, componentDidUpdate, componentWillUnmount).

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

## What’s the difference between a controlled and an uncontrolled component?

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

## How do you handle events in React (e.g., onClick, onChange)?

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

## How do you conditionally render elements in React?

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

## What is the key prop, and why is it important when rendering lists?

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

## How do you lift state up, and why would you do it?

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

## What are React Hooks? Name the most common built‑in hooks and their use cases.

## How does the useEffect hook work? How do you control when it runs?

## What’s the difference between useMemo and useCallback?

## Explain the Context API and when you’d use it instead of prop drilling.

## What are Higher‑Order Components (HOCs)? How do they differ from render props?

## How do you handle error boundaries in React?

## What is React Router, and how do you implement nested routes?

## How would you implement code‑splitting and lazy loading in a React app?

## Describe how you would manage global state (e.g., Redux, MobX, Zustand, React Context + Reducer).

## How do you test React components? Compare approaches using Jest, React Testing Library, and Enzyme.

# Advanced / Senior-Level

## Explain the React reconciliation algorithm (diffing) and how keys affect it.

## What is Concurrent Mode and how does it improve UI responsiveness?

## How do Suspense and React.lazy work together for data fetching and code‑splitting?

## Describe server‑side rendering (SSR) with frameworks like Next.js. What are the trade‑offs?

## How would you optimize performance in a large React application? (e.g., virtualization, memoization, windowing)

## Explain how you’d build a custom hook. What rules must you follow?

## How do you handle accessibility (a11y) in React applications?

## What patterns do you use for component composition (e.g., compound components, function as child)?

## Describe how you’d integrate React with a GraphQL API (e.g., Apollo Client, Relay).

## How would you architect a micro‑frontend system using React? What challenges arise?
