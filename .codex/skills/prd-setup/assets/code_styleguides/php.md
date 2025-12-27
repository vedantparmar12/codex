# PHP Style Guide Summary (PSR-12)

This document summarizes key rules and best practices from PSR-12 (PHP Standards Recommendations) and modern PHP conventions.

## 1. Basic Coding Standards
- **PHP Tags:** Always use `<?php` for PHP code, never short tags
- **Encoding:** UTF-8 without BOM
- **Line Endings:** Unix line endings (LF)
- **Indentation:** 4 spaces (never tabs)
- **Line Length:** Soft limit of 120 characters
- **Blank Lines:** Use to improve readability

```php
<?php

declare(strict_types=1);

namespace App\Services;

use App\Models\User;
use App\Repositories\UserRepository;

class UserService
{
    public function __construct(
        private UserRepository $repository
    ) {
    }
}
```

## 2. Naming Conventions
- **Classes:** `PascalCase` (e.g., `UserController`, `PaymentService`)
- **Methods:** `camelCase` (e.g., `getUserById`, `processPayment`)
- **Functions:** `snake_case` for global functions (but prefer classes)
- **Variables:** `camelCase` (e.g., `$userId`, `$emailAddress`)
- **Constants:** `UPPER_SNAKE_CASE` (e.g., `MAX_ATTEMPTS`, `API_KEY`)
- **Namespaces:** `PascalCase` matching directory structure

```php
<?php

namespace App\Http\Controllers;

class UserController
{
    private const MAX_LOGIN_ATTEMPTS = 3;

    private UserService $userService;

    public function getUserById(int $userId): ?User
    {
        return $this->userService->find($userId);
    }

    public function isActiveUser(User $user): bool
    {
        return $user->status === 'active';
    }
}
```

## 3. Type Declarations (PHP 7.4+)
- **Strict Types:** Use `declare(strict_types=1)` at top of files
- **Type Hints:** Use for parameters and return types
- **Property Types:** Use typed properties (PHP 7.4+)
- **Union Types:** Use for multiple possible types (PHP 8.0+)
- **Nullable Types:** Use `?Type` for nullable

```php
<?php

declare(strict_types=1);

namespace App\Models;

class User
{
    // Typed properties (PHP 7.4+)
    private int $id;
    private string $name;
    private ?string $email = null;
    private array $roles = [];

    public function __construct(
        int $id,
        string $name,
        ?string $email = null
    ) {
        $this->id = $id;
        $this->name = $name;
        $this->email = $email;
    }

    public function getId(): int
    {
        return $this->id;
    }

    // Union types (PHP 8.0+)
    public function setAge(int|float $age): void
    {
        $this->age = $age;
    }
}
```

## 4. Classes and Methods
- **Braces:** Opening brace on new line for classes, same line for methods
- **Visibility:** Always declare visibility (public, protected, private)
- **Order:** Constants, properties, constructor, methods
- **Abstract/Final:** Declare before visibility

```php
<?php

namespace App\Services;

abstract class BaseService
{
    protected const DEFAULT_TIMEOUT = 30;

    private LoggerInterface $logger;
    protected DatabaseInterface $db;

    public function __construct(
        LoggerInterface $logger,
        DatabaseInterface $db
    ) {
        $this->logger = $logger;
        $this->db = $db;
    }

    abstract public function process(): void;

    protected function log(string $message): void
    {
        $this->logger->info($message);
    }

    final public function validate(): bool
    {
        // Cannot be overridden
        return true;
    }
}
```

## 5. Control Structures
- **Spacing:** Space after control keyword, no space before parenthesis
- **Braces:** Opening brace on same line, closing on new line
- **Conditions:** Use strict comparisons (`===`, `!==`)

```php
<?php

// if/else
if ($user->isActive()) {
    processUser($user);
} elseif ($user->isPending()) {
    sendActivationEmail($user);
} else {
    logInactiveUser($user);
}

// foreach
foreach ($users as $user) {
    echo $user->getName();
}

// while
while ($row = $stmt->fetch()) {
    processRow($row);
}

// switch
switch ($status) {
    case 'active':
        handleActive();
        break;
    case 'pending':
        handlePending();
        break;
    default:
        handleUnknown();
}

// match (PHP 8.0+)
$result = match ($status) {
    'active' => 'User is active',
    'pending' => 'User is pending',
    default => 'Unknown status',
};
```

## 6. Arrays
- **Short Syntax:** Use `[]` instead of `array()`
- **Multi-line:** One element per line for readability
- **Trailing Comma:** Use for multi-line arrays

```php
<?php

// Short array syntax
$numbers = [1, 2, 3, 4, 5];

// Associative array
$user = [
    'id' => 1,
    'name' => 'John Doe',
    'email' => 'john@example.com',
    'roles' => ['admin', 'editor'],
];

// Multi-line with trailing comma
$config = [
    'host' => 'localhost',
    'port' => 3306,
    'database' => 'myapp',
    'charset' => 'utf8mb4',  // Trailing comma
];
```

## 7. Modern PHP Features
- **Constructor Property Promotion (PHP 8.0+):** Simplify constructors
- **Named Arguments (PHP 8.0+):** Use for clarity
- **Nullsafe Operator (PHP 8.0+):** Use `?->` for safe navigation
- **Arrow Functions (PHP 7.4+):** For short closures

```php
<?php

// Constructor property promotion (PHP 8.0+)
class User
{
    public function __construct(
        private int $id,
        private string $name,
        private ?string $email = null
    ) {
    }

    public function getName(): string
    {
        return $this->name;
    }
}

// Named arguments (PHP 8.0+)
$user = new User(
    id: 1,
    name: 'John Doe',
    email: 'john@example.com'
);

// Nullsafe operator (PHP 8.0+)
$country = $user?->getAddress()?->getCountry();

// Arrow functions (PHP 7.4+)
$numbers = [1, 2, 3, 4, 5];
$doubled = array_map(fn($n) => $n * 2, $numbers);

// Traditional closure
$multiplier = 3;
$tripled = array_map(function($n) use ($multiplier) {
    return $n * $multiplier;
}, $numbers);
```

## 8. Error Handling
- **Exceptions:** Use for error handling, not return codes
- **Try-Catch:** Catch specific exceptions
- **Custom Exceptions:** Extend built-in exceptions
- **Finally:** Use for cleanup code

```php
<?php

namespace App\Exceptions;

class UserNotFoundException extends \Exception
{
    public function __construct(int $userId)
    {
        parent::__construct("User with ID {$userId} not found");
    }
}

// Using exceptions
class UserService
{
    public function findUser(int $userId): User
    {
        try {
            $user = $this->repository->find($userId);

            if ($user === null) {
                throw new UserNotFoundException($userId);
            }

            return $user;
        } catch (DatabaseException $e) {
            $this->logger->error('Database error', ['exception' => $e]);
            throw $e;
        } finally {
            $this->connection->close();
        }
    }
}
```

## 9. Dependency Injection
- **Constructor Injection:** Prefer for required dependencies
- **Interfaces:** Depend on interfaces, not implementations
- **Type Hints:** Use for dependency injection

```php
<?php

namespace App\Services;

use App\Repositories\UserRepositoryInterface;
use Psr\Log\LoggerInterface;

class UserService
{
    public function __construct(
        private UserRepositoryInterface $repository,
        private LoggerInterface $logger
    ) {
    }

    public function createUser(array $data): User
    {
        $this->logger->info('Creating user', $data);
        return $this->repository->create($data);
    }
}
```

## 10. Documentation (PHPDoc)
- **Doc Blocks:** Use for classes, methods, and complex properties
- **Type Information:** Include parameter and return types
- **Annotations:** Use for frameworks (e.g., Laravel, Symfony)

```php
<?php

/**
 * User service for managing user operations.
 *
 * This service handles user CRUD operations and business logic
 * related to user management.
 */
class UserService
{
    /**
     * Find a user by their unique identifier.
     *
     * @param int $userId The unique identifier of the user
     * @return User|null The user if found, null otherwise
     * @throws DatabaseException If database connection fails
     */
    public function findUser(int $userId): ?User
    {
        return $this->repository->find($userId);
    }

    /**
     * @var array<string, mixed> Configuration options
     */
    private array $options = [];
}
```

## 11. Testing (PHPUnit)
- **Test Classes:** Suffix with `Test`
- **Test Methods:** Prefix with `test` or use `@test` annotation
- **Assertions:** Use descriptive assertions
- **Data Providers:** Use for testing multiple scenarios

```php
<?php

namespace Tests\Unit;

use PHPUnit\Framework\TestCase;
use App\Services\UserService;

class UserServiceTest extends TestCase
{
    private UserService $service;

    protected function setUp(): void
    {
        $this->service = new UserService(
            $this->createMock(UserRepository::class),
            $this->createMock(LoggerInterface::class)
        );
    }

    public function testFindUserReturnsUser(): void
    {
        $user = $this->service->findUser(1);

        $this->assertInstanceOf(User::class, $user);
        $this->assertEquals(1, $user->getId());
    }

    /**
     * @dataProvider invalidUserIdProvider
     */
    public function testFindUserWithInvalidId(int $invalidId): void
    {
        $this->expectException(InvalidArgumentException::class);
        $this->service->findUser($invalidId);
    }

    public function invalidUserIdProvider(): array
    {
        return [
            [-1],
            [0],
        ];
    }
}
```

## 12. Best Practices
- **PSR-4 Autoloading:** Follow namespace and directory structure
- **Single Responsibility:** Each class should have one purpose
- **Composer:** Use for dependency management
- **Static Analysis:** Use tools like PHPStan or Psalm
- **Code Style:** Use PHP-CS-Fixer or PHP_CodeSniffer

```php
<?php

// composer.json autoloading
{
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    }
}

// Avoid static methods when possible
// Bad
class UserHelper
{
    public static function formatName(string $name): string
    {
        return ucwords($name);
    }
}

// Good - Use dependency injection
class UserFormatter
{
    public function formatName(string $name): string
    {
        return ucwords($name);
    }
}
```

**BE CONSISTENT.** Follow PSR-12 standards and use automated tools (PHP-CS-Fixer, PHPStan) for enforcement.

*Sources: [PSR-12: Extended Coding Style](https://www.php-fig.org/psr/psr-12/), [PHP: The Right Way](https://phptherightway.com/)*
