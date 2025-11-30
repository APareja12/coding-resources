# React Context API

## Overview

The Context API is a built-in React feature that allows you to share data across your component tree without passing props through every level (prop drilling). It's ideal for global data like themes, user authentication, language preferences, or any data that multiple components need to access. Context provides a way to create a "global" state that any component can subscribe to and update.

## Key Concepts

- **Provider** - Component that supplies the context value to its children
- **Consumer** - Components that read and subscribe to context changes
- **useContext Hook** - Modern way to consume context in functional components
- **Prop drilling** - The problem Context solves (passing props through many layers)
- **Re-rendering** - Components using context re-render when context value changes
- **Multiple contexts** - You can use multiple contexts in the same app

## Code Example

```javascript
import React, { createContext, useContext, useState } from 'react';

// 1. Create a Context
const ThemeContext = createContext();

// 2. Create a Provider Component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
  };

  // Value object contains both state and functions
  const value = {
    theme,
    toggleTheme,
  };

  return (
    <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>
  );
}

// 3. Custom hook for easier consumption (optional but recommended)
function useTheme() {
  const context = useContext(ThemeContext);

  if (context === undefined) {
    throw new Error('useTheme must be used within ThemeProvider');
  }

  return context;
}

// 4. Using the Context in components
function App() {
  return (
    <ThemeProvider>
      <Header />
      <MainContent />
      <Footer />
    </ThemeProvider>
  );
}

function Header() {
  const { theme, toggleTheme } = useTheme();

  return (
    <header style={{ background: theme === 'light' ? '#fff' : '#333' }}>
      <h1>My App</h1>
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'Dark' : 'Light'} Mode
      </button>
    </header>
  );
}

function MainContent() {
  const { theme } = useTheme();

  return (
    <main
      style={{
        background: theme === 'light' ? '#f5f5f5' : '#222',
        color: theme === 'light' ? '#000' : '#fff',
      }}
    >
      <p>Current theme: {theme}</p>
    </main>
  );
}

// More complex example: Authentication Context
const AuthContext = createContext();

function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(false);

  const login = async (email, password) => {
    setLoading(true);
    try {
      // Simulated API call
      const response = await fetch('/api/login', {
        method: 'POST',
        body: JSON.stringify({ email, password }),
      });
      const userData = await response.json();
      setUser(userData);
    } catch (error) {
      console.error('Login failed:', error);
    } finally {
      setLoading(false);
    }
  };

  const logout = () => {
    setUser(null);
  };

  const value = {
    user,
    loading,
    login,
    logout,
    isAuthenticated: !!user,
  };

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}

// Using the Auth Context
function Dashboard() {
  const { user, logout, isAuthenticated } = useAuth();

  if (!isAuthenticated) {
    return <LoginForm />;
  }

  return (
    <div>
      <h1>Welcome, {user.name}!</h1>
      <button onClick={logout}>Logout</button>
    </div>
  );
}

// Combining multiple contexts
function App() {
  return (
    <AuthProvider>
      <ThemeProvider>
        <Dashboard />
      </ThemeProvider>
    </AuthProvider>
  );
}
```

## Common Use Cases

- **Theme management** - Light/dark mode, color schemes across the app
- **User authentication** - Sharing logged-in user data and auth methods
- **Language/localization** - Switching between different languages
- **Shopping cart** - Managing cart state accessible from any component
- **Notification system** - Showing alerts/toasts from anywhere in the app
- **App settings** - Global configuration options

## Additional Resources

- [React Documentation: Context](https://react.dev/reference/react/useContext)
- [React Context API Guide](https://react.dev/learn/passing-data-deeply-with-context)
- [When to Use Context](https://react.dev/learn/passing-data-deeply-with-context#before-you-use-context)
- [Context vs Redux](https://blog.logrocket.com/use-hooks-and-context-not-react-and-redux/)
