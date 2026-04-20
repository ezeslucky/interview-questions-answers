# Comprehensive Technical Interview Q&A Guide

## Table of Contents
1. [JavaScript](#javascript)
2. [TypeScript](#typescript)
3. [Golang](#golang)
4. [Rust](#rust)
5. [React.js](#reactjs)
6. [Next.js](#nextjs)
7. [Redux](#redux)
8. [Tailwind CSS](#tailwind-css)
9. [Node.js](#nodejs)
10. [Express.js](#expressjs)
11. [FastAPI](#fastapi)
12. [Actix](#actix)
13. [GraphQL](#graphql)
14. [PostgreSQL](#postgresql)
15. [MongoDB](#mongodb)
16. [Redis](#redis)
17. [Elasticsearch](#elasticsearch)
18. [Kafka](#kafka)
19. [AI/GenAI](#aigenai)
20. [AWS](#aws)
21. [Docker & Kubernetes](#docker--kubernetes)
22. [Web3](#web3)
23. [System Design](#system-design)

---

## JavaScript

### 1. What is the difference between `let`, `const`, and `var`?

**Answer:**
- `var`: Function-scoped, can be redeclared, hoisted with initial value of `undefined`
- `let`: Block-scoped, cannot be redeclared in same scope, hoisted but in temporal dead zone
- `const`: Block-scoped, cannot be reassigned or redeclared, must be initialized at declaration

```javascript
var x = 1; // function-scoped
let y = 2; // block-scoped, reassignable
const z = 3; // block-scoped, not reassignable
```

### 2. Explain closures with an example.

**Answer:** A closure is a function that remembers its outer variables even when executed outside its original scope.

```javascript
function outer() {
  let count = 0;
  return function inner() {
    count++;
    return count;
  };
}
const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2
```

### 3. What is the event loop in JavaScript?

**Answer:** The event loop is a mechanism that handles asynchronous callbacks. It has:
- **Call Stack**: Executes synchronous code
- **Web APIs**: Handle async operations (setTimeout, fetch)
- **Callback Queue**: Holds completed async callbacks
- **Microtask Queue**: Higher priority queue (Promises)

The event loop checks if the call stack is empty, then pushes callbacks from queues to the stack.

### 4. Explain `this` keyword in JavaScript.

**Answer:** `this` refers to the execution context:
- **Global context**: `window` (browser) or `global` (Node)
- **Object method**: The object itself
- **Constructor**: The new instance
- **Arrow functions**: Lexical `this` (inherits from parent)
- **call/apply/bind**: Explicitly set

### 5. What are Promises and async/await?

**Answer:**
- **Promise**: Object representing eventual completion/failure of async operation
- **States**: pending, fulfilled, rejected
- **async/await**: Syntactic sugar over Promises

```javascript
// Promise
fetch('/api/data')
  .then(res => res.json())
  .catch(err => console.error(err));

// async/await
async function getData() {
  try {
    const res = await fetch('/api/data');
    return await res.json();
  } catch (err) {
    console.error(err);
  }
}
```

### 6. What is hoisting?

**Answer:** Hoisting moves variable and function declarations to the top of their scope during compilation.

```javascript
console.log(a); // undefined (hoisted)
var a = 5;

console.log(b); // ReferenceError (TDZ)
let b = 10;

sayHi(); // "Hi" (function hoisted)
function sayHi() { console.log("Hi"); }
```

### 7. Explain prototypal inheritance.

**Answer:** Objects inherit from other objects via prototype chain.

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  return `Hello, ${this.name}`;
};

const p = new Person("John");
p.greet(); // "Hello, John"
```

### 8. What is the difference between `==` and `===`?

**Answer:**
- `==`: Loose equality (performs type coercion)
- `===`: Strict equality (no type coercion)

```javascript
5 == "5"   // true (coerces string to number)
5 === "5"  // false (different types)
```

### 9. Explain debounce and throttle.

**Answer:**
- **Debounce**: Execute function after delay since last call
- **Throttle**: Execute function at most once per interval

```javascript
// Debounce
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

// Throttle
function throttle(fn, limit) {
  let inThrottle;
  return (...args) => {
    if (!inThrottle) {
      fn(...args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}
```

### 10. What are JavaScript modules?

**Answer:** Modules are reusable pieces of code using `import`/`export`.

```javascript
// math.js
export const add = (a, b) => a + b;
export default function multiply(a, b) { return a * b; }

// main.js
import multiply, { add } from './math.js';
```

---

## TypeScript

### 1. What is TypeScript and why use it?

**Answer:** TypeScript is a typed superset of JavaScript that compiles to plain JavaScript.

**Benefits:**
- Static type checking at compile time
- Better IDE support (autocomplete, refactoring)
- Catch errors early
- Better documentation via types
- Improved maintainability

### 2. Difference between `interface` and `type`?

**Answer:**
- **interface**: Can be extended, merged, mainly for objects
- **type**: More flexible, supports unions, primitives, tuples

```typescript
// Interface
interface User {
  id: number;
  name: string;
}
interface Admin extends User {
  role: string;
}

// Type
type User = { id: number; name: string };
type Admin = User & { role: string };
type ID = string | number; // Union (can't do with interface)
```

### 3. What are generics?

**Answer:** Generics create reusable components that work with any type.

```typescript
function identity<T>(arg: T): T {
  return arg;
}
identity<string>("hello");
identity<number>(42);

// Generic interface
interface Container<T> {
  value: T;
}
```

### 4. Explain `any`, `unknown`, `never`.

**Answer:**
- `any`: Disables type checking (avoid using)
- `unknown`: Type-safe version of `any` (needs type guard)
- `never`: Represents impossible values (infinite loop, throws)

```typescript
let val: unknown = "hello";
if (typeof val === "string") {
  val.toUpperCase(); // OK after type guard
}

function throwError(): never {
  throw new Error("Error");
}
```

### 5. What are utility types?

**Answer:** Built-in type transformations.

```typescript
interface User { id: number; name: string; email: string; }

type PartialUser = Partial<User>; // All optional
type ReadonlyUser = Readonly<User>; // All readonly
type UserNoEmail = Omit<User, "email">; // Exclude keys
type UserOnlyEmail = Pick<User, "email">; // Include keys
type NullableUser = Nullable<User>; // Add null to all
```

### 6. What is type narrowing?

**Answer:** Refining types within conditional blocks.

```typescript
function process(value: string | number) {
  if (typeof value === "string") {
    return value.toUpperCase(); // value is string here
  }
  return value.toFixed(2); // value is number here
}

// Using discriminated unions
type Event = 
  | { type: "click"; x: number; y: number }
  | { type: "keydown"; key: string };

function handle(e: Event) {
  if (e.type === "click") {
    console.log(e.x, e.y); // e.key doesn't exist here
  }
}
```

### 7. What are decorators?

**Answer:** Special declarations that add metadata/behavior to classes.

```typescript
function logged(target: any, key: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function(...args: any[]) {
    console.log(`Calling ${key}`);
    return original.apply(this, args);
  };
}

class Service {
  @logged
  getData() { return "data"; }
}
```

### 8. Explain `extends` vs `implements`.

**Answer:**
- `extends`: Inherit from class/interface
- `implements`: Enforce class matches interface

```typescript
interface Animal { name: string; }
class Dog implements Animal {
  name: string;
  constructor(name: string) { this.name = name; }
}

class Labrador extends Dog {
  breed: string = "Labrador";
}
```

---

## Golang

### 1. What makes Go unique?

**Answer:**
- Simple, minimal syntax
- Built-in concurrency (goroutines, channels)
- Fast compilation
- Garbage collected
- Static typing
- No exceptions (error handling via return values)
- Standard library focused

### 2. Explain goroutines and channels.

**Answer:**
- **Goroutine**: Lightweight thread managed by Go runtime
- **Channel**: Typed conduit for communication between goroutines

```go
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        results <- j * 2
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)
    
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }
    
    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs)
}
```

### 3. How does error handling work in Go?

**Answer:** Errors are values returned from functions.

```go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

result, err := divide(10, 0)
if err != nil {
    log.Fatal(err)
}
```

### 4. What is the difference between array and slice?

**Answer:**
- **Array**: Fixed size, value type
- **Slice**: Dynamic size, reference to underlying array

```go
arr := [3]int{1, 2, 3} // Array of size 3
slice := []int{1, 2, 3} // Slice
slice = append(slice, 4) // Can grow
```

### 5. Explain interfaces in Go.

**Answer:** Interfaces define behavior implicitly.

```go
type Speaker interface {
    Speak() string
}

type Dog struct{}
func (d Dog) Speak() string { return "Woof!" }

func makeSound(s Speaker) {
    fmt.Println(s.Speak())
}
```

### 6. What is `defer`?

**Answer:** Defers execution until surrounding function returns.

```go
func readFile() {
    f, _ := os.Open("file.txt")
    defer f.Close() // Executes when function returns
    // ... read file
}
```

### 7. How do you handle concurrency patterns?

**Answer:**

```go
// WaitGroup
var wg sync.WaitGroup
for i := 0; i < 5; i++ {
    wg.Add(1)
    go func(id int) {
        defer wg.Done()
        // work
    }(i)
}
wg.Wait()

// Select for multiple channels
select {
case msg1 := <-ch1:
    fmt.Println(msg1)
case msg2 := <-ch2:
    fmt.Println(msg2)
case <-time.After(time.Second):
    fmt.Println("timeout")
}
```

### 8. What are Go modules?

**Answer:** Dependency management system.

```bash
go mod init myproject
go get github.com/package@v1.0.0
go mod tidy
```

---

## Rust

### 1. What is Rust's ownership system?

**Answer:** Ownership ensures memory safety without garbage collection.

**Rules:**
1. Each value has one owner
2. When owner goes out of scope, value is dropped
3. Borrowing: `&T` (immutable), `&mut T` (mutable, one at a time)

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; // Move, s1 invalid
    let s3 = s2.clone(); // Copy
    
    let r = &s2; // Immutable borrow
    // let m = &mut s2; // Error: can't mutably borrow while immutably borrowed
}
```

### 2. Explain lifetimes.

**Answer:** Lifetimes ensure references are valid.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}

struct Borrowed<'a> {
    part: &'a str,
}
```

### 3. What is the difference between `String` and `&str`?

**Answer:**
- `String`: Owned, heap-allocated, mutable
- `&str`: Borrowed string slice, immutable reference

```rust
let s: String = String::from("hello");
let slice: &str = &s; // Borrow
let literal: &str = "hello"; // String literal
```

### 4. How does error handling work in Rust?

**Answer:** Using `Result` and `Option` types.

```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        return Err("Division by zero".to_string());
    }
    Ok(a / b)
}

match divide(10.0, 2.0) {
    Ok(result) => println!("{}", result),
    Err(e) => println!("Error: {}", e),
}

// Using ? operator
fn process() -> Result<(), String> {
    let result = divide(10.0, 0.0)?; // Propagates error
    Ok(())
}
```

### 5. What are traits?

**Answer:** Traits define shared behavior (like interfaces).

```rust
trait Summary {
    fn summarize(&self) -> String;
    
    fn default_summary(&self) -> String {
        String::from("No summary")
    }
}

struct Article {
    title: String,
}

impl Summary for Article {
    fn summarize(&self) -> String {
        format!("Article: {}", self.title)
    }
}
```

### 6. Explain pattern matching.

**Answer:**

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
}

let msg = Message::Write(String::from("hello"));

match msg {
    Message::Quit => println!("Quit"),
    Message::Move { x, y } => println!("Move to ({}, {})", x, y),
    Message::Write(text) => println!("Write: {}", text),
}

// if let
if let Message::Write(text) = msg {
    println!("{}", text);
}
```

### 7. What is `Arc` and `Mutex`?

**Answer:** Thread-safe shared ownership and mutation.

```rust
use std::sync::{Arc, Mutex};

let data = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let data_clone = Arc::clone(&data);
    handles.push(thread::spawn(move || {
        let mut num = data_clone.lock().unwrap();
        *num += 1;
    }));
}
```

### 8. What is Cargo?

**Answer:** Rust's package manager and build system.

```bash
cargo new project
cargo build
cargo run
cargo test
cargo add dependency
```

---

## React.js

### 1. What is the virtual DOM?

**Answer:** Virtual DOM is a lightweight JavaScript representation of the real DOM. React:
1. Renders to virtual DOM on state change
2. Compares with previous virtual DOM (diffing)
3. Updates only changed parts in real DOM (reconciliation)

### 2. Explain React hooks.

**Answer:** Hooks let you use state and lifecycle in functional components.

```jsx
// useState
const [count, setCount] = useState(0);

// useEffect
useEffect(() => {
  document.title = `Count: ${count}`;
  return () => { /* cleanup */ };
}, [count]); // Dependency array

// useContext
const theme = useContext(ThemeContext);

// useReducer
const [state, dispatch] = useReducer(reducer, initialState);

// useMemo & useCallback
const memoized = useMemo(() => expensive(), [deps]);
const memoizedCb = useCallback(() => {}, [deps]);
```

### 3. What is prop drilling and how to avoid it?

**Answer:** Prop drilling is passing data through multiple component levels.

**Solutions:**
- Context API
- State management (Redux, Zustand)
- Component composition

```jsx
// Context API
const ThemeContext = createContext();

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Child />
    </ThemeContext.Provider>
  );
}
```

### 4. Controlled vs uncontrolled components?

**Answer:**
- **Controlled**: Form data handled by React state
- **Uncontrolled**: Form data handled by DOM (use ref)

```jsx
// Controlled
<input value={value} onChange={e => setValue(e.target.value)} />

// Uncontrolled
const inputRef = useRef();
<input ref={inputRef} />
// Access: inputRef.current.value
```

### 5. What are keys in React lists?

**Answer:** Keys help React identify which items changed.

```jsx
// Good: unique, stable ID
{items.map(item => <li key={item.id}>{item.name}</li>)}

// Bad: using index (causes issues with reordering)
{items.map((item, i) => <li key={i}>{item.name}</li>)}
```

### 6. Explain React lifecycle methods.

**Answer:**
- **Mounting**: `constructor` → `render` → `componentDidMount`
- **Updating**: `shouldComponentUpdate` → `render` → `componentDidUpdate`
- **Unmounting**: `componentWillUnmount`

**Hooks equivalent:**
```jsx
useEffect(() => {
  // componentDidMount + componentDidUpdate
  return () => {
    // componentWillUnmount
  };
}, [deps]);
```

### 7. What is React.memo?

**Answer:** Memoizes component to prevent unnecessary re-renders.

```jsx
const MemoizedChild = React.memo(({ value }) => {
  console.log("Rendered");
  return <div>{value}</div>;
});

// Only re-renders if value changes
```

### 8. Custom hooks?

**Answer:** Reusable hook logic.

```jsx
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });
  
  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);
  
  return [value, setValue];
}
```

---

## Next.js

### 1. What is Next.js?

**Answer:** React framework with SSR, SSG, API routes, and more.

**Features:**
- File-based routing
- Server-side rendering (SSR)
- Static site generation (SSG)
- API routes
- Image optimization
- Automatic code splitting

### 2. Explain rendering methods in Next.js.

**Answer:**
- **CSR**: `useEffect` fetch, client-side
- **SSG**: `getStaticProps` - build time
- **SSR**: `getServerSideProps` - request time
- **ISR**: `revalidate` in `getStaticProps` - incremental
- **Client Components**: `"use client"` - interactive
- **Server Components**: Default in App Router - no client JS

```jsx
// SSG
export async function getStaticProps() {
  return { props: { data } };
}

// SSR
export async function getServerSideProps() {
  return { props: { data } };
}
```

### 3. App Router vs Pages Router?

**Answer:**
- **Pages Router**: `pages/`, `getStaticProps`, `getServerSideProps`
- **App Router**: `app/`, React Server Components, Server Actions, layouts

```jsx
// App Router structure
app/
  layout.tsx
  page.tsx
  blog/
    [slug]/
      page.tsx
```

### 4. What are Server Actions?

**Answer:** Server-side functions called from client components.

```jsx
// Server Action
async function createUser(formData: FormData) {
  'use server';
  const name = formData.get('name');
  // DB operation
}

// Usage in form
<form action={createUser}>
  <input name="name" />
  <button type="submit">Create</button>
</form>
```

### 5. How does Next.js handle routing?

**Answer:** File-based routing.

```
app/
  page.tsx           -> /
  about/page.tsx     -> /about
  blog/[slug]/page.tsx -> /blog/:slug
  [...catchall]/page.tsx -> /*
```

### 6. What is Image component optimization?

**Answer:**

```jsx
<Image 
  src="/image.jpg" 
  alt="Description"
  width={500}
  height={300}
  priority  // For LCP images
  quality={75}
/>
```

Benefits: Auto WebP/AVIF, lazy loading, size optimization, CLS prevention.

---

## Redux

### 1. What is Redux?

**Answer:** Predictable state management library.

**Core concepts:**
- **Store**: Single source of truth
- **Actions**: Plain objects describing what happened
- **Reducers**: Pure functions that update state

### 2. Explain Redux Toolkit.

**Answer:** Official Redux package with simplified API.

```jsx
import { createSlice, configureStore } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => { state.value += 1; }, // Immer allows mutation
    decrement: state => { state.value -= 1; }
  }
});

const store = configureStore({
  reducer: { counter: counterSlice.reducer }
});

// Usage
dispatch(counterSlice.actions.increment());
```

### 3. What is Redux Thunk?

**Answer:** Middleware for async actions.

```jsx
const fetchUser = (id) => async (dispatch, getState) => {
  dispatch({ type: 'FETCH_START' });
  try {
    const res = await fetch(`/api/users/${id}`);
    const user = await res.json();
    dispatch({ type: 'FETCH_SUCCESS', payload: user });
  } catch (err) {
    dispatch({ type: 'FETCH_ERROR', error: err });
  }
};
```

### 4. When to use Redux?

**Answer:** Use when:
- Multiple components need same data
- Global state (auth, theme)
- Complex state logic
- Frequent state updates

Don't use for:
- Local component state
- Simple prop drilling
- Form state (use React Hook Form)

---

## Tailwind CSS

### 1. What is Tailwind CSS?

**Answer:** Utility-first CSS framework.

```html
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
  Button
</button>
```

### 2. How to customize Tailwind?

**Answer:**

```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: '#1DA1F2',
      },
      spacing: {
        '128': '32rem',
      }
    }
  },
  plugins: [require('@tailwindcss/forms')],
}
```

### 3. What are responsive utilities?

**Answer:**

```html
<div class="
  text-sm 
  md:text-base 
  lg:text-lg 
  xl:text-xl
  grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4
">
```

Breakpoints: `sm:640px`, `md:768px`, `lg:1024px`, `xl:1280px`, `2xl:1536px`

### 4. How to handle dark mode?

**Answer:**

```js
// tailwind.config.js
module.exports = {
  darkMode: 'class',
}
```

```html
<div class="bg-white dark:bg-gray-800 text-black dark:text-white">
```

---

## Node.js

### 1. What is Node.js?

**Answer:** JavaScript runtime built on Chrome's V8 engine.

**Features:**
- Event-driven, non-blocking I/O
- Single-threaded event loop
- npm package manager
- CommonJS modules (`require`/`module.exports`)
- ES Modules support (`import`/`export`)

### 2. Explain the event loop phases.

**Answer:**
1. **Timers**: setTimeout, setInterval callbacks
2. **Pending callbacks**: I/O callbacks
3. **Idle/prepare**: Internal use
4. **Poll**: Retrieve new I/O events
5. **Check**: setImmediate callbacks
6. **Close callbacks**: close event handlers

### 3. What is the difference between `process.nextTick` and `setImmediate`?

**Answer:**
- `process.nextTick`: Executes before event loop continues (higher priority)
- `setImmediate`: Executes in check phase of event loop

```javascript
setTimeout(() => console.log('timeout'), 0);
setImmediate(() => console.log('immediate'));
process.nextTick(() => console.log('nextTick'));
// Output: nextTick, immediate, timeout (usually)
```

### 4. How to handle errors in Node.js?

**Answer:**

```javascript
// Async/await with try-catch
async function getData() {
  try {
    const data = await fs.promises.readFile('file.txt');
  } catch (err) {
    console.error(err);
  }
}

// Error-first callbacks
fs.readFile('file.txt', (err, data) => {
  if (err) return callback(err);
  callback(null, data);
});

// Uncaught exceptions
process.on('uncaughtException', (err) => {
  console.error(err);
  process.exit(1);
});
```

### 5. What is the Cluster module?

**Answer:** Create child processes to utilize multiple CPU cores.

```javascript
const cluster = require('cluster');
const os = require('os');

if (cluster.isPrimary) {
  for (let i = 0; i < os.cpus().length; i++) {
    cluster.fork();
  }
} else {
  require('./app'); // Worker runs server
}
```

### 6. Explain streams in Node.js.

**Answer:**

```javascript
const { Readable, Writable, Transform } = require('stream');

// Readable stream
const readable = new Readable({
  read() { this.push('data'); this.push(null); }
});

// Writable stream
const writable = new Writable({
  write(chunk, enc, next) { console.log(chunk.toString()); next(); }
});

// Pipe
readable.pipe(writable);

// File streams
fs.createReadStream('input.txt').pipe(fs.createWriteStream('output.txt'));
```

### 7. What is libuv?

**Answer:** C library providing async I/O, thread pool, event loop.

---

## Express.js

### 1. What is Express.js?

**Answer:** Minimal web framework for Node.js.

### 2. How to create middleware?

**Answer:**

```javascript
// Custom middleware
const authMiddleware = (req, res, next) => {
  const token = req.headers.authorization;
  if (!token) return res.status(401).json({ error: 'Unauthorized' });
  req.user = verifyToken(token);
  next();
};

app.use('/api', authMiddleware);
```

### 3. Explain route parameters and query strings.

**Answer:**

```javascript
// Route params
app.get('/users/:id', (req, res) => {
  const { id } = req.params;
});

// Query params: /users?sort=asc&limit=10
app.get('/users', (req, res) => {
  const { sort, limit } = req.query;
});
```

### 4. How to handle errors in Express?

**Answer:**

```javascript
// Error handling middleware (4 params)
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something broke!' });
});

// Async wrapper
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

app.get('/users', asyncHandler(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));
```

---

## FastAPI

### 1. What is FastAPI?

**Answer:** Modern Python web framework for building APIs.

**Features:**
- Fast (Starlette + Pydantic)
- Auto interactive docs (Swagger, ReDoc)
- Type hints validation
- Async support
- Dependency injection

### 2. How to create endpoints?

**Answer:**

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float

@app.get("/")
async def root():
    return {"message": "Hello"}

@app.get("/items/{item_id}")
async def get_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}

@app.post("/items/")
async def create_item(item: Item):
    return item
```

### 3. What is dependency injection?

**Answer:**

```python
from fastapi import Depends, HTTPException

async def get_db():
    db = Database()
    try:
        yield db
    finally:
        db.close()

async def get_current_user(token: str, db = Depends(get_db)):
    user = db.get_user(token)
    if not user:
        raise HTTPException(401)
    return user

@app.get("/users/me")
async def read_user(current_user: User = Depends(get_current_user)):
    return current_user
```

### 4. How to handle async in FastAPI?

**Answer:**

```python
@app.get("/items/")
async def read_items():
    items = await db.query("SELECT * FROM items")
    return items

# Background tasks
async def send_email(email: str):
    # Send email

@app.post("/checkout/")
async def checkout(background_tasks: BackgroundTasks):
    background_tasks.add_task(send_email, "user@example.com")
    return {"message": "Processing"}
```

---

## Actix

### 1. What is Actix?

**Answer:** Actor-based web framework for Rust.

### 2. Explain the actor model.

**Answer:**

```rust
use actix::prelude::*;

struct MyActor;

impl Actor for MyActor {
    type Context = Context<Self>;
}

// Message
struct Msg(String);
impl Message for Msg {
    type Result = String;
}

// Handler
impl Handler<Msg> for MyActor {
    type Result = String;
    fn handle(&mut self, msg: Msg, _ctx: &mut Self::Context) -> Self::Result {
        msg.0
    }
}
```

### 3. How to create HTTP server with Actix-web?

**Answer:**

```rust
use actix_web::{web, App, HttpServer, HttpResponse, get};

#[get("/{id}/info")]
async fn index(path: web::Path<(u32,)>) -> HttpResponse {
    HttpResponse::Ok().json(format!("Info for {}", path.0))
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new().service(index)
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}
```

---

## GraphQL

### 1. What is GraphQL?

**Answer:** Query language and runtime for APIs.

**Benefits:**
- Fetch exactly what you need
- Single endpoint
- Strong typing
- Introspection

### 2. GraphQL vs REST?

**Answer:**
| GraphQL | REST |
|---------|------|
| Single endpoint | Multiple endpoints |
| Client specifies response | Server specifies response |
| No over/under-fetching | Over/under-fetching common |
| Versioning not needed | URL versioning common |

### 3. What are queries and mutations?

**Answer:**

```graphql
# Query (read)
query {
  user(id: "1") {
    id
    name
    posts {
      title
    }
  }
}

# Mutation (write)
mutation {
  createUser(name: "John", email: "john@example.com") {
    id
    name
  }
}

# Subscription (real-time)
subscription {
  messageAdded(roomId: "1") {
    text
    sender
  }
}
```

### 4. What is a resolver?

**Answer:** Function that returns data for a field.

```javascript
const resolvers = {
  Query: {
    user: (_, { id }, context) => {
      return context.db.user.findById(id);
    }
  },
  Mutation: {
    createUser: (_, args, context) => {
      return context.db.user.create(args);
    }
  }
};
```

### 5. What is schema stitching / federation?

**Answer:** Combining multiple GraphQL schemas into one.

---

## PostgreSQL

### 1. What makes PostgreSQL unique?

**Answer:**
- ACID compliant
- JSON/JSONB support
- Full-text search
- Window functions
- CTEs (Common Table Expressions)
- Extensions (PostGIS, pgcrypto)
- MVCC (Multi-version Concurrency Control)

### 2. What is the difference between JSON and JSONB?

**Answer:**
- `JSON`: Stored as text, slower queries
- `JSONB`: Binary format, supports indexing, faster queries

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  data JSONB
);

-- Query JSONB
SELECT * FROM users WHERE data->>'name' = 'John';

-- Create index
CREATE INDEX idx_data ON users USING GIN (data);
```

### 3. Explain indexing strategies.

**Answer:**

```sql
-- B-Tree (default)
CREATE INDEX idx_email ON users(email);

-- GIN (JSONB, arrays, full-text)
CREATE INDEX idx_data ON users USING GIN (data);

-- GiST (geometric, full-text)
CREATE INDEX idx_location ON users USING GIST (location);

-- Composite
CREATE INDEX idx_name_email ON users(last_name, first_name);

-- Partial
CREATE INDEX idx_active ON users(email) WHERE active = true;
```

### 4. What are CTEs?

**Answer:** Common Table Expressions (WITH clause).

```sql
WITH RECURSIVE cte AS (
  SELECT id, parent_id, 1 as level
  FROM categories WHERE parent_id IS NULL
  UNION ALL
  SELECT c.id, c.parent_id, cte.level + 1
  FROM categories c
  JOIN cte ON c.parent_id = cte.id
)
SELECT * FROM cte;
```

### 5. Explain transactions and isolation levels.

**Answer:**

```sql
BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

-- Isolation levels:
-- Read Committed (default)
-- Repeatable Read
-- Serializable
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### 6. What is MVCC?

**Answer:** Multi-Version Concurrency Control allows readers and writers to access data simultaneously without blocking.

---

## MongoDB

### 1. What is MongoDB?

**Answer:** Document-oriented NoSQL database.

**Features:**
- BSON (Binary JSON) format
- Schema-less
- Horizontal scaling (sharding)
- Replica sets
- Aggregation framework

### 2. MongoDB vs PostgreSQL?

**Answer:**
| MongoDB | PostgreSQL |
|---------|------------|
| Document store | Relational |
| Flexible schema | Fixed schema |
| Horizontal scaling | Vertical scaling |
| JSON documents | Tables with rows |
| No joins (usually) | Joins support |

### 3. How to create indexes?

**Answer:**

```javascript
db.users.createIndex({ email: 1 }, { unique: true });
db.users.createIndex({ name: 1, age: -1 }); // Compound
db.users.createIndex({ location: "2dsphere" }); // Geospatial
db.users.createIndex({ bio: "text" }); // Text search
```

### 4. Explain aggregation pipeline.

**Answer:**

```javascript
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { 
      _id: "$customerId", 
      total: { $sum: "$amount" },
      count: { $sum: 1 }
    } 
  },
  { $sort: { total: -1 } },
  { $limit: 10 }
]);
```

### 5. What is sharding?

**Answer:** Horizontal distribution of data across multiple servers.

**Components:**
- **Shard**: Stores subset of data
- **Mongos**: Query router
- **Config servers**: Store metadata

---

## Redis

### 1. What is Redis?

**Answer:** In-memory data structure store.

**Use cases:**
- Caching
- Session storage
- Pub/Sub messaging
- Leaderboards
- Rate limiting

### 2. What data structures does Redis support?

**Answer:**
- Strings
- Lists
- Sets
- Sorted Sets
- Hashes
- Streams
- HyperLogLog
- Geospatial

### 3. How to use Redis for caching?

**Answer:**

```python
import redis
r = redis.Redis()

# Cache with TTL
r.setex("user:1", 3600, json.dumps(user_data))

# Get from cache
data = r.get("user:1")
if data:
    return json.loads(data)
```

### 4. Explain Redis persistence.

**Answer:**
- **RDB**: Snapshot at intervals
- **AOF**: Append-only file (every write)
- **Both**: Recommended for durability

### 5. How to implement rate limiting?

**Answer:**

```python
def is_rate_limited(user_id, limit=100, window=3600):
    key = f"rate:{user_id}"
    current = r.incr(key)
    if current == 1:
        r.expire(key, window)
    return current > limit
```

---

## Elasticsearch

### 1. What is Elasticsearch?

**Answer:** Distributed search and analytics engine.

### 2. Explain inverted index.

**Answer:** Data structure mapping terms to documents containing them.

```
Term -> Document IDs
"hello" -> [1, 3, 5]
"world" -> [2, 3, 4]
```

### 3. How to create a search query?

**Answer:**

```json
GET /products/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "name": "laptop" }}
      ],
      "filter": [
        { "range": { "price": { "gte": 500, "lte": 1000 }}}
      ]
    }
  },
  "aggs": {
    "brands": {
      "terms": { "field": "brand" }
    }
  }
}
```

### 4. What is relevance scoring?

**Answer:** BM25 algorithm based on term frequency, inverse document frequency.

---

## Kafka

### 1. What is Kafka?

**Answer:** Distributed event streaming platform.

**Components:**
- **Producer**: Sends messages
- **Consumer**: Reads messages
- **Broker**: Kafka server
- **Topic**: Message category
- **Partition**: Topic subdivision
- **Consumer Group**: Consumers sharing load

### 2. Explain Kafka architecture.

**Answer:**
- Topics are partitioned for parallelism
- Partitions are replicated across brokers
- Consumers in group each get subset of partitions
- Messages are immutable and ordered within partition

### 3. What is exactly-once semantics?

**Answer:** Using idempotent producer and transactional API.

```java
props.put("enable.idempotence", "true");
props.put("isolation.level", "read_committed");
```

### 4. How to ensure message ordering?

**Answer:**
- Messages ordered within partition
- Use same key for related messages (goes to same partition)

---

## AI / GenAI

### 1. What is RAG?

**Answer:** Retrieval-Augmented Generation combines retrieval with LLM generation.

**Process:**
1. User query
2. Retrieve relevant documents from vector DB
3. Augment prompt with retrieved context
4. LLM generates response

```python
from langchain.vectorstores import FAISS
from langchain.chains import RetrievalQA

vectorstore = FAISS.from_documents(docs, embeddings)
qa = RetrievalQA.from_chain_type(
    llm, 
    retriever=vectorstore.as_retriever()
)
```

### 2. What are LangChain and LangGraph?

**Answer:**
- **LangChain**: Framework for LLM applications (chains, agents, memory)
- **LangGraph**: Extension for building stateful, multi-actor applications with cycles

```python
from langchain.agents import AgentExecutor, create_openai_functions_agent
from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory(memory_key="chat_history")
agent = create_openai_functions_agent(llm, tools, prompt)
executor = AgentExecutor.from_agent_and_tools(
    agent, tools, memory=memory, verbose=True
)
```

### 3. What are AI Agents?

**Answer:** Autonomous systems that can:
- Perceive environment
- Make decisions
- Take actions
- Learn from feedback

**Types:**
- Simple reflex agents
- Model-based agents
- Goal-based agents
- Learning agents

### 4. How to fine-tune an LLM?

**Answer:**

```python
from transformers import AutoModelForCausalLM, TrainingArguments, Trainer

model = AutoModelForCausalLM.from_pretrained("gpt2")
training_args = TrainingArguments(
    output_dir="./results",
    num_train_epochs=3,
    per_device_train_batch_size=4,
)

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
)
trainer.train()
```

### 5. What is prompt engineering?

**Answer:** Crafting effective prompts for better LLM outputs.

**Techniques:**
- Zero-shot: Direct question
- Few-shot: Include examples
- Chain-of-thought: "Let's think step by step"
- Role prompting: "You are an expert..."

### 6. What are embeddings?

**Answer:** Vector representations of text capturing semantic meaning.

```python
from sentence_transformers import SentenceTransformer

model = SentenceTransformer('all-MiniLM-L6-v2')
embedding = model.encode("Hello world")
print(embedding.shape)  # (384,)
```

---

## AWS

### 1. Explain core AWS services.

**Answer:**
- **EC2**: Virtual servers
- **S3**: Object storage
- **Lambda**: Serverless compute
- **RDS**: Managed databases
- **Route53**: DNS service
- **CloudFront**: CDN
- **IAM**: Identity management
- **VPC**: Virtual network

### 2. What is Lambda and when to use it?

**Answer:** Serverless compute running code without managing servers.

**Use when:**
- Event-driven workloads
- Sporadic traffic
- Short-running tasks (<15 min)
- Microservices

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Hello')
    }
```

### 3. Explain S3 storage classes.

**Answer:**
- **Standard**: Frequent access
- **IA (Infrequent Access)**: Less frequent
- **Glacier**: Archival (hours to retrieve)
- **Deep Archive**: Long-term archival

### 4. What is CloudFormation?

**Answer:** Infrastructure as Code (IaC) service.

```yaml
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-bucket
```

### 5. How to secure AWS resources?

**Answer:**
- IAM policies (least privilege)
- Security groups (firewall)
- VPC endpoints (private access)
- KMS (encryption)
- CloudTrail (audit logging)

---

## Docker & Kubernetes

### 1. What is Docker?

**Answer:** Containerization platform.

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

```bash
docker build -t myapp .
docker run -p 3000:3000 myapp
docker-compose up
```

### 2. Docker vs Virtual Machine?

**Answer:**
| Docker | VM |
|--------|-----|
| Shares host OS kernel | Full OS per VM |
| Lightweight, fast boot | Heavy, slow boot |
| Process isolation | Hardware isolation |

### 3. What is Kubernetes?

**Answer:** Container orchestration platform.

**Components:**
- **Pod**: Smallest unit (one or more containers)
- **Deployment**: Manages replica sets
- **Service**: Network abstraction
- **ConfigMap**: Configuration
- **Secret**: Sensitive data
- **Ingress**: External access

### 4. Explain Kubernetes deployment.

**Answer:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 3000
```

### 5. What is Helm?

**Answer:** Kubernetes package manager.

```bash
helm create mychart
helm install myrelease ./mychart
helm upgrade myrelease ./mychart
```

---

## Web3

### 1. What is blockchain?

**Answer:** Distributed ledger technology with:
- Decentralization
- Immutability
- Transparency
- Consensus mechanisms (PoW, PoS)

### 2. Explain smart contracts.

**Answer:** Self-executing code on blockchain.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    uint256 private value;
    
    function set(uint256 _value) public {
        value = _value;
    }
    
    function get() public view returns (uint256) {
        return value;
    }
}
```

### 3. What is Truffle/Hardhat?

**Answer:** Development frameworks for Ethereum.

```javascript
// Hardhat config
module.exports = {
  solidity: "0.8.19",
  networks: {
    hardhat: {},
    goerli: {
      url: process.env.ALCHEMY_URL,
      accounts: [process.env.PRIVATE_KEY]
    }
  }
};
```

### 4. What is IPFS?

**Answer:** InterPlanetary File System - decentralized storage.

```bash
ipfs add file.txt
ipfs get <hash>
```

### 5. What are NFTs?

**Answer:** Non-Fungible Tokens - unique digital assets.

```solidity
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract MyNFT is ERC721 {
    constructor() ERC721("MyNFT", "MNFT") {}
    
    function mint(address to, uint256 tokenId) public {
        _mint(to, tokenId);
    }
}
```

### 6. What is Ganache?

**Answer:** Personal blockchain for testing.

```bash
ganache-cli --port 8545
```

---

## System Design

### 1. Design a URL shortener (like bit.ly)

**Answer:**

**Requirements:**
- Shorten long URLs
- Redirect to original URL
- Custom aliases
- Analytics

**Design:**
```
Client -> Load Balancer -> API Service -> Database
                              |
                         Cache (Redis)
```

**Key decisions:**
- **ID generation**: Base62 encoding of auto-increment ID or UUID
- **Database**: Key-value store (Redis) for cache, SQL for persistence
- **Hash function**: MD5/SHA then truncate, or distributed ID generator

**API:**
```
POST /shorten { url: "long-url" } -> { short: "abc123" }
GET /:short -> 302 redirect
```

### 2. Design a chat application (like WhatsApp)

**Answer:**

**Requirements:**
- Real-time messaging
- Group chats
- Message history
- Read receipts
- Push notifications

**Design:**
```
Client <-> WebSocket Server <-> Message Queue (Kafka)
                                   |
                              Message Store (Cassandra/MongoDB)
                              Cache (Redis) for online users
```

**Key decisions:**
- **Protocol**: WebSocket for real-time, MQTT for mobile
- **Message storage**: Write-heavy, use NoSQL
- **Scaling**: Shard by user ID, consistent hashing

### 3. Design a video streaming service (like Netflix)

**Answer:**

**Requirements:**
- Video upload/encoding
- Streaming at different qualities
- Low latency
- Global distribution

**Design:**
```
Upload -> Transcoding Service -> CDN (CloudFront)
                                  |
                              Client (HLS/DASH)
```

**Key decisions:**
- **Video format**: HLS or DASH for adaptive streaming
- **CDN**: Edge caching for low latency
- **Database**: Metadata in SQL, analytics in NoSQL

### 4. Design a rate limiter

**Answer:**

**Algorithms:**
- **Token Bucket**: Tokens added at fixed rate
- **Leaky Bucket**: Fixed output rate
- **Sliding Window**: Track requests in time window

**Implementation (Redis):**
```python
def is_allowed(user_id, limit, window):
    key = f"rate:{user_id}"
    current = redis.incr(key)
    if current == 1:
        redis.expire(key, window)
    return current <= limit
```

### 5. Design a distributed cache

**Answer:**

**Requirements:**
- Low latency reads
- High availability
- Consistency options

**Design:**
```
Client -> Load Balancer -> Cache Cluster (Redis Cluster)
                              |
                         Write-through to DB
```

**Key decisions:**
- **Eviction**: LRU, LFU, TTL
- **Consistency**: Write-through, write-back, or cache-aside
- **Scaling**: Consistent hashing, sharding

### 6. Explain CAP theorem.

**Answer:** You can only have 2 of 3:
- **Consistency**: All nodes see same data
- **Availability**: Every request gets response
- **Partition Tolerance**: System works despite network failures

**Examples:**
- **CP**: MongoDB, HBase (choose consistency over availability)
- **AP**: Cassandra, DynamoDB (choose availability over consistency)
- **CA**: Traditional RDBMS (no partition tolerance)

### 7. What is load balancing?

**Answer:** Distributing traffic across servers.

**Algorithms:**
- Round Robin
- Least Connections
- IP Hash
- Weighted

**Tools:** Nginx, HAProxy, AWS ALB

### 8. Explain database sharding.

**Answer:** Horizontal partitioning of data.

**Strategies:**
- **Key-based**: Hash(key) % num_shards
- **Range-based**: User IDs 1-1M on shard 1
- **Directory-based**: Lookup service

**Challenges:**
- Cross-shard joins
- Rebalancing
- Distributed transactions

---

## Behavioral Questions

### 1. Tell me about a challenging technical problem you solved.

**Answer Structure (STAR):**
- **S**ituation: Set context
- **T**ask: Your responsibility
- **A**ction: What you did
- **R**esult: Outcome with metrics

### 2. How do you handle disagreements?

**Answer:** Focus on data, propose experiments, escalate when needed.

### 3. Why do you want to work here?

**Answer:** Research company, align with your goals, mention specific projects.

---

## Quick Reference

### Big-O Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Array access | O(1) | O(n) |
| Hash map get/set | O(1) avg | O(n) |
| Binary search | O(log n) | O(1) |
| Merge sort | O(n log n) | O(n) |
| DFS/BFS | O(V + E) | O(V) |

### HTTP Status Codes

- 200: OK
- 201: Created
- 204: No Content
- 301: Moved Permanently
- 302: Found
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error
- 502: Bad Gateway
- 503: Service Unavailable

### Common Design Patterns

- **Singleton**: One instance
- **Factory**: Object creation
- **Observer**: Event subscription
- **Strategy**: Interchangeable algorithms
- **Decorator**: Add behavior dynamically
- **Repository**: Data access abstraction
