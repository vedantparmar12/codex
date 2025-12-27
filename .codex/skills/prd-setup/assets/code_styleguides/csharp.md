# C# Coding Style Guide Summary

This document summarizes key rules and best practices for writing C# code based on Microsoft's official guidelines.

## 1. Naming Conventions
- **PascalCase:** Classes, methods, properties, namespaces, public fields
- **camelCase:** Local variables, parameters, private fields
- **_camelCase:** Private fields (with underscore prefix - common convention)
- **UPPER_CASE:** Constants (alternative: PascalCase is also acceptable)
- **Interfaces:** Prefix with `I` (e.g., `IEnumerable`, `IDisposable`)

```csharp
// Classes, Methods, Properties
public class UserService
{
    private readonly IUserRepository _userRepository;  // Private field
    public string UserName { get; set; }               // Property

    public async Task<User> GetUserAsync(int userId)   // Method
    {
        var user = await _userRepository.FindByIdAsync(userId);  // Local variable
        return user;
    }
}

// Interface
public interface IUserRepository
{
    Task<User> FindByIdAsync(int id);
}

// Constants
public const int MaxRetryCount = 3;
private const string DefaultConnectionString = "...";
```

## 2. Layout and Formatting
- **Indentation:** 4 spaces (not tabs)
- **Braces:** Allman style (opening brace on new line)
- **One Statement Per Line**
- **Line Length:** Keep reasonable (120-140 characters)
- **Blank Lines:** Use to separate logical groups

```csharp
public class Example
{
    public void Method()
    {
        if (condition)
        {
            DoSomething();
        }
        else
        {
            DoSomethingElse();
        }
    }
}
```

## 3. Language Features
- **var:** Use when type is obvious from right side
- **String Interpolation:** Prefer over concatenation
- **Null Checking:** Use null-conditional operators (`?.`, `??`)
- **Pattern Matching:** Use for cleaner code
- **Expression-Bodied Members:** Use for simple one-liners

```csharp
// var usage - Good
var user = new User();
var users = GetUsers();

// String interpolation
string message = $"Hello, {userName}!";

// Null-conditional operator
int? length = user?.Name?.Length;
string name = user?.Name ?? "Guest";

// Pattern matching
if (obj is User user)
{
    Console.WriteLine(user.Name);
}

// Expression-bodied members
public string FullName => $"{FirstName} {LastName}";
public void LogError(string message) => _logger.Error(message);
```

## 4. Async/Await
- **Async Suffix:** Add `Async` suffix to async method names
- **ConfigureAwait:** Use `ConfigureAwait(false)` in libraries
- **Return Task:** Don't use `async void` except for event handlers
- **Await All:** Always await async calls

```csharp
public async Task<User> GetUserAsync(int userId)
{
    var user = await _repository.FindByIdAsync(userId).ConfigureAwait(false);
    return user;
}

// Event handler - only place for async void
private async void OnButtonClick(object sender, EventArgs e)
{
    await ProcessDataAsync();
}
```

## 5. LINQ and Collections
- **LINQ:** Use for readable collection operations
- **Method Syntax:** Prefer method syntax over query syntax
- **Enumeration:** Use `IEnumerable<T>` for most cases
- **List Initialization:** Use collection initializers

```csharp
// Method syntax (preferred)
var adults = users.Where(u => u.Age >= 18).OrderBy(u => u.Name);

// Collection initializer
var numbers = new List<int> { 1, 2, 3, 4, 5 };

// Dictionary initializer
var config = new Dictionary<string, string>
{
    ["Host"] = "localhost",
    ["Port"] = "5432"
};
```

## 6. Exception Handling
- **Specific Exceptions:** Catch specific exceptions, not `Exception`
- **Never Swallow:** Don't catch and ignore exceptions
- **Custom Exceptions:** Inherit from `Exception` or specific base
- **When Clause:** Use `when` for conditional catch

```csharp
try
{
    await ProcessDataAsync();
}
catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.NotFound)
{
    _logger.Warning("Resource not found");
    return null;
}
catch (Exception ex)
{
    _logger.Error(ex, "Failed to process data");
    throw;  // Re-throw to preserve stack trace
}
```

## 7. Properties and Fields
- **Auto-Properties:** Use when no logic needed
- **Init-Only:** Use `init` for immutable properties
- **Required:** Use `required` for mandatory properties (C# 11+)
- **Private Fields:** Prefix with underscore

```csharp
public class User
{
    // Auto-property
    public string Name { get; set; }

    // Init-only property (immutable after construction)
    public int Id { get; init; }

    // Required property (C# 11+)
    public required string Email { get; set; }

    // Property with backing field
    private string _password;
    public string Password
    {
        get => _password;
        set => _password = HashPassword(value);
    }

    // Expression-bodied property
    public string DisplayName => $"{FirstName} {LastName}";
}
```

## 8. Dependency Injection
- **Constructor Injection:** Prefer constructor injection
- **Interface Dependencies:** Depend on interfaces, not implementations
- **Readonly Fields:** Mark injected dependencies as readonly

```csharp
public class UserService
{
    private readonly IUserRepository _repository;
    private readonly ILogger<UserService> _logger;

    public UserService(IUserRepository repository, ILogger<UserService> logger)
    {
        _repository = repository ?? throw new ArgumentNullException(nameof(repository));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }
}
```

## 9. Comments and Documentation
- **XML Documentation:** Use for public APIs
- **Comments:** Explain why, not what
- **TODO Comments:** Mark with `// TODO:` for future work

```csharp
/// <summary>
/// Retrieves a user by their unique identifier.
/// </summary>
/// <param name="userId">The unique identifier of the user.</param>
/// <returns>The user if found; otherwise, null.</returns>
/// <exception cref="ArgumentException">Thrown when userId is invalid.</exception>
public async Task<User> GetUserAsync(int userId)
{
    // Cache check to avoid unnecessary database calls
    if (_cache.TryGetValue(userId, out User cached))
    {
        return cached;
    }

    return await _repository.FindByIdAsync(userId);
}
```

## 10. Modern C# Features
- **Records:** Use for immutable data objects (C# 9+)
- **Pattern Matching:** Use switch expressions
- **Null-Forgiving Operator:** Use `!` sparingly when you know value is not null
- **Global Usings:** Define common usings in one place (C# 10+)

```csharp
// Record (C# 9+)
public record User(int Id, string Name, string Email);

// Switch expression
public string GetStatusText(Status status) => status switch
{
    Status.Active => "Active",
    Status.Inactive => "Inactive",
    Status.Pending => "Pending",
    _ => throw new ArgumentException("Invalid status")
};

// Target-typed new (C# 9+)
User user = new(1, "John", "john@example.com");
```

## 11. Best Practices
- **Single Responsibility:** Each class should have one purpose
- **Immutability:** Prefer immutable objects when possible
- **Dispose Pattern:** Implement `IDisposable` for unmanaged resources
- **Async All the Way:** Don't mix sync and async code
- **Nullable Reference Types:** Enable for new projects (C# 8+)

```csharp
#nullable enable

public class DatabaseConnection : IDisposable
{
    private SqlConnection? _connection;
    private bool _disposed;

    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing)
    {
        if (!_disposed)
        {
            if (disposing)
            {
                _connection?.Dispose();
            }
            _disposed = true;
        }
    }
}
```

**BE CONSISTENT.** Follow your team's conventions and use analyzers like StyleCop and Roslyn analyzers.

*Sources: [Microsoft C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions), [.NET Framework Design Guidelines](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/)*
