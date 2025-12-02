# JavaScript Destructuring

## Overview

Destructuring is a JavaScript syntax that allows you to unpack values from arrays or properties from objects into distinct variables. Instead of accessing properties with dot notation or bracket notation repeatedly, destructuring lets you extract multiple values in a single, clean statement. This feature, introduced in ES6, makes code more readable and reduces repetitive code when working with complex data structures.

## Key Concepts

- **Array destructuring** - Extract values from arrays by position
- **Object destructuring** - Extract values from objects by property name
- **Default values** - Provide fallback values if property doesn't exist
- **Rest operator** - Gather remaining elements with `...`
- **Nested destructuring** - Extract values from nested structures
- **Renaming** - Assign destructured values to different variable names

## Code Example

```javascript
// Array Destructuring
const colors = ['red', 'green', 'blue', 'yellow'];

// Old way
const first = colors[0];
const second = colors[1];

// Destructuring way
const [first, second] = colors;
console.log(first); // 'red'
console.log(second); // 'green'

// Skip elements
const [, , third] = colors;
console.log(third); // 'blue'

// Rest operator - gather remaining elements
const [primary, ...otherColors] = colors;
console.log(primary); // 'red'
console.log(otherColors); // ['green', 'blue', 'yellow']

// Object Destructuring
const user = {
  name: 'John Doe',
  age: 30,
  email: 'john@example.com',
  city: 'New York',
};

// Old way
const name = user.name;
const age = user.age;

// Destructuring way
const { name, age } = user;
console.log(name); // 'John Doe'
console.log(age); // 30

// Rename variables during destructuring
const { name: userName, age: userAge } = user;
console.log(userName); // 'John Doe'
console.log(userAge); // 30

// Default values
const { name, country = 'USA' } = user;
console.log(country); // 'USA' (user object doesn't have country property)

// Rest operator with objects
const { name, ...otherDetails } = user;
console.log(name); // 'John Doe'
console.log(otherDetails); // { age: 30, email: 'john@example.com', city: 'New York' }

// Nested Destructuring
const person = {
  name: 'Alice',
  address: {
    street: '123 Main St',
    city: 'Boston',
    coordinates: {
      lat: 42.3601,
      lng: -71.0589,
    },
  },
};

// Extract nested values
const {
  name,
  address: {
    city,
    coordinates: { lat, lng },
  },
} = person;

console.log(name); // 'Alice'
console.log(city); // 'Boston'
console.log(lat); // 42.3601

// Function Parameter Destructuring
// Instead of passing entire object
function displayUser(user) {
  console.log(user.name);
  console.log(user.email);
}

// Destructure in the parameter
function displayUser({ name, email }) {
  console.log(name);
  console.log(email);
}

displayUser({ name: 'Bob', email: 'bob@example.com', age: 25 });

// Swapping Variables
let a = 1;
let b = 2;

// Old way requires temporary variable
let temp = a;
a = b;
b = temp;

// Destructuring way
[a, b] = [b, a];
console.log(a); // 2
console.log(b); // 1

// Practical Examples

// API Response handling
const response = {
  data: {
    users: [
      { id: 1, name: 'User 1' },
      { id: 2, name: 'User 2' },
    ],
  },
  status: 200,
  message: 'Success',
};

const {
  data: { users },
  status,
  message,
} = response;

console.log(users); // Array of users
console.log(status); // 200

// React Props Destructuring
// Instead of props.name, props.onClick, etc.
function Button({ text, onClick, disabled = false }) {
  return (
    <button onClick={onClick} disabled={disabled}>
      {text}
    </button>
  );
}

// Array of objects
const students = [
  { name: 'Alice', grade: 95 },
  { name: 'Bob', grade: 87 },
  { name: 'Charlie', grade: 92 },
];

// Destructure in loop
for (const { name, grade } of students) {
  console.log(`${name}: ${grade}`);
}

// Combining with spread operator
const defaults = { theme: 'light', fontSize: 16 };
const userPrefs = { theme: 'dark' };

const settings = { ...defaults, ...userPrefs };
console.log(settings); // { theme: 'dark', fontSize: 16 }

// Import/Export with destructuring
// Instead of importing everything
import * as utils from './utils';
utils.formatDate();
utils.capitalize();

// Import only what you need
import { formatDate, capitalize } from './utils';
formatDate();
capitalize();
```

## Common Use Cases

- **API responses** - Extract specific fields from complex JSON responses
- **Function parameters** - Make function signatures clearer and more flexible
- **React components** - Destructure props and state for cleaner code
- **Configuration objects** - Pull out only needed settings with defaults
- **Array manipulation** - Swap values, extract first/last elements
- **Module imports** - Import only specific functions from libraries

## Additional Resources

- [MDN: Destructuring Assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- [JavaScript.info: Destructuring](https://javascript.info/destructuring-assignment)
- [ES6 Destructuring Guide](https://www.freecodecamp.org/news/array-and-object-destructuring-in-javascript/)
- [Destructuring in Practice](https://dmitripavlutin.com/javascript-object-destructuring/)
