# JavaScript Interview Questions and Answers Guide

## Core JavaScript Concepts

### 1. Explain Event Delegation

Event delegation is a technique where you attach an event listener to a parent element to handle events on its children.

```javascript
// Instead of adding event listeners to each button
document.getElementById('parent').addEventListener('click', function(e) {
    if (e.target.className === 'btn') {
        console.log('Button clicked:', e.target.textContent);
    }
});
```

### 2. Explain Closure and Its Use Cases

A closure is a function that has access to variables in its outer (enclosing) lexical scope, even after the outer function has returned.

```javascript
function createCounter() {
    let count = 0;
    return {
        increment: function() { return ++count; },
        getCount: function() { return count; }
    };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount()); // 2
```

Use cases:
- Data privacy
- Function factories
- Partial application and currying

### 3. Explain 'this' Keyword

The value of 'this' depends on how a function is called:

```javascript
// Global context
console.log(this); // window (browser) or global (Node.js)

// Object method
const obj = {
    name: 'Object',
    sayName() {
        console.log(this.name);
    }
};
obj.sayName(); // 'Object'

// Constructor function
function Person(name) {
    this.name = name;
}
const person = new Person('John');
console.log(person.name); // 'John'

// Arrow functions
const arrowFunc = () => {
    console.log(this);
};
// 'this' is inherited from the enclosing scope
```

### 4. Explain Promise and Async/Await

```javascript
// Promise example
function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const data = { id: 1, name: 'John' };
            resolve(data);
            // reject(new Error('Failed to fetch'));
        }, 1000);
    });
}

// Using Promise
fetchData()
    .then(data => console.log(data))
    .catch(error => console.error(error));

// Using Async/Await
async function getData() {
    try {
        const data = await fetchData();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}
```

### 5. Explain Prototypal Inheritance

```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    console.log(`${this.name} makes a sound.`);
};

function Dog(name) {
    Animal.call(this, name);
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
    console.log('Woof!');
};

const dog = new Dog('Rex');
dog.speak(); // "Rex makes a sound."
dog.bark(); // "Woof!"
```

### 6. Explain Hoisting

```javascript
console.log(x); // undefined
var x = 5;

// Function declarations are hoisted with their definition
sayHello(); // "Hello!"
function sayHello() {
    console.log('Hello!');
}

// Function expressions are not hoisted with their definition
sayHi(); // TypeError: sayHi is not a function
var sayHi = function() {
    console.log('Hi!');
};
```

### 7. Explain Event Loop

```javascript
console.log('Start');

setTimeout(() => {
    console.log('Timeout');
}, 0);

Promise.resolve()
    .then(() => console.log('Promise'));

console.log('End');

// Output:
// Start
// End
// Promise
// Timeout
```

### 8. Debounce and Throttle

```javascript
// Debounce
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => {
            func.apply(this, args);
        }, delay);
    };
}

// Throttle
function throttle(func, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}
```

### 9. Deep Clone vs Shallow Clone

```javascript
// Shallow Clone
const original = { a: 1, b: { c: 2 } };
const shallow = { ...original };
original.b.c = 3;
console.log(shallow.b.c); // 3

// Deep Clone
const deep = JSON.parse(JSON.stringify(original));
original.b.c = 4;
console.log(deep.b.c); // 3

// Custom Deep Clone
function deepClone(obj) {
    if (obj === null || typeof obj !== 'object') return obj;
    const copy = Array.isArray(obj) ? [] : {};
    
    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
            copy[key] = deepClone(obj[key]);
        }
    }
    return copy;
}
```

### 10. Memory Leaks and Prevention

Common causes:
```javascript
// 1. Global variables
window.data = { ... }; // Never cleaned up

// 2. Forgotten event listeners
element.addEventListener('click', handler); // Should be removed when not needed
element.removeEventListener('click', handler);

// 3. Closures holding references
function createLeak() {
    const largeData = new Array(1000000);
    return function() {
        console.log(largeData.length);
    };
}
```

### 11. ES6+ Features

```javascript
// Destructuring
const { name, age } = person;
const [first, ...rest] = array;

// Template Literals
const message = `Hello ${name}!`;

// Arrow Functions
const add = (a, b) => a + b;

// Class Syntax
class Person {
    constructor(name) {
        this.name = name;
    }
    sayName() {
        console.log(this.name);
    }
}

// Modules
export const helper = {};
import { helper } from './helper';

// Rest/Spread
const sum = (...numbers) => numbers.reduce((a, b) => a + b);
const arr = [...otherArr, 4, 5, 6];
```

## Best Practices

1. Use strict mode: `'use strict';`
2. Avoid global variables
3. Use const and let instead of var
4. Use === instead of ==
5. Handle errors properly
6. Use meaningful variable names
7. Write modular code
8. Use modern ES6+ features
9. Write unit tests
10. Document your code

## Common Coding Challenges

```javascript
// 1. FizzBuzz
function fizzBuzz(n) {
    for (let i = 1; i <= n; i++) {
        let output = '';
        if (i % 3 === 0) output += 'Fizz';
        if (i % 5 === 0) output += 'Buzz';
        console.log(output || i);
    }
}

// 2. Palindrome Check
function isPalindrome(str) {
    str = str.toLowerCase().replace(/[^a-z0-9]/g, '');
    return str === str.split('').reverse().join('');
}

// 3. Fibonacci Sequence
function fibonacci(n) {
    const result = [0, 1];
    for (let i = 2; i < n; i++) {
        result[i] = result[i - 1] + result[i - 2];
    }
    return result;
}
```

## Performance Optimization

1. Minimize DOM manipulation
2. Use appropriate data structures
3. Optimize loops
4. Use Web Workers for heavy computations
5. Implement caching when appropriate
6. Use debounce/throttle for frequent events
7. Load scripts asynchronously
8. Optimize images and assets
9. Use code splitting
10. Implement lazy loading

## Testing

```javascript
// Jest Example
describe('Calculator', () => {
    test('adds two numbers', () => {
        expect(add(2, 3)).toBe(5);
    });
    
    test('subtracts two numbers', () => {
        expect(subtract(5, 3)).toBe(2);
    });
});
```

## Additional Resources

1. MDN Web Docs
2. JavaScript.info
3. You Don't Know JS book series
4. Clean Code JavaScript
5. JavaScript Design Patterns
