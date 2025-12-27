# Google Java Style Guide Summary

This document summarizes key rules and best practices from the Google Java Style Guide.

## 1. Source File Basics
- **File Encoding:** UTF-8
- **Whitespace:** Use spaces, never tabs
- **Special Characters:** Use escape sequences (`\t`, `\n`) instead of octal or Unicode escapes

## 2. Source File Structure
Order:
1. License or copyright information (if present)
2. Package statement
3. Import statements (no wildcards)
4. Exactly one top-level class

- **Import Ordering:** Static imports first, then non-static. Each group alphabetically sorted.
- **One Top-Level Class:** Exactly one top-level class declaration per file.

## 3. Formatting
- **Braces:** Use K&R style (opening brace on same line)
- **Indentation:** 2 spaces (not 4)
- **Line Length:** Maximum 100 characters
- **Column Limit:** 100 characters
- **Whitespace:**
  - One blank line between class members (fields, constructors, methods, nested classes)
  - Blank lines should be used to improve readability

```java
class MyClass {
  int field;

  MyClass() {
    // constructor
  }

  void method() {
    if (condition) {
      // code
    }
  }
}
```

- **One Statement Per Line**
- **Array Initializers:** Can be block-like

## 4. Naming
- **Packages:** `com.example.deepspace`, all lowercase, no underscores
- **Classes:** `PascalCase` (e.g., `CharacterSet`)
- **Methods:** `camelCase` (e.g., `sendMessage`)
- **Constants:** `UPPER_SNAKE_CASE` (e.g., `MAX_COUNT`)
- **Non-Constants:** `camelCase` (e.g., `userName`)
- **Parameters:** `camelCase`
- **Local Variables:** `camelCase`
- **Type Parameters:** Single capital letter, optionally followed by a number (e.g., `T`, `E`, `K`, `V`, `T2`)

## 5. Programming Practices
- **@Override:** Always use when applicable
- **Caught Exceptions:** Never ignore. Handle appropriately or rethrow.
- **Static Members:** Qualify with class name, not instance
- **Finalizers:** Don't use them
- **Javadoc:** Required for all public classes and public/protected members
  - Start with summary fragment
  - Use `@param`, `@return`, `@throws` as appropriate

```java
/**
 * Returns the sum of two integers.
 *
 * @param a the first integer
 * @param b the second integer
 * @return the sum of a and b
 */
public int add(int a, int b) {
  return a + b;
}
```

## 6. Best Practices
- Use `Optional` to represent values that may be absent
- Prefer immutability: use `final` for fields when possible
- Use `@Nullable` and `@NonNull` annotations for nullability
- Avoid float/double for precise values (use `BigDecimal`)
- Use streams for collection processing when it improves readability

**BE CONSISTENT.** When editing code, match the existing style.

*Source: [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)*
