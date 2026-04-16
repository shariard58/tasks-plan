# JS Phase 02 — Data Types, Structures & Algorithms (Tasks 31-60)

> Data ছাড়া programming নেই। Data কিভাবে store হয়, access হয়, manipulate হয় — সব এখানে।

---

## Section A: Primitive Types Deep Dive (Tasks 31-38)

### Task 31 — Number Type Internals 🟡

- IEEE 754 double-precision:
  - কেন `0.1 + 0.2 !== 0.3` — binary floating point limitation
  - `Number.MAX_SAFE_INTEGER` (2^53 - 1) ও `Number.MIN_SAFE_INTEGER`
  - `Infinity`, `-Infinity`, `NaN`
  - `NaN !== NaN` — কেন? How to check: `Number.isNaN()` vs `isNaN()`
  - `Number.isFinite()`, `Number.isInteger()`, `Number.isSafeInteger()`
  - **Build:** নিজের `isApproximatelyEqual(a, b, epsilon)` function
  - **Build:** Currency calculator (avoid floating point issues — use integers for cents)

### Task 32 — BigInt — Arbitrary Precision 🟢

- BigInt:
  - `const big = 9007199254740993n;`
  - BigInt + Number mix করা যায় না (TypeError)
  - BigInt comparisons, math operations
  - **Build:** Large number calculator (factorial of 1000, fibonacci of 10000)
  - Performance comparison: BigInt vs Number

### Task 33 — String Internals & Unicode 🟡

- String deep dive:
  - Strings are immutable in JS
  - UTF-16 encoding internally
  - `"😀".length === 2` — কেন? (surrogate pairs)
  - `String.fromCodePoint()`, `codePointAt()`
  - Template literals internals (tagged templates)
  - String interning (engine optimization)
  - **Build:** Tagged template function:
    1. `html` tag — sanitizes HTML in template literals
    2. `sql` tag — parameterized SQL (prevents injection)
    3. `css` tag — CSS-in-JS concept
  - **Build:** Unicode-aware string functions (length, reverse, slice)

### Task 34 — Symbol — Unique Identifiers 🟡

- Symbol deep dive:
  - `Symbol('desc')` — always unique
  - `Symbol.for('key')` — global symbol registry
  - Symbol as object property key (hidden from `for...in`, `Object.keys`)
  - Well-known symbols:
    - `Symbol.iterator` — make object iterable
    - `Symbol.toPrimitive` — custom type conversion
    - `Symbol.hasInstance` — custom `instanceof`
    - `Symbol.toStringTag` — custom `toString()` output
    - `Symbol.species` — constructor for derived objects
  - **Build:** 5 examples using each well-known symbol

### Task 35 — Object Internals 🔴

- Object deep dive:
  - Property descriptors: `value`, `writable`, `enumerable`, `configurable`
  - `Object.defineProperty()` — define single property
  - `Object.defineProperties()` — define multiple
  - `Object.getOwnPropertyDescriptor()`
  - `Object.getOwnPropertyDescriptors()`
  - Accessor properties (get/set):
    ```js
    const user = {
      _name: 'John',
      get name() {
        return this._name;
      },
      set name(val) {
        if (val.length < 2) throw Error('Too short');
        this._name = val;
      },
    };
    ```
  - Property enumeration: `for...in`, `Object.keys`, `Object.values`, `Object.entries`, `Object.getOwnPropertyNames`, `Object.getOwnPropertySymbols`
  - **Build:** Reactive object (Vue.js-like reactivity) using getters/setters

### Task 36 — Object Methods Mastery 🟡

- All important Object methods:
  - `Object.assign()` — shallow copy, merge
  - `Object.create()` — create with prototype
  - `Object.freeze()` — immutable (shallow)
  - `Object.seal()` — no add/delete, can modify
  - `Object.preventExtensions()` — no add, can modify/delete
  - `Object.is()` — strict equality (but `NaN === NaN`, `+0 !== -0`)
  - `Object.fromEntries()` — entries → object
  - `structuredClone()` — deep clone (modern)
  - **Build:** Deep freeze function (recursive freeze)
  - **Build:** Deep clone function (handle circular references)

### Task 37 — WeakRef & FinalizationRegistry 🔴

- Weak references:
  - `WeakRef` — reference that doesn't prevent GC
  - `deref()` — get value (or undefined if GC'd)
  - `FinalizationRegistry` — callback when object is GC'd
  - Use case: caching without memory leaks
  - **Build:** Cache with auto-cleanup:
    ```js
    class WeakCache {
      // Stores WeakRef to values
      // Auto-cleans when values are GC'd
      // FinalizationRegistry to track cleanup
    }
    ```

### Task 38 — Immutability Patterns 🟡

- Immutable data in JS:
  - `Object.freeze()` (shallow)
  - Spread operator `{...obj}` (shallow copy)
  - `structuredClone()` (deep copy)
  - Immutable update patterns:
    - Add to array: `[...arr, newItem]`
    - Remove from array: `arr.filter(x => x.id !== id)`
    - Update in array: `arr.map(x => x.id === id ? {...x, name: 'new'} : x)`
    - Nested update: `{...obj, nested: {...obj.nested, key: 'new'}}`
  - **Build:** Immer-like `produce()` function (simplified)

---

## Section B: Built-in Data Structures (Tasks 39-48)

### Task 39 — Array Internals 🟡

- Array under the hood:
  - JS array is actually a special object (hash map with numeric keys)
  - Dense vs Sparse arrays: `[1,,3]` (sparse — slow)
  - Array-like objects (`arguments`, `NodeList`, `HTMLCollection`)
  - `Array.from()` — convert array-like to real array
  - `Array.isArray()` vs `instanceof Array` (cross-realm issue)
  - **Benchmark:** Dense array vs sparse array vs object — performance comparison

### Task 40 — Array Methods — Implement All From Scratch 🔴

- নিজে implement করো (prototype method হিসেবে):
  - Mutating: `push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`, `fill`, `copyWithin`
  - Non-mutating: `map`, `filter`, `reduce`, `reduceRight`, `find`, `findIndex`, `findLast`, `findLastIndex`, `some`, `every`, `includes`, `indexOf`, `lastIndexOf`, `flat`, `flatMap`, `slice`, `concat`, `join`
  - Iterator: `forEach`, `entries`, `keys`, `values`
  - Static: `Array.from`, `Array.of`, `Array.isArray`
  - **Each with:** implementation + test cases + edge cases
  - **Output:** Complete array method polyfill library

### Task 41 — Map vs Object 🟡

- Map deep dive:
  - Map: any type as key (including objects, functions)
  - Object: only string/symbol as key
  - Map: preserves insertion order
  - Map: `size` property (O(1))
  - Map: better for frequent add/delete
  - Map methods: `set`, `get`, `has`, `delete`, `clear`, `forEach`, `entries`, `keys`, `values`
  - **Build:** Benchmarking page — Map vs Object for different operations (100, 1000, 10000 items)
  - **When to use Map:** frequent CRUD, non-string keys, need size, need iteration order

### Task 42 — Set & Unique Operations 🟡

- Set deep dive:
  - Only unique values
  - Set methods: `add`, `has`, `delete`, `clear`, `forEach`, `entries`, `keys`, `values`
  - New set methods (ES2025): `union`, `intersection`, `difference`, `symmetricDifference`, `isSubsetOf`, `isSupersetOf`, `isDisjointFrom`
  - **Build:** Set operations polyfills (union, intersection, difference)
  - **Build:** Unique array utility: `uniqueBy(array, keyFn)`

### Task 43 — WeakMap & WeakSet 🟡

- Weak collections:
  - Keys must be objects (WeakMap) / values must be objects (WeakSet)
  - No enumeration (can't iterate)
  - Allows garbage collection of keys/values
  - Use cases:
    - Private data storage per object
    - Cache per object (auto-cleanup)
    - DOM element metadata
    - Object tagging (processed/visited)
  - **Build:** Private members using WeakMap:
    ```js
    const _private = new WeakMap();
    class User {
      constructor(name) {
        _private.set(this, { name });
      }
      getName() {
        return _private.get(this).name;
      }
    }
    ```

### Task 44 — Typed Arrays & ArrayBuffer 🟡

- Binary data handling:
  - `ArrayBuffer` — raw binary data
  - `DataView` — read/write specific formats
  - Typed Arrays: `Uint8Array`, `Int16Array`, `Float32Array`, `Float64Array`
  - **Build:**
    1. Binary file reader (parse BMP image header)
    2. Simple audio generator (create WAV file in browser)
    3. Binary protocol parser (custom packet format)

### Task 45 — Implement Stack, Queue, LinkedList 🟡

- Data structures in JS:
  - **Stack:** push, pop, peek, isEmpty, size
  - **Queue:** enqueue, dequeue, front, isEmpty, size
  - **Priority Queue:** enqueue with priority, dequeue highest priority
  - **Linked List:** append, prepend, insert, remove, find, reverse, toArray
  - **Doubly Linked List:** all above + traverseBackward
  - Visual representation render করো DOM এ
  - **Output:** Interactive data structure visualizer

### Task 46 — Implement Hash Table 🔴

- Hash table from scratch:
  - Hash function implementation
  - Collision handling: chaining (linked list) ও open addressing (linear probing)
  - Methods: set, get, delete, has, keys, values, entries
  - Resize (when load factor > 0.75)
  - **Benchmark:** Custom Hash Table vs native Map vs Object
  - **Output:** Working hash table with collision visualization

### Task 47 — Implement Tree & Graph 🔴

- Tree structures:
  - **Binary Search Tree:** insert, search, delete, inOrder, preOrder, postOrder, min, max
  - **Trie:** insert, search, startsWith, autoComplete
  - **Graph:** addVertex, addEdge, BFS, DFS, shortestPath
  - **Build:** File system tree navigator (like VS Code sidebar)
  - **Build:** Autocomplete using Trie
  - **Output:** Tree/Graph visualizer (render as SVG/Canvas)

### Task 48 — Sorting Algorithms Visualization 🟡

- Implement ও visualize:
  - Bubble Sort, Selection Sort, Insertion Sort
  - Merge Sort, Quick Sort, Heap Sort
  - Counting Sort, Radix Sort
  - Comparison chart: time complexity, space complexity, stability
  - **Build:** Sorting algorithm visualizer (animated bars)
  - **Build:** Benchmark page — sort 10k, 100k, 1M items — time comparison
  - Native `Array.sort()` — Timsort implementation, comparator function

---

## Section C: Regular Expressions (Tasks 49-54)

### Task 49 — Regex Fundamentals 🟡

- Regular expression deep dive:
  - Literals vs constructor: `/pattern/flags` vs `new RegExp('pattern', 'flags')`
  - Flags: `g` (global), `i` (case-insensitive), `m` (multiline), `s` (dotAll), `u` (unicode), `d` (indices), `v` (unicodeSets)
  - Character classes: `\d`, `\w`, `\s`, `\b`, `.`
  - Quantifiers: `*`, `+`, `?`, `{n}`, `{n,}`, `{n,m}`
  - Anchors: `^`, `$`
  - Groups: `()`, `(?:)`, `(?<name>)`
  - Alternation: `|`
  - Lookahead/Lookbehind: `(?=)`, `(?!)`, `(?<=)`, `(?<!)`
  - **Output:** Regex cheat sheet page

### Task 50 — Regex Methods in JavaScript 🟡

- All regex-related methods:
  - `RegExp.prototype.test()` — returns boolean
  - `RegExp.prototype.exec()` — returns match array (with lastIndex)
  - `String.prototype.match()` — returns matches
  - `String.prototype.matchAll()` — returns iterator of all matches
  - `String.prototype.search()` — returns index
  - `String.prototype.replace()` — replace with string/function
  - `String.prototype.replaceAll()` — replace all
  - `String.prototype.split()` — split by regex
  - **Challenge:** 20টা practical regex problems solve করো

### Task 51 — Build a Regex Tester 🟡

- Interactive regex tester (like regex101):
  - Input: regex pattern + flags
  - Input: test string
  - Real-time match highlighting
  - Match groups display
  - Named groups display
  - Match count
  - Replace preview
  - Common patterns library (email, phone, URL, etc.)
  - **Output:** Full regex tester tool

### Task 52 — Practical Regex Patterns 🟡

- Build validated patterns for:
  - Email validation
  - Phone number (BD + international)
  - URL validation
  - Password strength (min 8, upper, lower, number, special)
  - Date format (DD/MM/YYYY)
  - IP address (v4 and v6)
  - Credit card number
  - HTML tag matching
  - Markdown to HTML converter (simple)
  - **Output:** Regex patterns library with tests

---

## Section D: JSON, Date, Error (Tasks 53-60)

### Task 53 — JSON Deep Dive 🟡

- JSON internals:
  - `JSON.stringify()` — replacer function, space parameter
  - `JSON.parse()` — reviver function
  - toJSON() method — custom serialization
  - What JSON can't serialize: undefined, functions, Symbol, Infinity, NaN, circular references
  - **Build:**
    1. Deep JSON diff (compare two objects, show differences)
    2. JSON formatter/beautifier
    3. JSON to TypeScript type generator
    4. Circular reference safe stringifier

### Task 54 — Date & Time Mastery 🟡

- Date deep dive:
  - `Date` object internals (Unix timestamp — ms since Jan 1, 1970)
  - Date creation: `new Date()`, `new Date(ms)`, `new Date(string)`, `new Date(y,m,d,h,m,s,ms)`
  - Parsing quirks (inconsistent across browsers)
  - UTC vs Local time
  - `Date.now()` — current timestamp
  - Temporal API (upcoming — future of JS dates)
  - **Build:**
    1. Date formatting library (like date-fns but minimal)
    2. Relative time function ("2 hours ago", "in 3 days")
    3. Timezone converter
    4. Calendar date calculations (days in month, day of week, etc.)

### Task 55 — Intl API — Internationalization 🟡

- Built-in i18n:
  - `Intl.NumberFormat` — number/currency formatting
  - `Intl.DateTimeFormat` — date formatting
  - `Intl.RelativeTimeFormat` — "2 days ago"
  - `Intl.Collator` — locale-aware string comparison
  - `Intl.PluralRules` — pluralization
  - `Intl.ListFormat` — "A, B, and C"
  - `Intl.Segmenter` — text segmentation (word, sentence)
  - **Build:** Multi-locale formatter:
    ```js
    formatCurrency(1234.56, 'BDT', 'bn-BD'); // "১,২৩৪.৫৬৳"
    formatCurrency(1234.56, 'USD', 'en-US'); // "$1,234.56"
    ```

### Task 56 — Error Handling Mastery 🟡

- Error system:
  - Error types: `Error`, `TypeError`, `ReferenceError`, `SyntaxError`, `RangeError`, `URIError`, `EvalError`
  - Custom Error classes:
    ```js
    class ValidationError extends Error {
      constructor(field, message) {
        super(message);
        this.name = 'ValidationError';
        this.field = field;
      }
    }
    ```
  - `try/catch/finally` — flow control
  - Error stack trace reading
  - **Build:** Error handling framework:
    - Custom error classes (5 types)
    - Global error handler
    - Error serialization (for logging)
    - Error recovery patterns

### Task 57 — Iterators & Iterables 🔴

- Iterator protocol:
  - Iterable: object with `[Symbol.iterator]()` method
  - Iterator: object with `next()` method returning `{value, done}`
  - Built-in iterables: String, Array, Map, Set, arguments, NodeList
  - `for...of` loop uses iterator protocol
  - Spread operator `...` uses iterator protocol
  - Destructuring uses iterator protocol
  - **Build:**
    1. Range iterator: `for (const n of range(1, 10)) {...}`
    2. Infinite sequence: `fibonacci()` iterator
    3. Lazy evaluation: `lazy(array).map(fn).filter(fn).take(5)`
    4. Custom iterable class (LinkedList that works with `for...of`)

### Task 58 — Generators 🔴

- Generator functions:
  - `function*` syntax
  - `yield` expression — pause ও resume
  - `yield*` — delegate to another generator
  - Two-way communication: `generator.next(value)` sends value INTO generator
  - `generator.return()` ও `generator.throw()`
  - **Build:**
    1. Infinite ID generator
    2. Paginated data fetcher (yield each page)
    3. State machine using generator
    4. Coroutine pattern (cooperative multitasking)
    5. Async generator + `for await...of`

### Task 59 — Destructuring & Spread — Every Pattern 🟡

- All destructuring patterns:
  - Array destructuring: `[a, b, ...rest] = arr`
  - Object destructuring: `{name, age, ...rest} = obj`
  - Default values: `{name = 'anon'} = obj`
  - Rename: `{name: userName} = obj`
  - Nested: `{address: {city}} = obj`
  - Function parameter destructuring
  - Computed property destructuring
  - Swap variables: `[a, b] = [b, a]`
  - Spread in function call: `fn(...args)`
  - Spread in array: `[...arr1, ...arr2]`
  - Spread in object: `{...obj1, ...obj2}` (shallow merge)
  - Rest parameters vs `arguments`
  - **Challenge:** 20টা complex destructuring problems

### Task 60 — Collection Algorithms & Performance 🔴

- Algorithm practice with JS collections:
  - Two-pointer technique
  - Sliding window
  - Hash map for O(1) lookup
  - Binary search on sorted array
  - BFS/DFS on graph (Map of Sets)
  - **Problems:**
    1. Two Sum (hash map approach)
    2. Maximum subarray (Kadane's)
    3. Merge intervals
    4. Group anagrams
    5. LRU Cache (Map + Doubly Linked List)
    6. Word frequency counter
    7. Find duplicate in array
    8. Flatten deeply nested object
    9. Deep equality check
    10. Object path accessor (`get(obj, 'a.b[0].c')`)
  - **Output:** Each with time/space complexity analysis

---

## ✅ Phase 02 Completion Checklist

- [ ] Number internals ও floating point issues বোঝো
- [ ] String Unicode ও encoding বোঝো
- [ ] Symbol ও well-known symbols জানো
- [ ] Object property descriptors ও all methods জানো
- [ ] Array methods নিজে implement করতে পারো
- [ ] Map, Set, WeakMap, WeakSet কখন কোনটা ব্যবহার করবে জানো
- [ ] Stack, Queue, LinkedList, HashTable, Tree নিজে বানাতে পারো
- [ ] Sorting algorithms implement ও compare করতে পারো
- [ ] Regular expressions confidently ব্যবহার করতে পারো
- [ ] JSON, Date, Intl, Error handling ভালো পারো
- [ ] Iterators ও Generators বোঝো ও ব্যবহার করতে পারো

> ✅ Ready? → **JS Phase 03: Functions Deep Dive**
