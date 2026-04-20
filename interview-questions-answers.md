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

### 11. What is the spread operator and rest parameters?

**Answer:**
- **Spread**: Expands iterables into individual elements
- **Rest**: Collects multiple elements into an array

```javascript
// Spread
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]
const obj2 = { ...obj1, c: 3 };

// Rest
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
```

### 12. Explain map, filter, and reduce.

**Answer:**

```javascript
// Map - transform each element
[1, 2, 3].map(x => x * 2); // [2, 4, 6]

// Filter - keep elements matching condition
[1, 2, 3, 4].filter(x => x > 2); // [3, 4]

// Reduce - accumulate to single value
[1, 2, 3, 4].reduce((acc, x) => acc + x, 0); // 10
```

### 13. What are Symbol and BigInt?

**Answer:**
- **Symbol**: Unique primitive, often for object keys
- **BigInt**: Arbitrary precision integers

```javascript
const id = Symbol('id'); // Unique key
const big = 9007199254740991n; // BigInt
const big2 = BigInt(9007199254740991);
```

### 14. What is a generator function?

**Answer:** Function that can pause and resume execution.

```javascript
function* counter() {
  let i = 0;
  while (true) {
    yield i++;
  }
}

const gen = counter();
console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
```

### 15. Explain event delegation.

**Answer:** Single event listener on parent instead of multiple on children.

```javascript
// Instead of adding listener to each li
document.querySelector('ul').addEventListener('click', (e) => {
  if (e.target.tagName === 'LI') {
    console.log(e.target.textContent);
  }
});
```

### 16. What are Web Workers?

**Answer:** Run JavaScript in background threads.

```javascript
// main.js
const worker = new Worker('worker.js');
worker.postMessage({ data });
worker.onmessage = (e) => console.log(e.data);

// worker.js
onmessage = (e) => {
  // Heavy computation
  postMessage(result);
};
```

### 17. What is the difference between null and undefined?

**Answer:**
- `undefined`: Variable declared but not assigned
- `null`: Intentional absence of value (assigned by developer)

```javascript
let a; // undefined
let b = null; // null
typeof undefined; // "undefined"
typeof null; // "object" (bug in JS)
```

### 18. Explain call, apply, and bind.

**Answer:**

```javascript
const obj = { name: "John" };

function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

greet.call(obj, "Hello", "!"); // "Hello, John!"
greet.apply(obj, ["Hello", "!"]); // "Hello, John!"
const bound = greet.bind(obj, "Hello");
bound("!"); // "Hello, John!"
```

### 19. What is currying?

**Answer:** Transforming function with multiple args into chain of single-arg functions.

```javascript
// Normal
function add(a, b) { return a + b; }

// Curried
function curriedAdd(a) {
  return function(b) {
    return a + b;
  };
}
curriedAdd(5)(3); // 8

// Arrow function style
const add = a => b => a + b;
```

### 20. What is memoization?

**Answer:** Caching function results for same inputs.

```javascript
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}

const memoFib = memoize(function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
});
```

---

## TypeScript

### 1. What is TypeScript and why use it?

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

### 9. What is declaration merging?

**Answer:** TypeScript merges multiple declarations with same name.

```typescript
interface Box {
  height: number;
}
interface Box {
  width: number;
}
// Merged: { height: number; width: number; }

namespace Box {
  export function create() { return {}; }
}
```

### 10. What are conditional types?

**Answer:** Types that depend on a condition.

```typescript
type IsString<T> = T extends string ? true : false;
type A = IsString<string>; // true
type B = IsString<number>; // false

// Extract with infer
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
```

### 11. What are mapped types?

**Answer:** Transform types by iterating over keys.

```typescript
type Readonly<T> = {
  readonly [K in keyof T]: T[K];
};

type Partial<T> = {
  [K in keyof T]?: T[K];
};

type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};
```

### 12. Explain template literal types.

**Answer:** String manipulation at type level.

```typescript
type Greeting = `Hello, ${string}`;
type EventName = `on${Capitalize<string>}Change`;

type Lowercase<T extends string> = 
  T extends `${infer F}${infer R}` 
    ? `${Lowercase<F>}${Lowercase<R>}` 
    : T;
```

### 13. What is the satisfies keyword?

**Answer:** Validate type without widening.

```typescript
type Colors = "red" | "green" | "blue";

const palette = {
  red: "#ff0000",
  green: "#00ff00",
  blue: "#0000ff",
  yellow: "#ffff00", // Error without satisfies
} satisfies Record<Colors, string>;
```

### 14. What are type guards?

**Answer:** Narrow types within conditional blocks.

```typescript
// typeof guard
function process(x: string | number) {
  if (typeof x === "string") {
    return x.toUpperCase();
  }
  return x.toFixed(2);
}

// instanceof guard
function getLength(x: Date | string) {
  if (x instanceof Date) {
    return x.getTime();
  }
  return x.length;
}

// Custom type guard
function isString(x: unknown): x is string {
  return typeof x === "string";
}
```

### 15. What is tsconfig.json?

**Answer:** TypeScript compiler configuration.

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### 16. What is the difference as const and type assertion?

**Answer:**
- **Type assertion (`as Type`)**: Tell compiler you know better
- **`as const`**: Make value deeply immutable, infer literal types

```typescript
const arr = ["hello", "world"]; // string[]
const arrConst = ["hello", "world"] as const; // readonly ["hello", "world"]

const obj = { a: 1 } as const; // { readonly a: 1 }
```

### 17. What are branded/nominal types?

**Answer:** Create distinct types from same base type.

```typescript
type UserId = string & { readonly brand: unique symbol };
type Email = string & { readonly brand: unique symbol };

function createUserId(id: string): UserId {
  return id as UserId;
}

function sendEmail(to: Email) { }
sendEmail(createUserId("123")); // Error: not Email
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

### 9. What is the difference between `make` and `new`?

**Answer:**
- `make`: Allocates and initializes slices, maps, channels
- `new`: Allocates zeroed memory, returns pointer

```go
slice := make([]int, 5, 10) // []int with len 5, cap 10
m := make(map[string]int)   // Empty map
ptr := new(int)             // *int pointing to zeroed int
```

### 10. Explain Go's testing package.

**Answer:**

```go
// math_test.go
package math

import "testing"

func TestAdd(t *testing.T) {
    result := Add(2, 3)
    if result != 5 {
        t.Errorf("Expected 5, got %d", result)
    }
}

func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}

func TestMain(m *testing.M) {
    // Setup
    code := m.Run()
    // Teardown
    os.Exit(code)
}
```

### 11. What are Go's blank identifier and iota?

**Answer:**
- `_`: Ignore values
- `iota`: Auto-incrementing constant generator

```go
// Blank identifier
_, value := map["key"]
for _, v := range slice { }

// Iota
type Status int
const (
    Pending Status = iota // 0
    Approved              // 1
    Rejected              // 2
)
```

### 12. How does Go handle panics?

**Answer:**

```go
func risky() {
    defer func() {
        if r := recover(); r != nil {
            log.Println("Recovered:", r)
        }
    }()
    panic("something went wrong")
}

// Best practice: use errors, not panics
```

### 13. What is Go's context package?

**Answer:** Carry deadlines, cancellation signals across API boundaries.

```go
func process(ctx context.Context) error {
    select {
    case <-ctx.Done():
        return ctx.Err()
    case result := <-doWork():
        return nil
    }
}

ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()
process(ctx)
```

### 14. Explain Go's embed package.

**Answer:** Embed files at compile time.

```go
import "embed"

//go:embed templates/*.html
var templates embed.FS

//go:embed config.json
var config []byte

//go:embed assets
var assets embed.FS
```

### 15. What is Go's race detector?

**Answer:** Detects data races at runtime.

```bash
go run -race main.go
go test -race ./...
go build -race
```

### 16. How to implement inheritance in Go?

**Answer:** Composition over inheritance.

```go
type Animal struct {
    Name string
}

func (a Animal) Speak() string { return "..." }

type Dog struct {
    Animal // Embedding
    Breed  string
}

func (d Dog) Speak() string { return "Woof!" }
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

### 9. What is the difference between `Copy` and `Clone`?

**Answer:**
- `Copy`: Bitwise copy, type still usable (stack types)
- `Clone`: Deep copy, explicit call

```rust
#[derive(Copy, Clone)]
struct Point {
    x: i32,
    y: i32,
}

let p1 = Point { x: 1, y: 2 };
let p2 = p1; // Copy, p1 still usable
let p3 = p1.clone(); // Clone
```

### 10. What are smart pointers in Rust?

**Answer:**

```rust
// Box<T> - Heap allocation
let b = Box::new(5);

// Rc<T> - Reference counted (single-threaded)
use std::rc::Rc;
let rc = Rc::new(5);
let rc2 = Rc::clone(&rc);

// Arc<T> - Atomic reference counted (thread-safe)
use std::sync::Arc;
let arc = Arc::new(5);

// RefCell<T> - Interior mutability
use std::cell::RefCell;
let cell = RefCell::new(5);
*cell.borrow_mut() = 10;
```

### 11. Explain Rust macros.

**Answer:**

```rust
// Declarative macros (macro_rules!)
macro_rules! say_hello {
    () => {
        println!("Hello!");
    };
}
say_hello!();

// Procedural macros
#[derive(Debug)] // Custom derive
#[tokio::main]  // Attribute macro
fn main() {}
```

### 12. What is unsafe Rust?

**Answer:** Bypasses safety checks.

```rust
unsafe {
    // Dereference raw pointer
    let ptr = &mut 5 as *mut i32;
    *ptr = 10;
    
    // Call unsafe function
    unsafe_fn();
    
    // Access mutable static
    STATIC_VAR = 10;
}
```

### 13. What are closures in Rust?

**Answer:** Anonymous functions capturing environment.

```rust
let x = 5;
let add = |y| x + y;

// With type annotations
let add = |a: i32, b: i32| -> i32 { a + b };

// Fn, FnMut, FnOnce
fn call_it<F: Fn(i32) -> i32>(f: F, x: i32) -> i32 {
    f(x)
}
```

### 14. Explain Rust's type system features.

**Answer:**

```rust
// Newtype pattern
struct UserId(u32);

// Type alias
type Result<T> = std::result::Result<T, Error>;

// Associated types
trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}

// PhantomData
use std::marker::PhantomData;
struct Ghost<T> {
    _marker: PhantomData<T>,
}
```

### 15. How to handle async in Rust?

**Answer:**

```rust
use tokio;

#[tokio::main]
async fn main() {
    let task1 = async { println!("Task 1"); };
    let task2 = async { println!("Task 2"); };
    
    tokio::join!(task1, task2);
    
    // Spawn background task
    tokio::spawn(async {
        // Background work
    });
}
```

### 16. What is the borrow checker?

**Answer:** Compile-time enforcement of ownership rules.

```rust
let mut s = String::from("hello");
let r1 = &s; // OK
let r2 = &s; // OK
// let r3 = &mut s; // Error: can't mut borrow while immutably borrowed

drop(r1);
drop(r2);
let r3 = &mut s; // OK now
r3.push_str(", world");
```

### 17. Explain Rust's error handling best practices.

**Answer:**

```rust
// Use Result for recoverable errors
fn read_file(path: &str) -> Result<String, io::Error> {
    std::fs::read_to_string(path)
}

// Use Option for absence
fn find_user(id: u32) -> Option<User> { }

// ? operator for propagation
fn process() -> Result<(), Box<dyn std::error::Error>> {
    let content = std::fs::read_to_string("file.txt")?;
    Ok(())
}

// Custom error types
#[derive(Debug)]
enum AppError {
    Io(io::Error),
    Parse(ParseError),
}
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

### 9. What is the difference between useEffect and useLayoutEffect?

**Answer:**
- `useEffect`: Runs after paint (async)
- `useLayoutEffect`: Runs before paint (sync, blocks rendering)

```jsx
// Use useEffect for most cases
useEffect(() => { /* subscriptions, fetches */ }, []);

// Use useLayoutEffect for DOM measurements
useLayoutEffect(() => {
  const rect = ref.current.getBoundingClientRect();
  // Adjust position before paint
}, []);
```

### 10. What are error boundaries?

**Answer:** Catch JavaScript errors in child components.

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error(error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```

### 11. Explain React Fiber.

**Answer:** New reconciliation engine in React 16.
- Incremental rendering
- Pause/resume work
- Prioritize updates
- Better animation support

### 12. What is concurrent rendering?

**Answer:** Multiple versions of UI in memory.

```jsx
// Start transition without blocking
import { useTransition } from 'react';

const [isPending, startTransition] = useTransition();

startTransition(() => {
  setSearchQuery(input); // Non-urgent update
});

// useDeferredValue
const deferredQuery = useDeferredValue(query);
```

### 13. What are React portals?

**Answer:** Render children outside parent DOM hierarchy.

```jsx
function Modal({ children }) {
  return ReactDOM.createPortal(
    <div className="modal">{children}</div>,
    document.getElementById('modal-root')
  );
}
```

### 14. Explain forwardRef and useImperativeHandle.

**Answer:**

```jsx
// forwardRef - pass ref through components
const FancyInput = React.forwardRef((props, ref) => (
  <input ref={ref} className="fancy" />
));

// useImperativeHandle - customize exposed methods
function FancyInput(props, ref) {
  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
    clear: () => inputRef.current.value = ''
  }));
  
  return <input ref={inputRef} />;
}
```

### 15. What is the difference between useMemo and useCallback?

**Answer:**
- `useMemo`: Memoizes computed value
- `useCallback`: Memoizes function reference

```jsx
// useMemo - avoid expensive calculations
const sorted = useMemo(() => items.sort(), [items]);

// useCallback - prevent function re-creation
const handleClick = useCallback(() => {
  doSomething(id);
}, [id]);

// Pass to memoized child
<MemoChild onClick={handleClick} />
```

### 16. How to optimize React performance?

**Answer:**

```jsx
// 1. React.memo for components
const MemoChild = React.memo(Child);

// 2. useMemo for expensive calculations
const result = useMemo(() => heavy(items), [items]);

// 3. useCallback for stable function refs
const callback = useCallback(() => {}, []);

// 4. Code splitting
const LazyComponent = React.lazy(() => import('./Heavy'));

// 5. Virtual lists for large data
import { FixedSizeList } from 'react-window';

// 6. Avoid inline objects/functions in JSX
```

### 17. What are synthetic events?

**Answer:** React's cross-browser wrapper around native events.

```jsx
function handleClick(e) {
  e.preventDefault(); // Works across browsers
  e.stopPropagation();
  e.nativeEvent; // Access native event
}
```

### 18. Explain React Server Components.

**Answer:** Components rendered on server, no client JS.

```jsx
// Server Component (default in App Router)
async function Note({ id }) {
  const note = await db.note.find(id); // Direct DB access
  return <div>{note.content}</div>;
}

// Client Component
'use client';
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

### 19. What is Suspense?

**Answer:** Declaratively wait for something to load.

```jsx
<Suspense fallback={<Spinner />}>
  <ProfileDetails />
</Suspense>

// With lazy loading
const LazyComponent = React.lazy(() => import('./Heavy'));

<Suspense fallback={<Loading />}>
  <LazyComponent />
</Suspense>
```

### 20. How to handle forms in React?

**Answer:**

```jsx
// Controlled form
function Form() {
  const [values, setValues] = useState({ name: '', email: '' });
  
  const handleSubmit = (e) => {
    e.preventDefault();
    submit(values);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input 
        value={values.name}
        onChange={e => setValues({...values, name: e.target.value})}
      />
    </form>
  );
}

// Using React Hook Form
const { register, handleSubmit } = useForm();
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

### 7. What is Incremental Static Regeneration (ISR)?

**Answer:** Update static content after build without rebuilding.

```jsx
export async function getStaticProps() {
  return {
    props: { data },
    revalidate: 60, // Regenerate every 60 seconds
  };
}

// On-demand ISR
await res.revalidate('/blog/post-slug');
```

### 8. How to handle API routes in Next.js?

**Answer:**

```jsx
// pages/api/users.js
export default function handler(req, res) {
  if (req.method === 'GET') {
    res.status(200).json(users);
  } else if (req.method === 'POST') {
    const user = createUser(req.body);
    res.status(201).json(user);
  }
}

// App Router (app/api/route.ts)
export async function POST(request: Request) {
  const body = await request.json();
  return Response.json({ success: true });
}
```

### 9. What is middleware in Next.js?

**Answer:** Run code before request completion.

```ts
// middleware.ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function middleware(request: NextRequest) {
  const token = request.cookies.get('token');
  
  if (!token) {
    return NextResponse.redirect('/login');
  }
  
  return NextResponse.next();
}

export const config = {
  matcher: '/dashboard/:path*',
};
```

### 10. Explain Next.js routing features.

**Answer:**

```
app/
  (auth)/           # Route group (not in URL)
    login/page.tsx
  /blog/
    [slug]/         # Dynamic route
      page.tsx
    [...slug]/      # Catch-all
      page.tsx
    [[...slug]]/    # Optional catch-all
      page.tsx
  /api/             # API routes
```

### 11. How to implement authentication in Next.js?

**Answer:**

```jsx
// Using NextAuth.js
import NextAuth from 'next-auth';
import GoogleProvider from 'next-auth/providers/google';

export default NextAuth({
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_ID,
      clientSecret: process.env.GOOGLE_SECRET,
    }),
  ],
  callbacks: {
    async jwt({ token, user }) {
      return token;
    },
  },
});

// Usage
import { useSession, signIn, signOut } from 'next-auth/react';
```

### 12. What is the difference between client and server components?

**Answer:**

| Server Components | Client Components |
|-------------------|-------------------|
| Render on server | Render on client |
| No useState/useEffect | Full interactivity |
| Direct DB access | API calls |
| Smaller bundle size | Browser APIs access |
| Default in App Router | Need 'use client' |

### 13. How to handle SEO in Next.js?

**Answer:**

```jsx
// Metadata API (App Router)
export const metadata = {
  title: 'My Page',
  description: 'Page description',
  openGraph: {
    title: 'OG Title',
    images: ['/og-image.jpg'],
  },
};

// generateMetadata for dynamic pages
export async function generateMetadata({ params }) {
  const post = await getPost(params.slug);
  return { title: post.title };
}
```

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

### 5. What is Redux Selector?

**Answer:** Extract data from state.

```jsx
import { useSelector } from 'react-redux';

// Basic selector
const count = useSelector(state => state.counter.value);

// Memoized selector with reselect
import { createSelector } from '@reduxjs/toolkit';

const selectTodos = state => state.todos;
const selectCompletedTodos = createSelector(
  [selectTodos],
  todos => todos.filter(t => t.completed)
);
```

### 6. Explain Redux middleware.

**Answer:** Intercept and process actions.

```jsx
// Custom middleware
const loggerMiddleware = store => next => action => {
  console.log('Dispatching:', action);
  const result = next(action);
  console.log('Next state:', store.getState());
  return result;
};

// Built-in middleware
import { configureStore } from '@reduxjs/toolkit';
const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(loggerMiddleware),
});
```

### 7. What is Redux Persist?

**Answer:** Persist Redux state to localStorage.

```jsx
import { persistStore, persistReducer } from 'redux-persist';
import storage from 'redux-persist/lib/storage';

const persistConfig = {
  key: 'root',
  storage,
  whitelist: ['auth', 'preferences'],
};

const persistedReducer = persistReducer(persistConfig, reducer);
const store = configureStore({ reducer: persistedReducer });
const persistor = persistStore(store);
```

### 8. What is RTK Query?

**Answer:** Data fetching built into Redux Toolkit.

```jsx
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

const api = createApi({
  baseQuery: fetchBaseQuery({ baseUrl: '/api' }),
  endpoints: (builder) => ({
    getPosts: builder.query({
      query: () => 'posts',
    }),
    addPost: builder.mutation({
      query: (body) => ({
        url: 'posts',
        method: 'POST',
        body,
      }),
    }),
  }),
});

export const { useGetPostsQuery, useAddPostMutation } = api;
```

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

### 5. What are Tailwind plugins?

**Answer:**

```js
// tailwind.config.js
plugins: [
  require('@tailwindcss/forms'),
  require('@tailwindcss/typography'),
  require('@tailwindcss/aspect-ratio'),
  require('@tailwindcss/line-clamp'),
  
  // Custom plugin
  function({ addUtilities }) {
    addUtilities({
      '.text-shadow': {
        textShadow: '0 2px 4px rgba(0,0,0,0.1)',
      },
    });
  },
];
```

### 6. How to optimize Tailwind for production?

**Answer:**
- JIT mode (default in v3) - generates only used classes
- PurgeCSS removes unused classes
- Proper `content` configuration

```js
module.exports = {
  content: [
    './src/**/*.{js,ts,jsx,tsx}',
    './pages/**/*.{js,ts,jsx,tsx}',
  ],
  theme: { extend: {} },
  plugins: [],
}
```

### 7. What are arbitrary values in Tailwind?

**Answer:**

```html
<div class="w-[350px] h-[500px] bg-[#1a2b3c]">
  <p class="text-[14px] leading-[1.5]">
  <div class="grid grid-cols-[1fr,2fr,1fr]">
```

### 8. How to compose components with Tailwind?

**Answer:**

```jsx
// Using clsx or classnames
import clsx from 'clsx';

function Button({ variant = 'primary', className, ...props }) {
  const base = 'px-4 py-2 rounded font-medium';
  const variants = {
    primary: 'bg-blue-500 text-white hover:bg-blue-600',
    secondary: 'bg-gray-200 text-gray-800 hover:bg-gray-300',
  };
  
  return (
    <button className={clsx(base, variants[variant], className)} {...props} />
  );
}

// Using @apply in CSS
@apply px-4 py-2 rounded font-medium;
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

### 8. How to handle environment variables in Node.js?

**Answer:**

```javascript
// Using dotenv
require('dotenv').config();

const port = process.env.PORT || 3000;
const dbUrl = process.env.DATABASE_URL;

// Best practices
const config = {
  development: { port: 3000, debug: true },
  production: { port: 80, debug: false },
};

const env = process.env.NODE_ENV || 'development';
module.exports = config[env];
```

### 9. What is the Buffer class in Node.js?

**Answer:** Handle binary data.

```javascript
const buf = Buffer.from('Hello', 'utf8');
console.log(buf.toString('hex')); // 48656c6c6f
console.log(buf.toString('base64')); // SGVsbG8=

// Create buffer
const buf2 = Buffer.alloc(1024);
const buf3 = Buffer.from([0x62, 0x75, 0x66]);

// Concatenate
const combined = Buffer.concat([buf, buf2]);
```

### 10. Explain Node.js child_process module.

**Answer:** Spawn system processes.

```javascript
const { exec, execSync, spawn, fork } = require('child_process');

// exec - callback with stdout/stderr
exec('ls -la', (err, stdout, stderr) => {
  if (err) return console.error(err);
  console.log(stdout);
});

// spawn - streaming
const child = spawn('ls', ['-la']);
child.stdout.on('data', (data) => console.log(data.toString()));

// fork - communicate with Node processes
const worker = fork('worker.js');
worker.send({ cmd: 'process' });
worker.on('message', (msg) => console.log(msg));
```

### 11. How to debug Node.js applications?

**Answer:**

```bash
# Built-in debugger
node inspect app.js
node --inspect app.js  # Chrome DevTools

# Debug module
const debug = require('debug')('app:start');
debug('Starting application');

# Console methods
console.trace('Trace execution');
console.assert(condition, 'Message');
console.time('operation');
// ... operation
console.timeEnd('operation');
```

### 12. What is the EventEmitter class?

**Answer:** Handle custom events.

```javascript
const EventEmitter = require('events');
class MyEmitter extends EventEmitter {}

const emitter = new MyEmitter();

emitter.on('event', (arg) => {
  console.log('Event fired:', arg);
});

emitter.once('event', () => {
  // Only fires once
});

emitter.emit('event', 'data');
emitter.removeListener('event', callback);
```

### 13. Explain Node.js performance optimization.

**Answer:**

```javascript
// 1. Use clustering
const cluster = require('cluster');
if (cluster.isPrimary) {
  for (let i = 0; i < os.cpus().length; i++) cluster.fork();
}

// 2. Use streams for large files
readable.pipe(transform).pipe(writable);

// 3. Cache frequently accessed data
const cache = new Map();

// 4. Use worker threads for CPU-bound tasks
const { Worker } = require('worker_threads');

// 5. Connection pooling for databases

// 6. Monitor with clinic.js, 0x, or APM tools
```

### 14. What is the difference between setImmediate and process.nextTick?

**Answer:**

```javascript
// process.nextTick - executes immediately after current operation
process.nextTick(() => console.log('nextTick'));

// setImmediate - executes in check phase of event loop
setImmediate(() => console.log('immediate'));

// Example showing difference
fs.readFile('file.txt', () => {
  setImmediate(() => console.log('immediate'));
  process.nextTick(() => console.log('nextTick'));
});
// Output: nextTick, immediate
```

### 15. How to handle file uploads in Node.js?

**Answer:**

```javascript
// Using multer
const multer = require('multer');
const upload = multer({ dest: 'uploads/' });

app.post('/upload', upload.single('file'), (req, res) => {
  console.log(req.file); // Uploaded file info
  res.json({ filename: req.file.filename });
});

// Multiple files
app.post('/upload-multiple', upload.array('photos', 10), (req, res) => { });

// Using streams
const form = new IncomingForm();
form.on('file', (name, file) => {
  // Handle file
});
```

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

### 5. How to implement authentication in Express?

**Answer:**

```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');

// Register
app.post('/register', async (req, res) => {
  const { email, password } = req.body;
  const hashedPassword = await bcrypt.hash(password, 10);
  const user = await User.create({ email, password: hashedPassword });
  res.json({ id: user.id });
});

// Login
app.post('/login', async (req, res) => {
  const user = await User.findOne({ email: req.body.email });
  if (!user || !await bcrypt.compare(req.body.password, user.password)) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET);
  res.json({ token });
});

// Auth middleware
const auth = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  try {
    req.user = jwt.verify(token, process.env.JWT_SECRET);
    next();
  } catch {
    res.status(401).json({ error: 'Invalid token' });
  }
};
```

### 6. How to validate request data in Express?

**Answer:**

```javascript
// Using express-validator
const { body, validationResult } = require('express-validator');

app.post('/users',
  body('email').isEmail().normalizeEmail(),
  body('password').isLength({ min: 6 }),
  body('name').trim().notEmpty(),
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    // Process valid data
  }
);

// Using Joi
const Joi = require('joi');
const schema = Joi.object({
  email: Joi.string().email().required(),
  password: Joi.string().min(6).required(),
});
```

### 7. Explain Express response methods.

**Answer:**

```javascript
// Send JSON
res.json({ key: 'value' });

// Send HTML
res.send('<h1>Hello</h1>');

// Send file
res.sendFile(path.join(__dirname, 'public', 'index.html'));

// Download file
res.download('/path/to/file.pdf', 'custom-name.pdf');

// Redirect
res.redirect(301, '/new-path');
res.redirect('/login');

// Set headers
res.set('X-Custom-Header', 'value');
res.cookie('session', token, { httpOnly: true, maxAge: 3600000 });

// Status codes
res.status(201).json({ created: true });
res.status(404).json({ error: 'Not found' });
```

### 8. How to implement rate limiting in Express?

**Answer:**

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  message: { error: 'Too many requests, please try again later' },
  standardHeaders: true,
  legacyHeaders: false,
});

app.use('/api/', limiter);

// Per-user rate limiting
const userIdLimiter = rateLimit({
  windowMs: 60 * 1000,
  max: 10,
  keyGenerator: (req) => req.user.id,
});
```

### 9. How to handle CORS in Express?

**Answer:**

```javascript
const cors = require('cors');

// Enable all CORS
app.use(cors());

// Configure CORS
app.use(cors({
  origin: 'https://example.com',
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
}));

// Per-route CORS
app.post('/api/data', cors(), (req, res) => { });
```

### 10. How to structure a large Express application?

**Answer:**

```
src/
├── config/
│   ├── database.js
│   └── passport.js
├── controllers/
│   ├── userController.js
│   └── postController.js
├── models/
│   ├── User.js
│   └── Post.js
├── routes/
│   ├── users.js
│   └── posts.js
├── middleware/
│   ├── auth.js
│   └── validation.js
├── services/
│   └── emailService.js
├── utils/
│   └── helpers.js
└── app.js
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

### 5. How to implement authentication in FastAPI?

**Answer:**

```python
from fastapi.security import OAuth2PasswordBearer
from jose import JWTError, jwt
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

def verify_password(plain, hashed):
    return pwd_context.verify(plain, hashed)

def get_password_hash(password):
    return pwd_context.hash(password)

def create_access_token(data: dict, expires_delta: timedelta):
    to_encode = data.copy()
    expire = datetime.utcnow() + expires_delta
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm="HS256")

async def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
        username = payload.get("sub")
        if username is None:
            raise HTTPException(status_code=401)
        return get_user(username)
    except JWTError:
        raise HTTPException(status_code=401)
```

### 6. How to connect FastAPI to a database?

**Answer:**

```python
# SQLAlchemy integration
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "postgresql://user:pass@localhost/db"
engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(bind=engine)
Base = declarative_base()

# Dependency
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# Model
class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    email = Column(String, unique=True)

# CRUD
@app.post("/users/")
def create_user(user: UserCreate, db: Session = Depends(get_db)):
    db_user = User(email=user.email)
    db.add(db_user)
    db.commit()
    return db_user
```

### 7. How to document APIs in FastAPI?

**Answer:**

```python
# Automatic OpenAPI docs at /docs and /redoc
@app.post("/items/", tags=["items"], summary="Create item")
async def create_item(
    item: Item,
    description: str = "Item description",
    response_model=ItemOut,
    status_code=201,
    responses={
        400: {"description": "Bad request"},
        404: {"description": "Not found"},
    }
):
    """
    Create a new item.
    
    - **name**: Item name
    - **price**: Must be positive
    """
    return item

# Custom OpenAPI config
app = FastAPI(
    title="My API",
    description="API description",
    version="1.0.0",
    docs_url="/docs",
    redoc_url="/redoc",
)
```

### 8. How to handle file uploads in FastAPI?

**Answer:**

```python
from fastapi import UploadFile, File

@app.post("/upload/")
async def upload_file(file: UploadFile = File(...)):
    contents = await file.read()
    return {"filename": file.filename, "size": len(contents)}

# Multiple files
@app.post("/upload-multiple/")
async def upload_files(files: list[UploadFile] = File(...)):
    return [{"filename": f.filename} for f in files]

# With validation
@app.post("/upload-image/")
async def upload_image(
    file: UploadFile = File(..., description="Image file")
):
    if not file.content_type.startswith("image/"):
        raise HTTPException(400, "File must be an image")
    return {"filename": file.filename}
```

### 9. What is Pydantic and how does FastAPI use it?

**Answer:**

```python
from pydantic import BaseModel, EmailStr, Field, validator

class User(BaseModel):
    id: int
    email: EmailStr
    name: str = Field(..., min_length=2, max_length=50)
    age: int = Field(ge=0, le=150)
    
    @validator('email')
    def validate_email(cls, v):
        if '@' not in v:
            raise ValueError('Invalid email')
        return v
    
    class Config:
        orm_mode = True  # SQLAlchemy compatibility

# Nested models
class Post(BaseModel):
    author: User
    content: str
```

### 10. How to test FastAPI applications?

**Answer:**

```python
from fastapi.testclient import TestClient
from app import app

client = TestClient(app)

def test_read_main():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"message": "Hello"}

def test_create_user():
    response = client.post("/users/", json={
        "email": "test@example.com",
        "password": "secret"
    })
    assert response.status_code == 201
    assert "id" in response.json()

# Async tests with pytest
import pytest

@pytest.mark.asyncio
async def test_async_endpoint():
    async with AsyncClient(app=app, base_url="http://test") as ac:
        response = await ac.get("/async-endpoint")
        assert response.status_code == 200
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

### 4. How to handle JSON in Actix-web?

**Answer:**

```rust
use actix_web::{post, web, Responder};
use serde::{Deserialize, Serialize};

#[derive(Deserialize)]
struct Item {
    name: String,
    price: f64,
}

#[derive(Serialize)]
struct ItemResponse {
    id: u32,
    name: String,
}

#[post("/items")]
async fn create_item(item: web::Json<Item>) -> impl Responder {
    let response = ItemResponse {
        id: 1,
        name: item.name.clone(),
    };
    web::Json(response)
}
```

### 5. How to implement middleware in Actix-web?

**Answer:**

```rust
use actix_web::{dev, Error, FromRequest, HttpRequest};
use actix_web::middleware::Logger;

// Built-in logger
App::new().wrap(Logger::default());

// Custom middleware
use futures_util::future::LocalBoxFuture;

pub struct Authentication;

impl<S, B> Transform<S, actix_web::dev::ServerRequest> for Authentication
where
    S: Service<actix_web::dev::ServerRequest, Response = ServiceResponse<B>, Error = Error>,
    S::Future: 'static,
    B: 'static,
{
    type Response = ServiceResponse<B>;
    type Error = Error;
    type Transform = AuthenticationMiddleware<S>;
    type InitError = ();
    type Future = LocalBoxFuture<'static, Result<Self::Transform, Self::InitError>>;

    fn new_transform(&self, service: S) -> Self::Future {
        Box::pin(async move { Ok(AuthenticationMiddleware { service }) })
    }
}
```

### 6. How to handle database connections in Actix-web?

**Answer:**

```rust
use sqlx::{Pool, Postgres};
use actix_web::web;

// Database pool as app state
struct AppState {
    db: Pool<Postgres>,
}

async fn index(data: web::Data<AppState>) -> impl Responder {
    let users = sqlx::query!("SELECT * FROM users")
        .fetch_all(&data.db)
        .await
        .unwrap();
    web::Json(users)
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    let pool = Pool::new("postgres://user:pass@localhost/db").await.unwrap();
    
    HttpServer::new(move || {
        App::new()
            .app_data(web::Data::new(AppState { db: pool.clone() }))
            .service(index)
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}
```

### 7. How to handle CORS in Actix-web?

**Answer:**

```rust
use actix_cors::Cors;

HttpServer::new(|| {
    let cors = Cors::default()
        .allow_any_origin()
        .allow_any_method()
        .allow_any_header()
        .max_age(3600);
    
    App::new()
        .wrap(cors)
        .service(index)
})
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

### 6. How to implement subscriptions in GraphQL?

**Answer:**

```javascript
// Schema
type Subscription {
  messageAdded(roomId: ID!): Message!
}

// Resolver
const resolvers = {
  Subscription: {
    messageAdded: {
      subscribe: (_, { roomId }, { pubsub }) => {
        return pubsub.asyncIterator(['MESSAGE_' + roomId]);
      },
    },
  },
};

// Publishing
pubsub.publish('MESSAGE_' + roomId, {
  messageAdded: newMessage,
});
```

### 7. What is DataLoader and why use it?

**Answer:** Batching and caching for N+1 problem.

```javascript
const DataLoader = require('dataloader');

const userLoader = new DataLoader(async (userIds) => {
  const users = await db.user.findMany({
    where: { id: { in: userIds } },
  });
  return userIds.map(id => users.find(u => u.id === id));
});

// In resolver - batches multiple calls
const user = await userLoader.load(userId);
```

### 8. Explain GraphQL directives.

**Answer:**

```graphql
# Built-in directives
query {
  user @include(if: $showUser) {
    id
    name
  }
  posts @skip(if: $skipPosts) {
    title
  }
}

# Custom directive
directive @auth(requires: Role) on FIELD_DEFINITION

type Query {
  adminData: String @auth(requires: ADMIN)
}

# Schema directive implementation
class AuthDirective extends SchemaDirectiveVisitor {
  visitFieldDefinition(field) {
    const { requires } = this.args;
    // Add auth logic
  }
}
```

### 9. How to handle file uploads in GraphQL?

**Answer:**

```javascript
// Using graphql-upload
import { GraphQLUpload } from 'graphql-upload';

const schema = makeExecutableSchema({
  typeDefs: gql`
    scalar Upload
    type Mutation {
      uploadFile(file: Upload!): File!
    }
  `,
  resolvers: {
    Upload: GraphQLUpload,
    Mutation: {
      uploadFile: async (_, { file }) => {
        const { createReadStream, filename } = await file;
        const stream = createReadStream();
        // Process stream
        return { filename, url: '/uploads/' + filename };
      },
    },
  },
});
```

### 10. What is Apollo Federation?

**Answer:** Distributed GraphQL architecture.

```javascript
// Service 1 - Users
const typeDefs = gql`
  extend type Query {
    me: User
  }
  type User @key(fields: "id") {
    id: ID!
    name: String
  }
`;

// Service 2 - Posts
const typeDefs = gql`
  type Post @key(fields: "id") {
    id: ID!
    author: User @external
  }
  extend type User @key(fields: "id") {
    id: ID! @external
    posts: [Post]
  }
`;

// Gateway combines services
```

### 11. How to version GraphQL APIs?

**Answer:**

```graphql
# GraphQL doesn't need versioning like REST
# Add new fields without breaking existing queries

type Query {
  users: [User]
  usersV2(first: Int, after: String): UserConnection # New paginated version
}

# Deprecate old fields
type User {
  name: String @deprecated(reason: "Use fullName instead")
  fullName: String
}

# Use schema evolution
```

### 12. Explain GraphQL caching strategies.

**Answer:**

```javascript
// Client-side caching (Apollo Client)
const client = new ApolloClient({
  cache: new InMemoryCache({
    typePolicies: {
      Query: {
        fields: {
          posts: {
            keyArgs: [],
            merge(existing, incoming) {
              return incoming;
            },
          },
        },
      },
    },
  }),
});

// Server-side caching
// - Query result caching
// - DataLoader caching
// - CDN caching for persisted queries

// HTTP caching with cache-control
res.set('Cache-Control', 'max-age=60, public');
```

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

### 7. Explain PostgreSQL replication.

**Answer:**

```sql
-- Primary configuration
# postgresql.conf
wal_level = replica
max_wal_senders = 3
wal_keep_size = 64

# pg_hba.conf
host replication replicator 192.168.1.0/24 md5

-- Replica configuration
# standby.signal file
primary_conninfo = 'host=primary_host user=replicator password=secret'
```

**Types:**
- **Streaming replication**: Real-time sync
- **Logical replication**: Row-level sync between different schemas

### 8. What are PostgreSQL window functions?

**Answer:**

```sql
-- ROW_NUMBER, RANK, DENSE_RANK
SELECT 
  name,
  salary,
  ROW_NUMBER() OVER (ORDER BY salary DESC) as row_num,
  RANK() OVER (ORDER BY salary DESC) as rank,
  DENSE_RANK() OVER (ORDER BY salary DESC) as dense_rank
FROM employees;

-- LAG, LEAD
SELECT 
  date,
  revenue,
  LAG(revenue, 1) OVER (ORDER BY date) as prev_revenue,
  LEAD(revenue, 1) OVER (ORDER BY date) as next_revenue
FROM sales;

-- Running totals
SELECT 
  date,
  SUM(revenue) OVER (ORDER BY date) as running_total
FROM sales;
```

### 9. How to optimize PostgreSQL queries?

**Answer:**

```sql
-- Use EXPLAIN ANALYZE
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';

-- Index optimization
CREATE INDEX CONCURRENTLY idx_email ON users(email);

-- Vacuum and analyze
VACUUM ANALYZE users;

-- Connection pooling (pgbouncer)
-- Partitioning for large tables
CREATE TABLE measurements (
  city_id int,
  logdate date,
  peaktemp int,
  unitsales int
) PARTITION BY RANGE (logdate);

-- Materialized views for complex queries
CREATE MATERIALIZED VIEW sales_summary AS
SELECT date_trunc('month', date), SUM(amount)
FROM sales GROUP BY 1;

REFRESH MATERIALIZED VIEW CONCURRENTLY sales_summary;
```

### 10. What are PostgreSQL triggers?

**Answer:**

```sql
-- Create trigger function
CREATE OR REPLACE FUNCTION update_modified_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.modified_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Create trigger
CREATE TRIGGER update_users_modified
  BEFORE UPDATE ON users
  FOR EACH ROW
  EXECUTE FUNCTION update_modified_column();

-- Audit trigger
CREATE TABLE audit_log (
  table_name TEXT,
  operation TEXT,
  old_data JSONB,
  new_data JSONB,
  changed_at TIMESTAMPTZ DEFAULT NOW()
);
```

### 11. Explain PostgreSQL full-text search.

**Answer:**

```sql
-- Basic full-text search
SELECT * FROM articles
WHERE to_tsvector('english', content) @@ to_tsquery('english', 'database');

-- Create index for performance
CREATE INDEX idx_fts ON articles USING GIN (to_tsvector('english', content));

-- Use tsvector column
ALTER TABLE articles ADD COLUMN search_vector tsvector;
UPDATE articles SET search_vector = to_tsvector('english', content);
CREATE INDEX idx_search ON articles USING GIN (search_vector);

-- Ranking
SELECT 
  title,
  ts_rank(search_vector, query) as rank
FROM articles, to_tsquery('database') query
WHERE search_vector @@ query
ORDER BY rank DESC;
```

### 12. What is the difference between VACUUM and ANALYZE?

**Answer:**
- `VACUUM`: Reclaims storage from dead tuples
- `ANALYZE`: Updates statistics for query planner

```sql
VACUUM; -- Reclaim space
VACUUM FULL; -- Aggressive, locks table
ANALYZE; -- Update statistics
VACUUM ANALYZE; -- Both combined
```

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

### 6. What is the difference between embedded and referenced documents?

**Answer:**

```javascript
// Embedded (denormalized) - good for read-heavy, one-to-few
{
  _id: 1,
  name: "John",
  addresses: [
    { street: "123 Main", city: "NYC" },
    { street: "456 Oak", city: "LA" }
  ]
}

// Referenced (normalized) - good for many-to-many, large collections
{
  _id: 1,
  name: "John",
  addressIds: [ObjectId("..."), ObjectId("...")]
}
```

### 7. How to handle transactions in MongoDB?

**Answer:**

```javascript
const session = await db.startSession();
session.startTransaction();

try {
  await db.accounts.updateOne(
    { _id: 1 },
    { $inc: { balance: -100 } },
    { session }
  );
  await db.accounts.updateOne(
    { _id: 2 },
    { $inc: { balance: 100 } },
    { session }
  );
  await session.commitTransaction();
} catch (error) {
  await session.abortTransaction();
  throw error;
} finally {
  session.endSession();
}
```

### 8. Explain MongoDB indexes types.

**Answer:**

```javascript
// Single field
db.users.createIndex({ email: 1 });

// Compound
db.users.createIndex({ lastName: 1, firstName: 1 });

// Multikey (arrays)
db.users.createIndex({ tags: 1 });

// Text index
db.articles.createIndex({ content: "text" });

// Geospatial
db.places.createIndex({ location: "2dsphere" });

// Hashed (for sharding)
db.logs.createIndex({ timestamp: "hashed" });

// TTL (auto-expire)
db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 });

// Partial index
db.users.createIndex(
  { email: 1 },
  { partialFilterExpression: { active: true } }
);
```

### 9. How to backup and restore MongoDB?

**Answer:**

```bash
# Backup (mongodump)
mongodump --uri="mongodb://localhost:27017/mydb" --out=/backup

# Restore (mongorestore)
mongorestore --uri="mongodb://localhost:27017/mydb" /backup/mydb

# Export to JSON/BSON
mongoexport --db=mydb --collection=users --out=users.json

# Import
mongoimport --db=mydb --collection=users --file=users.json
```

### 10. What is MongoDB Atlas?

**Answer:** Managed MongoDB cloud service.

**Features:**
- Automated backups
- Auto-scaling
- Global clusters
- Built-in monitoring
- Serverless instances

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

### 6. What are Redis sorted sets use cases?

**Answer:**

```python
# Leaderboards
r.zadd("leaderboard", {"player1": 1000, "player2": 500})
r.zrevrank("leaderboard", "player1")  # Rank
r.zrevrange("leaderboard", 0, 9, withscores=True)  # Top 10

# Priority queues
r.zadd("tasks", {"task1": 1, "task2": 5})  # Score = priority
task = r.zpopmin("tasks")  # Get highest priority

# Rate limiting with sliding window
r.zadd("rate:user1", {str(time.time()): time.time()})
r.zremrangebyscore("rate:user1", 0, time.time() - 60)
count = r.zcard("rate:user1")
```

### 7. Explain Redis pub/sub.

**Answer:**

```python
# Publisher
r.publish("channel", "message")

# Subscriber
pubsub = r.pubsub()
pubsub.subscribe("channel")
pubsub.subscribe("user:123:*")  # Pattern

for message in pubsub.listen():
    print(message)

# Redis Streams (more robust than pub/sub)
r.xadd("mystream", {"field": "value"})
r.xread({"mystream": "0"}, count=1, block=1000)
```

### 8. How to use Redis for session management?

**Answer:**

```python
# Create session
session_id = str(uuid.uuid4())
r.setex(f"session:{session_id}", 3600, json.dumps(user_data))

# Get session
data = r.get(f"session:{session_id}")
if data:
    user = json.loads(data)

# Extend session
r.expire(f"session:{session_id}", 3600)

# Delete session (logout)
r.delete(f"session:{session_id}")
```

### 9. What is Redis Cluster?

**Answer:** Horizontal scaling with automatic sharding.

```bash
# Create cluster
redis-cli --cluster create 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 \
  127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 --cluster-replicas 1

# 16384 hash slots distributed across nodes
# Slot = CRC16(key) % 16384
```

### 10. What are Redis Lua scripts?

**Answer:** Atomic operations.

```python
# Register script
script = """
if redis.call('get', KEYS[1]) == ARGV[1] then
  return redis.call('del', KEYS[1])
else
  return 0
end
"""
unlock = r.register_script(script)
unlock(keys=['lock'], args=['token'])

# Use for complex atomic operations
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

### 5. How to design Elasticsearch mappings?

**Answer:**

```json
PUT /products
{
  "mappings": {
    "properties": {
      "name": { 
        "type": "text",
        "fields": { "keyword": { "type": "keyword" } }
      },
      "price": { "type": "float" },
      "created_at": { "type": "date" },
      "description": { "type": "text", "analyzer": "standard" },
      "category": { "type": "keyword" },
      "location": { "type": "geo_point" }
    }
  }
}
```

### 6. Explain Elasticsearch analyzers.

**Answer:**

```json
PUT /my-index
{
  "settings": {
    "analysis": {
      "analyzer": {
        "custom_analyzer": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": ["lowercase", "stop", "snowball"]
        }
      }
    }
  }
}

// Built-in analyzers:
// - standard: Default, removes punctuation
// - simple: Lowercase, splits on non-letters
// - whitespace: Splits on whitespace
// - keyword: No analysis
// - language-specific: english, french, etc.
```

### 7. How to handle aggregations in Elasticsearch?

**Answer:**

```json
GET /sales/_search
{
  "size": 0,
  "aggs": {
    "by_category": {
      "terms": { "field": "category" },
      "aggs": {
        "avg_price": { "avg": { "field": "price" } },
        "total_sales": { "sum": { "field": "amount" } }
      }
    },
    "date_histogram": {
      "date_histogram": { "field": "date", "calendar_interval": "month" }
    },
    "price_range": {
      "range": { "field": "price", "ranges": [{ "to": 50 }, { "from": 50, "to": 100 }, { "from": 100 }] }
    }
  }
}
```

### 8. What is the difference between query and filter context?

**Answer:**

```json
// Query context - calculates score
{
  "query": {
    "match": { "title": "laptop" }
  }
}

// Filter context - binary match, cached, no scoring
{
  "query": {
    "bool": {
      "filter": [
        { "range": { "price": { "gte": 500 } } },
        { "term": { "active": true } }
      ]
    }
  }
}
```

### 9. How to scale Elasticsearch?

**Answer:**

- **Sharding**: Split index into multiple shards
- **Replication**: Add replica shards for read scaling and HA
- **Index lifecycle**: Hot-warm-cold architecture
- **Rollup indices**: Pre-aggregated data for historical queries

```json
PUT /_ilm/policy/my_policy
{
  "policy": {
    "phases": {
      "hot": { "actions": { "rollover": { "max_size": "50GB" } } },
      "warm": { "min_age": "7d", "actions": { "shrink": { "number_of_shards": 1 } } },
      "cold": { "min_age": "30d", "actions": { "freeze": {} } },
      "delete": { "min_age": "90d", "actions": { "delete": {} } }
    }
  }
}
```

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

### 5. What is Kafka Connect?

**Answer:** Framework for connecting Kafka to external systems.

```json
// Source connector - import data
{
  "name": "jdbc-source",
  "config": {
    "connector.class": "io.confluent.connect.jdbc.JdbcSourceConnector",
    "connection.url": "jdbc:postgresql://localhost:5432/db",
    "topic.prefix": "pg-",
    "mode": "incrementing",
    "incrementing.column.name": "id"
  }
}

// Sink connector - export data
{
  "name": "s3-sink",
  "config": {
    "connector.class": "io.confluent.connect.s3.S3SinkConnector",
    "topics": "my-topic",
    "s3.bucket.name": "my-bucket",
    "format.class": "io.confluent.connect.s3.format.json.JsonFormat"
  }
}
```

### 6. Explain Kafka Streams.

**Answer:** Library for stream processing.

```java
KStream<String, String> stream = builder.stream("input-topic");

// Transform
stream.mapValues(value -> value.toUpperCase())
      .filter((key, value) -> value.length() > 3)
      .to("output-topic");

// Aggregation
KTable<String, Long> counts = stream
    .groupBy((key, value) -> value)
    .count();

// Join
KStream<String, String> joined = stream.join(otherStream, 
    (value1, value2) -> value1 + " " + value2,
    JoinWindows.of(Duration.ofMinutes(5))
);
```

### 7. How to handle schema evolution in Kafka?

**Answer:** Using Schema Registry.

```java
// Avro schema
{
  "type": "record",
  "name": "User",
  "fields": [
    {"name": "id", "type": "int"},
    {"name": "name", "type": "string"},
    {"name": "email", "type": ["null", "string"], "default": null}
  ]
}

// Compatibility modes:
// - BACKWARD: New schema can read old data
// - FORWARD: Old schema can read new data
// - FULL: Both backward and forward compatible
```

### 8. What is Kafka consumer rebalancing?

**Answer:** Redistributing partitions when consumers join/leave.

**Strategies:**
- `RangeAssignor`: Assigns contiguous ranges
- `RoundRobinAssignor`: Distributes evenly
- `StickyAssignor`: Minimizes partition movement
- `CooperativeStickyAssignor`: Incremental rebalancing

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

### 7. What is the difference between fine-tuning and RAG?

**Answer:**

| Fine-tuning | RAG |
|-------------|-----|
| Updates model weights | Keeps model frozen |
| Expensive, requires GPU | Cheaper, faster to implement |
| Good for style/domain adaptation | Good for factual knowledge |
| Static knowledge | Dynamic, updatable knowledge |
| Risk of hallucination | Grounded in retrieved docs |

### 8. How to evaluate LLM applications?

**Answer:**

```python
# Metrics to track
# 1. Relevance - Does output match intent?
# 2. Faithfulness - Is output grounded in context?
# 3. Answer relevance - Is answer direct and clear?
# 4. Context precision - Is relevant info ranked high?

# Using RAGAS
from ragas import evaluate
from ragas.metrics import faithfulness, answer_relevance

results = evaluate(
    dataset,
    metrics=[faithfulness, answer_relevance]
)

# Human evaluation
# A/B testing, preference ranking
```

### 9. What are vector databases?

**Answer:** Databases optimized for similarity search.

```python
# Pinecone
import pinecone
pinecone.init(api_key="key", environment="env")
index = pinecone.Index("my-index")
index.upsert([("id1", embedding, {"metadata": "value"})])
results = index.query(embedding, top_k=5)

# ChromaDB
import chromadb
client = chromadb.Client()
collection = client.create_collection("docs")
collection.add(embeddings=[emb], documents=["doc"], ids=["1"])
results = collection.query(query_embeddings=[emb], n_results=5)

# FAISS (Facebook AI Similarity Search)
import faiss
index = faiss.IndexFlatL2(384)
index.add(embeddings)
distances, indices = index.search(query_embedding, k=5)
```

### 10. Explain LangChain components.

**Answer:**

```python
# Prompts
from langchain.prompts import PromptTemplate
prompt = PromptTemplate.from_template("Summarize: {text}")

# Models
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model="gpt-4", temperature=0.7)

# Chains
from langchain.chains import LLMChain
chain = LLMChain(llm=llm, prompt=prompt)

# Memory
from langchain.memory import ConversationBufferMemory
memory = ConversationBufferMemory()

# Agents
from langchain.agents import initialize_agent, Tool
tools = [Tool(...)]
agent = initialize_agent(tools, llm, agent="zero-shot-react-description")

# Document loaders
from langchain.document_loaders import PyPDFLoader
loader = PyPDFLoader("file.pdf")
docs = loader.load()

# Text splitters
from langchain.text_splitter import RecursiveCharacterTextSplitter
splitter = RecursiveCharacterTextSplitter(chunk_size=1000)
chunks = splitter.split_documents(docs)
```

### 11. What is LangGraph?

**Answer:** Build stateful, multi-actor applications with cycles.

```python
from langgraph.graph import StateGraph, END

# Define state
class State(TypedDict):
    messages: list
    current_step: str

# Create graph
workflow = StateGraph(State)

# Add nodes
workflow.add_node("research", research_node)
workflow.add_node("analyze", analyze_node)
workflow.add_node("write", write_node)

# Add edges
workflow.set_entry_point("research")
workflow.add_edge("research", "analyze")
workflow.add_conditional_edges(
    "analyze",
    lambda x: "write" if x["complete"] else "research",
)
workflow.add_edge("write", END)

app = workflow.compile()
result = app.invoke({"messages": [], "current_step": ""})
```

### 12. How to handle token limits in LLMs?

**Answer:**

```python
# 1. Chunking
from langchain.text_splitter import RecursiveCharacterTextSplitter
splitter = RecursiveCharacterTextSplitter(chunk_size=4000, chunk_overlap=200)

# 2. Map-reduce
from langchain.chains import MapReduceDocumentsChain
# Process chunks in parallel, then combine

# 3. Refine
# Iterate through chunks, refining answer each time

# 4. Map-rerank
# Process all chunks, return highest scored answer

# 5. Sliding window
# Keep only recent context in conversation

# 6. Summary buffer
from langchain.memory import ConversationSummaryBufferMemory
memory = ConversationSummaryBufferMemory(llm=llm, max_token_limit=2000)
```

### 13. What are AI agents and how do they work?

**Answer:**

```python
# ReAct agent (Reason + Act)
from langchain.agents import AgentType, initialize_agent

agent = initialize_agent(
    tools,
    llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)

# Custom agent with tools
tools = [
    SearchTool(),  # Web search
    CalculatorTool(),
    PythonREPLTool(),
    SqlDatabaseToolkit(db),
]

# Agent with memory
from langchain.memory import ConversationBufferMemory
memory = ConversationBufferMemory(memory_key="chat_history")
agent = initialize_agent(tools, llm, memory=memory)

# Plan-and-execute
from langchain.experimental import PlanAndExecute
agent = PlanAndExecute.from_llm_and_tools(llm, tools)
```

### 14. How to deploy LLM applications?

**Answer:**

```python
# FastAPI + Uvicorn
from fastapi import FastAPI
app = FastAPI()

@app.post("/generate")
async def generate(prompt: str):
    response = await llm.agenerate([prompt])
    return {"response": response}

# Run: uvicorn app:app --host 0.0.0.0 --port 8000

# Docker deployment
FROM python:3.11
COPY . /app
RUN pip install -r requirements.txt
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]

# Serverless (AWS Lambda)
# Use Lambda with API Gateway
# Consider model size limits (250MB unzipped)

# Model serving (vLLM, TGI)
# docker run --gpus all ghcr.io/huggingface/text-generation-inference
```

### 15. What are common LLM security concerns?

**Answer:**

- **Prompt injection**: User manipulates prompt to bypass restrictions
- **Data leakage**: Sensitive data in training or context
- **Hallucination**: Model generates incorrect information
- **Rate limiting**: Prevent abuse and control costs
- **Output validation**: Sanitize model outputs

```python
# Mitigation strategies
# 1. Input sanitization
# 2. Output filtering
# 3. System prompts with clear instructions
# 4. Human-in-the-loop for sensitive operations
# 5. Logging and monitoring
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

### 6. Explain AWS Lambda cold starts.

**Answer:** Delay when Lambda initializes a new container.

**Mitigation:**
- Provisioned concurrency
- Keep functions warm with scheduled invocations
- Reduce deployment package size
- Use ARM64 (Graviton2) - faster initialization
- Avoid large dependencies

```python
# Initialize outside handler for reuse
import boto3
dynamodb = boto3.resource('dynamodb')  # Initialized once

def lambda_handler(event, context):
    # Reuses existing connection
    table = dynamodb.Table('my-table')
    return table.scan()
```

### 7. What is AWS SQS and when to use it?

**Answer:** Message queuing service for decoupling components.

```python
import boto3

sqs = boto3.client('sqs')

# Send message
response = sqs.send_message(
    QueueUrl=queue_url,
    MessageBody=json.dumps({'task': 'process'}),
    MessageGroupId='group1'  # For FIFO queues
)

# Receive messages
response = sqs.receive_message(
    QueueUrl=queue_url,
    MaxNumberOfMessages=10,
    WaitTimeSeconds=20  # Long polling
)

# Delete after processing
sqs.delete_message(QueueUrl=queue_url, ReceiptHandle=handle)
```

### 8. Explain AWS DynamoDB.

**Answer:** NoSQL key-value/document database.

```python
import boto3
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Users')

# Put item
table.put_item(Item={'userId': '123', 'name': 'John', 'age': 30})

# Get item
response = table.get_item(Key={'userId': '123'})

# Query
response = table.query(
    KeyConditionExpression=Key('userId').eq('123'),
    FilterExpression=Key('age').gt(18)
)

# Scan (avoid for large tables)
response = table.scan(FilterExpression=Key('status').eq('active'))

# Batch operations
with table.batch_writer() as batch:
    batch.put_item(Item={'userId': '1', 'name': 'A'})
    batch.put_item(Item={'userId': '2', 'name': 'B'})
```

### 9. What is AWS CloudWatch?

**Answer:** Monitoring and observability service.

```python
# Custom metrics
cloudwatch = boto3.client('cloudwatch')
cloudwatch.put_metric_data(
    Namespace='MyApp',
    MetricData=[{
        'MetricName': 'ProcessingTime',
        'Value': 1.25,
        'Unit': 'Seconds'
    }]
)

# Alarms
# CPU > 80% for 5 minutes -> SNS notification
# Custom metric thresholds
# Log metric filters
```

### 10. Explain AWS ECS vs EKS vs Fargate.

**Answer:**

| Service | Description | Use Case |
|---------|-------------|----------|
| ECS | AWS container orchestration | AWS-native, simpler |
| EKS | Managed Kubernetes | Multi-cloud, k8s expertise |
| Fargate | Serverless containers | No infrastructure management |

```yaml
# ECS Task Definition
{
  "family": "my-app",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512",
  "containerDefinitions": [{
    "name": "app",
    "image": "my-app:latest",
    "portMappings": [{"containerPort": 8080}]
  }]
}
```

### 11. What is AWS API Gateway?

**Answer:** Managed API creation and management.

```yaml
# Serverless Framework
functions:
  api:
    handler: handler.api
    events:
      - http:
          path: /users
          method: GET
          cors: true
      - http:
          path: /users/{id}
          method: GET
          authorizer:
            name: jwtAuth
            type: TOKEN
```

**Features:** Rate limiting, authentication, request/response transformation, caching

### 12. Explain AWS Step Functions.

**Answer:** Orchestrate serverless workflows.

```json
{
  "Comment": "Data Processing Workflow",
  "StartAt": "Extract",
  "States": {
    "Extract": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:region:account:function:extract",
      "Next": "Transform"
    },
    "Transform": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:region:account:function:transform",
      "Next": "Load"
    },
    "Load": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:region:account:function:load",
      "End": true
    }
  }
}
```

### 13. What is AWS EventBridge?

**Answer:** Event bus for event-driven architectures.

```python
events = boto3.client('events')

# Put rule
events.put_rule(
    Name='DailyReport',
    ScheduleExpression='cron(0 8 * * ? *)',
    State='ENABLED'
)

# Put target
events.put_targets(
    Rule='DailyReport',
    Targets=[{
        'Id': '1',
        'Arn': 'arn:aws:lambda:region:account:function:generate-report'
    }]
)

# Custom event bus for SaaS integrations
```

### 14. How to implement CI/CD on AWS?

**Answer:**

```yaml
# AWS CodePipeline + CodeBuild
# codebuild-buildspec.yml
version: 0.2
phases:
  install:
    commands:
      - npm install
  build:
    commands:
      - npm test
      - npm run build
  post_build:
    commands:
      - aws s3 sync ./dist s3://my-bucket/
      - aws cloudfront create-invalidation --distribution-id $CF_ID --paths "/*"

# Or use GitHub Actions with AWS OIDC
- name: Configure AWS credentials
  uses: aws-actions/configure-aws-credentials@v2
  with:
    role-to-assume: arn:aws:iam::123456789:role/github-role
    aws-region: us-east-1
```

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
helm rollback myrelease 1
helm list
```

### 6. Explain Kubernetes networking.

**Answer:**

```yaml
# ClusterIP - Internal service
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 8080
  type: ClusterIP

# NodePort - Expose on node IP
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30007

# LoadBalancer - Cloud provider LB
spec:
  type: LoadBalancer

# Ingress - HTTP/S routing
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    - host: api.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```

### 7. What are Kubernetes ConfigMaps and Secrets?

**Answer:**

```yaml
# ConfigMap
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DATABASE_URL: "postgres://localhost/db"
  LOG_LEVEL: "info"

# Secret (base64 encoded)
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  API_KEY: c2VjcmV0LWtleQ==  # echo -n "secret-key" | base64

# Usage in Pod
spec:
  containers:
    - envFrom:
        - configMapRef:
            name: app-config
        - secretRef:
            name: app-secret
      env:
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: DB_PASSWORD
```

### 8. Explain Kubernetes storage.

**Answer:**

```yaml
# PersistentVolume (PV)
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-data
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: standard
  hostPath:
    path: "/mnt/data"

# PersistentVolumeClaim (PVC)
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-data
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: standard

# Usage in Pod
spec:
  containers:
    - volumeMounts:
        - mountPath: "/data"
          name: data-volume
  volumes:
    - name: data-volume
      persistentVolumeClaim:
        claimName: pvc-data
```

### 9. What is a Kubernetes StatefulSet?

**Answer:** For stateful applications with stable identities.

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: "mysql"
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:8.0
          volumeMounts:
            - name: data
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: data
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 10Gi
```

### 10. How to handle Kubernetes autoscaling?

**Answer:**

```yaml
# Horizontal Pod Autoscaler (HPA)
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: myapp-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70

# Vertical Pod Autoscaler (VPA)
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: myapp-vpa
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp
  updatePolicy:
    updateMode: Auto

# Cluster Autoscaler
# Automatically adds/removes nodes based on pod pending state
```

### 11. What are Kubernetes operators?

**Answer:** Custom controllers for managing complex applications.

```yaml
# Custom Resource Definition (CRD)
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: myapps.example.com
spec:
  group: example.com
  versions:
    - name: v1
      served: true
      storage: true
  scope: Namespaced
  names:
    plural: myapps
    singular: myapp
    kind: MyApp

# Operator watches CR and reconciles state
```

### 12. Explain Kubernetes security best practices.

**Answer:**

```yaml
# ServiceAccount with limited permissions
apiVersion: v1
kind: ServiceAccount
metadata:
  name: app-sa

# Role-based access control (RBAC)
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list"]

# Pod security context
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
  containers:
    - securityContext:
        allowPrivilegeEscalation: false
        readOnlyRootFilesystem: true
        capabilities:
          drop: ["ALL"]

# Network Policies
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
```

### 13. How to debug Kubernetes issues?

**Answer:**

```bash
# Check pod status
kubectl get pods
kubectl describe pod <pod-name>

# View logs
kubectl logs <pod-name>
kubectl logs <pod-name> -c <container-name>
kubectl logs -f <pod-name>  # Follow

# Execute commands in pod
kubectl exec -it <pod-name> -- /bin/sh
kubectl exec -it <pod-name> -- env

# Port forwarding
kubectl port-forward <pod-name> 8080:80

# Check events
kubectl get events --sort-by='.lastTimestamp'

# Debug ephemeral containers
kubectl debug -it <pod-name> --image=busybox

# Check resource usage
kubectl top pods
kubectl top nodes
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

### 7. Explain Ethereum Virtual Machine (EVM).

**Answer:** Runtime environment for smart contracts.

**Components:**
- Stack machine (256-bit words)
- Memory (volatile)
- Storage (persistent)
- Gas system (prevents infinite loops)

### 8. What are ERC standards?

**Answer:**

```solidity
// ERC-20 - Token standard
interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address to, uint amount) external returns (bool);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address from, address to, uint amount) external returns (bool);
}

// ERC-721 - NFT standard
interface IERC721 {
    function ownerOf(uint256 tokenId) external view returns (address);
    function safeTransferFrom(address from, address to, uint256 tokenId) external;
    function approve(address to, uint256 tokenId) external;
}

// ERC-1155 - Multi-token standard
// ERC-4626 - Tokenized Vault standard
```

### 9. What is gas in Ethereum?

**Answer:**

```solidity
// Gas costs
// SLOAD: 2100 gas
// SSTORE: 20000 gas (cold) / 2900 gas (warm)
// CALL: 700 gas
// CREATE: 32000 gas

// Optimizing gas
contract Optimized {
    // Use calldata instead of memory for function args
    function test(string calldata s) external {
        // ...
    }
    
    // Pack variables
    uint128 a; // 16 bytes
    uint128 b; // 16 bytes
    uint256 c; // 32 bytes - packed together
    
    // Use events instead of storage
    event Transfer(address indexed from, address indexed to, uint256 amount);
}
```

### 10. What is Web3.js and ethers.js?

**Answer:**

```javascript
// Web3.js
const web3 = new Web3(window.ethereum);
const accounts = await web3.eth.requestAccounts();
const balance = await web3.eth.getBalance(address);
const tx = await contract.methods.transfer(to, amount).send({ from: accounts[0] });

// ethers.js (preferred)
const provider = new ethers.BrowserProvider(window.ethereum);
const signer = await provider.getSigner();
const balance = await provider.getBalance(address);
const contract = new ethers.Contract(address, abi, signer);
const tx = await contract.transfer(to, amount);
```

### 11. What is IPFS and how does it work?

**Answer:**

```javascript
// IPFS uses content-addressed storage
// File content -> hash -> unique CID

// Adding file
const ipfs = window.IpfsHttpClient.create({ host: 'ipfs.infura.io', port: 5001 });
const { path } = await ipfs.add(file);
console.log(path); // QmHash...

// Reading file
const stream = ipfs.cat(cid);
for await (const chunk of stream) {
  // Process chunk
}

// IPNS - mutable content pointers
await ipfs.name.publish(cid); // Publish to IPNS
```

### 12. Explain DeFi concepts.

**Answer:**

```solidity
// AMM (Automated Market Maker)
contract AMM {
    function swap(uint amountIn, address tokenIn) external {
        uint amountOut = getAmountOut(amountIn, tokenIn);
        IERC20(tokenIn).transferFrom(msg.sender, address(this), amountIn);
        IERC20(tokenOut).transfer(msg.sender, amountOut);
    }
}

// Lending protocol
// - Supply collateral
// - Borrow against it
// - Interest accrues over time

// Flash loans
// Borrow without collateral within single transaction
// Must return funds + fee before transaction ends
```

### 13. What is the difference between Layer 1 and Layer 2?

**Answer:**

| Layer 1 | Layer 2 |
|---------|---------|
| Base blockchain (Ethereum) | Built on L1 |
| High security | Lower security |
| Slower, expensive | Faster, cheaper |
| Full decentralization | Some trade-offs |
| Examples: ETH, BTC | Examples: Optimism, Arbitrum, zkSync |

**Rollups:**
- **Optimistic**: Fraud proofs, challenge period
- **ZK**: Validity proofs, instant finality

### 14. What are common smart contract security vulnerabilities?

**Answer:**

```solidity
// 1. Reentrancy attack
// VULNERABLE
function withdraw() public {
    uint bal = balances[msg.sender];
    (bool success, ) = msg.sender.call{value: bal}("");
    balances[msg.sender] = 0;
}

// FIX: Checks-Effects-Interactions
function withdraw() public {
    uint bal = balances[msg.sender];
    balances[msg.sender] = 0; // State first
    (bool success, ) = msg.sender.call{value: bal}("");
}

// 2. Integer overflow (Solidity < 0.8.0)
// FIX: Use SafeMath or Solidity 0.8+

// 3. Access control
// FIX: Add onlyOwner modifier
modifier onlyOwner() {
    require(msg.sender == owner, "Not owner");
    _;
}
```

### 15. How to test smart contracts?

**Answer:**

```javascript
// Hardhat test
const { expect } = require("chai");

describe("Token", function() {
  let token, owner, addr1;

  beforeEach(async function() {
    const Token = await ethers.getContractFactory("Token");
    token = await Token.deploy();
    [owner, addr1] = await ethers.getSigners();
  });

  it("Should transfer tokens", async function() {
    await token.transfer(addr1.address, 100);
    expect(await token.balanceOf(addr1.address)).to.equal(100);
  });

  it("Should fail for insufficient balance", async function() {
    await expect(
      token.connect(addr1).transfer(owner.address, 100)
    ).to.be.revertedWith("Insufficient balance");
  });
});

// Run: npx hardhat test
// Coverage: npx hardhat coverage
```

### 16. What is a DAO?

**Answer:** Decentralized Autonomous Organization - community-governed protocol.

```solidity
contract SimpleDAO {
    struct Proposal {
        string description;
        uint256 votesFor;
        uint256 votesAgainst;
        uint256 deadline;
        bool executed;
    }
    
    mapping(uint256 => Proposal) public proposals;
    mapping(address => uint256) public votingPower;
    uint256 public proposalCount;
    
    function propose(string memory desc, uint256 period) external {
        proposalCount++;
        proposals[proposalCount] = Proposal(desc, 0, 0, block.timestamp + period, false);
    }
    
    function vote(uint256 id, bool support) external {
        Proposal storage p = proposals[id];
        require(block.timestamp < p.deadline, "Ended");
        if (support) p.votesFor += votingPower[msg.sender];
        else p.votesAgainst += votingPower[msg.sender];
    }
    
    function execute(uint256 id) external {
        Proposal storage p = proposals[id];
        require(block.timestamp >= p.deadline && !p.executed);
        require(p.votesFor > p.votesAgainst);
        p.executed = true;
        // Execute proposal logic
    }
}
```

### 17. How to deploy a smart contract?

**Answer:**

```javascript
// Hardhat deploy script
async function main() {
  const Token = await ethers.getContractFactory("Token");
  const token = await Token.deploy("MyToken", "MTK", 1000000);
  await token.waitForDeployment();
  console.log("Deployed to:", await token.getAddress());
}

// Verify on Etherscan
// npx hardhat verify --network mainnet ADDRESS "MyToken" "MTK" 1000000
```

### 18. What is a multisig wallet?

**Answer:** Wallet requiring multiple signatures.

```solidity
contract MultiSig {
    address[] public owners;
    mapping(address => bool) public isOwner;
    uint256 public required;
    
    struct Tx { address to; uint256 value; bytes data; bool executed; }
    Tx[] public transactions;
    mapping(uint256 => mapping(address => bool)) public confirmed;
    
    constructor(address[] memory _owners, uint256 _required) {
        for (uint i = 0; i < _owners.length; i++) isOwner[_owners[i]] = true;
        owners = _owners;
        required = _required;
    }
    
    function submit(address to, uint256 value, bytes memory data) external {
        transactions.push(Tx(to, value, data, false));
    }
    
    function confirm(uint256 txId) external {
        require(isOwner[msg.sender]);
        confirmed[txId][msg.sender] = true;
    }
    
    function execute(uint256 txId) external {
        // Check confirmations >= required, then execute
        Tx storage tx = transactions[txId];
        tx.executed = true;
        (bool ok, ) = tx.to.call{value: tx.value}(tx.data);
        require(ok);
    }
}
```

### 19. What is a token bridge?

**Answer:** Transfer tokens between blockchains.

**Flow:**
1. Lock tokens on source chain
2. Emit event with proof
3. Relayer submits proof to destination
4. Mint/burn tokens on destination

### 20. Explain upgradeable smart contracts.

**Answer:**

```solidity
// Proxy pattern
contract Proxy {
    address public implementation;
    
    fallback() external {
        assembly {
            let ptr := mload(0x40)
            calldatacopy(ptr, 0, calldatasize())
            let result := delegatecall(gas(), sload(implementation.slot), ptr, calldatasize(), 0, 0)
            returndatacopy(ptr, 0, returndatasize())
            switch result case 0 { revert(ptr, returndatasize()) } default { return(ptr, returndatasize()) }
        }
    }
}

// Use OpenZeppelin Upgrades plugin
// npx hardhat upgrade
```

### 21. How to get randomness on-chain?

**Answer:** Use Chainlink VRF.

```solidity
import "@chainlink/contracts/src/v0.8/VRFConsumerBaseV2.sol";

contract Random is VRFConsumerBaseV2 {
    mapping(uint256 => uint256) public randomValues;
    
    function requestRandom() external returns (uint256) {
        return COORDINATOR.requestRandomWords(keyHash, subId, 3, 100000, 1);
    }
    
    function fulfillRandomWords(uint256 reqId, uint256[] memory words) internal override {
        randomValues[reqId] = words[0];
    }
}
```

### 22. What is a Merkle tree used for?

**Answer:** Efficient proof of membership.

**Uses:**
- Airdrop verification
- Whitelist proofs
- Balance proofs

```javascript
// Generate proof
const tree = new MerkleTree(leaves, keccak256);
const proof = tree.getProof(leaf);

// Verify on-chain
function verify(bytes32[] calldata proof, bytes32 leaf, bytes32 root) pure returns (bool) {
    bytes32 hash = leaf;
    for (uint i = 0; i < proof.length; i++) {
        hash = proof[i] < hash ? keccak256(abi.encodePacked(proof[i], hash)) : keccak256(abi.encodePacked(hash, proof[i]));
    }
    return hash == root;
}
```

### 23. What is front-running?

**Answer:** Exploiting transaction ordering for profit.

**Mitigations:**
- Commit-reveal schemes
- Private transactions (Flashbots)
- Batch auctions

### 24. What is a flash loan?

**Answer:** Borrow without collateral, repay in same transaction.

**Uses:**
- Arbitrage
- Collateral swapping
- Debt refinancing

**Fee:** Typically 0.09% (Aave) or 0.3% (Uniswap)

### 25. Explain ERC-1155.

**Answer:** Multi-token standard (fungible + NFT in one contract).

```solidity
interface IERC1155 {
    function balanceOf(address account, uint256 id) external view returns (uint256);
    function safeTransferFrom(address from, address to, uint256 id, uint256 amount, bytes calldata data) external;
    function safeBatchTransferFrom(address from, address to, uint256[] calldata ids, uint256[] calldata amounts, bytes calldata data) external;
}
```

**Benefits:**
- Batch transfers
- Single contract for multiple tokens
- Gas efficient

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

### 9. Design a notification system.

**Answer:**

```
User -> API -> Message Queue (Kafka) -> Workers -> Push/Email/SMS
                                        |
                                   Database (preferences)
                                   Redis (online status)
```

**Key decisions:**
- Separate queues by notification type
- Exponential backoff retry
- Per-user rate limiting

### 10. What is eventual consistency?

**Answer:** System guarantees all replicas will eventually become consistent.

**Use cases:** DNS, caching, social media likes, leaderboards

### 11. Explain message queues vs event streaming.

**Answer:**

| Message Queues | Event Streaming |
|----------------|-----------------|
| Point-to-point | Pub/Sub |
| Message deleted | Messages retained |
| Examples: SQS, RabbitMQ | Examples: Kafka, Pulsar |

### 12. Design a distributed ID generator.

**Answer:**

**Approaches:**
- **UUID**: 128-bit, no coordination needed
- **Snowflake**: 64-bit (timestamp + machine ID + sequence)
- **Database sequences**: Centralized, slow at scale

### 13. What is a circuit breaker?

**Answer:** Pattern to prevent cascading failures.

```python
class CircuitBreaker:
    def __init__(self, threshold=5, timeout=60):
        self.failures = 0
        self.state = "CLOSED"
        self.timeout = timeout
    
    def call(self, func):
        if self.state == "OPEN":
            if time.time() - self.last_failure > self.timeout:
                self.state = "HALF_OPEN"
            else:
                raise Exception("Circuit open")
        try:
            result = func()
            self.failures = 0
            return result
        except:
            self.failures += 1
            if self.failures >= self.threshold:
                self.state = "OPEN"
            raise
```

### 14. Design a feed system (like Twitter).

**Answer:**

**Approaches:**
- **Pull**: Fetch from followed users on load
- **Push**: Fan-out to follower caches on post
- **Hybrid**: Push for active users, pull for celebrities

### 15. Explain consistent hashing.

**Answer:** Distribute data with minimal reorganization when nodes change.

```python
def get_node(key, nodes):
    hash_val = hash(key)
    for i in range(len(nodes) * 100):  # Virtual nodes
        node = nodes[(hash_val + i) % len(nodes)]
        return node
```

### 16. Design a rate limiter.

**Answer:**

**Algorithms:**
- Token Bucket
- Leaky Bucket
- Sliding Window

```python
# Redis-based sliding window
def is_allowed(user_id, limit=100, window=60):
    key = f"rate:{user_id}"
    now = time.time()
    pipe = redis.pipeline()
    pipe.zremrangebyscore(key, 0, now - window)
    pipe.zadd(key, {str(now): now})
    pipe.expire(key, window)
    count = pipe.execute()[1]
    return count <= limit
```

### 17. Design a URL shortener.

**Answer:**

**Requirements:** Shorten URLs, redirect, analytics, custom aliases

**Design:**
```
Client -> LB -> API -> Cache (Redis) -> DB
```

**Key decisions:**
- Base62 encoding for short codes
- Distributed ID generator (Snowflake)
- 301 redirect for SEO

### 18. Design a chat application.

**Answer:**

```
Client <-> WebSocket <-> Message Queue -> Message Store
                         |
                    Cache (online users)
```

**Key decisions:**
- WebSocket for real-time
- Shard by user ID
- Store in NoSQL (write-heavy)

### 19. Design a video streaming service.

**Answer:**

```
Upload -> Transcoding -> CDN -> Client (HLS/DASH)
```

**Key decisions:**
- Adaptive bitrate streaming
- Edge caching for latency
- Separate metadata/analytics stores

### 20. What is load balancing?

**Answer:** Distribute traffic across servers.

**Algorithms:** Round Robin, Least Connections, IP Hash, Weighted

**Tools:** Nginx, HAProxy, AWS ALB

---

## CI/CD

### 1. What is CI/CD?

**Answer:**
- **CI (Continuous Integration)**: Automatically build, test, merge code
- **CD (Continuous Delivery/Deployment)**: Automatically deploy to production

**Pipeline stages:** Source -> Build -> Test -> Deploy

### 2. Explain GitHub Actions.

**Answer:**

```yaml
# .github/workflows/ci.yml
name: CI Pipeline
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm test
      - run: npm run build
```

### 3. How to deploy with GitHub Actions?

**Answer:**

```yaml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Configure AWS
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_KEY }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET }}
          aws-region: us-east-1
      
      - run: aws s3 sync ./dist s3://my-bucket --delete
      - run: aws cloudfront create-invalidation --distribution-id $CF_ID --paths "/*"
```

### 4. What is Jenkins?

**Answer:** Automation server for CI/CD.

```groovy
// Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Build') {
            steps { sh 'npm install && npm run build' }
        }
        stage('Test') {
            steps { sh 'npm test' }
        }
        stage('Deploy') {
            steps { sh 'kubectl apply -f k8s/' }
        }
    }
}
```

### 5. Explain GitLab CI/CD.

**Answer:**

```yaml
# .gitlab-ci.yml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  image: node:20
  script:
    - npm install
    - npm run build

test:
  stage: test
  script:
    - npm test

deploy:
  stage: deploy
  script:
    - docker build -t myapp .
    - docker push myapp
  only:
    - main
```

### 6. CI/CD best practices?

**Answer:**

1. Small, frequent commits
2. Fail fast - run quick tests first
3. Use feature flags
4. Environment parity (dev = staging = prod)
5. Immutable artifacts
6. Rollback strategy
7. Security scanning (SAST, DAST)
8. Monitor deployments

### 7. What is ArgoCD?

**Answer:** GitOps continuous delivery for Kubernetes.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: myapp
spec:
  source:
    repoURL: https://github.com/myorg/myapp.git
    targetRevision: HEAD
    path: k8s/production
  destination:
    server: https://kubernetes.default.svc
    namespace: production
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

### 8. What is container security scanning?

**Answer:** Scan images for vulnerabilities.

```yaml
# GitHub Actions with Trivy
- name: Scan
  uses: aquasecurity/trivy-action@master
  with:
    scan-type: 'fs'
    format: 'sarif'
    output: 'trivy-results.sarif'

- name: Upload
  uses: github/codeql-action/upload-sarif@v3
  with:
    sarif_file: 'trivy-results.sarif'
```

### 9. Canary vs Blue-Green deployment?

**Answer:**

| Canary | Blue-Green |
|--------|------------|
| Gradual rollout to subset | Full swap |
| Monitor metrics | Instant switch |
| Traffic splitting | Complete copy |

```yaml
# Kubernetes canary with weights
annotations:
  nginx.ingress.kubernetes.io/canary: "true"
  nginx.ingress.kubernetes.io/canary-weight: "20"
```

### 10. What is a self-hosted runner?

**Answer:** Your own server running GitHub Actions jobs.

```bash
# Setup
mkdir actions-runner && cd actions-runner
curl -O https://github.com/actions/runner/releases/download/v2.313.0/actions-runner-linux-x64-2.313.0.tar.gz
tar xzf actions-runner-linux-x64-2.313.0.tar.gz
./config.sh --url https://github.com/myorg/myrepo --token TOKEN
./svc.sh install
./svc.sh start
```

### 11. What is Infrastructure as Code (IaC)?

**Answer:** Manage infrastructure via code.

**Tools:**
- Terraform
- CloudFormation
- Pulumi
- Ansible

```yaml
# Terraform
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-bucket"
}

resource "aws_lambda_function" "my_function" {
  function_name = "my-function"
  role          = aws_iam_role.lambda.arn
  handler       = "index.handler"
  runtime       = "nodejs18.x"
}
```

### 12. Explain deployment strategies.

**Answer:**

| Strategy | Description | Downtime |
|----------|-------------|----------|
| Rolling | Update pods gradually | No |
| Blue-Green | Switch traffic instantly | No |
| Canary | Gradual traffic shift | No |
| Recreate | Kill all, create new | Yes |

### 13. How to handle secrets in CI/CD?

**Answer:**

```yaml
# GitHub Secrets
# Store in Settings -> Secrets -> Actions
# Access via ${{ secrets.SECRET_NAME }}

# AWS Secrets Manager
aws secretsmanager get-secret-value --secret-id my-secret

# HashiCorp Vault
vault kv get secret/myapp
```

### 14. What is GitOps?

**Answer:** Git as single source of truth for infrastructure.

**Principles:**
1. Declarative configuration
2. Versioned in Git
3. Automatically synced
4. Continuously reconciled

**Tools:** ArgoCD, Flux, Jenkins X

### 15. How to implement rollback in Kubernetes?

**Answer:**

```bash
# Rollback deployment
kubectl rollout undo deployment/myapp

# Rollback to specific revision
kubectl rollout undo deployment/myapp --to-revision=2

# View rollout history
kubectl rollout history deployment/myapp

# Pause rollout
kubectl rollout pause deployment/myapp

# Resume rollout
kubectl rollout resume deployment/myapp
```

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
