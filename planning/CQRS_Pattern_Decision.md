# CQRS PATTERN DECISION DOCUMENT
## Command Query Responsibility Segregation in Backend Architecture
### Version 1.0

**Prepared:** February 12, 2026  
**Decision:** Use CQRS with Wolverine Framework  
**Status:** Approved  
**Classification:** Internal

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [What is CQRS?](#2-what-is-cqrs)
3. [Why We're Using CQRS](#3-why-were-using-cqrs)
4. [Wolverine Framework](#4-wolverine-framework)
5. [Implementation Strategy](#5-implementation-strategy)
6. [Benefits for Our Project](#6-benefits-for-our-project)
7. [Alternatives Considered](#7-alternatives-considered)
8. [Code Examples](#8-code-examples)
9. [When NOT to Use CQRS](#9-when-not-to-use-cqrs)
10. [Decision Rationale](#10-decision-rationale)

---

## 1. EXECUTIVE SUMMARY

### Decision

**YES, we ARE using CQRS (Command Query Responsibility Segregation) pattern in our backend architecture.**

### Implementation

- **Framework:** Wolverine (modern .NET CQRS and messaging framework)
- **Pattern Type:** CQRS without Event Sourcing (simpler approach)
- **Scope:** All backend modules (Security, Notifications, Logging, Chat, etc.)

### Key Benefits

1. ✅ **Clear Separation** - Commands (write) and Queries (read) are explicitly separated
2. ✅ **Testability** - Easy to unit test business logic in isolation
3. ✅ **Scalability** - Can optimize reads and writes independently
4. ✅ **Maintainability** - Single Responsibility Principle for each operation
5. ✅ **Audit Trail** - Every command is an explicit, traceable action

---

## 2. WHAT IS CQRS?

### Definition

**CQRS (Command Query Responsibility Segregation)** is an architectural pattern that separates:

- **Commands** - Operations that CHANGE state (Create, Update, Delete)
- **Queries** - Operations that READ state (Get, List, Search)

### Basic Concept

```
Traditional Approach (Without CQRS):
┌─────────────────────────┐
│   Service Layer         │
│  ┌──────────────────┐   │
│  │ CreateUser()     │   │
│  │ UpdateUser()     │   │
│  │ DeleteUser()     │   │
│  │ GetUser()        │   │
│  │ GetAllUsers()    │   │
│  └──────────────────┘   │
└─────────────────────────┘
         ↓
    Single Database


CQRS Approach:
┌─────────────────────────┐       ┌─────────────────────────┐
│   Commands (Write)      │       │   Queries (Read)        │
│  ┌──────────────────┐   │       │  ┌──────────────────┐   │
│  │ CreateUser       │   │       │  │ GetUserById      │   │
│  │ UpdateUser       │   │       │  │ GetAllUsers      │   │
│  │ DeleteUser       │   │       │  │ SearchUsers      │   │
│  └──────────────────┘   │       │  └──────────────────┘   │
└─────────────────────────┘       └─────────────────────────┘
         ↓                                   ↓
    Write Database                     Read Database
    (can be same or different)
```

### Key Principles

1. **Commands** (Write Operations):
   - Change system state
   - Return success/failure (not data)
   - Can trigger side effects
   - Examples: `RegisterUser`, `LoginUser`, `AssignRole`

2. **Queries** (Read Operations):
   - Never change state
   - Return data
   - Can be cached
   - Examples: `GetUserById`, `GetUserPermissions`, `GetAuditLog`

---

## 3. WHY WE'RE USING CQRS

### Reason #1: Clear Business Operations

Each business operation becomes an **explicit, named command**:

```csharp
// WITHOUT CQRS (unclear intent)
await userService.UpdateUser(userId, new { email = newEmail });
// What is this doing? Updating user? Changing email? Verifying email?

// WITH CQRS (explicit intent)
await bus.InvokeAsync(new ChangeUserEmailCommand(userId, newEmail));
// Crystal clear: We're changing the user's email
```

✅ **Benefit:** Code is self-documenting, business operations are explicit

### Reason #2: Single Responsibility Principle

Each handler does **ONE THING**:

```csharp
// WITHOUT CQRS (God object, does everything)
public class UserService
{
    public async Task<User> CreateUser(CreateUserDto dto) { /* ... */ }
    public async Task<User> UpdateUser(Guid id, UpdateUserDto dto) { /* ... */ }
    public async Task DeleteUser(Guid id) { /* ... */ }
    public async Task<User> GetUser(Guid id) { /* ... */ }
    public async Task<List<User>> GetAllUsers() { /* ... */ }
    public async Task<bool> ValidateEmail(string email) { /* ... */ }
    public async Task SendWelcomeEmail(Guid userId) { /* ... */ }
    // 20+ more methods...
}

// WITH CQRS (focused handlers)
public class CreateUserHandler
{
    public async Task<CreateUserResult> Handle(CreateUserCommand command)
    {
        // Only handles user creation
        // 30 lines of focused logic
    }
}

public class GetUserByIdHandler
{
    public async Task<User> Handle(GetUserByIdQuery query)
    {
        // Only handles getting user by ID
        // 10 lines of focused logic
    }
}
```

✅ **Benefit:** Easier to understand, test, and maintain

### Reason #3: Perfect for Modular Architecture

Each module (Security, Notifications, Chat) has clear operations:

```
Security Module Commands:
- RegisterUser
- LoginUser
- LogoutUser
- ChangePassword
- EnableMFA
- AssignRole

Security Module Queries:
- GetUserById
- GetUserPermissions
- GetAuditLog
- GetActiveSessions
```

✅ **Benefit:** Clear API boundaries for each module

### Reason #4: Testability

Each handler is **independently testable**:

```csharp
[Fact]
public async Task LoginUser_WithValidCredentials_ReturnsToken()
{
    // Arrange
    var handler = new LoginHandler(mockDb, mockPasswordService, mockTokenService);
    var command = new LoginCommand("test@example.com", "password123");

    // Act
    var result = await handler.Handle(command);

    // Assert
    Assert.NotNull(result.AccessToken);
}
```

✅ **Benefit:** Easy unit testing, no mocking frameworks needed for simple cases

### Reason #5: Audit Trail

Every command is a **traceable business event**:

```csharp
public class AuditLoggingMiddleware
{
    public async Task Invoke(ICommand command, Func<Task> next)
    {
        var commandName = command.GetType().Name;
        var userId = GetCurrentUserId();
        
        await LogAudit($"{userId} executed {commandName}", command);
        await next();
        await LogAudit($"{commandName} completed successfully");
    }
}
```

✅ **Benefit:** Built-in audit trail for all operations

### Reason #6: Performance Optimization

Commands and Queries can be **optimized independently**:

```csharp
// Commands: Write to normalized database
public class CreateUserHandler
{
    public async Task Handle(CreateUserCommand cmd)
    {
        // Write to normalized tables
        await _db.Users.AddAsync(user);
        await _db.UserRoles.AddAsync(userRole);
        await _db.SaveChangesAsync();
    }
}

// Queries: Read from denormalized views or cache
public class GetUserDashboardHandler
{
    public async Task<UserDashboard> Handle(GetUserDashboardQuery query)
    {
        // Read from materialized view or Redis cache
        return await _cache.GetOrCreateAsync(
            $"dashboard:{query.UserId}",
            async () => await BuildDashboard(query.UserId)
        );
    }
}
```

✅ **Benefit:** Optimize reads and writes separately for better performance

---

## 4. WOLVERINE FRAMEWORK

### What is Wolverine?

**Wolverine** is a modern .NET framework for:
- CQRS (Command Query Responsibility Segregation)
- Messaging and event-driven architecture
- Durable message processing
- Local and distributed messaging

### Why Wolverine (vs Alternatives)?

| Feature | Wolverine | MediatR | NServiceBus | Mass Transit |
|---------|-----------|---------|-------------|--------------|
| **CQRS Support** | ✅ Excellent | ✅ Excellent | ✅ Good | ✅ Good |
| **Performance** | ⚡ Fastest | ⚡ Fast | 🐢 Moderate | 🐢 Moderate |
| **Learning Curve** | 📚 Easy | 📚 Easy | 📚 Steep | 📚 Steep |
| **Built-in Messaging** | ✅ Yes | ❌ No | ✅ Yes | ✅ Yes |
| **Durable Messages** | ✅ Yes | ❌ No | ✅ Yes | ✅ Yes |
| **Cost** | 💰 Free | 💰 Free | 💰 Commercial | 💰 Free |
| **License** | MIT | Apache 2.0 | Commercial | Apache 2.0 |
| **Best For** | Modern .NET | Simple CQRS | Enterprise | Microservices |

### Wolverine Key Features

#### 1. Convention-Based Handlers

```csharp
// Handler is automatically discovered (no registration needed!)
public class RegisterUserHandler
{
    // Wolverine finds this method automatically
    public async Task<RegisterUserResult> Handle(RegisterUserCommand command)
    {
        // Your logic here
    }
}
```

#### 2. Built-in Dependency Injection

```csharp
public class LoginHandler
{
    // Dependencies injected automatically
    private readonly SecurityDbContext _db;
    private readonly IPasswordService _passwordService;
    private readonly ITokenService _tokenService;

    public LoginHandler(
        SecurityDbContext db,
        IPasswordService passwordService,
        ITokenService tokenService)
    {
        _db = db;
        _passwordService = passwordService;
        _tokenService = tokenService;
    }
}
```

#### 3. Middleware Pipeline

```csharp
// Add middleware for cross-cutting concerns
builder.Services.AddWolverine(opts =>
{
    // Transaction per command
    opts.Policies.AutoApplyTransactions();
    
    // Audit logging
    opts.Policies.UseDurableInbox();
    
    // Validation
    opts.Policies.UseDurableOutbox();
});
```

#### 4. Local and Distributed Messaging

```csharp
// Local message (in-process)
await bus.InvokeAsync(new CreateUserCommand(...));

// Distributed message (across services)
await bus.PublishAsync(new UserCreatedEvent(...));
```

### Wolverine Setup

```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);

// Add Wolverine
builder.Host.UseWolverine(opts =>
{
    // Discover handlers automatically
    opts.Discovery.IncludeAssembly(typeof(RegisterUserHandler).Assembly);
    
    // Add local queue for async processing
    opts.LocalQueue("important")
        .MaximumParallelMessages(5);
});

var app = builder.Build();
app.Run();
```

---

## 5. IMPLEMENTATION STRATEGY

### Our CQRS Structure

```
CompanyName.Security.Core/
├── Commands/
│   ├── Auth/
│   │   ├── RegisterUserCommand.cs
│   │   ├── LoginCommand.cs
│   │   └── LogoutCommand.cs
│   ├── Users/
│   │   ├── CreateUserCommand.cs
│   │   ├── UpdateUserCommand.cs
│   │   └── DeleteUserCommand.cs
│   └── Roles/
│       ├── CreateRoleCommand.cs
│       └── AssignRoleCommand.cs
├── Queries/
│   ├── Auth/
│   │   └── GetCurrentUserQuery.cs
│   ├── Users/
│   │   ├── GetUserByIdQuery.cs
│   │   ├── GetAllUsersQuery.cs
│   │   └── SearchUsersQuery.cs
│   └── Roles/
│       ├── GetRoleByIdQuery.cs
│       └── GetUserRolesQuery.cs
└── Handlers/
    ├── Auth/
    │   ├── RegisterUserHandler.cs
    │   ├── LoginHandler.cs
    │   └── LogoutHandler.cs
    ├── Users/
    │   ├── CreateUserHandler.cs
    │   ├── GetUserByIdHandler.cs
    │   └── GetAllUsersHandler.cs
    └── Roles/
        ├── CreateRoleHandler.cs
        └── GetRoleByIdHandler.cs
```

### Naming Conventions

**Commands (Write Operations):**
- **Verb + Noun** format
- Examples: `CreateUser`, `UpdateProfile`, `DeleteRole`, `AssignPermission`
- Suffix with `Command`: `CreateUserCommand`

**Queries (Read Operations):**
- **Get + Noun** format or **Search/List**
- Examples: `GetUserById`, `GetAllUsers`, `SearchUsers`
- Suffix with `Query`: `GetUserByIdQuery`

**Handlers:**
- **Command/Query Name + Handler**
- Examples: `CreateUserHandler`, `GetUserByIdHandler`

---

## 6. BENEFITS FOR OUR PROJECT

### Benefit #1: Module Clarity

Each module has **explicit operations**:

```
Security Module:
├── Commands: RegisterUser, LoginUser, ChangePassword, EnableMFA
└── Queries: GetUserById, GetUserPermissions, GetAuditLog

Notifications Module:
├── Commands: SendEmail, SendSMS, ScheduleNotification
└── Queries: GetNotificationHistory, GetDeliveryStatus

Chat Module:
├── Commands: SendMessage, CreateChannel, InviteUser
└── Queries: GetMessages, GetChannels, GetUserPresence
```

✅ **Result:** Clear module boundaries, easy to understand what each module does

### Benefit #2: Easy Testing

```csharp
// Test a command handler
[Fact]
public async Task RegisterUser_WithValidData_CreatesUser()
{
    var handler = new RegisterUserHandler(mockDb, mockPasswordService);
    var command = new RegisterUserCommand("test@example.com", "password123");
    
    var result = await handler.Handle(command);
    
    Assert.True(result.Success);
}

// Test a query handler
[Fact]
public async Task GetUserById_WithValidId_ReturnsUser()
{
    var handler = new GetUserByIdHandler(mockDb);
    var query = new GetUserByIdQuery(userId);
    
    var user = await handler.Handle(query);
    
    Assert.NotNull(user);
}
```

✅ **Result:** 80%+ test coverage easily achievable

### Benefit #3: Audit Trail

Every command is **automatically logged**:

```csharp
// Middleware logs all commands
public class AuditMiddleware
{
    public async Task Invoke(object command, Func<Task> next)
    {
        await _auditLog.LogAsync(new AuditEntry
        {
            Action = command.GetType().Name,
            UserId = GetCurrentUserId(),
            Timestamp = DateTime.UtcNow,
            Details = JsonSerializer.Serialize(command)
        });
        
        await next();
    }
}
```

✅ **Result:** Complete audit trail for compliance

### Benefit #4: Performance Optimization

Queries can be **cached independently**:

```csharp
public class GetUserPermissionsHandler
{
    public async Task<List<string>> Handle(GetUserPermissionsQuery query)
    {
        // Cache query results
        return await _cache.GetOrCreateAsync(
            $"permissions:{query.UserId}",
            async () => await FetchPermissions(query.UserId),
            TimeSpan.FromMinutes(5)
        );
    }
}

// Commands don't use cache
public class AssignPermissionHandler
{
    public async Task Handle(AssignPermissionCommand command)
    {
        await _db.UserPermissions.AddAsync(permission);
        await _db.SaveChangesAsync();
        
        // Invalidate cache
        await _cache.RemoveAsync($"permissions:{command.UserId}");
    }
}
```

✅ **Result:** Faster reads, reliable writes

### Benefit #5: Future-Proof for Microservices

If we need to split into microservices later, CQRS makes it easy:

```
Monolith (Current):
┌────────────────────────────┐
│  Security Module           │
│  ├── Commands → Database   │
│  └── Queries → Database    │
└────────────────────────────┘


Microservices (Future):
┌────────────────────┐       ┌────────────────────┐
│  Write Service     │       │  Read Service      │
│  Commands → Write  │       │  Queries → Read    │
│  Database          │   →   │  Database (replica)│
└────────────────────┘       └────────────────────┘
        ↓ Events
┌────────────────────┐
│  Event Store       │
└────────────────────┘
```

✅ **Result:** Easy to scale and distribute

---

## 7. ALTERNATIVES CONSIDERED

### Option 1: Traditional Service Layer (Repository Pattern)

**WITHOUT CQRS:**

```csharp
public interface IUserService
{
    Task<User> CreateUser(CreateUserDto dto);
    Task<User> UpdateUser(Guid id, UpdateUserDto dto);
    Task DeleteUser(Guid id);
    Task<User> GetUser(Guid id);
    Task<List<User>> GetAllUsers();
}

public class UserService : IUserService
{
    private readonly IUserRepository _userRepository;
    
    public async Task<User> CreateUser(CreateUserDto dto)
    {
        // Business logic mixed with data access
        var user = new User { /* ... */ };
        return await _userRepository.AddAsync(user);
    }
    
    // ... more methods
}
```

**Pros:**
- ✅ Simpler architecture
- ✅ Familiar to most developers
- ✅ Less boilerplate

**Cons:**
- ❌ Service classes become large (God objects)
- ❌ Unclear operation boundaries
- ❌ Harder to test (mock entire service)
- ❌ No explicit audit trail
- ❌ Performance optimization harder

**Why NOT chosen:** Doesn't scale well for large, modular systems

---

### Option 2: MediatR (Simpler CQRS)

**WITH MediatR:**

```csharp
// Command
public record CreateUserCommand(string Email, string Password) : IRequest<CreateUserResult>;

// Handler
public class CreateUserHandler : IRequestHandler<CreateUserCommand, CreateUserResult>
{
    public async Task<CreateUserResult> Handle(CreateUserCommand request, CancellationToken ct)
    {
        // Implementation
    }
}

// Usage
await _mediator.Send(new CreateUserCommand("test@example.com", "pass123"));
```

**Pros:**
- ✅ Simpler than Wolverine
- ✅ CQRS pattern
- ✅ Testable
- ✅ Large community

**Cons:**
- ❌ No built-in messaging
- ❌ No durable message processing
- ❌ Less performant than Wolverine
- ❌ More boilerplate (need to register handlers)

**Why NOT chosen:** Wolverine offers more features with similar simplicity

---

### Option 3: NServiceBus (Enterprise Service Bus)

**WITH NServiceBus:**

```csharp
// Command
public class CreateUserCommand : ICommand
{
    public string Email { get; set; }
    public string Password { get; set; }
}

// Handler
public class CreateUserHandler : IHandleMessages<CreateUserCommand>
{
    public async Task Handle(CreateUserCommand message, IMessageHandlerContext context)
    {
        // Implementation
    }
}
```

**Pros:**
- ✅ Enterprise-grade
- ✅ Distributed messaging
- ✅ Durable processing
- ✅ Advanced features

**Cons:**
- ❌ Commercial license required
- ❌ Steep learning curve
- ❌ Overkill for monolith
- ❌ Complex setup

**Why NOT chosen:** Too complex and expensive for our needs

---

### Comparison Table

| Approach | Complexity | Performance | Features | Cost | Best For |
|----------|------------|-------------|----------|------|----------|
| **Traditional Service** | Low | Good | Basic | Free | Simple apps |
| **MediatR** | Medium | Good | CQRS | Free | Medium apps |
| **Wolverine** | Medium | Excellent | CQRS + Messaging | Free | ✅ Our use case |
| **NServiceBus** | High | Excellent | Enterprise | $$ | Large enterprises |

---

## 8. CODE EXAMPLES

### Example 1: User Registration (Complete Flow)

#### 1. Command Definition

```csharp
// Commands/Auth/RegisterUserCommand.cs
namespace CompanyName.Security.Core.Commands.Auth;

public record RegisterUserCommand(
    string Email,
    string Username,
    string Password,
    string? FirstName,
    string? LastName
);

public record RegisterUserResult(
    Guid UserId,
    string Message,
    bool Success
);
```

#### 2. Command Handler

```csharp
// Handlers/Auth/RegisterUserHandler.cs
namespace CompanyName.Security.Core.Handlers.Auth;

public class RegisterUserHandler
{
    private readonly SecurityDbContext _db;
    private readonly IPasswordService _passwordService;
    private readonly ILogger<RegisterUserHandler> _logger;

    public RegisterUserHandler(
        SecurityDbContext db,
        IPasswordService passwordService,
        ILogger<RegisterUserHandler> logger)
    {
        _db = db;
        _passwordService = passwordService;
        _logger = logger;
    }

    // Wolverine automatically calls this method
    public async Task<RegisterUserResult> Handle(RegisterUserCommand command)
    {
        // Validate email uniqueness
        if (await _db.Users.AnyAsync(u => u.Email == command.Email))
        {
            return new RegisterUserResult(
                Guid.Empty,
                "Email already exists",
                Success: false
            );
        }

        // Validate password complexity
        if (!_passwordService.ValidatePasswordComplexity(command.Password))
        {
            return new RegisterUserResult(
                Guid.Empty,
                "Password does not meet complexity requirements",
                Success: false
            );
        }

        // Create user
        var user = new User
        {
            Id = Guid.NewGuid(),
            Email = command.Email,
            Username = command.Username,
            PasswordHash = _passwordService.HashPassword(command.Password),
            FirstName = command.FirstName,
            LastName = command.LastName,
            CreatedAt = DateTime.UtcNow,
            IsActive = true
        };

        _db.Users.Add(user);

        // Assign default role
        var userRole = await _db.Roles.FirstOrDefaultAsync(r => r.Name == "User");
        if (userRole != null)
        {
            _db.UserRoles.Add(new UserRole
            {
                UserId = user.Id,
                RoleId = userRole.Id,
                AssignedAt = DateTime.UtcNow
            });
        }

        // Save to database
        await _db.SaveChangesAsync();

        _logger.LogInformation(
            "User {Email} registered successfully with ID {UserId}",
            user.Email,
            user.Id
        );

        return new RegisterUserResult(
            user.Id,
            "User registered successfully",
            Success: true
        );
    }
}
```

#### 3. Controller Usage

```csharp
// Controllers/AuthController.cs
[ApiController]
[Route("api/[controller]")]
public class AuthController : ControllerBase
{
    private readonly IMessageBus _bus;

    public AuthController(IMessageBus bus)
    {
        _bus = bus;
    }

    [HttpPost("register")]
    public async Task<IActionResult> Register([FromBody] RegisterUserCommand command)
    {
        // Invoke the command through Wolverine
        var result = await _bus.InvokeAsync<RegisterUserResult>(command);

        if (!result.Success)
            return BadRequest(new { error = result.Message });

        return CreatedAtAction(
            nameof(Register),
            new { id = result.UserId },
            result
        );
    }
}
```

#### 4. Unit Test

```csharp
// Tests/RegisterUserHandlerTests.cs
public class RegisterUserHandlerTests
{
    [Fact]
    public async Task Handle_WithValidData_CreatesUser()
    {
        // Arrange
        var dbContext = CreateInMemoryDbContext();
        var passwordService = new PasswordService();
        var logger = Mock.Of<ILogger<RegisterUserHandler>>();
        
        var handler = new RegisterUserHandler(dbContext, passwordService, logger);
        var command = new RegisterUserCommand(
            "test@example.com",
            "testuser",
            "SecurePass123!",
            "Test",
            "User"
        );

        // Act
        var result = await handler.Handle(command);

        // Assert
        Assert.True(result.Success);
        Assert.NotEqual(Guid.Empty, result.UserId);
        
        var userInDb = await dbContext.Users.FindAsync(result.UserId);
        Assert.NotNull(userInDb);
        Assert.Equal("test@example.com", userInDb.Email);
    }

    [Fact]
    public async Task Handle_WithDuplicateEmail_ReturnsError()
    {
        // Arrange
        var dbContext = CreateInMemoryDbContext();
        await dbContext.Users.AddAsync(new User { Email = "existing@example.com" });
        await dbContext.SaveChangesAsync();
        
        var handler = new RegisterUserHandler(dbContext, new PasswordService(), Mock.Of<ILogger>());
        var command = new RegisterUserCommand(
            "existing@example.com",
            "testuser",
            "SecurePass123!",
            null,
            null
        );

        // Act
        var result = await handler.Handle(command);

        // Assert
        Assert.False(result.Success);
        Assert.Contains("already exists", result.Message);
    }
}
```

---

### Example 2: Get User Query

#### 1. Query Definition

```csharp
// Queries/Users/GetUserByIdQuery.cs
public record GetUserByIdQuery(Guid UserId);

public record UserDto(
    Guid Id,
    string Email,
    string Username,
    string? FirstName,
    string? LastName,
    bool IsActive,
    List<string> Roles
);
```

#### 2. Query Handler

```csharp
// Handlers/Users/GetUserByIdHandler.cs
public class GetUserByIdHandler
{
    private readonly SecurityDbContext _db;

    public GetUserByIdHandler(SecurityDbContext db)
    {
        _db = db;
    }

    public async Task<UserDto?> Handle(GetUserByIdQuery query)
    {
        var user = await _db.Users
            .Include(u => u.UserRoles)
                .ThenInclude(ur => ur.Role)
            .FirstOrDefaultAsync(u => u.Id == query.UserId);

        if (user == null)
            return null;

        return new UserDto(
            user.Id,
            user.Email,
            user.Username,
            user.FirstName,
            user.LastName,
            user.IsActive,
            user.UserRoles.Select(ur => ur.Role.Name).ToList()
        );
    }
}
```

#### 3. Controller Usage

```csharp
[HttpGet("{id}")]
public async Task<IActionResult> GetUser(Guid id)
{
    var query = new GetUserByIdQuery(id);
    var user = await _bus.InvokeAsync<UserDto?>(query);

    if (user == null)
        return NotFound();

    return Ok(user);
}
```

---

## 9. WHEN NOT TO USE CQRS

CQRS is **NOT always the right choice**. Avoid CQRS when:

### ❌ 1. Very Simple CRUD Applications

```
Example: Simple blog with posts and comments
- Only Create, Read, Update, Delete operations
- No complex business logic
- No need for audit trail

Better choice: Traditional Repository Pattern
```

### ❌ 2. Small Team, Low Complexity

```
Example: Internal tool with 3 tables
- Team of 1-2 developers
- Simple data entry forms
- No scalability requirements

Better choice: Entity Framework + Controllers directly
```

### ❌ 3. Rapid Prototyping / MVP

```
Example: Startup MVP to test market
- Need to ship in 2 weeks
- Requirements will change drastically
- Simple operations

Better choice: Traditional layered architecture (faster initial development)
```

### ❌ 4. Report-Heavy Systems

```
Example: Business Intelligence dashboard
- 95% read operations
- Complex queries, joins, aggregations
- Minimal write operations

Better choice: Direct database queries or stored procedures
```

---

## 10. DECISION RATIONALE

### Why CQRS is RIGHT for Our Project

| Our Requirement | How CQRS Helps |
|-----------------|----------------|
| **6+ Modules** | Clear operation boundaries per module |
| **Audit Trail** | Every command is explicitly logged |
| **Testability** | 80%+ test coverage target easily achievable |
| **Team Size** | Multiple developers work independently on handlers |
| **Complexity** | Medium-high business logic (auth, workflows, etc.) |
| **Scalability** | Can optimize reads/writes independently |
| **Maintenance** | Small, focused handlers easier to maintain |
| **Future-Proof** | Easy to migrate to microservices if needed |

### Scoring Matrix

| Criterion | Weight | Traditional | MediatR | Wolverine | Score |
|-----------|--------|-------------|---------|-----------|-------|
| Module Clarity | 25% | 5/10 | 9/10 | 10/10 | Wolverine |
| Testability | 20% | 6/10 | 9/10 | 9/10 | Tie |
| Performance | 15% | 8/10 | 7/10 | 10/10 | Wolverine |
| Learning Curve | 15% | 10/10 | 8/10 | 8/10 | Traditional |
| Features | 15% | 5/10 | 7/10 | 10/10 | Wolverine |
| Scalability | 10% | 6/10 | 7/10 | 10/10 | Wolverine |
| **Weighted Score** | | **6.6/10** | **8.0/10** | **9.3/10** | **Wolverine** ✅ |

### Final Decision

**We will use CQRS pattern with Wolverine framework across all backend modules.**

### Trade-offs Accepted

✅ **Slightly more boilerplate code**
- More files (Commands, Queries, Handlers)
- Mitigation: Code generators, templates

✅ **Learning curve for team**
- Need to understand CQRS concepts
- Mitigation: Training session, documentation

### Trade-offs Avoided

By using CQRS, we avoid:
- ❌ Large, monolithic service classes
- ❌ Unclear operation boundaries
- ❌ Difficult testing
- ❌ Performance bottlenecks
- ❌ Poor scalability

---

## CONCLUSION

**YES, we ARE using CQRS with Wolverine framework.**

### Summary

✅ **Clear Architecture** - Commands and Queries explicitly separated  
✅ **Testable** - Each handler independently testable  
✅ **Scalable** - Optimize reads and writes separately  
✅ **Maintainable** - Small, focused handlers  
✅ **Audit Trail** - Every command is logged  
✅ **Future-Proof** - Easy to scale and distribute  
✅ **Modern .NET** - Wolverine is cutting-edge and performant  

### Implementation Plan

1. ✅ **Phase 1** - Use CQRS in Security Module
2. ⏳ **Phase 2** - Validate approach, refine patterns
3. ⏳ **Phase 3** - Roll out to Notifications and Logging modules
4. ⏳ **Phase 4+** - Apply to all remaining modules

---

**Document Status:** Approved  
**Implementation:** In Progress (Security Module)  
**Review Date:** After Phase 1 completion

---

## REFERENCES

- Wolverine Documentation: https://wolverine.netlify.app/
- CQRS Pattern (Martin Fowler): https://martinfowler.com/bliki/CQRS.html
- Wolverine GitHub: https://github.com/JasperFx/wolverine

---

**Prepared by:** Backend Architecture Team  
**Date:** February 12, 2026  
**Version:** 1.0
