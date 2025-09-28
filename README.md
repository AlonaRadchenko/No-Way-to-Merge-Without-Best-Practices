# No Way to Merge Without Best Practices

*A comprehensive guide to coding best practices that will make your mentor (and team) happier*

> **Note**: This guide covers fundamental best practices across multiple programming languages and development workflows. It emphasizes clean code, maintainable practices, and collaborative development standards.

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Code Quality](https://img.shields.io/badge/Code%20Quality-Best%20Practices-blue.svg)](#code-quality)

This guide is available in other languages too. See [Translation](#translation)

Other Best Practice Guides
  - [JavaScript Best Practices](javascript/)
  - [Python Best Practices](python/)
  - [Git & Version Control](git/)
  - [Code Review Guidelines](code-review/)
  - [Testing Strategies](testing/)

## Table of Contents

  1. [Code Quality](#code-quality)
  1. [Naming Conventions](#naming-conventions)
  1. [Code Structure](#code-structure)
  1. [Comments & Documentation](#comments--documentation)
  1. [Version Control](#version-control)
  1. [Testing](#testing)
  1. [Code Review](#code-review)
  1. [Error Handling](#error-handling)
  1. [Performance](#performance)
  1. [Security](#security)
  1. [Refactoring](#refactoring)
  1. [Dependencies](#dependencies)
  1. [Development Workflow](#development-workflow)
  1. [Communication](#communication)
  1. [Resources](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Translation](#translation)
  1. [Contributors](#contributors)
  1. [License](#license)
  1. [Amendments](#amendments)

## Code Quality

  <a name="quality--readability"></a><a name="1.1"></a>
  - [1.1](#quality--readability) **Readability First**: Write code as if the person who ends up maintaining it is a violent psychopath who knows where you live.

    > Why? Code is read much more often than it's written. Prioritize clarity over cleverness.

    ```javascript
    // bad
    const d = new Date();
    const y = d.getFullYear();
    const m = d.getMonth();

    // good
    const currentDate = new Date();
    const currentYear = currentDate.getFullYear();
    const currentMonth = currentDate.getMonth();
    ```

  <a name="quality--consistency"></a><a name="1.2"></a>
  - [1.2](#quality--consistency) **Consistency**: Follow the established patterns in your codebase. If you can't find patterns, establish them.

    > Why? Consistency reduces cognitive load and makes onboarding new team members easier.

    ```python
    # bad - mixing conventions
    def get_user_name():
        pass
    
    def getUserAge():
        pass

    # good - consistent naming
    def get_user_name():
        pass
    
    def get_user_age():
        pass
    ```

  <a name="quality--single-responsibility"></a><a name="1.3"></a>
  - [1.3](#quality--single-responsibility) **Single Responsibility**: Each function, class, and module should have one reason to change.

    > Why? This makes your code more modular, testable, and maintainable.

    ```python
    # bad
    def process_user_data(user):
        # validates user data
        if not user.email or '@' not in user.email:
            raise ValueError("Invalid email")
        
        # saves to database
        database.save(user)
        
        # sends welcome email
        send_email(user.email, "Welcome!")

    # good
    def validate_user(user):
        if not user.email or '@' not in user.email:
            raise ValueError("Invalid email")

    def save_user(user):
        database.save(user)

    def send_welcome_email(user):
        send_email(user.email, "Welcome!")
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

  <a name="naming--descriptive"></a><a name="2.1"></a>
  - [2.1](#naming--descriptive) **Use Descriptive Names**: Names should clearly express what the variable, function, or class does.

    ```javascript
    // bad
    const u = getUserData();
    const dt = new Date();

    // good
    const currentUser = getUserData();
    const timestamp = new Date();
    ```

  <a name="naming--avoid-mental-mapping"></a><a name="2.2"></a>
  - [2.2](#naming--avoid-mental-mapping) **Avoid Mental Mapping**: Don't force readers to translate your names.

    ```python
    # bad
    for i in range(len(users)):
        # ... do something with users[i]

    # good
    for user in users:
        # ... do something with user
    ```

  <a name="naming--searchable"></a><a name="2.3"></a>
  - [2.3](#naming--searchable) **Use Searchable Names**: Magic numbers and single-letter variables are hard to find.

    ```java
    // bad
    if (user.age > 18) { ... }

    // good
    private static final int LEGAL_AGE = 18;
    if (user.age > LEGAL_AGE) { ... }
    ```

**[⬆ back to top](#table-of-contents)**

## Code Structure

  <a name="structure--small-functions"></a><a name="3.1"></a>
  - [3.1](#structure--small-functions) **Keep Functions Small**: Functions should do one thing and do it well. Aim for 20 lines or fewer.

    ```python
    # bad
    def process_order(order):
        # validate order (10 lines)
        # calculate discount (15 lines)  
        # apply taxes (10 lines)
        # save to database (8 lines)
        # send confirmation (5 lines)
        pass

    # good
    def process_order(order):
        validate_order(order)
        discount = calculate_discount(order)
        total = apply_taxes(order, discount)
        save_order(order, total)
        send_confirmation(order)
    ```

  <a name="structure--avoid-nesting"></a><a name="3.2"></a>
  - [3.2](#structure--avoid-nesting) **Minimize Nesting**: Use early returns to reduce indentation levels.

    ```javascript
    // bad
    function processUser(user) {
        if (user) {
            if (user.isActive) {
                if (user.hasPermission) {
                    // do something
                }
            }
        }
    }

    // good
    function processUser(user) {
        if (!user) return;
        if (!user.isActive) return;
        if (!user.hasPermission) return;
        
        // do something
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comments & Documentation

  <a name="comments--why-not-what"></a><a name="4.1"></a>
  - [4.1](#comments--why-not-what) **Explain Why, Not What**: Good code is self-documenting. Comments should explain the reasoning.

    ```python
    # bad
    # Increment i by 1
    i += 1

    # good
    # Retry mechanism: attempt failed operations up to 3 times
    # before giving up to handle temporary network issues
    for attempt in range(MAX_RETRIES):
        try:
            result = api_call()
            break
        except TemporaryError:
            continue
    ```

  <a name="comments--update"></a><a name="4.2"></a>
  - [4.2](#comments--update) **Keep Comments Current**: Outdated comments are worse than no comments.

    ```java
    // bad
    // TODO: Remove this hack once API v2 is ready (written 2 years ago)
    
    // good  
    // Workaround for API limitation: endpoint doesn't support batch operations
    // Consider migrating to batch API when available
    ```

  <a name="comments--readme"></a><a name="4.3"></a>
  - [4.3](#comments--readme) **Maintain Good Documentation**: Every project should have clear setup instructions and usage examples.

    ```markdown
    ## Quick Start

    1. Clone the repository
    2. Install dependencies: `npm install`
    3. Copy config: `cp .env.example .env`
    4. Run tests: `npm test`
    5. Start development: `npm run dev`
    ```

**[⬆ back to top](#table-of-contents)**

## Version Control

  <a name="git--atomic-commits"></a><a name="5.1"></a>
  - [5.1](#git--atomic-commits) **Make Atomic Commits**: Each commit should represent one logical change.

    ```bash
    # bad
    git commit -m "Fix bugs and add new feature"

    # good
    git commit -m "Fix user authentication timeout issue"
    git commit -m "Add email validation to registration form"
    ```

  <a name="git--descriptive-messages"></a><a name="5.2"></a>
  - [5.2](#git--descriptive-messages) **Write Descriptive Commit Messages**: Follow conventional commit format.

    ```bash
    # Format: type(scope): description
    
    feat(auth): add two-factor authentication
    fix(api): resolve memory leak in user service
    docs(readme): update installation instructions
    refactor(utils): extract common validation functions
    ```

  <a name="git--branch-naming"></a><a name="5.3"></a>
  - [5.3](#git--branch-naming) **Use Meaningful Branch Names**: Branch names should describe the work being done.

    ```bash
    # bad
    git checkout -b stuff
    git checkout -b john-branch

    # good
    git checkout -b feature/user-profile-edit
    git checkout -b bugfix/login-validation-error
    git checkout -b hotfix/security-patch-2023
    ```

**[⬆ back to top](#table-of-contents)**

## Testing

  <a name="testing--write-tests"></a><a name="6.1"></a>
  - [6.1](#testing--write-tests) **Write Tests First**: Test-driven development leads to better design and fewer bugs.

    ```python
    def test_user_validation():
        # Arrange
        user = User(email="test@example.com", age=25)
        
        # Act
        result = validate_user(user)
        
        # Assert
        assert result.is_valid == True
        assert result.errors == []
    ```

  <a name="testing--test-names"></a><a name="6.2"></a>
  - [6.2](#testing--test-names) **Use Descriptive Test Names**: Test names should describe the scenario being tested.

    ```javascript
    // bad
    test('user test', () => { ... });

    // good
    test('should return error when user email is invalid', () => { ... });
    test('should successfully create user with valid data', () => { ... });
    ```

  <a name="testing--coverage"></a><a name="6.3"></a>
  - [6.3](#testing--coverage) **Aim for High Coverage**: Strive for 80%+ code coverage, but focus on quality over quantity.

    > Why? High coverage doesn't guarantee bug-free code, but it does ensure your critical paths are tested.

**[⬆ back to top](#table-of-contents)**

## Code Review

  <a name="review--constructive"></a><a name="7.1"></a>
  - [7.1](#review--constructive) **Be Constructive**: Provide specific, actionable feedback with examples.

    ```markdown
    # bad
    "This is wrong"

    # good
    "Consider extracting this logic into a separate function for better reusability.
    Example: `validateEmailFormat(email)` could be used in multiple places."
    ```

  <a name="review--ask-questions"></a><a name="7.2"></a>
  - [7.2](#review--ask-questions) **Ask Questions**: Use questions to understand reasoning and suggest improvements.

    ```markdown
    "What happens if this API call fails? Should we add error handling here?"
    "Could we use a more descriptive variable name here? What does 'data' represent?"
    ```

  <a name="review--praise"></a><a name="7.3"></a>
  - [7.3](#review--praise) **Recognize Good Work**: Acknowledge clever solutions and good practices.

    ```markdown
    "Nice use of the adapter pattern here - makes the code much more flexible!"
    "Great test coverage on this feature!"
    ```

**[⬆ back to top](#table-of-contents)**

## Error Handling

  <a name="errors--fail-fast"></a><a name="8.1"></a>
  - [8.1](#errors--fail-fast) **Fail Fast**: Detect problems early and provide clear error messages.

    ```python
    # bad
    def divide(a, b):
        return a / b  # Will throw unclear error

    # good
    def divide(a, b):
        if b == 0:
            raise ValueError("Cannot divide by zero")
        return a / b
    ```

  <a name="errors--meaningful-messages"></a><a name="8.2"></a>
  - [8.2](#errors--meaningful-messages) **Provide Meaningful Error Messages**: Help users understand what went wrong and how to fix it.

    ```javascript
    // bad
    throw new Error("Invalid input");

    // good
    throw new Error(
        `Invalid email format: "${email}". ` +
        `Please provide a valid email address (e.g., user@example.com)`
    );
    ```

**[⬆ back to top](#table-of-contents)**

## Performance

  <a name="performance--premature-optimization"></a><a name="9.1"></a>
  - [9.1](#performance--premature-optimization) **Avoid Premature Optimization**: Write clear code first, optimize when needed.

    > "Premature optimization is the root of all evil" - Donald Knuth

  <a name="performance--measure"></a><a name="9.2"></a>
  - [9.2](#performance--measure) **Measure Before Optimizing**: Use profiling tools to identify actual bottlenecks.

    ```python
    import time

    def profile_function(func):
        def wrapper(*args, **kwargs):
            start = time.time()
            result = func(*args, **kwargs)
            end = time.time()
            print(f"{func.__name__} took {end - start:.2f} seconds")
            return result
        return wrapper
    ```

**[⬆ back to top](#table-of-contents)**

## Security

  <a name="security--input-validation"></a><a name="10.1"></a>
  - [10.1](#security--input-validation) **Validate All Inputs**: Never trust user input or external data.

    ```python
    # bad
    def get_user(user_id):
        query = f"SELECT * FROM users WHERE id = {user_id}"
        return database.execute(query)

    # good
    def get_user(user_id):
        if not isinstance(user_id, int) or user_id <= 0:
            raise ValueError("Invalid user ID")
        
        query = "SELECT * FROM users WHERE id = %s"
        return database.execute(query, (user_id,))
    ```

  <a name="security--secrets"></a><a name="10.2"></a>
  - [10.2](#security--secrets) **Keep Secrets Secret**: Never commit passwords, API keys, or other sensitive data.

    ```bash
    # Use environment variables
    DATABASE_PASSWORD=your_password_here

    # Or configuration files (that are gitignored)
    echo "config.local.json" >> .gitignore
    ```

**[⬆ back to top](#table-of-contents)**

## Resources

**Books**

  - [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) - Robert C. Martin
  - [The Pragmatic Programmer](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/) - David Thomas & Andrew Hunt
  - [Code Complete](https://www.amazon.com/Code-Complete-Practical-Handbook-Construction/dp/0735619670) - Steve McConnell
  - [Refactoring](https://martinfowler.com/books/refactoring.html) - Martin Fowler

**Online Resources**

  - [GitHub Flow](https://guides.github.com/introduction/flow/)
  - [Semantic Versioning](https://semver.org/)
  - [Keep a Changelog](https://keepachangelog.com/)
  - [Conventional Commits](https://www.conventionalcommits.org/)

**Tools**

  - Code Formatters: Prettier, Black, gofmt
  - Linters: ESLint, Pylint, Rubocop
  - Static Analysis: SonarQube, CodeClimate
  - CI/CD: GitHub Actions, GitLab CI, Jenkins

**[⬆ back to top](#table-of-contents)**

## In the Wild

Organizations using these best practices:
  - **Google**: [Google Style Guides](https://google.github.io/styleguide/)
  - **Airbnb**: [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
  - **GitHub**: [GitHub Engineering](https://github.blog/category/engineering/)

**[⬆ back to top](#table-of-contents)**

## Translation

This guide is available in other languages:
  - 🇪🇸 **Spanish**: Coming soon
  - 🇫🇷 **French**: Coming soon
  - 🇩🇪 **German**: Coming soon

## Contributors

  - [View Contributors](https://github.com/AlonaRadchenko/No-Way-to-Merge-Without-Best-Practices/graphs/contributors)

## License

[MIT License](LICENSE)

Copyright (c) 2024

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

**[⬆ back to top](#table-of-contents)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team's style guide. Below, you may list some amendments to the style guide. This allows you to periodically update your style guide without having to deal with merge conflicts.
