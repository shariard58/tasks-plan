# JS Phase 14 — Compiler & Language Theory (Tasks 431-460)

> Programming language build করতে পারলে তুমি আর normal developer না — তুমি language designer।
> এটা Computer Science এর most prestigious topic। এই phase complete করলে তুমি top 0.1% developer।
> "Crafting Interpreters" by Robert Nystrom — এই phase এর Bible। Free online পড়তে পারো।

---

## Section A: Lexical Analysis — Tokenizer/Lexer (Tasks 431-437)

### Task 431 — Character Stream & Source Reader 🟡

- Source code process এর প্রথম step:
  - Source code হলো just একটা long string
  - Character by character পড়তে হবে
  - **Build: SourceReader class**

    ```js
    class SourceReader {
      #source;
      #pos = 0;
      #line = 1;
      #col = 1;

      constructor(source) {
        this.#source = source;
      }

      peek() {
        return this.#source[this.#pos] || '\0';
      }
      advance() {
        const ch = this.#source[this.#pos++];
        if (ch === '\n') {
          this.#line++;
          this.#col = 1;
        } else {
          this.#col++;
        }
        return ch;
      }
      isAtEnd() {
        return this.#pos >= this.#source.length;
      }
      getPosition() {
        return { line: this.#line, col: this.#col, offset: this.#pos };
      }
    }
    ```

  - **Track:** line number, column number, offset — for error reporting
  - **Handle:** Unicode, BOM, different line endings (\n, \r\n, \r)

### Task 432 — Token Types & Token Definition 🟡

- Define what tokens your language has:

  ```js
  const TokenType = {
    // Literals
    NUMBER: 'NUMBER', // 42, 3.14
    STRING: 'STRING', // "hello", 'world'
    BOOLEAN: 'BOOLEAN', // true, false
    NULL: 'NULL', // null

    // Identifiers & Keywords
    IDENTIFIER: 'IDENTIFIER', // myVar, foo
    LET: 'LET',
    CONST: 'CONST',
    FUNCTION: 'FUNCTION',
    RETURN: 'RETURN',
    IF: 'IF',
    ELSE: 'ELSE',
    WHILE: 'WHILE',
    FOR: 'FOR',
    CLASS: 'CLASS',
    NEW: 'NEW',
    THIS: 'THIS',
    IMPORT: 'IMPORT',
    EXPORT: 'EXPORT',

    // Operators
    PLUS: 'PLUS', // +
    MINUS: 'MINUS', // -
    STAR: 'STAR', // *
    SLASH: 'SLASH', // /
    PERCENT: 'PERCENT', // %
    EQUAL: 'EQUAL', // =
    EQUAL_EQUAL: 'EQUAL_EQUAL', // ==
    STRICT_EQUAL: 'STRICT_EQUAL', // ===
    BANG: 'BANG', // !
    BANG_EQUAL: 'BANG_EQUAL', // !=
    LESS: 'LESS', // <
    GREATER: 'GREATER', // >
    LESS_EQUAL: 'LESS_EQUAL', // <=
    GREATER_EQUAL: 'GREATER_EQUAL', // >=
    AND: 'AND', // &&
    OR: 'OR', // ||
    ARROW: 'ARROW', // =>
    DOT: 'DOT', // .
    SPREAD: 'SPREAD', // ...

    // Delimiters
    LEFT_PAREN: 'LEFT_PAREN', // (
    RIGHT_PAREN: 'RIGHT_PAREN', // )
    LEFT_BRACE: 'LEFT_BRACE', // {
    RIGHT_BRACE: 'RIGHT_BRACE', // }
    LEFT_BRACKET: 'LEFT_BRACKET', // [
    RIGHT_BRACKET: 'RIGHT_BRACKET', // ]
    COMMA: 'COMMA', // ,
    SEMICOLON: 'SEMICOLON', // ;
    COLON: 'COLON', // :

    // Special
    EOF: 'EOF',
  };

  class Token {
    constructor(type, value, line, col) {
      this.type = type;
      this.value = value;
      this.line = line;
      this.col = col;
    }
  }
  ```

  - **Design decision:** কোন keywords তোমার language এ থাকবে? কোন operators?

### Task 433 — Build: Complete Lexer/Tokenizer 🔴

- Source code → Token array:
  ```js
  const lexer = new Lexer('let x = 42 + 3.14;');
  const tokens = lexer.tokenize();
  // [
  //   Token(LET, 'let', 1, 1),
  //   Token(IDENTIFIER, 'x', 1, 5),
  //   Token(EQUAL, '=', 1, 7),
  //   Token(NUMBER, '42', 1, 9),
  //   Token(PLUS, '+', 1, 12),
  //   Token(NUMBER, '3.14', 1, 14),
  //   Token(SEMICOLON, ';', 1, 18),
  //   Token(EOF, null, 1, 19),
  // ]
  ```

  - **Handle:**
    - Numbers: integers, floats, negative numbers
    - Strings: single quotes, double quotes, escape sequences (`\n`, `\t`, `\\`, `\"`)
    - Template literals: backticks with `${expression}` interpolation
    - Identifiers vs keywords (look up in keyword table)
    - Multi-character operators: `==`, `===`, `!=`, `!==`, `>=`, `<=`, `=>`, `...`
    - Comments: `// single line` ও `/* multi line */`
    - Whitespace: skip but track for position
  - **Error handling:**
    - Unterminated string: `"hello` → error with line/col
    - Unexpected character: `@` → error
    - Invalid number: `3.14.15` → error

### Task 434 — Lexer Error Recovery 🟡

- Don't stop at first error:
  ```
  let x = "unterminated string
  let y = 42;
  let z = @invalid;
  ```

  - Should report ALL errors, not just the first:
    ```
    Error [1:9]: Unterminated string literal
    Error [3:9]: Unexpected character '@'
    ```
  - **Synchronization:** after error, skip to next valid point (next line, next semicolon)
  - **Build:** Lexer that collects errors ও continues scanning

### Task 435 — Lexer for Regular Expressions 🔴

- Tokenize regex literals:

  ```js
  // Challenge: how to distinguish division from regex?
  let x = 10 / 2;           // Division
  let re = /pattern/flags;   // Regex literal

  // Solution: context-dependent tokenization
  // After: number, identifier, ), ] → SLASH (division)
  // After: =, (, [, {, ;, ,, operator → REGEX
  ```

  - **Regex token:** parse `/pattern/flags` as single token
  - **Handle:** escape in regex `\/`, character classes `[abc]`, quantifiers
  - **This is why lexing JS is harder than most languages**

### Task 436 — Build: Lexer Test Suite 🟡

- Comprehensive lexer testing:

  ```js
  describe('Lexer', () => {
    it('should tokenize numbers', () => {
      expect(lex('42')).toEqual([token(NUMBER, '42'), token(EOF)]);
      expect(lex('3.14')).toEqual([token(NUMBER, '3.14'), token(EOF)]);
    });

    it('should tokenize strings', () => {
      expect(lex('"hello"')).toEqual([token(STRING, 'hello'), token(EOF)]);
      expect(lex('"line\\nbreak"')).toEqual([token(STRING, 'line\nbreak'), token(EOF)]);
    });

    it('should handle edge cases', () => {
      expect(lex('')).toEqual([token(EOF)]);
      expect(lex('// comment\n42')).toEqual([token(NUMBER, '42'), token(EOF)]);
    });

    it('should report errors', () => {
      const { tokens, errors } = lexWithErrors('"unterminated');
      expect(errors[0].message).toContain('Unterminated string');
    });
  });
  ```

  - **Test:** every token type, edge cases, errors, Unicode, empty input
  - **Use your test framework from Phase 12!**

### Task 437 — Build: Interactive Tokenizer Visualizer 🟡

- Browser-based tool:
  - Text input: type code
  - Real-time token output (table: type, value, line, col)
  - Syntax highlighting based on token types
  - Error highlighting (red underline)
  - Token statistics (count per type)
  - **Build with pure JS (no framework)**

---

## Section B: Parsing — AST Construction (Tasks 438-447)

### Task 438 — Grammar & Formal Language Theory 🟡

- Understand grammar notation:
  - **BNF (Backus-Naur Form):**
    ```
    <expression> ::= <term> (('+' | '-') <term>)*
    <term>       ::= <factor> (('*' | '/') <factor>)*
    <factor>     ::= NUMBER | '(' <expression> ')' | '-' <factor>
    ```
  - **EBNF (Extended BNF):** with `*` (zero or more), `+` (one or more), `?` (optional)
  - **Operator precedence:** which operations bind tighter
    ```
    Lowest  → Assignment (=)
            → Logical OR (||)
            → Logical AND (&&)
            → Equality (==, !=)
            → Comparison (<, >, <=, >=)
            → Addition (+, -)
            → Multiplication (*, /, %)
            → Unary (-, !)
    Highest → Call (fn()), Member (obj.prop), Grouping (())
    ```
  - **Associativity:** left-to-right (`2-3-4` = `(2-3)-4`) or right-to-left (`a=b=c` = `a=(b=c)`)
  - **Write the complete grammar** for your language

### Task 439 — AST Node Definitions 🟡

- Define the tree structure:

  ```js
  // Programs
  class Program { constructor(body) { this.body = body; } }

  // Statements
  class VariableDeclaration {
    constructor(kind, name, initializer) {
      this.kind = kind;       // 'let' | 'const'
      this.name = name;       // Identifier
      this.initializer = initializer; // Expression | null
    }
  }

  class FunctionDeclaration {
    constructor(name, params, body) {
      this.name = name;       // Identifier
      this.params = params;   // Identifier[]
      this.body = body;       // BlockStatement
    }
  }

  class IfStatement { constructor(condition, consequent, alternate) { ... } }
  class WhileStatement { constructor(condition, body) { ... } }
  class ForStatement { constructor(init, condition, update, body) { ... } }
  class ReturnStatement { constructor(value) { ... } }
  class BlockStatement { constructor(statements) { ... } }
  class ExpressionStatement { constructor(expression) { ... } }

  // Expressions
  class BinaryExpression { constructor(operator, left, right) { ... } }
  class UnaryExpression { constructor(operator, operand, prefix) { ... } }
  class AssignmentExpression { constructor(target, value) { ... } }
  class CallExpression { constructor(callee, arguments) { ... } }
  class MemberExpression { constructor(object, property, computed) { ... } }
  class ArrayExpression { constructor(elements) { ... } }
  class ObjectExpression { constructor(properties) { ... } }
  class ArrowFunctionExpression { constructor(params, body) { ... } }
  class ConditionalExpression { constructor(test, consequent, alternate) { ... } } // ternary
  class Identifier { constructor(name) { ... } }
  class Literal { constructor(value) { ... } } // number, string, boolean, null
  ```

  - **Every AST node stores:** line, column (for error reporting)

### Task 440 — Build: Recursive Descent Parser 🔴

- Token array → AST:

  ```js
  class Parser {
    #tokens;
    #current = 0;

    parse(tokens) {
      this.#tokens = tokens;
      const body = [];
      while (!this.#isAtEnd()) {
        body.push(this.#parseStatement());
      }
      return new Program(body);
    }

    #parseStatement() {
      if (this.#match('LET', 'CONST')) return this.#parseVariableDeclaration();
      if (this.#match('FUNCTION')) return this.#parseFunctionDeclaration();
      if (this.#match('IF')) return this.#parseIfStatement();
      if (this.#match('WHILE')) return this.#parseWhileStatement();
      if (this.#match('FOR')) return this.#parseForStatement();
      if (this.#match('RETURN')) return this.#parseReturnStatement();
      if (this.#match('LEFT_BRACE')) return this.#parseBlockStatement();
      return this.#parseExpressionStatement();
    }

    // Expression parsing using Pratt parsing / precedence climbing
    #parseExpression(precedence = 0) {
      let left = this.#parsePrefix(); // literals, identifiers, unary, grouping

      while (precedence < this.#getPrecedence(this.#peek())) {
        left = this.#parseInfix(left); // binary ops, call, member access
      }

      return left;
    }

    // ... helper methods
  }
  ```

  - **Pratt parser (operator precedence parsing):**
    - Each token has a precedence (binding power)
    - Prefix: `-x`, `!x`, literals, identifiers, `(`
    - Infix: `+`, `-`, `*`, `==`, `.`, `[`, `(`
    - Parse left side → check if next token's precedence is higher → if yes, parse right
  - **Why Pratt:** simpler ও more flexible than classic recursive descent for expressions

### Task 441 — Parse Complex Expressions 🔴

- Handle all expression types:

  ```js
  // Arithmetic with precedence
  parse('1 + 2 * 3');
  // → BinaryExpression(+, Literal(1), BinaryExpression(*, Literal(2), Literal(3)))

  // Function calls
  parse('add(1, 2)');
  // → CallExpression(Identifier(add), [Literal(1), Literal(2)])

  // Chained member access
  parse('obj.method().value');
  // → MemberExpression(CallExpression(MemberExpression(obj, method), []), value)

  // Arrow functions
  parse('(x, y) => x + y');
  // → ArrowFunctionExpression([x, y], BinaryExpression(+, x, y))

  // Ternary
  parse('x > 0 ? "positive" : "negative"');
  // → ConditionalExpression(BinaryExpression(>, x, 0), "positive", "negative")

  // Array ও Object literals
  parse('[1, 2, 3]');
  parse('{ name: "John", age: 30 }');

  // Nested
  parse('arr[0].name.toUpperCase()');
  ```

  - **Challenges:**
    - Arrow function parsing (lookahead needed)
    - Object literal vs block statement (`{` ambiguity)
    - Operator precedence ও associativity

### Task 442 — Parse Statements 🔴

- All statement types:

  ```js
  // Variable declarations
  let x = 42;
  const name = 'John';
  let arr = [1, 2, 3];

  // Function declarations
  function add(a, b) {
    return a + b;
  }

  // If/else
  if (x > 0) {
    console.log('positive');
  } else if (x < 0) {
    console.log('negative');
  } else {
    console.log('zero');
  }

  // Loops
  while (x > 0) {
    x = x - 1;
  }
  for (let i = 0; i < 10; i = i + 1) {
    sum = sum + i;
  }

  // Classes (advanced)
  class Animal {
    constructor(name) {
      this.name = name;
    }
    speak() {
      return this.name + ' speaks';
    }
  }
  ```

  - **Each statement type:** dedicated parse method
  - **Error recovery:** if one statement fails, try to continue

### Task 443 — Parser Error Handling & Recovery 🟡

- Meaningful error messages:

  ```
  let x = ;
  ^^^^^^^^^
  Error [1:9]: Expected expression after '='

  if x > 0 {
  ^^^^^^^^^^
  Error [1:4]: Expected '(' after 'if'

  function (a, b) { return a + b; }
  ^^^^^^^^^^
  Error [1:10]: Expected function name
  ```

  - **Panic mode recovery:**
    - On error, skip tokens until synchronization point (`;`, `}`, keyword)
    - Continue parsing rest of program
    - Collect all errors
  - **Build:** Parser that reports multiple errors per file

### Task 444 — Build: AST Visualizer 🟡

- Display AST as tree:
  ```
  Program
  └── VariableDeclaration (let)
      ├── name: Identifier (x)
      └── init: BinaryExpression (+)
          ├── left: Literal (2)
          └── right: BinaryExpression (*)
              ├── left: Literal (3)
              └── right: Literal (4)
  ```

  - **Two outputs:**
    1. Console: indented text tree (like above)
    2. Browser: interactive tree (Canvas or DOM) — collapsible nodes, hover for details
  - **JSON export:** AST → JSON (like astexplorer.net)

### Task 445 — Build: AST Printer (Code Generation from AST) 🟡

- AST → back to source code:
  ```js
  class ASTPrinter {
    print(node) {
      switch (node.constructor) {
        case BinaryExpression:
          return `${this.print(node.left)} ${node.operator} ${this.print(node.right)}`;
        case VariableDeclaration:
          return `${node.kind} ${node.name.name} = ${this.print(node.initializer)};`;
        case IfStatement:
          let code = `if (${this.print(node.condition)}) ${this.print(node.consequent)}`;
          if (node.alternate) code += ` else ${this.print(node.alternate)}`;
          return code;
        // ... all node types
      }
    }
  }
  ```

  - **Pretty printing:** proper indentation, line breaks
  - **Minification:** remove all whitespace (opposite of pretty print)
  - **Test:** parse → print → parse again → compare ASTs (round-trip test)

### Task 446 — Build: Source-to-Source Transformer (Transpiler) 🔴

- Transform AST → modified AST → output:

  ```js
  // Input (modern JS):
  const add = (a, b) => a + b;
  const name = `Hello ${user}`;
  const { x, y } = point;

  // Output (ES5):
  var add = function (a, b) {
    return a + b;
  };
  var name = 'Hello ' + user;
  var x = point.x;
  var y = point.y;
  ```

  - **Transformations:**
    1. Arrow functions → regular functions
    2. Template literals → string concatenation
    3. `const`/`let` → `var`
    4. Destructuring → manual assignment
    5. Default parameters → `if` checks
    6. Spread operator → `Array.concat` / `Object.assign`
  - **This is what Babel does!** তুমি mini-Babel build করবে

### Task 447 — Build: Parser Test Suite 🟡

- Test every grammar rule:

  ```js
  describe('Parser', () => {
    it('should parse variable declarations', () => {
      const ast = parse('let x = 42;');
      expect(ast.body[0]).toBeInstanceOf(VariableDeclaration);
      expect(ast.body[0].kind).toBe('let');
      expect(ast.body[0].name.name).toBe('x');
      expect(ast.body[0].initializer.value).toBe(42);
    });

    it('should handle operator precedence', () => {
      const ast = parse('1 + 2 * 3');
      // Should be: 1 + (2 * 3), NOT (1 + 2) * 3
      expect(ast.body[0].expression.operator).toBe('+');
      expect(ast.body[0].expression.right.operator).toBe('*');
    });

    it('should report error for invalid syntax', () => {
      const { errors } = parseWithErrors('let = ;');
      expect(errors.length).toBeGreaterThan(0);
    });
  });
  ```

  - **Snapshot tests:** complex ASTs → snapshot comparison
  - **Edge cases:** empty program, nested expressions, deeply nested blocks

---

## Section C: Semantic Analysis (Tasks 448-452)

### Task 448 — Scope Analysis & Symbol Table 🔴

- Track variables ও their scopes:

  ```js
  class Scope {
    #parent;
    #symbols = new Map();

    constructor(parent = null) {
      this.#parent = parent;
    }

    define(name, info) {
      if (this.#symbols.has(name)) throw new Error(`'${name}' already declared in this scope`);
      this.#symbols.set(name, info);
    }

    lookup(name) {
      if (this.#symbols.has(name)) return this.#symbols.get(name);
      if (this.#parent) return this.#parent.lookup(name);
      return null; // Undefined variable
    }
  }

  class SemanticAnalyzer {
    #currentScope;

    analyze(ast) {
      this.#currentScope = new Scope(); // Global scope
      this.#visitProgram(ast);
    }

    #visitVariableDeclaration(node) {
      // Check: is this variable already declared in this scope?
      this.#currentScope.define(node.name.name, { kind: node.kind, node });
      if (node.initializer) this.#visitExpression(node.initializer);
    }

    #visitIdentifier(node) {
      // Check: is this variable declared?
      if (!this.#currentScope.lookup(node.name)) {
        this.#error(`Undefined variable: '${node.name}'`, node);
      }
    }

    #visitBlockStatement(node) {
      this.#currentScope = new Scope(this.#currentScope); // Enter new scope
      for (const stmt of node.statements) this.#visitStatement(stmt);
      this.#currentScope = this.#currentScope.parent; // Exit scope
    }
  }
  ```

  - **Detect:**
    - Undeclared variables
    - Duplicate declarations in same scope
    - Use before declaration (TDZ for `let`/`const`)
    - Assignment to `const`

### Task 449 — Type Inference (Basic) 🔴

- Infer types without annotations:

  ```js
  let x = 42; // Infer: x is number
  let y = 'hello'; // Infer: y is string
  let z = x + y; // Infer: z is string (number + string = string)
  let w = x > 0; // Infer: w is boolean

  function add(a, b) {
    return a + b; // Infer from usage: if add(1,2) called → numbers
  }

  let result = add(1, 2); // Infer: result is number
  ```

  - **Type inference rules:**
    - Literals have known types
    - Binary operators: number + number → number, string + any → string
    - Comparison operators: any op any → boolean
    - Function return: inferred from return statements
    - Variable: inferred from initializer
  - **Warn on type mismatches** (don't block, just warn)

### Task 450 — Control Flow Analysis 🟡

- Detect unreachable code ও missing returns:

  ```js
  function example(x) {
    if (x > 0) {
      return 'positive';
    } else {
      return 'non-positive';
    }
    console.log('unreachable!'); // ⚠️ Warning: unreachable code
  }

  function broken(x) {
    if (x > 0) {
      return 'positive';
    }
    // ⚠️ Warning: not all code paths return a value
  }
  ```

  - **Build:**
    - Unreachable code detection (code after return/throw/break/continue)
    - Missing return detection (some paths return, others don't)
    - Dead code detection (variables declared but never used)
    - Infinite loop detection (basic — `while(true)` without break)

### Task 451 — Build: Complete Semantic Analyzer 🔴

- Combine all checks:
  1. **Scope analysis** — variable resolution
  2. **Type inference** — basic type checking
  3. **Control flow** — unreachable code, missing returns
  4. **Additional checks:**
     - Break/continue outside loop
     - Return outside function
     - `this` outside class/method
     - Duplicate function parameters
     - Invalid left-hand side in assignment (`42 = x`)
  - **Output:** List of errors ও warnings with line/col

### Task 452 — Build: Variable Resolver (Closure Support) 🔴

- Resolve variable bindings for closures:
  ```js
  function makeCounter() {
    let count = 0; // Local to makeCounter
    return function () {
      count = count + 1; // Captures 'count' from outer scope
      return count;
    };
  }
  ```

  - **Resolution:** determine at compile time which scope each variable belongs to
  - **Upvalue:** when inner function references outer function's variable
  - **Build:** Resolver that annotates each variable reference with:
    - Which scope it belongs to (local, upvalue, global)
    - Distance from current scope (how many scopes to traverse)

---

## Section D: Interpretation & Execution (Tasks 453-460)

### Task 453 — Build: Tree-Walk Interpreter 🔴

- Execute AST directly:

  ```js
  class Interpreter {
    #environment; // Variable storage

    interpret(ast) {
      this.#environment = new Environment(); // Global env
      for (const stmt of ast.body) {
        this.#executeStatement(stmt);
      }
    }

    #evaluateExpression(node) {
      if (node instanceof Literal) return node.value;
      if (node instanceof Identifier) return this.#environment.get(node.name);
      if (node instanceof BinaryExpression) {
        const left = this.#evaluateExpression(node.left);
        const right = this.#evaluateExpression(node.right);
        switch (node.operator) {
          case '+':
            return left + right;
          case '-':
            return left - right;
          case '*':
            return left * right;
          case '/':
            return left / right;
          case '==':
            return left == right;
          case '<':
            return left < right;
          // ... etc
        }
      }
      if (node instanceof CallExpression) {
        const func = this.#evaluateExpression(node.callee);
        const args = node.arguments.map((a) => this.#evaluateExpression(a));
        return func.call(args);
      }
      // ... all expression types
    }

    #executeStatement(node) {
      if (node instanceof VariableDeclaration) {
        const value = node.initializer ? this.#evaluateExpression(node.initializer) : undefined;
        this.#environment.define(node.name.name, value);
      }
      if (node instanceof IfStatement) {
        if (this.#evaluateExpression(node.condition)) {
          this.#executeStatement(node.consequent);
        } else if (node.alternate) {
          this.#executeStatement(node.alternate);
        }
      }
      // ... all statement types
    }
  }
  ```

  - **Environment chain** for nested scopes (like Scope but for runtime values)
  - **Demo:** run simple programs:
    ```
    let x = 10;
    let y = 20;
    print(x + y); // 30
    ```

### Task 454 — Build: Functions & Closures in Interpreter 🔴

- Functions as first-class values:

  ```js
  class MyFunction {
    #declaration;
    #closure; // The environment where function was DEFINED

    constructor(declaration, closure) {
      this.#declaration = declaration;
      this.#closure = closure;
    }

    call(args, interpreter) {
      // Create new environment with closure as parent
      const env = new Environment(this.#closure);

      // Bind parameters
      for (let i = 0; i < this.#declaration.params.length; i++) {
        env.define(this.#declaration.params[i].name, args[i]);
      }

      // Execute body
      return interpreter.executeBlock(this.#declaration.body, env);
    }
  }
  ```

  - **Test closures work:**
    ```
    function makeAdder(x) {
      return function(y) { return x + y; };
    }
    let add5 = makeAdder(5);
    print(add5(3));  // Should print 8
    ```
  - **Return value handling** (early return from function — use exception/signal)

### Task 455 — Build: Classes & Objects in Interpreter 🔴

- OOP support:

  ```
  class Animal {
    constructor(name) {
      this.name = name;
    }
    speak() {
      return this.name + " makes a sound";
    }
  }

  class Dog extends Animal {
    speak() {
      return this.name + " barks";
    }
  }

  let dog = new Dog("Rex");
  print(dog.speak()); // "Rex barks"
  ```

  - **Implement:**
    - Class as callable (returns new instance)
    - `this` binding
    - Method lookup
    - Inheritance (`extends`)
    - `super` keyword
    - Constructor auto-chaining
    - Property access ও assignment

### Task 456 — Build: Standard Library 🟡

- Built-in functions for your language:

  ```js
  const builtins = {
    // I/O
    print: (...args) => console.log(...args),
    input: (prompt) => readline(prompt),

    // Math
    abs: Math.abs,
    floor: Math.floor,
    ceil: Math.ceil,
    round: Math.round,
    random: Math.random,
    max: Math.max,
    min: Math.min,
    sqrt: Math.sqrt,
    pow: Math.pow,

    // String
    len: (s) => s.length,
    substr: (s, start, len) => s.substring(start, start + len),
    upper: (s) => s.toUpperCase(),
    lower: (s) => s.toLowerCase(),
    trim: (s) => s.trim(),
    split: (s, sep) => s.split(sep),
    join: (arr, sep) => arr.join(sep),

    // Array
    push: (arr, val) => {
      arr.push(val);
      return arr;
    },
    pop: (arr) => arr.pop(),
    map: (arr, fn) => arr.map(fn),
    filter: (arr, fn) => arr.filter(fn),
    reduce: (arr, fn, init) => arr.reduce(fn, init),

    // Type checking
    type: (val) => typeof val,
    isArray: Array.isArray,
    toString: (val) => String(val),
    toNumber: (val) => Number(val),

    // Time
    clock: () => Date.now(),
  };
  ```

  - Register builtins in global environment
  - **Test:** write programs using all builtins

### Task 457 — Build: REPL (Read-Eval-Print Loop) 🟡

- Interactive command line:
  ```
  > let x = 42
  > x + 8
  50
  > function greet(name) { return "Hello " + name; }
  > greet("World")
  "Hello World"
  > let arr = [1, 2, 3]
  > map(arr, function(x) { return x * 2; })
  [2, 4, 6]
  ```

  - **Features:**
    - Prompt with `>`
    - Evaluate expressions ও print result
    - Execute statements (no print for statements)
    - Persistent state (variables survive between lines)
    - Multi-line input (detect incomplete expressions)
    - History (up/down arrow)
    - Tab completion (variables, builtins)
    - `.help` command
    - `.clear` command (reset state)
    - `.exit` command
    - Error recovery (don't crash on bad input)

### Task 458 — Build: Bytecode Compiler (Advanced) ⚫

- Compile AST → bytecode for faster execution:

  ```js
  // Bytecode instructions:
  const OpCode = {
    CONST: 0x01, // Push constant to stack
    ADD: 0x02, // Pop 2, add, push result
    SUB: 0x03,
    MUL: 0x04,
    DIV: 0x05,
    NEGATE: 0x06, // Pop 1, negate, push
    EQUAL: 0x07,
    LESS: 0x08,
    GREATER: 0x09,
    NOT: 0x0a,
    LOAD: 0x10, // Load variable value → push
    STORE: 0x11, // Pop value → store in variable
    JUMP: 0x20, // Unconditional jump
    JUMP_IF_FALSE: 0x21, // Conditional jump
    CALL: 0x30, // Function call
    RETURN: 0x31, // Return from function
    PRINT: 0x40, // Print top of stack
    POP: 0x41, // Discard top of stack
    HALT: 0xff, // Stop execution
  };

  // Compiler: AST → bytecode
  class Compiler {
    #bytecode = [];
    #constants = [];

    compile(ast) {
      for (const stmt of ast.body) {
        this.#compileStatement(stmt);
      }
      this.#emit(OpCode.HALT);
      return { bytecode: this.#bytecode, constants: this.#constants };
    }

    #compileExpression(node) {
      if (node instanceof Literal) {
        const index = this.#addConstant(node.value);
        this.#emit(OpCode.CONST, index);
      }
      if (node instanceof BinaryExpression) {
        this.#compileExpression(node.left);
        this.#compileExpression(node.right);
        switch (node.operator) {
          case '+':
            this.#emit(OpCode.ADD);
            break;
          case '-':
            this.#emit(OpCode.SUB);
            break;
          // ... etc
        }
      }
    }
  }
  ```

  - **Stack-based VM:** operands on stack, operations pop/push
  - **This is how V8, JVM, Python work!**

### Task 459 — Build: Virtual Machine (Bytecode Executor) ⚫

- Execute bytecode:

  ```js
  class VM {
    #bytecode;
    #constants;
    #stack = [];
    #ip = 0; // Instruction pointer

    execute(program) {
      this.#bytecode = program.bytecode;
      this.#constants = program.constants;

      while (true) {
        const op = this.#bytecode[this.#ip++];

        switch (op) {
          case OpCode.CONST:
            const index = this.#bytecode[this.#ip++];
            this.#stack.push(this.#constants[index]);
            break;

          case OpCode.ADD:
            const b = this.#stack.pop();
            const a = this.#stack.pop();
            this.#stack.push(a + b);
            break;

          case OpCode.JUMP_IF_FALSE:
            const offset = this.#bytecode[this.#ip++];
            if (!this.#stack.pop()) this.#ip = offset;
            break;

          case OpCode.CALL:
            const argCount = this.#bytecode[this.#ip++];
            // Set up call frame...
            break;

          case OpCode.HALT:
            return;
        }
      }
    }
  }
  ```

  - **Call frames** for function calls (separate stack frames)
  - **Benchmark:** compare tree-walk vs bytecode speed (bytecode should be 5-10x faster)

### Task 460 — Build: Complete Programming Language ⚫

- **The capstone — combine everything:**

  ```bash
  # File execution
  node mylang.js run program.mylang

  # REPL
  node mylang.js repl

  # Compile to bytecode
  node mylang.js compile program.mylang -o program.mybc

  # Execute bytecode
  node mylang.js exec program.mybc

  # Syntax check
  node mylang.js check program.mylang
  ```

  - **Your language features:**
    - Variables (`let`, `const`)
    - Functions (first-class, closures)
    - Classes (inheritance, `this`, `super`)
    - Control flow (`if/else`, `while`, `for`)
    - Arrays ও Objects
    - String interpolation
    - Standard library (20+ builtins)
    - Error handling (`try/catch`)
    - Module system (`import/export`) — basic
  - **Tooling:**
    - Lexer + Parser + Semantic Analyzer
    - Tree-walk interpreter (development mode)
    - Bytecode compiler + VM (production mode)
    - REPL
    - Error reporting with line/column
    - Syntax highlighter (for the visualizer)
  - **Documentation:** Language spec, tutorial, 10 example programs
  - **Test suite:** 100+ tests covering all features

---

## ✅ Phase 14 Completion Checklist

- [ ] All 30 tasks complete
- [ ] Built a complete lexer/tokenizer with error recovery
- [ ] Built a recursive descent parser (Pratt parsing for expressions)
- [ ] Built semantic analyzer (scope, type inference, control flow)
- [ ] Built tree-walk interpreter with closures ও classes
- [ ] Built bytecode compiler ও virtual machine
- [ ] Built a REPL with history ও tab completion
- [ ] Built a transpiler (modern → ES5)
- [ ] Language has 20+ builtin functions
- [ ] 100+ tests for the language
- [ ] Can explain how ANY programming language works internally

> **তুমি এখন programming language designer — CS এর most prestigious skill তোমার হাতে।** 🎓
> **V8, Python, Ruby — সব language runtime তুমি internally বুঝো কারণ তুমি নিজে build করেছো।**
