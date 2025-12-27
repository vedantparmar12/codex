# Ruby Style Guide Summary

This document summarizes key rules and best practices from the community-driven Ruby Style Guide.

## 1. Source Code Layout
- **Indentation:** 2 spaces (never tabs)
- **Line Length:** Maximum 80-120 characters
- **Empty Lines:** Use to separate logical sections
- **Trailing Whitespace:** Avoid trailing whitespace
- **File Encoding:** UTF-8

```ruby
class User
  attr_reader :name, :email

  def initialize(name, email)
    @name = name
    @email = email
  end

  def greeting
    "Hello, #{@name}!"
  end
end
```

## 2. Naming Conventions
- **snake_case:** Methods, variables, files, directories
- **PascalCase:** Classes and modules
- **SCREAMING_SNAKE_CASE:** Constants
- **Predicate Methods:** End with `?` for boolean methods
- **Dangerous Methods:** End with `!` for methods that modify receiver

```ruby
# Classes and Modules
class UserAccount
  MAX_LOGIN_ATTEMPTS = 3  # Constant

  def initialize(user_name)  # Method and variable
    @user_name = user_name
  end

  def active?  # Predicate method
    @status == :active
  end

  def activate!  # Dangerous method (modifies state)
    @status = :active
  end
end
```

## 3. Syntax
- **Parentheses:** Optional for method calls, but use for clarity
- **do/end vs {}:** Use `{}` for single-line blocks, `do/end` for multi-line
- **String Literals:** Use single quotes unless interpolation needed
- **Symbols:** Prefer symbols for hash keys
- **Percent Literals:** Use for arrays of strings or regex with many slashes

```ruby
# Single-line block
names = users.map { |user| user.name }

# Multi-line block
users.each do |user|
  puts user.name
  puts user.email
end

# String interpolation
message = "Hello, #{name}!"

# Symbols for hash keys (modern syntax)
user = { name: 'John', email: 'john@example.com' }

# Percent literals
words = %w[apple banana cherry]
pattern = %r{^https?://}
```

## 4. Collections
- **Array Literals:** Use `[]` for array creation
- **Hash Literals:** Use `{}` for hash creation
- **Hash Syntax:** Use modern symbol key syntax when possible
- **Fetch:** Use `fetch` for safer hash access
- **Iteration:** Use collection methods over manual iteration

```ruby
# Arrays
numbers = [1, 2, 3, 4, 5]
users = []

# Hashes (modern syntax)
config = {
  host: 'localhost',
  port: 3000,
  ssl: true
}

# Safe hash access
port = config.fetch(:port, 8080)  # Default value if key missing

# Iteration
users.each { |user| puts user.name }
active_users = users.select(&:active?)
user_names = users.map(&:name)
```

## 5. String Manipulation
- **Interpolation:** Prefer over concatenation
- **Multi-line Strings:** Use heredocs for long strings
- **String Mutations:** Use `!` methods when modifying in place

```ruby
# Good: Interpolation
greeting = "Hello, #{user.name}!"

# Bad: Concatenation
greeting = 'Hello, ' + user.name + '!'

# Heredoc for multi-line strings
sql = <<~SQL
  SELECT users.*
  FROM users
  WHERE active = true
  ORDER BY created_at DESC
SQL

# String mutation
name.strip!
name.downcase!
```

## 6. Conditionals
- **if/unless:** Use `unless` for negative conditions (but avoid with else)
- **Ternary:** Use for simple conditions
- **Guard Clauses:** Use early returns
- **case/when:** Use for multiple conditions

```ruby
# Guard clause
def process(user)
  return unless user.active?

  # Process active user
end

# unless (no else clause)
send_notification unless user.opted_out?

# Ternary for simple conditions
status = user.active? ? 'Active' : 'Inactive'

# case/when
case status
when :pending
  process_pending
when :active
  process_active
when :archived
  process_archived
else
  handle_unknown
end
```

## 7. Methods
- **Short Methods:** Keep methods small and focused
- **Default Arguments:** Use for optional parameters
- **Keyword Arguments:** Use for methods with multiple parameters
- **Return Values:** Implicit return (last expression)
- **Bang Methods:** Use `!` suffix for methods that modify receiver or raise exceptions

```ruby
# Default arguments
def greet(name, greeting = 'Hello')
  "#{greeting}, #{name}!"
end

# Keyword arguments
def create_user(name:, email:, role: 'user')
  User.new(name: name, email: email, role: role)
end

# Implicit return
def full_name
  "#{first_name} #{last_name}"
end

# Bang method (modifies receiver)
def activate!
  self.status = :active
  save!
end
```

## 8. Classes and Modules
- **attr_accessor/reader/writer:** Use for simple getters/setters
- **Single Responsibility:** Each class should have one purpose
- **Modules:** Use for mixins and namespacing
- **Class Methods:** Use `self.` prefix or `class << self`

```ruby
class User
  attr_reader :name, :email
  attr_accessor :role

  def initialize(name:, email:)
    @name = name
    @email = email
    @role = :user
  end

  # Class method
  def self.find_by_email(email)
    # Find user
  end

  # Or using class << self
  class << self
    def active
      where(status: :active)
    end
  end
end

# Module for mixins
module Authenticatable
  def authenticate(password)
    password_digest == encrypt(password)
  end
end

class User
  include Authenticatable
end
```

## 9. Exceptions
- **Specific Exceptions:** Rescue specific exceptions, not all
- **Custom Exceptions:** Inherit from StandardError
- **ensure:** Use for cleanup code
- **Fail vs Raise:** Use `raise` (fail is an alias)

```ruby
# Rescue specific exceptions
begin
  process_payment
rescue PaymentError => e
  log_error(e)
  notify_user
rescue NetworkError => e
  retry_later(e)
ensure
  close_connection
end

# Custom exception
class ValidationError < StandardError
  attr_reader :errors

  def initialize(errors)
    @errors = errors
    super("Validation failed: #{errors.join(', ')}")
  end
end

# Raising exceptions
raise ValidationError.new(['Email is invalid', 'Password too short'])
```

## 10. Testing (RSpec)
- **describe/context:** Organize tests clearly
- **let/let!:** Use for test data setup
- **subject:** Define subject under test
- **Expectations:** Use clear, readable expectations

```ruby
RSpec.describe User do
  describe '#activate!' do
    let(:user) { User.new(name: 'John', status: :pending) }

    context 'when user is pending' do
      it 'changes status to active' do
        expect { user.activate! }.to change { user.status }.from(:pending).to(:active)
      end

      it 'sends activation email' do
        expect(UserMailer).to receive(:activation_email).with(user)
        user.activate!
      end
    end
  end
end
```

## 11. Rails-Specific Conventions
- **ActiveRecord:** Use scopes, validations, and associations
- **Controllers:** Keep thin, delegate to models/services
- **Routes:** Use RESTful routes
- **Strong Parameters:** Always use for mass assignment

```ruby
# Model
class User < ApplicationRecord
  has_many :posts
  validates :email, presence: true, uniqueness: true

  scope :active, -> { where(status: :active) }

  def full_name
    "#{first_name} #{last_name}"
  end
end

# Controller
class UsersController < ApplicationController
  def create
    @user = User.new(user_params)

    if @user.save
      redirect_to @user, notice: 'User created successfully'
    else
      render :new
    end
  end

  private

  def user_params
    params.require(:user).permit(:name, :email, :role)
  end
end
```

## 12. Best Practices
- **DRY:** Don't Repeat Yourself
- **SOLID Principles:** Follow object-oriented design principles
- **Rubocop:** Use for style enforcement
- **Code Comments:** Explain why, not what
- **Performance:** Use `&:method_name` syntax for simple blocks

```ruby
# Good: Symbol to proc
user_names = users.map(&:name)

# Instead of
user_names = users.map { |user| user.name }

# Memoization for expensive operations
def expensive_calculation
  @expensive_calculation ||= perform_calculation
end

# Safe navigation operator (Ruby 2.3+)
user&.profile&.avatar_url
```

**BE CONSISTENT.** Follow the community Ruby Style Guide and use Rubocop for automated checks.

*Sources: [Ruby Style Guide](https://rubystyle.guide/), [Rails Style Guide](https://rails.rubystyle.guide/)*
