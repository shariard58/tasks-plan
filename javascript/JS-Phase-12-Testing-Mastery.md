# JS Phase 12 — Testing Mastery: Pure JavaScript (Tasks 356-385)

> সবাই Jest use করে test লেখে। তুমি নিজে Jest BUILD করবে। Testing framework কীভাবে কাজ করে ভিতর থেকে বুঝলে — তুমি testing expert।
> এই phase এ কোনো testing library use করবে না — সব নিজে build করবে।

---

## Section A: Testing Fundamentals from Scratch (Tasks 356-365)

### Task 356 — Build: Assertion Library 🟡

- নিজে assertion functions লেখো:
  ```js
  function expect(actual) {
    return {
      toBe(expected) {
        if (actual !== expected) throw new AssertionError(`Expected ${expected}, got ${actual}`);
      },
      toEqual(expected) {
        if (!deepEqual(actual, expected)) throw new AssertionError(`Deep equality failed`);
      },
      toThrow(message) {
        try {
          actual();
          throw new AssertionError('Expected function to throw');
        } catch (e) {
          if (message && !e.message.includes(message))
            throw new AssertionError(`Wrong error: ${e.message}`);
        }
      },
      toBeTruthy() {
        if (!actual) throw new AssertionError(`Expected truthy, got ${actual}`);
      },
      toBeFalsy() {
        if (actual) throw new AssertionError(`Expected falsy, got ${actual}`);
      },
      toContain(item) {
        if (!actual.includes(item)) throw new AssertionError(`Expected to contain ${item}`);
      },
      toMatch(regex) {
        if (!regex.test(actual)) throw new AssertionError(`Expected to match ${regex}`);
      },
      toBeGreaterThan(n) {
        if (!(actual > n)) throw new AssertionError(`Expected ${actual} > ${n}`);
      },
      toBeLessThan(n) {
        if (!(actual < n)) throw new AssertionError(`Expected ${actual} < ${n}`);
      },
      toBeNull() {
        if (actual !== null) throw new AssertionError(`Expected null`);
      },
      toBeUndefined() {
        if (actual !== undefined) throw new AssertionError(`Expected undefined`);
      },
      toBeInstanceOf(cls) {
        if (!(actual instanceof cls)) throw new AssertionError(`Expected instance of ${cls.name}`);
      },
      toHaveLength(n) {
        if (actual.length !== n)
          throw new AssertionError(`Expected length ${n}, got ${actual.length}`);
      },
      toHaveProperty(key, val) {
        if (!(key in actual)) throw new AssertionError(`Missing property: ${key}`);
        if (val !== undefined && actual[key] !== val)
          throw new AssertionError(`Property ${key}: expected ${val}, got ${actual[key]}`);
      },
      not: {
        /* Negate all matchers */
      },
    };
  }
  ```

  - **Deep equality function** (handle nested objects, arrays, Date, RegExp, Map, Set)
  - **Custom matchers:** allow users to add their own matchers
  - **Output:** Assertion library package (CommonJS + ESM)

### Task 357 — Build: Test Runner 🔴

- নিজে test runner build করো:

  ```js
  // User writes tests like this:
  describe('Calculator', () => {
    let calc;

    beforeEach(() => {
      calc = new Calculator();
    });

    it('should add numbers', () => {
      expect(calc.add(2, 3)).toBe(5);
    });

    it('should subtract numbers', () => {
      expect(calc.subtract(5, 3)).toBe(2);
    });

    describe('division', () => {
      it('should divide numbers', () => {
        expect(calc.divide(10, 2)).toBe(5);
      });

      it('should throw on division by zero', () => {
        expect(() => calc.divide(1, 0)).toThrow('Division by zero');
      });
    });
  });
  ```

  - **Features:**
    - `describe` blocks (nestable)
    - `it` / `test` functions
    - `beforeEach`, `afterEach`, `beforeAll`, `afterAll` hooks
    - Nested describe scoping (hooks cascade)
    - Test isolation (each test independent)
    - Async test support (`async it`)
    - Timeout per test (fail if too slow)
    - `.skip` — skip tests
    - `.only` — run only marked tests
    - Test discovery: find `*.test.js` files automatically

### Task 358 — Build: Test Reporter 🟡

- Console output formatting:

  ```
  Calculator
    ✓ should add numbers (2ms)
    ✓ should subtract numbers (1ms)
    division
      ✓ should divide numbers (1ms)
      ✓ should throw on division by zero (3ms)

  4 passing (7ms)
  0 failing
  ```

  - **Reporter types:**
    1. **Spec reporter:** indented tree (like above)
    2. **Dot reporter:** `.` for pass, `F` for fail, `S` for skip
    3. **JSON reporter:** machine-readable output
    4. **HTML reporter:** visual report page
  - **Failure output:**
    ```
    ✗ should add numbers
      Expected: 5
      Received: 6
      at Calculator.test.js:8:19
    ```
  - **Colors:** green for pass, red for fail, yellow for skip (ANSI codes)
  - **Summary:** total, passing, failing, skipped, duration

### Task 359 — Build: Mock & Spy System 🔴

- Function mocking:

  ```js
  // Create mock function
  const mockFn = mock.fn();
  mockFn('hello', 'world');

  expect(mockFn).toHaveBeenCalled();
  expect(mockFn).toHaveBeenCalledWith('hello', 'world');
  expect(mockFn).toHaveBeenCalledTimes(1);
  expect(mockFn.calls[0]).toEqual(['hello', 'world']);

  // Return values
  mockFn.mockReturnValue(42);
  mockFn.mockReturnValueOnce(1).mockReturnValueOnce(2);
  mockFn.mockImplementation((a, b) => a + b);

  // Spy on existing method
  const spy = mock.spyOn(calculator, 'add');
  calculator.add(2, 3); // Real implementation runs
  expect(spy).toHaveBeenCalledWith(2, 3);

  // Replace implementation
  spy.mockImplementation(() => 999);
  calculator.add(2, 3); // Returns 999

  // Restore
  spy.mockRestore(); // Back to original
  ```

  - **Track:** calls, arguments, return values, `this` context
  - **Mock modules:** replace `require()`/`import` at module level
  - **Timer mocks:** `mock.useFakeTimers()`, advance time manually
  - **Build:** Complete mock/spy library

### Task 360 — Build: Code Coverage Tool 🔴

- Track which lines are executed:
  - **Approach 1: Source instrumentation**
    - Parse source code → AST
    - Insert counter at each statement/branch
    - Run instrumented code → collect coverage data

    ```js
    // Original:
    function add(a, b) {
      if (a < 0) return 0;
      return a + b;
    }

    // Instrumented:
    function add(a, b) {
      __coverage__['add'][0]++;
      if (a < 0) {
        __coverage__['add'][1]++;
        return 0;
      }
      __coverage__['add'][2]++;
      return a + b;
    }
    ```

  - **Coverage metrics:**
    - Line coverage: % of lines executed
    - Branch coverage: % of if/else branches taken
    - Function coverage: % of functions called
    - Statement coverage: % of statements executed
  - **Output:**
    - Console summary (table format)
    - HTML report (source code with green/red highlights)
  - **Build:** Code coverage tool (AST-based instrumentation)

### Task 361 — Test Doubles: Stubs, Fakes, Dummies 🟡

- Different types of test doubles:
  - **Dummy:** passed around but never used
  - **Stub:** returns predefined values
  - **Fake:** working implementation (simplified)
  - **Mock:** records calls, verifiable
  - **Spy:** wraps real implementation, records calls
  - **Build examples:**

    ```js
    // Fake: In-memory database
    class FakeDatabase {
      #data = new Map();
      async get(id) {
        return this.#data.get(id);
      }
      async set(id, value) {
        this.#data.set(id, value);
      }
      async delete(id) {
        this.#data.delete(id);
      }
    }

    // Stub: Always returns same value
    const getUser = stub().returns({ id: 1, name: 'Test User' });

    // Fake: HTTP client
    class FakeHTTP {
      #responses = new Map();
      register(url, response) {
        this.#responses.set(url, response);
      }
      async fetch(url) {
        return this.#responses.get(url) || { status: 404 };
      }
    }
    ```

  - **Build:** Test doubles library with all 5 types

### Task 362 — Async Testing Patterns 🟡

- Test async code properly:
  - **Promise-based tests:**
    ```js
    it('should fetch user', async () => {
      const user = await getUser(1);
      expect(user.name).toBe('John');
    });
    ```
  - **Timer testing:**

    ```js
    it('should debounce', () => {
      const fn = mock.fn();
      const debounced = debounce(fn, 100);

      debounced();
      debounced();
      debounced();

      expect(fn).toHaveBeenCalledTimes(0); // Not called yet

      mock.advanceTimersByTime(100);

      expect(fn).toHaveBeenCalledTimes(1); // Called once after delay
    });
    ```

  - **Event testing:**
    ```js
    it('should emit event', (done) => {
      emitter.on('data', (val) => {
        expect(val).toBe(42);
        done();
      });
      emitter.emit('data', 42);
    });
    ```
  - **Build:**
    1. Fake timer system (`useFakeTimers`, `advanceTimersByTime`, `runAllTimers`)
    2. Async test runner (handles Promise, done callback, timeout)
    3. Retry mechanism for flaky async tests

### Task 363 — Snapshot Testing 🟡

- Compare output against saved snapshots:
  ```js
  it('should render user card', () => {
    const html = renderUserCard({ name: 'John', age: 30 });
    expect(html).toMatchSnapshot();
    // First run: saves snapshot to __snapshots__/
    // Next runs: compares against saved snapshot
  });
  ```

  - **Features:**
    - Auto-save snapshots on first run
    - Compare against saved snapshot
    - Update snapshots on demand (`--update-snapshots`)
    - Inline snapshots (stored in test file)
    - Snapshot serializer (handle Date, functions, etc.)
  - **Build:** Snapshot testing module for your test framework

### Task 364 — Property-Based Testing 🔴

- Generate random inputs to find edge cases:

  ```js
  // Instead of specific test cases:
  property('sort should return sorted array', () => {
    const arr = arbitrary.array(arbitrary.integer(-1000, 1000));
    const sorted = mySort(arr);

    // Properties that must ALWAYS be true:
    expect(sorted.length).toBe(arr.length);
    for (let i = 1; i < sorted.length; i++) {
      expect(sorted[i]).toBeGreaterThanOrEqual(sorted[i - 1]);
    }
    // Every element in original exists in sorted
    expect(sorted.sort()).toEqual(arr.sort());
  });
  ```

  - **Arbitrary generators:**
    - `arbitrary.integer(min, max)`
    - `arbitrary.string(minLen, maxLen)`
    - `arbitrary.boolean()`
    - `arbitrary.array(generator, minLen, maxLen)`
    - `arbitrary.object(schema)`
    - `arbitrary.oneOf(...generators)`
  - **Shrinking:** when test fails, find minimal failing input
  - **Build:** Property-based testing library with generators ও shrinking

### Task 365 — Mutation Testing 🔴

- Test your tests — are they actually catching bugs?
  - **Concept:** modify source code (mutations) → run tests → if tests still pass, they're weak
  - **Mutation operators:**
    - Arithmetic: `+` → `-`, `*` → `/`
    - Comparison: `>` → `>=`, `===` → `!==`
    - Boolean: `true` → `false`, `&&` → `||`
    - Remove statements
    - Replace return values
  - **Mutation score:** % of mutations caught (killed) by tests
  - **Build:**
    1. AST-based mutation generator (parse → mutate → serialize)
    2. Mutation runner (apply mutation → run tests → check if caught)
    3. Report: killed, survived, timed out mutations
    4. Minimum mutation score threshold

---

## Section B: Testing Strategies & Patterns (Tasks 366-375)

### Task 366 — TDD (Test-Driven Development) Practice 🟡

- Red → Green → Refactor cycle:
  - **Red:** write a failing test first
  - **Green:** write minimum code to pass
  - **Refactor:** clean up while tests stay green
  - **Practice projects (TDD from scratch):**
    1. Stack data structure
    2. String calculator (parse "1,2,3" → sum)
    3. FizzBuzz (with extensions)
    4. Roman numeral converter
    5. Bowling game scorer
    6. Mars Rover simulator
  - **Each project:** commit after each Red-Green-Refactor cycle
  - **Document:** test count, refactoring decisions, lessons learned

### Task 367 — BDD (Behavior-Driven Development) 🟡

- Write tests in natural language style:

  ```js
  feature('Shopping Cart', () => {
    scenario('Adding item to empty cart', () => {
      given('an empty cart', () => {
        const cart = new Cart();

        when('I add a product', () => {
          cart.add({ name: 'Book', price: 20 });

          then('cart should have 1 item', () => {
            expect(cart.items.length).toBe(1);
          });

          then('total should be 20', () => {
            expect(cart.total).toBe(20);
          });
        });
      });
    });
  });
  ```

  - **Build:**
    1. BDD DSL (`feature`, `scenario`, `given`, `when`, `then`)
    2. Gherkin-like syntax parser:
       ```
       Feature: Shopping Cart
         Scenario: Add item
           Given an empty cart
           When I add "Book" for $20
           Then the cart has 1 item
           And the total is $20
       ```
    3. Step definitions that map Gherkin to test code

### Task 368 — Integration Testing Patterns 🟡

- Test components working together:
  - **API integration tests:**

    ```js
    describe('User API', () => {
      let server;

      beforeAll(async () => {
        server = await startServer(3001);
      });

      afterAll(async () => {
        await server.close();
      });

      it('should create user', async () => {
        const res = await fetch('http://localhost:3001/api/users', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ name: 'John', email: 'john@test.com' }),
        });
        expect(res.status).toBe(201);
        const user = await res.json();
        expect(user.name).toBe('John');
      });
    });
    ```

  - **Database integration tests:** setup → test → teardown
  - **Build:**
    1. Test HTTP helper (supertest-like)
    2. Database test helper (setup/teardown, transactions)
    3. Test fixtures system
    4. Test data factory

### Task 369 — Testing Error Handling & Edge Cases 🟡

- Test the unhappy paths:
  - **Error scenarios:**
    - Network failure
    - Invalid input
    - Timeout
    - Race conditions
    - Concurrent access
    - Disk full / Permission denied
  - **Edge cases:**
    - Empty input
    - Null / undefined
    - Very large numbers
    - Unicode characters
    - Deeply nested objects
    - Circular references
  - **Build:**
    1. Error injection framework (simulate failures)
    2. Edge case generator (automatically create edge case inputs)
    3. Chaos testing tool (randomly fail operations)

### Task 370 — Testing Performance & Benchmarks 🟡

- Measure and verify performance:
  ```js
  benchmark(
    'Array.push vs spread',
    {
      push: () => {
        const arr = [1, 2, 3];
        arr.push(4);
      },
      spread: () => {
        const arr = [1, 2, 3];
        const newArr = [...arr, 4];
      },
    },
    { iterations: 1000000, warmup: 1000 }
  );
  ```

  - **Build:**
    1. Benchmark runner (warm-up, iterations, statistics)
    2. Performance regression detector (compare against baseline)
    3. Memory usage tester (check for leaks during tests)
    4. Output: ops/sec, mean, median, p95, p99, standard deviation

### Task 371 — Testing DOM & Browser Code 🟡

- Test DOM manipulation without a browser:
  - **Build mini DOM environment:**

    ```js
    class MockDocument {
      createElement(tag) {
        return new MockElement(tag);
      }
      getElementById(id) {
        /* ... */
      }
      querySelector(sel) {
        /* ... */
      }
    }

    class MockElement {
      constructor(tag) {
        this.tagName = tag;
        this.children = [];
        this.classList = new MockClassList();
      }
      appendChild(child) {
        this.children.push(child);
      }
      addEventListener(type, handler) {
        /* ... */
      }
      // ... etc
    }
    ```

  - **Event simulation:** click, input, keydown, submit
  - **Build:**
    1. Mini JSDOM-like environment
    2. DOM testing utilities (query, click, type, submit)
    3. Event simulation helpers
    4. DOM snapshot testing

### Task 372 — Contract Testing 🟡

- Verify API contracts between services:
  - **Consumer-driven contracts:**

    ```js
    // Consumer defines what it needs:
    const contract = {
      'GET /api/users/:id': {
        response: {
          status: 200,
          body: {
            id: contractType.number,
            name: contractType.string,
            email: contractType.string.matching(/@/),
          },
        },
      },
    };

    // Verify provider matches contract:
    verifyContract(contract, providerUrl);
    ```

  - **Build:**
    1. Contract definition DSL
    2. Contract verifier (against real API)
    3. Contract mock server (use contracts to create mocks)
    4. Contract diff detector (breaking changes)

### Task 373 — Fuzz Testing 🔴

- Find bugs with random inputs:
  - **Fuzzing approach:**
    1. Generate random/semi-random inputs
    2. Feed to function
    3. Check for: crashes, hangs, assertion failures, memory issues
  - **Guided fuzzing:** mutate inputs that trigger new code paths
  - **Build:**
    1. Basic fuzzer (random string/object/array generation)
    2. Grammar-based fuzzer (generate valid-ish inputs based on grammar)
    3. Coverage-guided fuzzer (prefer inputs that explore new branches)
    4. Crash reproducer (save failing inputs, replay)

### Task 374 — Testing CLI Applications 🟡

- Test command-line tools:

  ```js
  describe('My CLI', () => {
    it('should show help', async () => {
      const { stdout, exitCode } = await runCLI(['--help']);
      expect(exitCode).toBe(0);
      expect(stdout).toContain('Usage:');
    });

    it('should process files', async () => {
      await writeFile('test.txt', 'hello');
      const { stdout } = await runCLI(['process', 'test.txt']);
      expect(stdout).toContain('Processed: test.txt');
    });
  });
  ```

  - **Build:**
    1. CLI test runner (spawn process, capture stdout/stderr)
    2. Interactive CLI tester (send stdin input)
    3. File system test helper (temp directory, cleanup)
    4. Exit code verification

### Task 375 — Build: Complete Testing Framework ⚫

- Combine everything into one framework:
  - **Package name:** `mytest` (or your creative name)
  - **CLI:** `mytest --watch --coverage --reporter=spec`
  - **Features:**
    - ✅ Assertion library (20+ matchers + `.not`)
    - ✅ Test runner (`describe`, `it`, hooks, `.skip`, `.only`)
    - ✅ Reporter (spec, dot, JSON, HTML)
    - ✅ Mock/Spy system
    - ✅ Snapshot testing
    - ✅ Code coverage (line, branch, function)
    - ✅ Async support (Promise, done, timeout)
    - ✅ Watch mode (re-run on file change)
    - ✅ Parallel test execution (Worker threads)
    - ✅ Config file support (`.mytestrc.json`)
  - **This is your "Testing Showcase" project — publish to npm**

---

## ✅ Phase 12 Completion Checklist

- [ ] All 30 tasks complete
- [ ] Built assertion library with 20+ matchers
- [ ] Built complete test runner from scratch
- [ ] Built mock/spy system
- [ ] Built code coverage tool (AST-based)
- [ ] Built property-based testing library
- [ ] Built mutation testing tool
- [ ] Practiced TDD on 6+ projects
- [ ] Built complete testing framework (npm publishable)
- [ ] Can test any JavaScript application thoroughly

> **তুমি এখন testing expert — Jest/Vitest internally কীভাবে কাজ করে সব জানো।** 🧪
