# React Style Guide Summary

This document summarizes key rules and best practices for writing React applications.

## 1. Component Structure
- **Functional Components:** Prefer functional components with hooks over class components
- **File Organization:** One component per file
- **File Extension:** `.jsx` or `.tsx` (TypeScript)
- **Naming:** `PascalCase` for components, files should match component name

```jsx
// Good: UserProfile.jsx
export function UserProfile({ user }) {
  return <div>{user.name}</div>;
}
```

## 2. Component Patterns
- **Props Destructuring:** Destructure props in function parameters
- **Default Props:** Use default parameters instead of `defaultProps`
- **Prop Types:** Use TypeScript or PropTypes for type checking

```jsx
// Good
function Button({ text = 'Click me', onClick, variant = 'primary' }) {
  return (
    <button className={`btn btn-${variant}`} onClick={onClick}>
      {text}
    </button>
  );
}

// With TypeScript
interface ButtonProps {
  text?: string;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
}

function Button({ text = 'Click me', onClick, variant = 'primary' }: ButtonProps) {
  return (
    <button className={`btn btn-${variant}`} onClick={onClick}>
      {text}
    </button>
  );
}
```

## 3. Hooks
- **Rules of Hooks:**
  - Only call hooks at the top level
  - Only call hooks from React functions
- **Custom Hooks:** Name with `use` prefix (e.g., `useUser`)
- **Dependencies:** Always include all dependencies in dependency arrays
- **ESLint:** Use `eslint-plugin-react-hooks` to enforce rules

```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function fetchUser() {
      setLoading(true);
      const data = await api.getUser(userId);
      setUser(data);
      setLoading(false);
    }
    fetchUser();
  }, [userId]); // Include all dependencies

  if (loading) return <Loading />;
  return <div>{user.name}</div>;
}
```

## 4. JSX Formatting
- **Self-Closing Tags:** Use self-closing tags when no children
- **Quotes:** Use double quotes for JSX attributes
- **Spacing:** No spaces in curly braces
- **Multi-line:** When multiple props, put each on new line
- **Conditional Rendering:** Use ternary for simple conditions, `&&` for single branch

```jsx
// Good
<Button
  text="Submit"
  onClick={handleClick}
  variant="primary"
  disabled={isLoading}
/>

// Conditional rendering
{isLoggedIn ? <Dashboard /> : <Login />}
{showError && <ErrorMessage error={error} />}
```

## 5. State Management
- **Local State:** Use `useState` for component-local state
- **Shared State:** Lift state to common ancestor or use Context
- **Complex State:** Use `useReducer` for complex state logic
- **Global State:** Consider Redux, Zustand, or Jotai for app-wide state

```jsx
// Simple local state
const [count, setCount] = useState(0);

// Complex state with useReducer
const [state, dispatch] = useReducer(reducer, initialState);

// Context for shared state
const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}
```

## 6. Performance Optimization
- **Memoization:** Use `useMemo` for expensive calculations
- **Callback Memoization:** Use `useCallback` for functions passed as props
- **Component Memoization:** Use `React.memo` for expensive components
- **Code Splitting:** Use `React.lazy` and `Suspense` for route-based splitting

```jsx
const expensiveValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);

const handleClick = useCallback(() => {
  doSomething(a, b);
}, [a, b]);

const MemoizedComponent = React.memo(ExpensiveComponent);

const LazyComponent = React.lazy(() => import('./LazyComponent'));
```

## 7. Event Handlers
- **Naming:** Prefix with `handle` (e.g., `handleClick`, `handleSubmit`)
- **Inline Functions:** Avoid in JSX for performance-critical components
- **Prevent Default:** Call `e.preventDefault()` when needed

```jsx
function Form() {
  const handleSubmit = (e) => {
    e.preventDefault();
    // Handle form submission
  };

  return <form onSubmit={handleSubmit}>...</form>;
}
```

## 8. Styling
- **CSS Modules:** Use CSS Modules for scoped styles
- **Styled Components:** Or use CSS-in-JS libraries like styled-components
- **Tailwind:** Or use utility-first frameworks like Tailwind CSS
- **Consistency:** Pick one approach and stick to it

```jsx
// CSS Modules
import styles from './Button.module.css';
<button className={styles.primary}>Click</button>

// Styled Components
import styled from 'styled-components';
const StyledButton = styled.button`
  background: blue;
`;
```

## 9. File Structure
```
src/
  components/
    common/        # Reusable components
    features/      # Feature-specific components
  hooks/           # Custom hooks
  contexts/        # Context providers
  utils/           # Utility functions
  services/        # API calls
  pages/           # Page components (routing)
```

## 10. Best Practices
- **Key Prop:** Always provide unique keys for list items
- **Fragment:** Use `<>` or `<Fragment>` instead of unnecessary divs
- **Avoid Index as Key:** Don't use array index as key if list can change
- **Accessibility:** Use semantic HTML and ARIA attributes
- **Error Boundaries:** Implement error boundaries for production
- **Testing:** Write tests for components using React Testing Library

```jsx
// Good: Unique keys
{users.map(user => (
  <UserCard key={user.id} user={user} />
))}

// Good: Fragments
<>
  <Header />
  <Main />
  <Footer />
</>
```

**BE CONSISTENT.** Follow your team's conventions and use linters (ESLint) and formatters (Prettier).

*Sources: [React Documentation](https://react.dev/), [Airbnb React Style Guide](https://github.com/airbnb/javascript/tree/master/react)*
