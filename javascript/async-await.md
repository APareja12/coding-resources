# Async/Await in JavaScript

## Overview
Async/await is modern JavaScript syntax that makes working with asynchronous code look and behave more like synchronous code. Built on top of Promises, it provides a cleaner way to handle asynchronous operations without callback hell or long promise chains. The `async` keyword declares an asynchronous function, while `await` pauses execution until a Promise resolves.

## Key Concepts
- **async function** - Always returns a Promise automatically
- **await keyword** - Pauses execution until the Promise settles (resolves or rejects)
- **Error handling** - Use try/catch blocks instead of `.catch()`
- **Sequential vs parallel** - Control whether operations run one after another or simultaneously
- **Top-level await** - Can use await at module level in modern JavaScript

## Code Example
```javascript
// Basic async/await syntax
async function fetchUser(id) {
  const response = await fetch(`https://api.example.com/users/${id}`);
  const user = await response.json();
  return user;
}

// Using the async function
fetchUser(123)
  .then(user => console.log(user))
  .catch(error => console.error(error));

// Error handling with try/catch
async function getUserData(id) {
  try {
    const response = await fetch(`https://api.example.com/users/${id}`);
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const user = await response.json();
    return user;
  } catch (error) {
    console.error('Failed to fetch user:', error);
    throw error; // Re-throw if needed
  }
}

// Sequential operations (one after another)
async function getSequentialData() {
  const user = await fetchUser(1);        // Wait for this
  const posts = await fetchPosts(user.id); // Then wait for this
  const comments = await fetchComments(posts[0].id); // Then this
  
  return { user, posts, comments };
}

// Parallel operations (all at once) - FASTER!
async function getParallelData() {
  const [user, posts, comments] = await Promise.all([
    fetchUser(1),
    fetchPosts(1),
    fetchComments(1)
  ]);
  
  return { user, posts, comments };
}

// Async arrow function
const loadData = async () => {
  const data = await fetch('/api/data');
  return data.json();
};

// Multiple awaits with conditions
async function processOrder(orderId) {
  const order = await getOrder(orderId);
  
  if (order.status === 'pending') {
    const payment = await processPayment(order);
    const inventory = await updateInventory(order);
    const shipping = await scheduleShipping(order);
    
    return { payment, inventory, shipping };
  }
  
  return order;
}
```

## Common Use Cases
- **API calls** - Fetching data from REST APIs or external services
- **Database operations** - Querying databases with asynchronous drivers
- **File operations** - Reading/writing files in Node.js
- **Multiple dependent requests** - When one API call depends on another's result
- **Combining multiple operations** - Using Promise.all() for parallel requests

## Additional Resources
- [MDN: async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN: await operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
- [JavaScript.info: Async/await](https://javascript.info/async-await)
- [Async/Await vs Promises](https://blog.logrocket.com/async-await-in-javascript/)