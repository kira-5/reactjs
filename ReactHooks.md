React Hooks Overview
--------------------

### 1. `useState`

#### What It Is

* A hook that allows you to add state to functional components.

#### Purpose

* To manage and update component state.

#### Use Cases

* **Managing State**: Track simple values like numbers, strings, arrays, and objects.
    
    * **Example**: Counter, form input values.
    
    ```jsx
    import React, { useState } from 'react';
    
    function Counter() {
      const [count, setCount] = useState(0);
    
      return (
        <div>
          <p>You clicked {count} times</p>
          <button onClick={() => setCount(count + 1)}>Click me</button>
        </div>
      );
    }
    ```
    

#### When to Use

* To manage state in functional components.

#### When Not to Use

* For state that requires complex logic or is managed globally (consider context or state management libraries).

### 2. `useEffect`

#### What It Is

* A hook that lets you perform side effects in functional components.

#### Purpose

* To handle operations like data fetching, subscriptions, or manually changing the DOM.

#### Use Cases

* **Side Effects**: Fetching data, setting up subscriptions, manual DOM manipulations.
    
    * **Example**: Fetching user data on component mount.
    
    ```jsx
    import React, { useEffect, useState } from 'react';
    
    function UserProfile() {
      const [user, setUser] = useState(null);
    
      useEffect(() => {
        fetch('https://api.example.com/user')
          .then(response => response.json())
          .then(data => setUser(data));
      }, []); // Empty dependency array means this runs once after the initial render
    
      return <div>{user ? `Hello, ${user.name}` : 'Loading...'}</div>;
    }
    ```
    

#### When to Use

* To perform side effects in functional components.

#### When Not to Use

* For operations that should not be side effects or that are purely synchronous and do not need to wait for render.

### 3. `useContext`

#### What It Is

* A hook that allows you to access the value of a context directly.

#### Purpose

* To access context values without wrapping components in `Context.Consumer`.

#### Use Cases

* **Context Access**: Consuming context values in deeply nested components.
    
    * **Example**: Accessing theme or user authentication context.
    
    ```jsx
    import React, { useContext } from 'react';
    import { ThemeContext } from './ThemeProvider';
    
    function ThemedButton() {
      const theme = useContext(ThemeContext);
    
      return <button style={{ background: theme.background, color: theme.color }}>Click me</button>;
    }
    ```
    

#### When to Use

* To access context values directly in functional components.

#### When Not to Use

* When you don’t need context or are using class components.

### 4. `useReducer`

#### What It Is

* A hook that is an alternative to `useState` for managing more complex state logic.

#### Purpose

* To handle complex state transitions in a predictable way.

#### Use Cases

* **Complex State Logic**: When state logic involves multiple sub-values or when the next state depends on the previous one.
    
    * **Example**: Managing form state with multiple fields.
    
    ```jsx
    import React, { useReducer } from 'react';
    
    const initialState = { count: 0 };
    
    function reducer(state, action) {
      switch (action.type) {
        case 'increment':
          return { count: state.count + 1 };
        case 'decrement':
          return { count: state.count - 1 };
        default:
          throw new Error();
      }
    }
    
    function Counter() {
      const [state, dispatch] = useReducer(reducer, initialState);
    
      return (
        <div>
          <p>Count: {state.count}</p>
          <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
          <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
        </div>
      );
    }
    ```
    

#### When to Use

* When state management is complex or involves multiple actions and transitions.

#### When Not to Use

* For simple state logic where `useState` suffices.

### 5. `useRef`

#### What It Is

* A hook that allows you to persist values between renders without causing re-renders.

#### Purpose

* To access DOM elements or keep mutable values that do not trigger re-renders.

#### Use Cases

* **DOM Access**: Referencing DOM elements.
    
* **Mutable Values**: Storing values that persist across renders.
    
    * **Example**: Storing previous props or state.
    
    ```jsx
    import React, { useRef, useEffect } from 'react';
    
    function PreviousValue({ value }) {
      const prevValueRef = useRef();
    
      useEffect(() => {
        prevValueRef.current = value;
      }, [value]);
    
      return <div>Previous value: {prevValueRef.current}</div>;
    }
    ```
    

#### When to Use

* For referencing DOM elements or storing mutable values that should not cause re-renders.

#### When Not to Use

* For state management where re-rendering is needed.

### 6. `useMemo`

#### What It Is

* A hook that memoizes the result of a computation to avoid expensive recalculations.

#### Purpose

* To optimize performance by memoizing values.

#### Use Cases

* **Performance Optimization**: Memoizing expensive calculations or operations.
    
    * **Example**: Calculating derived data from props or state.
    
    ```jsx
    import React, { useMemo, useState } from 'react';
    
    function ExpensiveComponent({ data }) {
      const computedValue = useMemo(() => {
        // Expensive computation
        return data.reduce((acc, item) => acc + item, 0);
      }, [data]);
    
      return <div>Computed Value: {computedValue}</div>;
    }
    ```
    

#### When to Use

* To optimize performance when computations are expensive or depend on specific values.

#### When Not to Use

* For simple calculations where performance is not an issue.

### 7. `useCallback`

#### What It Is

* A hook that memoizes callback functions to prevent unnecessary re-renders.

#### Purpose

* To avoid recreating functions on every render and optimize performance.

#### Use Cases

* **Function Memoization**: Preventing unnecessary re-renders by memoizing callbacks.
    
    * **Example**: Passing a stable callback to a child component.
    
    ```jsx
    import React, { useCallback, useState } from 'react';
    
    function Child({ onClick }) {
      return <button onClick={onClick}>Click me</button>;
    }
    
    function Parent() {
      const [count, setCount] = useState(0);
    
      const handleClick = useCallback(() => {
        setCount(count + 1);
      }, [count]);
    
      return <Child onClick={handleClick} />;
    }
    ```
    

#### When to Use

* To memoize callback functions that are passed as props to avoid unnecessary re-renders.

#### When Not to Use

* For simple functions where memoization does not provide a performance benefit.

### 8. `useImperativeHandle`

#### What It Is

* A hook that customizes the instance value that is exposed when using `ref` with `forwardRef`.

#### Purpose

* To control what is exposed to parent components through a ref.

#### Use Cases

* **Custom Ref Exposure**: When you want to expose specific methods or properties from a child component.
    
    * **Example**: Exposing a focus method to a parent component.
    
    ```jsx
    import React, { forwardRef, useImperativeHandle, useRef, useState } from 'react';
    
    const CustomInput = forwardRef((props, ref) => {
      const inputRef = useRef(null);
    
      useImperativeHandle(ref, () => ({
        focus: () => {
          inputRef.current.focus();
        }
      }));
    
      return <input ref={inputRef} {...props} />;
    });
    
    function Parent() {
      const inputRef = useRef(null);
    
      return (
        <div>
          <CustomInput ref={inputRef} />
          <button onClick={() => inputRef.current.focus()}>Focus the input</button>
        </div>
      );
    }
    ```
    

#### When to Use

* When you need to control what methods or properties are exposed through a ref.

#### When Not to Use

* For components where direct manipulation of refs is not needed.

### 9. `useLayoutEffect`

#### What It Is

* A hook similar to `useEffect`, but it fires synchronously after all DOM mutations.

#### Purpose

* To perform operations that need to happen before the browser has painted.

#### Use Cases

* **DOM Measurements**: Reading layout or visual information before the browser paints.
    
    * **Example**: Calculating and applying styles based on element dimensions.
    
    ```jsx
    import React, { useLayoutEffect, useRef, useState } from 'react';
    
    function LayoutComponent() {
      const [width, setWidth] = useState(0);
      const ref = useRef(null);
    
      useLayoutEffect(() => {
        setWidth(ref.current.offsetWidth);
      }, []);
    
      return <div ref={ref}>Width: {width}px</div>;
    }
    ```
    

#### When to Use

* When you need to read layout measurements or make DOM changes before the browser repaints.

#### When Not to Use

* For typical side effects that can be handled with `useEffect`.

### 10. `useDebugValue`

#### What It Is

* A hook that allows you to display a label for custom hooks in React DevTools.

#### Purpose

* To provide debugging information for custom hooks.

#### Use Cases

* **Debugging Custom Hooks**: Displaying useful information about the state of custom hooks in DevTools.
    
    * **Example**: Showing the current state or value in DevTools.
    
    ```jsx
    import { useDebugValue, useState } from 'react';
    
    function useCustomHook(value) {
      const [state, setState] = useState(value);
    
      useDebugValue(state ? 'Has Value' : 'No Value');
    
      return [state, setState];
    }
    ```
    

#### When to Use

* To provide more context and improve debugging for custom hooks in DevTools.

#### When Not to Use

* For regular hooks or when debugging information is not needed.

* * *

Custom hooks are a powerful feature in React that allow you to extract and reuse stateful logic across components. Here’s a comprehensive guide on custom hooks, including their purpose, use cases, and examples:

* * *

Custom Hooks
------------

### What They Are

* **Custom Hooks** are JavaScript functions that use React’s built-in hooks (like `useState`, `useEffect`, `useReducer`, etc.) to encapsulate reusable stateful logic. They follow the naming convention `useSomething` and can be used within functional components.

### Purpose

* **Encapsulation of Logic**: To encapsulate and reuse complex logic across different components.
* **Code Reusability**: To avoid code duplication by sharing logic between components.
* **Separation of Concerns**: To keep components clean and focused on rendering by moving logic into hooks.

### Use Cases

1. **Managing Form State**:
    
    * Custom hooks can simplify form handling, validation, and submission logic.
    
    ```jsx
    import { useState } from 'react';
    
    function useForm(initialValues) {
      const [values, setValues] = useState(initialValues);
    
      const handleChange = (e) => {
        setValues({
          ...values,
          [e.target.name]: e.target.value,
        });
      };
    
      const resetForm = () => {
        setValues(initialValues);
      };
    
      return [values, handleChange, resetForm];
    }
    
    function MyForm() {
      const [formValues, handleChange, resetForm] = useForm({ name: '', email: '' });
    
      return (
        <form>
          <input name="name" value={formValues.name} onChange={handleChange} />
          <input name="email" value={formValues.email} onChange={handleChange} />
          <button type="button" onClick={resetForm}>Reset</button>
        </form>
      );
    }
    ```
    
2. **Fetching Data**:
    
    * Custom hooks can handle data fetching and manage loading and error states.
    
    ```jsx
    import { useState, useEffect } from 'react';
    
    function useFetch(url) {
      const [data, setData] = useState(null);
      const [loading, setLoading] = useState(true);
      const [error, setError] = useState(null);
    
      useEffect(() => {
        fetch(url)
          .then(response => response.json())
          .then(data => {
            setData(data);
            setLoading(false);
          })
          .catch(error => {
            setError(error);
            setLoading(false);
          });
      }, [url]);
    
      return { data, loading, error };
    }
    
    function DataDisplay({ url }) {
      const { data, loading, error } = useFetch(url);
    
      if (loading) return <p>Loading...</p>;
      if (error) return <p>Error: {error.message}</p>;
      return <div>{JSON.stringify(data)}</div>;
    }
    ```
    
3. **Managing WebSocket Connections**:
    
    * Custom hooks can manage WebSocket connections and handle real-time updates.
    
    ```jsx
    import { useState, useEffect } from 'react';
    
    function useWebSocket(url) {
      const [message, setMessage] = useState(null);
    
      useEffect(() => {
        const socket = new WebSocket(url);
    
        socket.onmessage = (event) => {
          setMessage(event.data);
        };
    
        return () => {
          socket.close();
        };
      }, [url]);
    
      return message;
    }
    
    function RealTimeComponent() {
      const message = useWebSocket('wss://example.com/socket');
    
      return <div>Received message: {message}</div>;
    }
    ```
    
4. **Debouncing User Input**:
    
    * Custom hooks can handle debouncing to limit the rate at which a function is executed.
    
    ```jsx
    import { useState, useEffect } from 'react';
    
    function useDebounce(value, delay) {
      const [debouncedValue, setDebouncedValue] = useState(value);
    
      useEffect(() => {
        const handler = setTimeout(() => {
          setDebouncedValue(value);
        }, delay);
    
        return () => {
          clearTimeout(handler);
        };
      }, [value, delay]);
    
      return debouncedValue;
    }
    
    function SearchInput() {
      const [query, setQuery] = useState('');
      const debouncedQuery = useDebounce(query, 500);
    
      useEffect(() => {
        if (debouncedQuery) {
          // Fetch data based on debouncedQuery
          console.log('Fetching data for:', debouncedQuery);
        }
      }, [debouncedQuery]);
    
      return <input value={query} onChange={(e) => setQuery(e.target.value)} />;
    }
    ```
    
5. **Local Storage**:
    
    * Custom hooks can interact with local storage for persisting data.
    
    ```jsx
    import { useState, useEffect } from 'react';
    
    function useLocalStorage(key, initialValue) {
      const [value, setValue] = useState(() => {
        const storedValue = window.localStorage.getItem(key);
        return storedValue ? JSON.parse(storedValue) : initialValue;
      });
    
      useEffect(() => {
        window.localStorage.setItem(key, JSON.stringify(value));
      }, [key, value]);
    
      return [value, setValue];
    }
    
    function LocalStorageComponent() {
      const [name, setName] = useLocalStorage('name', '');
    
      return (
        <div>
          <input value={name} onChange={(e) => setName(e.target.value)} />
          <p>Stored Name: {name}</p>
        </div>
      );
    }
    ```
    

### When to Use

* **Reusability**: When you have logic that is used in multiple components.
* **Separation of Concerns**: To separate logic from component rendering.
* **Complex Logic**: When dealing with complex state logic or side effects.

### When Not to Use

* **Over-engineering**: Avoid creating custom hooks for trivial or simple logic.
* **Non-Hook Logic**: Custom hooks should only use other hooks. They shouldn’t include non-hook logic or perform actions outside the scope of React's functional components.

* * *

These notes cover the concept of custom hooks, their purpose, and practical examples of their use. Custom hooks are a flexible way to encapsulate and reuse stateful logic across your React application.