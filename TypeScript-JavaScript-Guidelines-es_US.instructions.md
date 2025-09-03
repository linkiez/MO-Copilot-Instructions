# TypeScript/JavaScript Coding Guidelines for GitHub Copilot

Follow these rules when generating, reviewing, or modifying TypeScript/JavaScript code. Rules are categorized by severity for better understanding.

## 🚨 VULNERABILITIES (Critical Security Issues)

### Authentication & SSL/TLS
- **A new session should be created during user authentication** - To prevent session fixation attacks.
- **Administration services access should be restricted to specific IP addresses** - Whitelist trusted IPs to reduce attack surface.
- **Server certificates should be verified during SSL/TLS connections** - Enable certificate validation
- **Server hostnames should be verified during SSL/TLS connections** - Prevent man-in-the-middle attacks
- **Weak SSL/TLS protocols should not be used** - Use TLS 1.2+
- **XML parsers should not be vulnerable to XXE attacks** - Disable external entity processing

### Data Security
- **"alert(...)" should not be used** - Can expose sensitive information
- **Web SQL databases should not be used** - Deprecated and insecure
- **Cipher algorithms should be robust** - Use strong, modern encryption algorithms (e.g., AES-256).
- **Cryptographic keys should be robust** - Use keys of sufficient length and complexity.
- **Encryption algorithms should be used with secure mode and padding scheme** - Avoid insecure modes like ECB.
- **File uploads should be restricted** - Validate file types and sizes, and scan for malware.
- **JWT should be signed and verified with strong cipher algorithms** - Avoid `none` algorithm; use RS256 or similar.
- **Origins should be verified during cross-origin communications** - Validate the origin of messages in `postMessage` handlers.

### AWS Security
- **AWS IAM policies should not allow privilege escalation** - Avoid policies that let users modify their own permissions.

## 🐛 BUGS (Runtime Errors & Logic Issues)

### Type Safety & Validation
- **"delete" should be used only with object properties** - Not for variables or functions
- **"for...in" loops should filter properties before acting on them** - Check hasOwnProperty()
- **"in" should not be used with primitive types** - Will always return false
- **"NaN" should not be used in comparisons** - Comparing with NaN always returns false.
- **"new" operators should be used with functions** - Using 'new' with non-constructor functions is an error.
- **The base should be provided to "parseInt"** - Always specify radix parameter
- **Template literal placeholder syntax should not be used in regular strings** - Use actual template literals
- **A compare function should be provided when using "Array.prototype.sort()"** - Default sort is string-based, which can lead to incorrect results for numbers.
- **All branches in a conditional structure should not have exactly the same implementation** - This indicates a copy-paste error or dead code.
- **All code should be reachable** - Remove unreachable code to avoid confusion and bugs.
- **Assertions should not be given twice the same argument** - This makes the assertion useless.
- **Bitwise operators should not be used in boolean contexts** - This is likely a typo for logical operators (`&&`, `||`).
- **Built-in objects should not be overridden** - Modifying global objects can lead to unpredictable behavior.
- **Collection elements should not be replaced unconditionally** - This can lead to infinite loops or unexpected behavior.
- **Collection sizes and array length comparisons should make sense** - Avoid nonsensical comparisons (e.g., `length < 0`).
- **Comma and logical OR operators should not be used in switch cases** - This is a syntax error.
- **Constructors should not be declared inside interfaces** - Interfaces define contracts, not implementations.
- **Destructuring patterns should not be empty** - Empty patterns have no effect.
- **Empty collections should not be accessed or iterated** - This will cause errors.
- **Errors should not be created without being thrown** - Creating an `Error` object without throwing it has no effect.
- **Exclusive tests should not be commited to version control** - Remove `.only` from tests before committing.
- **Function declarations should not be made within blocks** - Behavior can be inconsistent across JavaScript engines.
- **Function parameters, caught exceptions and foreach variables' initial values should not be ignored** - This indicates a logic error.
- **Generators should "yield" something** - A generator that doesn't `yield` is likely a mistake.
- **Getters and setters should access the expected fields** - Avoid typos and logic errors in property accessors.
- **Identical expressions should not be used on both sides of a binary operator** - This is usually a typo.
- **Jump statements should not occur in "finally" blocks** - This can discard exceptions and hide bugs.
- **Loops with at most one iteration should be refactored** - The loop is likely unnecessary.

### Async/Await & Promises
- **"await" should only be used with promises** - Don't await non-promise values
- **Promise rejection handlers should not return nothing** - Return a value or re-throw
- **Promise results should be awaited in tests** - Ensure async operations complete

### Regular Expressions
- **Regular expressions should be syntactically valid** - Test regex patterns
- **Regular expressions should not contain control characters** - Use escape sequences
- **Regular expressions should not contain empty groups** - Remove or make meaningful
- **Regular expressions with the global flag should be used with caution** - State persists between calls
- **Repeated patterns in regular expressions should not match the empty string** - Causes infinite loops
- **Replacement strings should reference existing regular expression groups** - Use valid group references

### Logic & Control Flow
- **Related "if/else if" statements should not have the same condition** - Check for copy-paste errors
- **"super()" should be invoked appropriately** - `super()` must be called in derived class constructors.
- **A "for" loop update clause should move the counter in the right direction** - Ensure loops terminate correctly.
- **Results of "in" and "instanceof" should be negated rather than operands** - Use !(x in y) not (!x in y)
- **Return values from functions without side effects should not be ignored** - Use the result
- **Values should not be uselessly incremented** - Remove unnecessary operations
- **Variables should not be self-assigned** - Check for typos

### React Specific
- **React props should be read-only** - Don't modify props directly
- **React lifecycle methods should not return a value** - Except render() and getSnapshotBeforeUpdate()

### TypeScript Specific
- **Types without members, 'any' and 'never' should not be used in type intersections** - Results in never type
- **Unicode Grapheme Clusters should be avoided inside regex character classes** - Use proper Unicode handling

## 🔧 CODE SMELLS (Maintainability Issues)

### Modern JavaScript/TypeScript Features
- **"===" and "!==" should be used instead of "==" and "!="** - Strict equality checking
- **"arguments" should not be accessed directly** - Use rest parameters (...args)
- **"arguments.caller" and "arguments.callee" should not be used** - They are deprecated and hinder optimizations.
- **"import" should be used to include external code** - Replace require() with import
- **Variables should be declared with "let" or "const"** - Replace var declarations
- **Template strings should be used instead of concatenation** - More readable and efficient
- **Shorthand promises should be used** - Use async/await or Promise methods
- **Type assertions should use "as"** - Use 'value as Type' not '<Type>value'

### TypeScript Best Practices
- **The "any" type should not be used** - Use specific types or unknown
- **Type aliases should be used** - For complex types
- **Type guards should be used** - For runtime type checking
- **"in" should not be used on arrays** - It checks for keys, not values. Use `includes`.
- **"module" should not be used** - Use ES6 modules (`import`/`export`) instead of TypeScript's `module`.
- **"this" should not be assigned to variables** - It can lead to confusing behavior.
- **"undefined" should not be assigned** - It's a language primitive, not a variable to be reassigned.
- **"undefined" should not be passed as the value of optional parameters** - It's redundant.
- **"void" should not be used** - The `void` operator can be confusing.
- **Union types should not have too many elements** - Consider using enums or interfaces
- **Redundant casts and non-null assertions should be avoided** - Only when necessary
- **Unchanged variables should be marked "const"** - Immutability when possible

### Performance & Efficiency
- **"await" should not be used redundantly** - Don't await already resolved values
- **"continue" should not be used** - Use early returns or filter methods
- **"delete" should not be used on arrays** - Use `splice` or other array methods instead.
- **"for of" should be used with Iterables** - More readable than traditional for loops
- **"for in" should not be used with iterables** - Use for...of for arrays
- **Regular expression literals should be used when possible** - More efficient than new RegExp()
- **Array constructors should not be used** - Use array literals (`[]`) for clarity and consistency.
- **Array-mutating methods should not be used misleadingly** - Don't use mutating methods if you intend to create a new array.

### Code Organization
- **"default" clauses should be last** - In switch statements
- **"if ... else if" constructs should end with "else" clauses** - Handle all cases
- **"for" loop increment clauses should modify the loops' counters** - Avoid infinite loops or unexpected behavior.
- **"indexOf" checks should not be for positive numbers** - Checking for `>= 0` or `!== -1` is less error-prone.
- **"strict" mode should be used with caution** - Understand its implications.
- **"switch" statements should have at least 3 "case" clauses** - A `switch` with fewer cases might be better as an `if`.
- **"switch" statements should not be nested** - Nested switches are hard to read.
- **"switch" statements should not contain non-case labels** - Avoid confusing code.
- **"switch" statements should not have too many "case" clauses** - Break down large switches.
- **A "while" loop should be used instead of a "for" loop** - Use `while` for loops with non-numeric or complex conditions.
- **Array indexes should be numeric** - Using non-numeric indexes on arrays is confusing.
- **Assertion arguments should be passed in the correct order** - E.g., `expect(actual).to.equal(expected)`.
- **Assignments should not be made from within sub-expressions** - This can make code difficult to read and debug.
- **Assignments should not be redundant** - Remove unnecessary variable assignments.
- **Boolean checks should not be inverted** - `!x` is clearer than `x === false`.
- **Boolean expressions should not be gratuitous** - Simplify `if (x) return true; else return false;` to `return x;`.
- **Boolean literals should not be used in comparisons** - `if (x === true)` is redundant; use `if (x)`.
- **Braces and parentheses should be used consistently with arrow functions** - Follow a consistent style for clarity.
- **Chai assertions should have only one reason to succeed** - Write focused, single-purpose tests.
- **Class methods should be used instead of "prototype" assignments** - Use modern `class` syntax.
- **Class names should comply with a naming convention** - Use PascalCase for class names.
- **Collapsible "if" statements should be merged** - `if (x) { if (y) { ... } }` can be `if (x && y) { ... }`.
- **Collection and array contents should be used** - Don't create collections and then not use them.
- **Comma operator should not be used** - It can be confusing and is often used by mistake.
- **Comparison operators should not be used with strings** - This can lead to unexpected behavior.
- **Conditionals should start on new lines** - For better readability.
- **Control flow statements "if", "for", "while", "switch" and "try" should not be nested too deeply** - High nesting increases complexity.
- **Control structures should use curly braces** - Always use braces for `if`, `for`, `while`, etc., to avoid ambiguity.
- **Cyclomatic Complexity of functions should not be too high** - High complexity makes code hard to test and maintain.
- **Default export names and file names should match** - For consistency and predictability.
- **Default type parameters should be omitted** - If a default type parameter is sufficient, don't specify it.
- **Dependencies should be explicit** - Avoid relying on global state or implicit dependencies.
- **Deprecated APIs should not be used** - They may be removed in future versions.
- **Destructuring syntax should be used for assignments** - It can make code more concise.
- **Equality operators should not be used in "for" loop termination conditions** - Use `<` or `>` to avoid infinite loops with non-integer increments.
- **Expressions should not be too complex** - Break down complex expressions into smaller, more readable parts.
- **Function and method names should comply with a naming convention** - Use camelCase for functions and methods.
- **Function call arguments should not start on new lines** - For better readability.
- **Function parameters with default values should be last** - This is a requirement in JavaScript.
- **Function returns should not be invariant** - A function that always returns the same value may be a bug.
- **Functions should not be defined inside loops** - This can lead to performance issues and unexpected behavior with closures.
- **Functions should not be empty** - Empty functions should be removed or have a comment explaining why they are empty.
- **Functions should not have identical implementations** - This indicates duplicate code that should be refactored.
- **Functions should not have too many lines of code** - Keep functions small and focused.
- **Functions should not have too many parameters** - Consider using an options object for functions with many parameters.
- **Functions should use "return" consistently** - A function should either always return a value or never return a value.
- **Imports from the same modules should be merged** - `import { a, b } from 'module'` is better than two separate imports.
- **Increment (++) and decrement (--) operators should not be used in a method call or mixed with other operators in an expression** - This can lead to confusing, order-dependent code.
- **Interfaces should not be empty** - An empty interface provides no value.
- **Jump statements should not be redundant** - Remove unnecessary `return`, `break`, or `continue` statements.
- **Labels should not be used** - They are a form of `goto` and can make code hard to follow.
- **Literals should not be thrown** - Throw `Error` objects, not strings or other primitives.
- **Local variables should not be declared and then immediately returned or thrown** - Return or throw the value directly.
- **Loop counters should not be assigned to from within the loop body** - This can lead to unexpected behavior.
- **Loops should not contain more than a single "break" or "continue" statement** - Multiple breaks or continues can make a loop's logic hard to follow.
- **Magic numbers should not be used** - Use named constants instead of unexplained numbers.
- **Method overloads should be grouped together** - Keep overloaded method signatures adjacent in the code.
- **Multiline blocks should be enclosed in curly braces** - To avoid ambiguity and potential bugs.
- **Multiline string literals should not be used** - They are not supported by all browsers and can be confusing. Use template literals instead.
- **Names of regular expressions named groups should be used** - Use named groups for better readability.
- **Nested blocks of code should not be left empty** - If a block is intentionally empty, add a comment explaining why.
- **Non-empty statements should change control flow or have at least one side-effect** - Otherwise, the statement is useless.
- **Object literal shorthand syntax should be used** - It's more concise and readable.
- **Octal values should not be used** - They are deprecated and can be confusing. Use the `0o` prefix for octal literals.
- **Only "while", "do", "for" and "switch" statements should be labelled** - Labelling other statements is not useful.
- **Optional boolean parameters should have default value** - To avoid ambiguity.
- **Optional property declarations should not use both '?' and 'undefined' syntax** - This is redundant.
- **Parameters should be passed in the correct order** - Mismatched parameter order can lead to subtle bugs.
- **Primitive return types should be used** - Don't return wrapper objects like `String` or `Number`.
- **Primitive types should be omitted from initialized or defaulted declarations** - TypeScript can infer the type, making the code cleaner.
- **Private properties that are only assigned in the constructor or at declaration should be "readonly"** - This enforces immutability.
- **Property getters and setters should come in pairs** - A property with only a setter is write-only, which is unusual.
- **Property names should not be duplicated within a class or object literal** - This is a syntax error.
- **Regular expression quantifiers and character classes should be used concisely** - For better readability and performance.
- **Sections of code should not be commented out** - Remove dead code instead of commenting it out.
- **Shorthand object properties should be grouped at thebeginning or end of an object declaration** - For consistency.
- **Shorthand promises should be used** - `async/await` is generally preferred over `.then()` and `.catch()`.
- **Single-character alternations in regular expressions should be replaced with character classes** - `[abc]` is more efficient and readable than `a|b|c`.
- **Sparse arrays should not be declared** - They can lead to confusing behavior.
- **Standard outputs should not be used directly to log anything** - Use a dedicated logger for better control over log levels and output streams.
- **String literals should not be duplicated** - Extract them to constants to avoid typos and ease maintenance.
- **Strings and non-strings should not be added** - Explicitly convert to a string first to avoid unexpected results.
- **Switch cases should end with an unconditional "break" statement** - To prevent unintentional fall-through.
- **Template literals should not be nested** - Nested template literals are hard to read.
- **The global "this" object should not be used** - It can lead to unpredictable behavior.
- **The ternary operator should not be used** - Simple ternaries are fine, but complex ones should be refactored to `if/else`.
- **Two branches in a conditional structure should not have exactly the same implementation** - This indicates a copy-paste error or redundant code.
- **Union and intersection types should not be defined with duplicated elements** - This is redundant.
- **Unused local variables and functions should be removed** - To keep the codebase clean.
- **Variables and functions should not be redeclared** - This is a syntax error and indicates a bug.
- **Variables declared with "var" should be declared before they are used** - To avoid issues with hoisting. Better yet, use `let` or `const`.
- **Variables should be used in the blocks where they are declared** - Keep variable scope as small as possible.
- **Variables should not be shadowed** - This can lead to confusion and bugs.
- **Wildcard imports should not be used** - They can pollute the namespace and make it unclear where a dependency comes from.
- **Wrapper objects should not be used for primitive types** - They are less efficient and can lead to unexpected behavior.
- **Mocha timeout should be disabled by setting it to "0".** - This can hide performance problems in tests.
- **Functions should not be nested too deeply** - Extract to separate functions
- **Cognitive complexity should not be too high** - Break down complex functions

### Error Handling
- **"catch" clauses should do more than rethrow** - Add logging or context
- **Exception types should not be tested using "instanceof" in catch blocks** - Use specific catch blocks

### Naming & Conventions
- **Variable, property and parameter names should comply with a naming convention** - Use camelCase
- **Variables should not be shadowed** - Use different names for inner scope
- **String literals should not be duplicated** - Extract to constants

### Testing
- **Tests should check which exception is thrown** - Be specific about expected errors
- **Tests should include assertions** - Every test should verify something
- **Tests should not execute any code after "done()" is called** - Callback-based test completion

### Unused Code
- **Unnecessary imports should be removed** - Clean up import statements
- **Unused assignments should be removed** - Remove dead code
- **Unused function parameters should be removed** - Or prefix with underscore
- **Unused methods of React components should be removed** - Clean up component code

## 🔒 SECURITY HOTSPOTS (Review Required)

### Input/Output Security
- **Searching OS commands in PATH is security-sensitive** - Validate command paths
- **Using shell interpreter when executing OS commands is security-sensitive** - Avoid shell=true
- **Using slow regular expressions is security-sensitive** - Review for ReDoS attacks
- **Setting loose POSIX file permissions is security-sensitive** - Use restrictive permissions

### Network Security
- **Using clear-text protocols is security-sensitive** - Use HTTPS/TLS
- **Using hardcoded IP addresses is security-sensitive** - Use configuration
- **Using remote artifacts without integrity checks is security-sensitive** - Verify checksums
- **Allowing browsers to perform DNS prefetching is security-sensitive** - Can leak information about user navigation.
- **Allowing browsers to sniff MIME types is security-sensitive** - Can lead to content-sniffing attacks.
- **Allowing confidential information to be logged is security-sensitive** - Avoid logging sensitive data like passwords or personal information.
- **Allowing mixed-content is security-sensitive** - Loading HTTP content on an HTTPS page compromises security.
- **Allowing public ACLs or policies on a S3 bucket is security-sensitive** - Can expose sensitive data.
- **Allowing public network access to cloud resources is security-sensitive** - Restrict access to trusted networks.
- **Allowing requests with excessive content length is security-sensitive** - Can lead to denial-of-service attacks.
- **Creating cookies without the "HttpOnly" flag is security-sensitive** - Protects against XSS attacks stealing session cookies.
- **Creating cookies without the "secure" flag is security-sensitive** - Ensures cookies are only sent over HTTPS.
- **Creating public APIs is security-sensitive** - Ensure proper authentication and authorization are in place.
- **Delivering code in production with debug features activated is security-sensitive** - Can expose sensitive information.
- **Disabling Angular built-in sanitization is security-sensitive** - Can open the door to XSS attacks.
- **Disabling auto-escaping in template engines is security-sensitive** - Can lead to XSS attacks.
- **Disabling Certificate Transparency monitoring is security-sensitive** - Reduces the ability to detect malicious certificates.
- **Disabling content security policy fetch directives is security-sensitive** - Weakens protection against data exfiltration.
- **Disabling content security policy frame-ancestors directive is security-sensitive** - Makes the application vulnerable to clickjacking.
- **Disabling CSRF protections is security-sensitive** - Exposes the application to Cross-Site Request Forgery attacks.
- **Disabling server-side encryption of S3 buckets is security-sensitive** - Leaves data at rest unencrypted.
- **Disabling strict HTTP no-referrer policy is security-sensitive** - Can leak sensitive information through the Referer header.
- **Disabling Strict-Transport-Security policy is security-sensitive** - Leaves the application vulnerable to man-in-the-middle attacks.
- **Disabling versioning of S3 buckets is security-sensitive** - Increases the risk of data loss.
- **Disabling Vue.js built-in escaping is security-sensitive** - Can lead to XSS attacks.
- **Disclosing fingerprints from web application technologies is security-sensitive** - Can help attackers identify potential vulnerabilities.
- **Dynamically executing code is security-sensitive** - `eval()`, `new Function()`, etc., can execute malicious code.
- **Expanding archive files without controlling resource consumption is security-sensitive** - Can lead to "zip bomb" denial-of-service attacks.
- **Formatting SQL queries is security-sensitive** - Use parameterized queries to prevent SQL injection.
- **Forwarding client IP address is security-sensitive** - Ensure proper validation to prevent IP spoofing.
- **Granting access to S3 buckets to all or authenticated users is security-sensitive** - Follow the principle of least privilege.
- **Hard-coded credentials are security-sensitive** - Use environment variables or a secrets management system.
- **Having a permissive Cross-Origin Resource Sharing policy is security-sensitive** - Restrict CORS to trusted origins.
- **Policies authorizing public access to resources are security-sensitive** - Avoid granting broad public access.
- **Policies granting access to all resources of an account are security-sensitive** - Follow the principle of least privilege.
- **Policies granting all privileges are security-sensitive** - Avoid granting wildcard permissions.
- **Reading the Standard Input is security-sensitive** - Be careful when handling user input from the command line.
- **Using command line arguments is security-sensitive** - Validate and sanitize command line arguments.
- **Using regular expressions is security-sensitive** - Be aware of ReDoS vulnerabilities.
- **Using Sockets is security-sensitive** - Ensure proper authentication and encryption when using sockets.
- **Using unencrypted Elasticsearch domains is security-sensitive** - Enable encryption at rest and in transit.
- **Writing cookies is security-sensitive** - Ensure cookies are created with `HttpOnly` and `Secure` flags.

### Data Storage & Privacy
- **Using pseudorandom number generators (PRNGs) is security-sensitive** - Use crypto-secure random
- **Using publicly writable directories is security-sensitive** - Secure file operations
- **Using intrusive permissions is security-sensitive** - Request minimal permissions
- **Using weak hashing algorithms is security-sensitive** - Use SHA-256 or better

### AWS Security
- **Using unencrypted EBS volumes is security-sensitive** - Enable encryption
- **Using unencrypted EFS file systems is security-sensitive** - Enable encryption
- **Using unencrypted RDS DB resources is security-sensitive** - Enable encryption
- **Using unencrypted SageMaker notebook instances is security-sensitive** - Enable encryption
- **Using unencrypted SNS topics is security-sensitive** - Enable encryption
- **Using unencrypted SQS queues is security-sensitive** - Enable encryption

### Web Security
- **Statically serving hidden files is security-sensitive** - Configure server properly
- **Authorizing an opened window to access back to the originating window is security-sensitive** - Use rel="noopener"
- **Disabling resource integrity features is security-sensitive** - Use SRI for external resources

## 🌐 HTML GUIDELINES

### Accessibility (WCAG 2.0/2.1)
- **"<html>" element should have a language attribute** - For screen readers
- **"<title>" should be present in all pages** - Required for SEO and accessibility
- **Image, area and button with image tags should have an "alt" attribute** - Describe images
- **"input", "select" and "textarea" tags should be labeled** - Associate with labels
- **"<table>" tags should have a description** - Use caption or summary
- **"<th>" tags should have "id" or "scope" attributes** - Table header relationships
- **Table cells should reference their headers** - Use headers attribute
- **"<fieldset>" tags should contain a "<legend>"** - Group related form controls
- **"<frames>" should have a "title" attribute** - For accessibility.
- **"<object>" tags should provide an alternative content** - For users who cannot view the object.
- **"aria-label" or "aria-labelledby" attributes should be used to differentiate similar elements** - To help screen reader users.
- **Mouse events should have corresponding keyboard events** - For users who cannot use a mouse.
- **Tables should have headers** - To provide context for the data.
- **Videos should have subtitles** - For deaf or hard-of-hearing users.

### Semantic HTML
- **"<!DOCTYPE>" declarations should appear before "<html>" tags** - To ensure the page renders in standards mode.
- **"<li>" and "<dt>" item tags should be in "<ul>", "<ol>" or "<dl>" container tags** - For correct list structure.
- **<script>...</script> elements should not be nested** - This is invalid HTML.
- **All HTML tags should be closed** - To ensure the page is well-formed.
- **Attributes deprecated in HTML5 should not be used** - Use modern alternatives.
- **Elements deprecated in HTML5 should not be used** - Use modern alternatives
- **HTML "<table>" should not be used for layout purposes** - Use CSS Grid/Flexbox
- **Heading tags should be used consecutively from "H1" to "H6"** - Proper document structure
- **"<strong>" and "<em>" tags should be used** - For semantic emphasis

### Performance & UX
- **Image tags should have "width" and "height" attributes** - Prevent layout shift
- **Links should not directly target images** - Provide a more descriptive link text or context.
- **Meta tags should not be used to refresh or redirect** - Use JavaScript or server-side
- **Links should not target "#" or "javascript:void(0)"** - Use proper handlers
- **Favicons should be used in all pages** - For branding and user experience.
- **Server-side image maps ("ismap" attribute) should not be used** - They are not accessible.
- **The "style" attribute should not be used** - Use external or internal stylesheets for better separation of concerns.
- **Web pages should not contain absolute URIs** - Use relative URIs for better portability.

### Conventions & Best Practices
- **Attributes should be quoted using double quotes rather than single ones** - For consistency.
- **Dynamic includes should not be used** - They can hurt performance.
- **JSF expressions should be syntactically valid** - To avoid runtime errors.
- **JSP expressions should not be used** - Use EL or JSTL instead.
- **Labels should be defined in the resource bundle** - For internationalization.
- **Links with identical texts should have identical targets** - To avoid user confusion.
- **Multiple "page" directives should not be used** - Combine them into a single directive.
- **White space should be used in JSP/JSF tags** - For readability.

### Security
- **Using HTML comments is security-sensitive** - Do not put sensitive information in HTML comments.

## 📋 BEST PRACTICES SUMMARY

### Always Do:
- Use strict equality (=== / !==)
- Prefer const over let, never use var
- Use template literals for string interpolation
- Use async/await for promises
- Provide proper TypeScript types
- Include alt text for images
- Validate user inputs
- Use HTTPS for network requests

### Never Do:
- Use == or != for comparisons
- Ignore promise rejections
- Use any type without justification
- Access arguments object directly
- Use deprecated HTML elements
- Hardcode sensitive information
- Use weak cryptographic functions

### Performance Tips:
- Use for...of for iterables
- Compile regex literals once
- Avoid nested ternary operators
- Remove unused imports and code
- Use early returns to reduce nesting
- Prefer built-in array methods over loops

### Security Checklist:
- ✅ Validate all inputs
- ✅ Use HTTPS/TLS
- ✅ Enable CSP headers
- ✅ Sanitize HTML content
- ✅ Use secure random generators
- ✅ Encrypt sensitive data
- ✅ Follow principle of least privilege

