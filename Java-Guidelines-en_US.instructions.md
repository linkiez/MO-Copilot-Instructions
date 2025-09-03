---
applyTo: '**/*.java'
---

# Java Coding Guidelines for GitHub Copilot

Follow these rules when generating, reviewing, or modifying Java code. Rules are categorized by severity for better understanding.

## 📋 BEST PRACTICES SUMMARY

### Always Do:
- Use modern Java features (Streams, Optional, Records, Pattern Matching).
- Implement `equals()` and `hashCode()` together.
- Use constructor injection in Spring.
- Handle `InterruptedException` properly.
- Use parameterized SQL queries.
- Validate SSL/TLS certificates.
- Use strong encryption algorithms.
- Use a logger for logging.
- Use `@deprecated` Javadoc tags for deprecated code.
- Use `try-with-resources` for automatic resource management.

### Never Do:
- Ignore exceptions or catch them too broadly.
- Use `==` for String/Object comparison.
- Hardcode passwords or secrets.
- Use deprecated APIs.
- Synchronize on `getClass()`.
- Call `Thread.run()` directly.
- Return `null` from `toString()` or `clone()`.
- Use `System.out` or `System.err`.
- Comment out sections of code.
- Use synchronized collections like `Vector` or `Hashtable`.

### Performance Tips:
- Reuse `Random` objects.
- Use `entrySet()` for Map iteration.
- Prefer `Arrays.stream()` for primitive arrays.
- Use `java.time` instead of `Date`/`Calendar`.
- Choose appropriate Collection implementations.

---

## 🚨 VULNERABILITIES (Critical Security Issues)

### Authentication & Authorization
- **A new session should be created during user authentication** - Prevents session fixation attacks.
- **Authorizations should be based on strong decisions** - Implement proper access controls.
- **Basic authentication should not be used** - Use secure authentication mechanisms.
- **Hard-coded passwords/secrets are security-sensitive** - Never embed credentials in code.

### Cryptography & Encryption
- **Cipher algorithms should be robust** - Use AES-256, avoid DES/3DES.
- **Cipher Block Chaining IVs should be unpredictable** - Generate random IVs.
- **Counter Mode initialization vectors should not be reused** - Each encryption must use a unique IV.
- **Encryption algorithms should be used with secure mode and padding scheme** - Use GCM mode when possible.
- **Hashes should include an unpredictable salt** - Salt passwords before hashing.
- **JWT should be signed and verified with strong cipher algorithms** - Use RS256 or ES256.
- **Passwords should not be stored in plain-text or with a fast hashing algorithm** - Use bcrypt, Argon2, or PBKDF2.
- **"SecureRandom" seeds should not be predictable** - Use proper entropy sources.

### Web Security
- **"HttpSecurity" URL patterns should be correctly ordered** - Most specific patterns first.
- **"HttpServletRequest.getRequestedSessionId()" should not be used** - Vulnerable to session attacks.
- **Server certificates should be verified during SSL/TLS connections** - Enable certificate validation.
- **Server hostnames should be verified during SSL/TLS connections** - Prevent man-in-the-middle attacks.
- **Weak SSL/TLS protocols should not be used** - Use TLS 1.2+.

### Data Security
- **"ActiveMQConnectionFactory" should not be vulnerable to malicious code deserialization** - Configure secure deserialization.
- **LDAP connections should be authenticated** - Never use anonymous LDAP binds.
- **XML parsers should not be vulnerable to XXE attacks** - Disable external entity processing.
- **Mobile database encryption keys should not be disclosed** - Secure key storage.

### Spring Security
- **A secure password should be used when connecting to a database** - Use strong DB credentials.
- **Members of Spring components should be injected** - Prevent injection vulnerabilities.
- **OpenSAML2 should be configured to prevent authentication bypass** - Proper SAML validation.
- **Persistent entities should not be used as arguments of "@RequestMapping" methods** - Mass assignment protection.

## 🐛 BUGS (Runtime Errors & Logic Issues)

### Null Safety & Optional
- **"@NonNull" values should not be set to null** - Respect nullability annotations.
- **"null" should not be used with "Optional"** - Use `Optional.empty()` instead.
- **"NullPointerException" should not be caught** - Fix the root cause instead.
- **"toString()" and "clone()" methods should not return null** - Return valid objects.
- **Optional value should only be accessed after calling `isPresent()`** - Check before accessing.

### Collections & Arrays
- **"PreparedStatement" and "ResultSet" methods should be called with valid indices** - Check bounds.
- **"Random" objects should be reused** - Create once, use multiple times.
- **"toArray" should be passed an array of the proper type** - Type safety.

### Strings & Text Processing
- **"StringBuilder" should not be instantiated with a character** - Use the string constructor.
- **String literals should not be duplicated** - Use constants.
- **Strings should not be concatenated using `+` in loops** - Use `StringBuilder`.
- **String offset-based methods should be preferred** - Performance optimization.
- **String operations should not rely on the default locale** - I18n considerations.

### Comparison & Equality
- **"compareTo" results should not be checked for specific values** - Check the sign only.
- **"compareTo" should not be overloaded** - Override, don't overload the method.
- **"equals" method parameters should not be marked "@Nonnull"** - Can break the `equals` contract.
- **"equals(Object obj)" should be overridden along with the "compareTo(T obj)" method** - Maintain consistency between equality and ordering.
- **"equals(Object obj)" should test argument type** - Avoid `ClassCastException`.
- **"equals" methods should be symmetric** - Follow the `equals` contract.

### Concurrency
- **".equals()" should not be used to test the values of "Atomic" classes** - Use the `get()` method.
- **"InterruptedException" should not be ignored** - Handle interruption properly.
- **"notifyAll" should be used** instead of `notify()` for multiple waiting threads.

## 🔧 CODE SMELLS (Maintainability Issues)

### Modern Java Features
- **"java.time" classes should be used for dates and times** - Replace `Date`/`Calendar`.
- **"Stream.toList()" method should be used instead of "collectors"** - Java 16+ feature.
- **Anonymous inner classes containing only one method should become lambdas** - Use Java 8+ features.
- **"ThreadLocal.withInitial" should be preferred** - More readable initialization.
- **Pattern Matching for "instanceof" operator should be used** - Java 16+ feature.
- **Records should be used instead of ordinary classes for immutable data** - Java 14+ feature.
- **Text blocks should be used for multiline strings** - Java 14+ feature.

### Performance
- **"Arrays.stream" should be used for primitive arrays** - More efficient.
- **"entrySet()" should be iterated when both the key and value are needed** - Single iteration.
- **"String#replace" should be preferred to "String#replaceAll"** - Faster for literals.
- **"deleteOnExit" should not be used** - Can cause memory leaks.
- **"DateUtils.truncate" from Apache Commons Lang library should not be used** - Use `java.time`.

### Spring Framework
- **"@Override" should be used on overriding and implementing methods** - Clear intent.
- **"@RequestMapping" methods should not be "private"** - Must be accessible.
- **Constructor injection should be used instead of field injection** - Better testability.
- **Spring components should use constructor injection** - Immutable dependencies.
- **Non-public methods should not be "@Transactional"** - Won't work with proxies.
- **Composed "@RequestMapping" variants should be preferred** - Use `@GetMapping` etc.

### Deprecation & Obsolete Code
- **"@Deprecated" code marked for removal should never be used** - The code will be removed in future versions.
- **"Class.forName()" should not load JDBC 4.0+ drivers** - Unnecessary with modern drivers that use the Service Provider mechanism.

### Exception Handling
- **"catch" clauses should do more than rethrow** - Add value or let it propagate.
- **"Exception" should not be caught when not required** - Be specific.
- **Exception handlers should preserve the original exceptions** - Don't lose stack traces.
- **Exceptions should not be created without being thrown** - Wasteful and confusing.
- **Generic exceptions should never be thrown** - Use specific exception types.

### Threading & Concurrency
- **"getClass" should not be used for synchronization** - Synchronize on private final fields.
- **"ThreadLocal" variables should be cleaned up when no longer used** - Prevent memory leaks.
- **"Thread.run()" should not be called directly** - Use `start()` method.
- **"volatile" variables should not be used with compound operators** - Not atomic operations.
- **"wait" should not be called when multiple locks are held** - Risk of deadlock.
- **Synchronized classes (Vector, Hashtable) should not be used** - Use concurrent alternatives.
- **Double-checked locking should not be used** - Use better patterns.

### I/O & Resources
- **Resources should be closed** - Use `try-with-resources`.
- **"Files.delete" should be preferred over `File.delete()`** - Better error handling.
- **"close()" calls should not be redundant** - Avoid closing an already closed resource.
- **Files should not be empty** - Remove unused files.

### Testing Best Practices
- **"Thread.sleep" should not be used in tests** - Use proper synchronization.
- **Assertion arguments should be passed in the correct order** - Expected, actual.
- **Assertions should be complete** - Include meaningful messages.
- **AssertJ assertions should be simplified to corresponding dedicated assertions** - More readable.

### General Coding Practices
- **Cognitive Complexity of methods should not be too high** - Keep methods simple and focused.
- **Methods should not have too many parameters** - Use parameter objects or refactor.
- **Methods should not have too many return statements** - A single exit point is preferred.
- **Nested blocks should not be too deep** - Refactor to reduce nesting.
- **Standard outputs should not be used directly** - Use a logger instead.
- **Utility classes should not have public constructors** - They are not meant to be instantiated.
- **Utility classes should not have instances** - All members should be static.

### Java Beans
- **"equals(Object obj)" and "hashCode()" should be overridden in pairs** - Maintain the contract.
- **"hashCode()" and "toString()" should not be called on array instances** - Use `Arrays` utility methods.
- **"Serializable" classes should have a "serialVersionUID"** - Versioning for serialization.
- **The "clone" method should be overridden with care** - Complex and error-prone.

### Logging
- **A logger should be declared "static final"** - Performance and consistency.
- **A logger should be used to log exceptions** - Don't just print stack traces.
- **Logging statements should not be wrapped in a conditional** - Let the logging framework handle levels.
- **More than one logger should not be used in a class** - Use a single logger per class.

### Comments
- **"@deprecated" Javadoc tags should be used** - Clearly mark deprecated code.
- **Sections of code should not be "commented out"** - Remove dead code instead.
- **TODO tags should be used for tasks to be done** - Track future work.

## 🔒 SECURITY HOTSPOTS (Review Required)

### Input Validation
- **Allowing requests with excessive content length is security-sensitive** - Set reasonable limits.
- **Formatting SQL queries is security-sensitive** - Use parameterized queries.
- **Using slow regular expressions is security-sensitive** - Review for ReDoS.
- **XPath expressions should not be vulnerable to injection attacks** - Use parameterized XPath queries.

### Configuration
- **Configuring loggers is security-sensitive** - Don't log sensitive data.
- **Creating cookies without the "HttpOnly" flag is security-sensitive** - XSS protection.
- **Creating cookies without the "secure" flag is security-sensitive** - HTTPS only.
- **Disabling CSRF protections is security-sensitive** - Keep CSRF enabled.
- **The "SameSite" attribute should be set on cookies** - Prevent CSRF attacks.

### Mobile Security (Android)
- **Accessing Android external storage is security-sensitive** - Validate permissions.
- **Authorizing non-authenticated users to use keys in the Android KeyStore** - Require authentication.
- **Broadcasting intents is security-sensitive** - Use explicit intents.
- **"WebView" should not be used with "setJavaScriptEnabled"** - Can lead to XSS vulnerabilities.

