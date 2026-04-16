# JS Phase 04 — OOP, Prototypes & Design Patterns (Tasks 91-120)

> JavaScript এর OOP unique — prototype-based inheritance। এটা properly না বুঝলে JS কখনো master করা যাবে না।

---

## Section A: Prototype System (Tasks 91-100)

### Task 91 — `__proto__` vs `prototype` 🔴

- Prototype chain fundamentals:
  - Every object has `[[Prototype]]` (internal, accessed via `__proto__` or `Object.getPrototypeOf()`)
  - Only functions have `.prototype` property
  - `__proto__` = "I inherit from..." (instance)
  - `.prototype` = "Objects I create will inherit from..." (constructor)
  - **Diagram:**

    ```
    function Dog(name) { this.name = name; }
    const dog = new Dog('Tommy');

    dog.__proto__ === Dog.prototype  ✅
    Dog.prototype.__proto__ === Object.prototype  ✅
    Object.prototype.__proto__ === null  ✅ (end of chain)
    Dog.__proto__ === Function.prototype  ✅
    ```

  - **Build:** Interactive prototype chain visualizer (DOM-based)

### Task 92 — Prototype Chain Lookup 🔴

- How property lookup works:
  1. Check own properties
  2. Check `__proto__`
  3. Check `__proto__.__proto__`
  4. ... until `null`
  - `hasOwnProperty()` vs `in` operator
  - `Object.hasOwn()` (ES2022 — better alternative)
  - Property shadowing: own property "hides" prototype property
  - **Code:**
    ```js
    const animal = {
      speak() {
        return 'generic';
      },
    };
    const dog = Object.create(animal);
    dog.speak = function () {
      return 'woof';
    }; // shadows
    dog.speak(); // 'woof'
    delete dog.speak;
    dog.speak(); // 'generic' (from prototype)
    ```
  - **Challenge:** 15 prototype chain puzzles — predict the output

### Task 93 — `new` Keyword — What Actually Happens 🔴

- When you write `new Dog('Tommy')`:
  1. Empty object created: `{}`
  2. `__proto__` set to `Dog.prototype`
  3. `Dog` called with `this` = new object
  4. If Dog returns object → that object returned; else → new object returned
  - **Build:** `myNew` function:
    ```js
    function myNew(Constructor, ...args) {
      const obj = Object.create(Constructor.prototype);
      const result = Constructor.apply(obj, args);
      return result instanceof Object ? result : obj;
    }
    ```
  - **Test:** `myNew(Dog, 'Tommy')` should work exactly like `new Dog('Tommy')`

### Task 94 — `Object.create()` Deep Dive 🟡

- Creating objects with specific prototype:
  - `Object.create(proto)` — create object with given prototype
  - `Object.create(null)` — no prototype at all (pure dictionary)
  - `Object.create(proto, propertyDescriptors)` — with properties
  - **Use case:** Inheritance without constructor:

    ```js
    const animal = {
      init(name) {
        this.name = name;
        return this;
      },
      speak() {
        return `${this.name} speaks`;
      },
    };
    const dog = Object.create(animal);
    dog.bark = function () {
      return 'Woof!';
    };

    const myDog = Object.create(dog).init('Tommy');
    myDog.speak(); // "Tommy speaks"
    myDog.bark(); // "Woof!"
    ```

  - **Build:** OLOO (Objects Linking to Other Objects) pattern demo

### Task 95 — Constructor Functions & Prototypal Inheritance 🔴

- Pre-class inheritance:

  ```js
  function Animal(name) {
    this.name = name;
  }
  Animal.prototype.speak = function () {
    return `${this.name} speaks`;
  };

  function Dog(name, breed) {
    Animal.call(this, name); // super()
    this.breed = breed;
  }
  Dog.prototype = Object.create(Animal.prototype);
  Dog.prototype.constructor = Dog; // fix constructor
  Dog.prototype.bark = function () {
    return 'Woof!';
  };
  ```

  - Why `Object.create` instead of `new Animal()`
  - Constructor property fixing
  - `instanceof` check: `dog instanceof Dog`, `dog instanceof Animal`
  - **Build:** Complete class hierarchy: Shape → Circle, Rectangle, Triangle

### Task 96 — ES6 Classes — Syntactic Sugar 🟡

- Class syntax:

  ```js
  class Animal {
    constructor(name) {
      this.name = name;
    }
    speak() {
      return `${this.name} speaks`;
    }
  }

  class Dog extends Animal {
    constructor(name, breed) {
      super(name);
      this.breed = breed;
    }
    bark() {
      return 'Woof!';
    }
  }
  ```

  - Classes are still functions! (`typeof Dog === 'function'`)
  - Methods go on prototype (not instance)
  - `super` keyword: `super()` (constructor), `super.method()` (parent method)
  - Static methods: `static create() {}`
  - Getters/Setters in class
  - **Proof:** Show that class ও constructor function create identical prototype chains

### Task 97 — Private Fields & Methods 🟡

- Encapsulation in JS:
  - Convention: `_private` (prefix underscore)
  - Closure-based: WeakMap for private data
  - Native: `#private` fields (ES2022):
    ```js
    class BankAccount {
      #balance = 0;
      #pin;

      constructor(pin) {
        this.#pin = pin;
      }

      deposit(amount) {
        this.#balance += amount;
      }
      withdraw(amount, pin) {
        if (pin !== this.#pin) throw new Error('Invalid PIN');
        this.#balance -= amount;
      }

      get balance() {
        return this.#balance;
      }

      #validateAmount(amount) {
        return amount > 0;
      } // private method
      static #instanceCount = 0; // private static
    }
    ```
  - Private in subclass: can't access parent's #private
  - **Build:** Bank account system with full private fields

### Task 98 — Static Members & Factory Pattern 🟡

- Static in JS:
  - `static` methods — on class itself, not instances
  - `static` fields — shared data
  - `static` initialization blocks (ES2022):
    ```js
    class Config {
      static #data;
      static {
        // Complex initialization
        this.#data = loadConfig();
      }
    }
    ```
  - Factory pattern:
    ```js
    class User {
      constructor(name, role) {
        this.name = name;
        this.role = role;
      }
      static createAdmin(name) {
        return new User(name, 'admin');
      }
      static createGuest() {
        return new User('Guest', 'guest');
      }
    }
    ```
  - **Build:** Shape factory — `Shape.create('circle', {radius: 5})`

### Task 99 — Mixins — Multiple Inheritance in JS 🔴

- JS has single inheritance, but mixins solve it:

  ```js
  const Serializable = (Base) =>
    class extends Base {
      serialize() {
        return JSON.stringify(this);
      }
      static deserialize(json) {
        return Object.assign(new this(), JSON.parse(json));
      }
    };

  const Validatable = (Base) =>
    class extends Base {
      validate() {
        /* validation logic */
      }
    };

  class User extends Serializable(Validatable(Base)) {
    // Has serialize(), deserialize(), validate()
  }
  ```

  - Object.assign mixin: `Object.assign(Dog.prototype, Swimmable, Fetchable)`
  - **Build:** 5 useful mixins: Serializable, Validatable, EventEmitter, Comparable, Cloneable
  - **Challenge:** Diamond problem — how JS handles it

### Task 100 — `instanceof` ও `Symbol.hasInstance` 🟡

- How `instanceof` works:
  - Walks up prototype chain looking for `.prototype`
  - `dog instanceof Dog` → `Dog.prototype` in `dog`'s chain?
  - Cross-realm issues (different iframes)
  - Custom `instanceof` with `Symbol.hasInstance`:
    ```js
    class Even {
      static [Symbol.hasInstance](num) {
        return num % 2 === 0;
      }
    }
    4 instanceof Even; // true
    5 instanceof Even; // false
    ```
  - **Build:** Type checking system using `Symbol.hasInstance`

---

## Section B: Design Patterns in JavaScript (Tasks 101-115)

### Task 101 — Singleton Pattern 🟡

- One instance only:
  ```js
  class Database {
    static #instance;
    constructor() {
      if (Database.#instance) return Database.#instance;
      Database.#instance = this;
      this.connection = null;
    }
    static getInstance() {
      if (!this.#instance) this.#instance = new Database();
      return this.#instance;
    }
  }
  ```

  - Module as singleton (ES modules are singletons by default)
  - **Build:** Config manager singleton, Logger singleton

### Task 102 — Factory & Abstract Factory 🟡

- Factory method:
  ```js
  function createNotification(type, message) {
    switch (type) {
      case 'email':
        return new EmailNotification(message);
      case 'sms':
        return new SMSNotification(message);
      case 'push':
        return new PushNotification(message);
    }
  }
  ```

  - Abstract Factory — family of related objects
  - **Build:** UI component factory (creates Button, Input, Card based on theme)
  - **Build:** Database adapter factory (MySQL, PostgreSQL, MongoDB)

### Task 103 — Observer Pattern 🟡

- Subject notifies observers:
  ```js
  class Subject {
    #observers = new Set();
    subscribe(observer) {
      this.#observers.add(observer);
    }
    unsubscribe(observer) {
      this.#observers.delete(observer);
    }
    notify(data) {
      this.#observers.forEach((obs) => obs.update(data));
    }
  }
  ```

  - **Build:** Stock price ticker — price changes → UI updates
  - **Build:** Todo app model with observer pattern

### Task 104 — Strategy Pattern 🟡

- Swap algorithms at runtime:

  ```js
  class Sorter {
    #strategy;
    setStrategy(strategy) {
      this.#strategy = strategy;
    }
    sort(data) {
      return this.#strategy.sort(data);
    }
  }

  const bubbleSort = {
    sort(data) {
      /* ... */
    },
  };
  const quickSort = {
    sort(data) {
      /* ... */
    },
  };

  sorter.setStrategy(quickSort);
  sorter.sort(data);
  ```

  - **Build:** Payment processor (CreditCard, BDTWallet, bKash strategies)
  - **Build:** Validator with swappable validation strategies

### Task 105 — Decorator Pattern 🟡

- Add behavior without changing original:

  ```js
  function withLogging(fn) {
    return function (...args) {
      console.log(`Calling ${fn.name} with`, args);
      const result = fn(...args);
      console.log(`Result:`, result);
      return result;
    };
  }

  const add = (a, b) => a + b;
  const loggedAdd = withLogging(add);
  ```

  - TC39 decorators proposal (stage 3):
    ```js
    @logged
    class MyClass {
      @cached
      getData() {
        /* ... */
      }
    }
    ```
  - **Build:** 5 useful function decorators: `@logged`, `@timed`, `@cached`, `@retry`, `@deprecated`

### Task 106 — Adapter Pattern 🟡

- Make incompatible interfaces work together:
  - **Build:** API adapter:
    ```js
    // Old API returns XML-like format
    // New code expects JSON
    class APIAdapter {
      constructor(oldAPI) {
        this.oldAPI = oldAPI;
      }
      async getUser(id) {
        const xml = await this.oldAPI.fetchUser(id);
        return this.xmlToJson(xml);
      }
    }
    ```
  - **Build:** Storage adapter (LocalStorage, SessionStorage, Cookies — same interface)

### Task 107 — Proxy Pattern (not JS Proxy) 🟡

- Control access:
  - Virtual Proxy: lazy loading
  - Protection Proxy: access control
  - Cache Proxy: cached results
  - **Build:** Image lazy loader:
    ```js
    class ImageProxy {
      constructor(url) {
        this.url = url;
        this.image = null;
      }
      display() {
        if (!this.image) {
          console.log('Loading...');
          this.image = new RealImage(this.url);
        }
        this.image.display();
      }
    }
    ```
  - **Build:** API proxy with rate limiting ও caching

### Task 108 — Builder Pattern 🟡

- Step-by-step construction:
  ```js
  class QueryBuilder {
    #query = {};

    select(...fields) {
      this.#query.select = fields;
      return this;
    }
    from(table) {
      this.#query.from = table;
      return this;
    }
    where(condition) {
      this.#query.where = condition;
      return this;
    }
    orderBy(field, dir = 'ASC') {
      this.#query.orderBy = { field, dir };
      return this;
    }
    limit(n) {
      this.#query.limit = n;
      return this;
    }
    build() {
      return this.#query;
    }
  }
  ```

  - **Build:** HTML element builder
  - **Build:** Form builder (dynamic forms)
  - **Build:** Configuration builder

### Task 109 — Iterator Pattern (Custom) 🟡

- Sequential access without exposing internals:
  - **Build:** Collection with custom iterator:

    ```js
    class TreeCollection {
      [Symbol.iterator]() {
        // BFS or DFS traversal
      }
      bfs() {
        return {
          [Symbol.iterator]: () => {
            /* BFS */
          },
        };
      }
      dfs() {
        return {
          [Symbol.iterator]: () => {
            /* DFS */
          },
        };
      }
    }

    for (const node of tree.bfs()) {
      /* ... */
    }
    ```

  - **Build:** Paginated iterator — fetches next page on demand

### Task 110 — Chain of Responsibility 🟡

- Pass request along chain:

  ```js
  class Handler {
    setNext(handler) { this.next = handler; return handler; }
    handle(request) {
      if (this.next) return this.next.handle(request);
      return null;
    }
  }

  class AuthHandler extends Handler { ... }
  class ValidationHandler extends Handler { ... }
  class RateLimitHandler extends Handler { ... }
  ```

  - **Build:** Request processing chain (auth → validate → rate-limit → process)
  - **Build:** Logging level chain (error → warn → info → debug)

### Task 111 — Mediator Pattern 🟡

- Centralized communication:
  - Objects don't talk directly — go through mediator
  - **Build:** Chat room mediator:
    ```js
    class ChatRoom {
      send(message, from, to) {
        to.receive(message, from);
      }
      broadcast(message, from) {
        /* send to all except from */
      }
    }
    ```
  - **Build:** Form mediator — fields depend on each other

### Task 112 — Facade Pattern 🟢

- Simple interface for complex subsystem:
  - **Build:** API Facade:
    ```js
    class ShoppingFacade {
      constructor() {
        this.inventory = new InventorySystem();
        this.payment = new PaymentSystem();
        this.shipping = new ShippingSystem();
      }

      async purchaseItem(item, user) {
        const available = this.inventory.check(item);
        if (!available) throw new Error('Out of stock');
        await this.payment.charge(user, item.price);
        await this.shipping.schedule(item, user.address);
        this.inventory.decrease(item);
        return { success: true };
      }
    }
    ```

### Task 113 — Repository Pattern 🟡

- Abstract data access:
  ```js
  class UserRepository {
    constructor(dataSource) {
      this.ds = dataSource;
    }
    async findById(id) {
      return this.ds.query('users', { id });
    }
    async findAll(filter) {
      return this.ds.query('users', filter);
    }
    async save(user) {
      return this.ds.insert('users', user);
    }
    async delete(id) {
      return this.ds.remove('users', { id });
    }
  }
  ```

  - **Build:** In-memory ও localStorage repository implementations
  - Easy to swap data sources (LocalStorage → IndexedDB → API)

### Task 114 — Module Pattern (Pre-ES6) 🟡

- IIFE-based module:
  ```js
  const CounterModule = (function () {
    let count = 0; // private

    return {
      increment() {
        count++;
      },
      decrement() {
        count--;
      },
      getCount() {
        return count;
      },
    };
  })();
  ```

  - Revealing Module Pattern
  - CommonJS: `module.exports` / `require()`
  - AMD: `define()` / `require()`
  - ES Modules: `import` / `export`
  - **Comparison:** All 4 module systems — syntax, loading, compatibility

### Task 115 — Composite Pattern 🟡

- Tree structures with uniform interface:

  ```js
  class FileSystemItem {
    constructor(name) {
      this.name = name;
    }
    getSize() {
      throw new Error('Override me');
    }
  }

  class File extends FileSystemItem {
    constructor(name, size) {
      super(name);
      this.size = size;
    }
    getSize() {
      return this.size;
    }
  }

  class Folder extends FileSystemItem {
    children = [];
    add(item) {
      this.children.push(item);
    }
    getSize() {
      return this.children.reduce((sum, c) => sum + c.getSize(), 0);
    }
  }
  ```

  - **Build:** File system with size calculation
  - **Build:** UI component tree (components contain other components)

---

## Section C: Advanced OOP (Tasks 116-120)

### Task 116 — Prototype Pollution ও Prevention 🔴

- Security vulnerability:
  ```js
  // Attacker sets: obj.__proto__.isAdmin = true;
  // Now ALL objects have isAdmin = true!
  ```

  - How it happens: unvalidated JSON merge, deep merge without checks
  - Prevention:
    - `Object.create(null)` for dictionaries
    - Don't allow `__proto__`, `constructor`, `prototype` keys in user input
    - Use `Map` instead of plain objects for user data
    - `Object.freeze(Object.prototype)` (extreme)
  - **Build:** Safe deep merge function (blocks prototype pollution)

### Task 117 — SOLID Principles in JavaScript 🟡

- Apply each principle:
  - **S** — Single Responsibility: each module does one thing
  - **O** — Open/Closed: extend behavior without modifying source
  - **L** — Liskov Substitution: subtypes must be substitutable
  - **I** — Interface Segregation: small, focused interfaces
  - **D** — Dependency Inversion: depend on abstractions
  - **Challenge:** Refactor a poorly designed 200-line class into SOLID code
  - **Output:** Before/after comparison for each principle

### Task 118 — Composition Over Inheritance 🔴

- Why deep inheritance hierarchies are problematic:
  - Gorilla/Banana problem
  - Fragile base class problem
  - **Composition approach:**

    ```js
    const canWalk = (state) => ({ walk: () => `${state.name} walks` });
    const canSwim = (state) => ({ swim: () => `${state.name} swims` });
    const canFly = (state) => ({ fly: () => `${state.name} flies` });

    function createDuck(name) {
      const state = { name };
      return { ...state, ...canWalk(state), ...canSwim(state), ...canFly(state) };
    }
    ```

  - **Build:** Game character system using composition (warrior, mage, archer with mixed abilities)

### Task 119 — Object-Oriented Design Exercise 🔴

- Design ও implement:
  1. **Library Management System:** Book, Member, Loan, Library classes with proper OOP
  2. **E-commerce Cart:** Product, Cart, Order, Payment with design patterns
  3. **Social Media:** User, Post, Comment, Feed with observer pattern
  - Each with: class diagram, proper encapsulation, inheritance where needed, composition where needed
  - **Output:** 3 well-designed OOP systems

### Task 120 — OOP vs FP — Complete Comparison ⚫

- Phase 04 graduation:
  - Same problem solved both ways:
    1. **Todo App** — OOP approach vs FP approach
    2. **Shopping Cart** — OOP approach vs FP approach
    3. **User Management** — OOP approach vs FP approach
  - Compare: readability, maintainability, testability, extensibility
  - When to use OOP vs FP vs hybrid
  - JS is multi-paradigm — know when to use what
  - **Output:** Side-by-side comparison article with code

---

## ✅ Phase 04 Completion Checklist

- [ ] Prototype chain mechanically বোঝো (`__proto__` vs `.prototype`)
- [ ] `new` keyword internally কি করে বলতে পারো
- [ ] `Object.create()` confidently ব্যবহার করতে পারো
- [ ] ES6 classes ও pre-ES6 constructor functions দুটোই পারো
- [ ] Private fields (`#`) ব্যবহার করতে পারো
- [ ] Mixins implement করতে পারো
- [ ] 15+ design patterns জানো ও apply করতে পারো
- [ ] SOLID principles বোঝো ও JS এ apply করতে পারো
- [ ] Composition vs Inheritance — কখন কোনটা জানো
- [ ] OOP vs FP — both approaches confidently পারো

> ✅ Ready? → **JS Phase 05: Async JavaScript Mastery**
