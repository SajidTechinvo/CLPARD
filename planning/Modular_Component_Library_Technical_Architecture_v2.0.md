# MODULAR COMPONENT LIBRARY
## Enterprise Plug & Play Module Ecosystem
### Technical Architecture & Implementation Proposal - Version 2.0

**Prepared:** February 11, 2026  
**Version:** 2.0 (Enhanced)  
**Status:** Planning Phase  
**Classification:** Internal

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Feb 9, 2026 | Original Team | Initial technical proposal |
| 2.0 | Feb 11, 2026 | AI-Enhanced Review | Merged analysis, added recommendations, technology updates |
| 2.1 | Feb 12, 2026 | Architecture Review | Changed module distribution from Module Federation to NPM packages for simplicity and efficiency |
| 2.2 | Feb 12, 2026 | Roadmap Update | Updated roadmap to reflect prioritized development order: Security → Notifications → Logging → Workflow → Rule Engine → Chat/Chatbot |

---

## IMPORTANT: Architecture Update (v2.1)

**🔄 Distribution Strategy Changed:** This document has been updated from version 2.0 to 2.1 with a significant architectural improvement.

**What Changed:**
- **Previous (v2.0):** Module Federation for runtime module loading
- **Current (v2.1):** Standard NPM package distribution

**Why the Change:**
The Module Federation approach, while powerful, introduced unnecessary complexity for our use case. The new NPM package approach provides:
- ✅ **Better Developer Experience** - Full TypeScript support, IDE autocomplete, easier debugging
- ✅ **Simpler Setup** - Standard npm workflow, no complex federation configuration
- ✅ **Better Performance** - Tree-shaking and build optimization work naturally
- ✅ **Type Safety** - Compile-time type checking between host and modules
- ✅ **Standard Tooling** - All React/Vite tooling works out of the box

**What This Means:**
- Modules are installed as standard npm packages (`npm install @company/security-module`)
- Full type safety at compile time
- Simpler integration code
- Lazy loading still available via `React.lazy()` when needed
- Requires host app rebuild to update modules (acceptable trade-off)

See sections 1.2, 3.2, 4.1.3, 5.3, and ADR-004 for detailed information.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Technical Architecture](#2-technical-architecture)
3. [Technology Stack (Enhanced)](#3-technology-stack-enhanced)
4. [Module Architecture Deep Dive](#4-module-architecture-deep-dive)
5. [Implementation Strategy](#5-implementation-strategy)
6. [Testing & Quality Assurance](#6-testing--quality-assurance)
7. [Deployment & Operations](#7-deployment--operations)
8. [Risk Management](#8-risk-management)
9. [Roadmap & Timeline](#9-roadmap--timeline)
10. [Appendices](#10-appendices)

---

## 1. EXECUTIVE SUMMARY

### 1.1 Vision & Strategic Goals

This document outlines the development of a **comprehensive Modular Component Library ecosystem** designed to accelerate application development across our organization. The ecosystem provides enterprise-grade, plug-and-play modules that can be seamlessly integrated into any web application, significantly reducing development time while maintaining high quality and consistency.

#### Strategic Objectives

| Objective | Target | Impact |
|-----------|--------|--------|
| **Development Time Reduction** | 60-70% | Faster time-to-market |
| **UI/UX Consistency** | 100% | Unified brand experience |
| **Code Reusability** | 80%+ | Reduced technical debt |
| **Maintenance Efficiency** | 50% reduction | Lower operational costs |
| **Quality Improvement** | Enterprise-grade | Battle-tested components |

#### Key Differentiators

✅ **Technology Agnostic:** Support for multiple databases (PostgreSQL, MS SQL Server) and UI frameworks (Tailwind CSS, Material-UI, Ant Design)  
✅ **100% Open Source:** All components built with MIT/Apache 2.0 licensed libraries  
✅ **Plug & Play:** Minimal configuration required (2-3 lines of code)  
✅ **Modern Architecture:** React 19, .NET 10, Wolverine, Vite  
✅ **Production Ready:** Comprehensive testing, security, and scalability built-in

---

### 1.2 High-Level Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    HOST APPLICATION                              │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │              React 19 + TypeScript                        │  │
│  │                                                           │  │
│  │  import { AuthProvider } from '@company/security-module'  │  │
│  │  import { ChatWidget } from '@company/chat-module'        │  │
│  │  import { WorkflowEngine } from '@company/workflow-module'│  │
│  │                                                           │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │  │
│  │  │  Security   │  │    Chat     │  │ Workflow    │      │  │
│  │  │   Module    │  │   Module    │  │   Module    │ ...  │  │
│  │  │(NPM Package)│  │(NPM Package)│  │(NPM Package)│      │  │
│  │  └─────────────┘  └─────────────┘  └─────────────┘      │  │
│  │                                                           │  │
│  │  Standard npm install + TypeScript imports                │  │
│  │  • Full type safety at compile-time                       │  │
│  │  • Tree-shaking & code splitting                          │  │
│  │  • Lazy loading via React.lazy() when needed              │  │
│  └───────────────────────────────────────────────────────────┘  │
│                           │                                      │
│                           │ REST/WebSocket                       │
│                           ↓                                      │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │              YARP API Gateway (.NET 10)                   │  │
│  └───────────────────────────────────────────────────────────┘  │
│                           │                                      │
│         ┌─────────────────┼─────────────────┐                   │
│         ↓                 ↓                 ↓                    │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │  Security   │  │    Chat     │  │  Workflow   │            │
│  │     API     │  │    API      │  │    API      │   ...      │
│  │  (Wolverine)│  │ (Wolverine) │  │ (Wolverine) │            │
│  │(NuGet Pkg)  │  │(NuGet Pkg)  │  │(NuGet Pkg)  │            │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘            │
│         │                 │                 │                    │
│         └─────────────────┴─────────────────┘                   │
│                           │                                      │
│                           ↓                                      │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │        PostgreSQL / MS SQL Server                         │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │  │
│  │  │  security.* │  │   chat.*    │  │ workflow.*  │      │  │
│  │  │   schema    │  │   schema    │  │   schema    │ ...  │  │
│  │  └─────────────┘  └─────────────┘  └─────────────┘      │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

#### Architecture Principles

1. **Vertical Slice Architecture:** Each module contains its complete stack (UI, API, Business Logic, Data Access)
2. **Plug & Play Integration:** Modules can be added to applications with minimal configuration
3. **Technology Agnostic:** Support for multiple databases and UI frameworks
4. **Microservices Ready:** Modules can be deployed independently or as part of a monolith
5. **Event-Driven Communication:** Modules communicate via Wolverine messaging for loose coupling
6. **Shared Database with Schema Isolation:** All modules share one database but use separate schemas
7. **Monorepo Structure:** Single repository with Turborepo for efficient builds
8. **NPM Package Distribution:** Modules distributed as standard NPM packages for simplicity and type safety

#### Distribution Strategy Change (v2.1)

**Previous Approach (v2.0):** Module Federation for runtime loading
- Modules loaded dynamically at runtime via Webpack Module Federation
- Allowed independent deployment and version updates
- Added significant complexity in configuration and debugging

**Current Approach (v2.1):** NPM Package Distribution
- Modules distributed as standard NPM packages
- Build-time dependency resolution with full type safety
- Simpler integration, better IDE support, easier debugging
- Still maintains modularity and reusability
- Better tree-shaking and bundle optimization
- Lazy loading available via React.lazy() for code splitting when needed

**Rationale for Change:**
- ✅ **Simpler Setup:** Standard npm workflow, no Module Federation configuration
- ✅ **Better Type Safety:** Compile-time type checking between host and modules
- ✅ **Improved Developer Experience:** Better IDE autocomplete and refactoring support
- ✅ **Easier Debugging:** No runtime module resolution complexity
- ✅ **Standard Tooling:** Works with all standard React/Vite tooling out of the box
- ✅ **Better Performance:** Tree-shaking and optimization work naturally
- ⚠️ **Trade-off:** Requires host app rebuild to update modules (acceptable for most use cases)

---

### 1.3 Module Ecosystem Overview

#### Core Application Modules (20+)

| Category | Modules | Priority |
|----------|---------|----------|
| **Security & Identity** | Security, Authentication, Authorization, Audit Trail | 🔴 Critical |
| **Communication** | Notifications (Email/SMS/Push), Chat, Chat Bot (AI) | 🟠 High |
| **Content Management** | CMS, Media Content Service, DMS, Knowledge Base | 🟠 High |
| **Business Process** | Workflow Engine, Rule Engine, Form Builder, Survey Management | 🟡 Medium |
| **Financial** | Payment Gateway, Digital Signature | 🟡 Medium |
| **Analytics** | Analytics, BI/Reporting, Monitoring Management, Tracking | 🟢 Low |
| **Engagement** | Gamification, Risk Management | 🟢 Low |

#### Supporting Services

- **Application Logging Service**
- **Identity Management Service**
- **Business Rule Engine**
- **Business Audit Trail Service**
- **Content Delivery Service**
- **Data Encryption & Vault Service**
- **Integration Management Service**
- **VoIP Coordinator & Scheduling Service**

#### Background Services

- **Notifications Engine** (Queue-based, Wolverine)
- **Scheduling & Task Engine** (Cron jobs, Recurring tasks)
- **Settlement Engine** (Financial processing, Reconciliation)

#### External Integrations

- **TDRA** (Telecom Regulatory Authority)
- **ADM** (Abu Dhabi Municipality)
- **AGPAY** (Payment Gateway)
- **Active Directory** (SSO, LDAP)
- **SMS Gateway** (Multi-provider: Twilio, AWS SNS)
- **Emirates Post** (Shipping & Logistics)

---

### 1.4 Expected ROI & Benefits

#### Quantifiable Benefits

| Metric | Current | Target | Improvement |
|--------|---------|--------|-------------|
| **Development Time** | 10 weeks | 3-4 weeks | **60-70% reduction** |
| **Code Duplication** | 40-50% | <10% | **80% reduction** |
| **Bug Rate** | Industry avg | 50% below avg | **Battle-tested code** |
| **Time to Market** | 6 months | 2 months | **3x faster** |
| **Developer Onboarding** | 2-3 weeks | 3-5 days | **5x faster** |

#### Strategic Benefits

✅ **Faster Time-to-Market:** Launch new applications 60-70% faster  
✅ **Consistent UX:** Unified experience across all organizational applications  
✅ **Reduced Technical Debt:** Standardized, maintainable codebase  
✅ **Enhanced Security:** Centralized security module with enterprise-grade practices  
✅ **Improved Scalability:** Built-in performance and scalability patterns  
✅ **Cost Savings:** Reduced developer hours and infrastructure costs  
✅ **Future-Proof:** Modern technology stack with long-term support

---

## 2. TECHNICAL ARCHITECTURE

### 2.1 Architecture Patterns

#### 2.1.1 Vertical Slice Architecture

Each module is a complete vertical slice containing all layers:

```
Security Module
├── Frontend (React)
│   ├── Components (LoginForm, RegisterForm)
│   ├── Hooks (useAuth, usePermissions)
│   ├── API Client (authClient.ts)
│   └── State (authStore.ts)
│
└── Backend (.NET)
    ├── API Layer (Controllers/Endpoints)
    ├── Application Layer (Wolverine Commands/Queries)
    ├── Domain Layer (Business Logic)
    └── Data Layer (EF Core DbContext)
```

**Benefits:**
- Complete feature ownership
- Independent development and deployment
- Clear boundaries and responsibilities
- Easier testing and maintenance

---

#### 2.1.2 Headless Component Architecture

UI components are built in three layers:

```typescript
// Layer 1: Headless Core (Framework-agnostic logic)
export const useButton = (props: ButtonProps) => {
  const [isPressed, setIsPressed] = useState(false);
  
  const buttonProps = {
    onClick: (e) => {
      if (!props.disabled) props.onClick?.(e);
    },
    onMouseDown: () => setIsPressed(true),
    onMouseUp: () => setIsPressed(false),
    role: 'button',
    tabIndex: props.disabled ? -1 : 0,
    'aria-label': props.ariaLabel,
    'aria-disabled': props.disabled,
  };
  
  return { buttonProps, isPressed };
};

// Layer 2: Theme Adapter (Tailwind)
export const TailwindButton: React.FC<ButtonProps> = (props) => {
  const { buttonProps, isPressed } = useButton(props);
  
  return (
    <button
      {...buttonProps}
      className={cn(
        'px-4 py-2 rounded-md font-medium transition-colors',
        'bg-blue-500 hover:bg-blue-600 active:bg-blue-700',
        'text-white disabled:opacity-50 disabled:cursor-not-allowed',
        isPressed && 'scale-95',
        props.className
      )}
    >
      {props.children}
    </button>
  );
};

// Layer 3: Theme Adapter (Material-UI)
export const MuiButton: React.FC<ButtonProps> = (props) => {
  const { buttonProps } = useButton(props);
  
  return (
    <MuiButtonBase
      {...buttonProps}
      variant="contained"
      color="primary"
    >
      {props.children}
    </MuiButtonBase>
  );
};
```

**Key Advantages:**
- ✅ Single logic implementation
- ✅ Multiple visual representations
- ✅ Easy theme switching
- ✅ Consistent behavior across themes
- ✅ Better testability (test logic once)

---

#### 2.1.3 CQRS with Wolverine

**Command Query Responsibility Segregation** pattern for clear separation:

```csharp
// Command (Write operation)
public record LoginCommand(string Email, string Password);

public class LoginCommandHandler
{
    private readonly SecurityDbContext _db;
    private readonly IJwtTokenService _tokenService;
    private readonly IMessageBus _bus;
    
    public async Task<LoginResult> Handle(LoginCommand command)
    {
        var user = await _db.Users
            .FirstOrDefaultAsync(u => u.Email == command.Email);
        
        if (user == null || !BCrypt.Verify(command.Password, user.PasswordHash))
            throw new InvalidCredentialsException();
        
        var token = _tokenService.GenerateToken(user);
        
        // Publish event for other modules
        await _bus.PublishAsync(new UserLoggedInEvent(user.Id));
        
        return new LoginResult(token, user);
    }
}

// Query (Read operation)
public record GetCurrentUserQuery(int UserId);

public class GetCurrentUserQueryHandler
{
    private readonly SecurityDbContext _db;
    
    public async Task<UserDto> Handle(GetCurrentUserQuery query)
    {
        return await _db.Users
            .Where(u => u.Id == query.UserId)
            .Select(u => new UserDto(u.Id, u.Email, u.FullName))
            .FirstOrDefaultAsync();
    }
}
```

**Wolverine Benefits:**
- ✅ Built-in durable messaging (outbox pattern)
- ✅ Source generation for performance
- ✅ Less boilerplate than MediatR
- ✅ Message persistence in database
- ✅ Retry logic and error handling

---

### 2.2 Database Architecture

#### 2.2.1 Shared Database with Schema Isolation

**Strategy:** All modules share one database but use separate schemas.

```sql
-- PostgreSQL Schema Structure
CREATE SCHEMA security;
CREATE SCHEMA notifications;
CREATE SCHEMA chat;
CREATE SCHEMA workflow;
CREATE SCHEMA media;
CREATE SCHEMA payment;

-- Example: Security schema tables
CREATE TABLE security.users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    full_name VARCHAR(255),
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE security.roles (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE security.permissions (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) UNIQUE NOT NULL,
    resource VARCHAR(100),
    action VARCHAR(50)
);
```

**Benefits:**
| Benefit | Description |
|---------|-------------|
| **Simplified Operations** | Single connection string, one backup/restore |
| **Cross-Module Queries** | JOIN across schemas when needed (rare but possible) |
| **Transactional Consistency** | ACID transactions across modules |
| **Lower Costs** | One database instance vs many |
| **Easier Migration** | Manage migrations per-schema independently |

**Challenges Addressed:**
- ✅ Schema naming conflicts prevented through prefixes
- ✅ Module independence maintained through clear boundaries
- ✅ Migration management handled per-schema
- ✅ Access control via schema-level permissions

---

#### 2.2.2 Multi-Database Support

**Implementation:** Configuration-based provider selection

```csharp
// appsettings.json
{
  "DatabaseProvider": "PostgreSQL",  // or "SqlServer"
  "ConnectionStrings": {
    "Default": "Host=localhost;Database=modular_components;..."
  }
}

// Startup configuration
public static class DatabaseConfiguration
{
    public static IServiceCollection AddModuleDatabase(
        this IServiceCollection services,
        IConfiguration config)
    {
        var provider = config["DatabaseProvider"];
        var connectionString = config.GetConnectionString("Default");
        
        if (provider == "PostgreSQL")
        {
            services.AddDbContext<ModuleDbContext>(opt => 
                opt.UseNpgsql(connectionString, 
                    npgsql => npgsql.MigrationsHistoryTable("__EFMigrationsHistory", "security")));
        }
        else if (provider == "SqlServer")
        {
            services.AddDbContext<ModuleDbContext>(opt => 
                opt.UseSqlServer(connectionString,
                    sqlServer => sqlServer.MigrationsHistoryTable("__EFMigrationsHistory", "security")));
        }
        
        return services;
    }
}
```

**Testing Strategy:**
- All migrations tested against **both** PostgreSQL and SQL Server
- CI/CD pipeline runs integration tests on both databases
- Provider-specific features isolated in database abstraction layer

---

#### 2.2.3 Connection Management & Pooling

```csharp
// Connection pooling configuration
services.AddDbContext<ModuleDbContext>(options =>
{
    options.UseNpgsql(connectionString, npgsqlOptions =>
    {
        npgsqlOptions.EnableRetryOnFailure(
            maxRetryCount: 3,
            maxRetryDelay: TimeSpan.FromSeconds(5),
            errorCodesToAdd: null);
        
        npgsqlOptions.CommandTimeout(30);
        npgsqlOptions.MinBatchSize(1);
        npgsqlOptions.MaxBatchSize(100);
    });
}, ServiceLifetime.Scoped, ServiceLifetime.Singleton);
```

**Best Practices:**
- Minimum pool size: **5 connections**
- Maximum pool size: **100 connections** (adjustable per load)
- Connection lifetime: **30 minutes** (allows server-side rebalancing)
- Retry logic: **3 attempts** with exponential backoff
- Query timeout: **30 seconds** (configurable per query)

---

### 2.3 Module Communication Architecture

#### 2.3.1 Event-Driven Communication (Wolverine)

Modules communicate through events, not direct calls:

```csharp
// Payment Module publishes event
public class PaymentCompletedHandler
{
    private readonly IMessageBus _bus;
    
    public async Task Handle(ProcessPaymentCommand command)
    {
        // Process payment logic...
        
        // Publish event for other modules
        await _bus.PublishAsync(new PaymentCompletedEvent
        {
            OrderId = command.OrderId,
            Amount = command.Amount,
            Currency = command.Currency,
            TransactionId = transactionId,
            Timestamp = DateTime.UtcNow
        });
    }
}

// Notifications Module subscribes to event
public class PaymentCompletedEventHandler
{
    private readonly INotificationService _notificationService;
    
    public async Task Handle(PaymentCompletedEvent evt)
    {
        await _notificationService.SendEmailAsync(new SendEmailRequest
        {
            Template = "payment-receipt",
            Variables = new { evt.OrderId, evt.Amount },
            To = evt.CustomerEmail
        });
    }
}

// Analytics Module also subscribes (independent)
public class PaymentAnalyticsEventHandler
{
    private readonly IAnalyticsService _analytics;
    
    public async Task Handle(PaymentCompletedEvent evt)
    {
        await _analytics.TrackEvent("payment_completed", new
        {
            evt.Amount,
            evt.Currency,
            evt.TransactionId
        });
    }
}
```

**Benefits:**
- ✅ **Loose Coupling:** Modules don't know about each other
- ✅ **Scalability:** Events can be processed asynchronously
- ✅ **Reliability:** Wolverine's durable messaging ensures delivery
- ✅ **Flexibility:** Add new subscribers without modifying publishers
- ✅ **Testability:** Easy to test modules in isolation

---

#### 2.3.2 Module Registry & Discovery

**Enhancement:** Runtime module discovery and health monitoring

```typescript
// packages/core/src/module-registry.ts
export interface ModuleMetadata {
  id: string;
  name: string;
  version: string;
  dependencies: string[];
  status: 'active' | 'inactive' | 'error';
  health: () => Promise<HealthStatus>;
  config: ModuleConfig;
}

export interface HealthStatus {
  healthy: boolean;
  checks: {
    database: boolean;
    api: boolean;
    cache: boolean;
  };
  lastCheck: Date;
}

export class ModuleRegistry {
  private modules = new Map<string, ModuleMetadata>();
  
  register(module: ModuleMetadata) {
    console.log(`Registering module: ${module.name} v${module.version}`);
    this.modules.set(module.id, module);
  }
  
  async healthCheck(): Promise<Map<string, HealthStatus>> {
    const results = new Map<string, HealthStatus>();
    
    for (const [id, module] of this.modules) {
      try {
        const health = await module.health();
        results.set(id, health);
      } catch (error) {
        results.set(id, {
          healthy: false,
          checks: { database: false, api: false, cache: false },
          lastCheck: new Date()
        });
      }
    }
    
    return results;
  }
  
  getModule(id: string): ModuleMetadata | undefined {
    return this.modules.get(id);
  }
  
  getAllModules(): ModuleMetadata[] {
    return Array.from(this.modules.values());
  }
}

// Usage in host application
const registry = new ModuleRegistry();

// Modules self-register on load
registry.register({
  id: 'security',
  name: 'Security Module',
  version: '1.0.0',
  dependencies: [],
  status: 'active',
  health: async () => ({
    healthy: true,
    checks: {
      database: await checkDbConnection(),
      api: await checkApiEndpoint(),
      cache: await checkRedisConnection()
    },
    lastCheck: new Date()
  }),
  config: { apiUrl: '/api/security' }
});
```

---

## 3. TECHNOLOGY STACK (ENHANCED)

### 3.1 Technology Selection Principles

All technology choices follow these criteria:

✅ **Open Source:** MIT or Apache 2.0 license  
✅ **Production Ready:** Battle-tested in enterprise environments  
✅ **Active Maintenance:** Regular updates and security patches  
✅ **Strong Community:** Large developer community and resources  
✅ **Performance:** Proven performance characteristics  
✅ **Future-Proof:** Long-term viability and support

---

### 3.2 Frontend Stack (100% Open Source)

| Category | Technology | Version | License | Rationale |
|----------|-----------|---------|---------|-----------|
| **Framework** | React | 19.x | MIT | Industry standard, concurrent features, massive ecosystem |
| **Language** | TypeScript | 5.x | Apache 2.0 | Type safety, better DX, refactoring confidence |
| **Build Tool** | Vite | 6.x | MIT | Lightning-fast builds, excellent HMR, modern |
| **Monorepo** | Turborepo | 2.x | MIT | Parallel builds, intelligent caching, 10x faster |
| **Package Manager** | pnpm | 9.x | MIT | Fast, efficient, disk space saving |
| **Package Distribution** | NPM | - | - | Standard package distribution for modules |
| **State Management** | Zustand | 5.x | MIT | Minimal boilerplate, small bundle, flexible |
| **Routing** | React Router | 6.x | MIT | De facto standard, type-safe |
| **Forms** | React Hook Form | 7.x | MIT | Performant, great DX, minimal re-renders |
| **Validation** | Zod | 3.x | MIT | Type-safe schemas, runtime validation |
| **API Client** | TanStack Query | 5.x | MIT | Server state management, caching, sync |
| **HTTP Client** | Axios | 1.x | MIT | Promise-based, interceptors, widespread |
| **UI Primitives** | Radix UI | 1.x | MIT | Headless, accessible, battle-tested |
| **Design Tokens** | Style Dictionary | 4.x | Apache 2.0 | Multi-platform tokens, single source of truth |
| **CSS Framework** | Tailwind CSS | 4.x | MIT | Utility-first, customizable, small bundles |
| **Alternative CSS** | Material-UI (MUI) | 6.x | MIT | Material Design, rich components |
| **Alternative CSS** | Ant Design | 5.x | MIT | Enterprise-grade, data-heavy UIs |
| **Component Docs** | Storybook | 8.x | MIT | Component playground, documentation |
| **Testing** | Vitest | 2.x | MIT | Vite-native, fast, Jest-compatible API |
| **Component Testing** | Testing Library | Latest | MIT | User-centric testing, best practices |
| **E2E Testing** | Playwright | Latest | Apache 2.0 | Cross-browser, reliable, Microsoft-backed |
| **API Mocking** | MSW | 2.x | MIT | Service worker mocking, realistic tests |

---

### 3.3 Backend Stack (100% Open Source)

| Category | Technology | Version | License | Rationale |
|----------|-----------|---------|---------|-----------|
| **Framework** | .NET | 10.x | MIT | Performance, modern, native AOT, enterprise-grade |
| **CQRS/Messaging** | Wolverine | 3.x | MIT | Modern, durable messaging, less boilerplate |
| **ORM** | Entity Framework Core | 9.x | MIT | Official, powerful, multi-database |
| **Validation** | FluentValidation | 11.x | Apache 2.0 | Expressive, testable, popular |
| **Logging** | Serilog | 4.x | Apache 2.0 | Structured logging, rich sinks |
| **Authentication** | ASP.NET Core Identity | Built-in | MIT | Official, secure, well-documented |
| **JWT** | System.IdentityModel.Tokens.Jwt | Latest | MIT | Official JWT implementation |
| **Password Hashing** | BCrypt.Net-Next | Latest | MIT | Industry standard, secure |
| **Real-time** | SignalR | Built-in | MIT | WebSocket abstraction, reliable |
| **API Gateway** | YARP | 2.x | MIT | Microsoft's reverse proxy, high-performance |
| **API Documentation** | Swashbuckle (Swagger) | 6.x | MIT | OpenAPI/Swagger generation |
| **Testing** | xUnit | Latest | Apache 2.0 | Modern, extensible, recommended by MS |
| **Mocking** | NSubstitute | Latest | BSD | Simple, powerful mocking |
| **Integration Testing** | Alba | 8.x | MIT | Modern WebApplicationFactory successor |
| **Test Containers** | Testcontainers | Latest | MIT | Docker containers for integration tests |

---

### 3.4 Infrastructure Stack (100% Free/Open Source)

| Category | Technology | License | Rationale |
|----------|-----------|---------|-----------|
| **Primary Database** | PostgreSQL | PostgreSQL | Free, robust, JSON support, performance |
| **Alternative DB** | MS SQL Server | Commercial | Enterprise support, Azure integration |
| **Cache** | Redis (Community) | BSD-3 | Fast, proven, widely supported |
| **Message Queue** | Wolverine (built-in) | MIT | Integrated with CQRS, durable messaging |
| **File Storage** | MinIO | AGPL/Commercial | S3-compatible, self-hosted |
| **Search** | PostgreSQL Full-Text | PostgreSQL | Built-in, no extra infrastructure |
| **Monitoring** | OpenTelemetry | Apache 2.0 | Vendor-neutral, comprehensive |
| **CI/CD** | GitHub Actions | Free for public | Native Git integration, powerful |
| **Containers** | Docker | Apache 2.0 | Industry standard, portable |
| **Orchestration** | Docker Compose | Apache 2.0 | Simple, development-friendly |

---

### 3.5 Development Tools

| Category | Tool | License | Purpose |
|----------|------|---------|---------|
| **IDE** | VS Code / Visual Studio / Rider | Free/Commercial | Development environment |
| **API Testing** | Postman / Bruno | Free/MIT | API testing and documentation |
| **Database Client** | DBeaver / pgAdmin | Apache 2.0 | Database management |
| **Git** | Git | GPLv2 | Version control |
| **Package Registry** | Verdaccio | MIT | Private npm registry |
| **NuGet Feed** | Azure Artifacts / BaGet | Commercial/MIT | Private NuGet hosting |

---

### 3.6 Technology Comparison & Justification

#### 3.6.1 Why Wolverine over MediatR?

| Feature | Wolverine | MediatR |
|---------|-----------|---------|
| **Durable Messaging** | ✅ Built-in (outbox pattern) | ❌ Requires additional library |
| **Performance** | ✅ Source generation | ⚠️ Reflection-based |
| **HTTP Integration** | ✅ Built-in endpoints | ❌ Manual setup |
| **Message Persistence** | ✅ Database-backed | ❌ In-memory only |
| **Retry Logic** | ✅ Built-in | ❌ Manual implementation |
| **Learning Curve** | ⚠️ Steeper (more features) | ✅ Simple |
| **Boilerplate** | ✅ Less code | ⚠️ More interfaces |

**Decision:** Wolverine for enterprise features and performance.

---

#### 3.6.2 Why Turborepo over Nx/Lerna?

| Feature | Turborepo | Nx | Lerna |
|---------|-----------|----|----|
| **Speed** | ✅ Extremely fast | ✅ Fast | ⚠️ Slower |
| **Caching** | ✅ Remote caching | ✅ Remote caching | ❌ No remote cache |
| **Simplicity** | ✅ Minimal config | ⚠️ More setup | ✅ Simple |
| **DX** | ✅ Excellent | ✅ Excellent | ⚠️ Good |
| **React Support** | ✅ First-class | ✅ First-class | ✅ Supported |
| **Maintenance** | ✅ Active (Vercel) | ✅ Active (Nrwl) | ⚠️ Less active |

**Decision:** Turborepo for speed and simplicity.

---

#### 3.6.3 Why Radix UI over Building from Scratch?

| Aspect | Radix UI | Custom Implementation |
|--------|----------|----------------------|
| **Development Time** | ✅ Days | ❌ Months |
| **Accessibility** | ✅ WCAG 2.1 AA compliant | ⚠️ Requires expertise |
| **Browser Support** | ✅ Comprehensive | ⚠️ Manual testing needed |
| **Keyboard Navigation** | ✅ Built-in | ⚠️ Manual implementation |
| **Screen Reader** | ✅ Tested | ⚠️ Manual testing |
| **Focus Management** | ✅ Automatic | ⚠️ Complex logic |
| **Maintenance** | ✅ Community-maintained | ❌ Our responsibility |

**Decision:** Use Radix UI as foundation, customize as needed.

---

## 4. MODULE ARCHITECTURE DEEP DIVE

### 4.1 Security Module (Foundation Module)

**Priority:** 🔴 Critical (Phase 1)  
**Dependencies:** None (foundation module)

#### 4.1.1 Module Responsibilities

- User authentication (email/password, OAuth, SSO)
- JWT token generation and refresh token management
- Role-based access control (RBAC)
- Permission-based authorization (fine-grained)
- Password policies and enforcement
- Multi-factor authentication (MFA)
- Session management
- Audit trail logging
- Active Directory / LDAP integration
- Password reset workflows
- Account lockout policies

#### 4.1.2 Technical Implementation

**Backend Structure:**

```
Modules.Security/
├── Commands/
│   ├── LoginCommand.cs
│   ├── RegisterCommand.cs
│   ├── LogoutCommand.cs
│   ├── RefreshTokenCommand.cs
│   └── ResetPasswordCommand.cs
│
├── Queries/
│   ├── GetCurrentUserQuery.cs
│   ├── GetUserPermissionsQuery.cs
│   └── GetUserRolesQuery.cs
│
├── Handlers/
│   ├── LoginCommandHandler.cs
│   ├── RegisterCommandHandler.cs
│   └── RefreshTokenCommandHandler.cs
│
├── Events/
│   ├── UserLoggedInEvent.cs
│   ├── UserRegisteredEvent.cs
│   └── UserLoggedOutEvent.cs
│
├── Entities/
│   ├── User.cs
│   ├── Role.cs
│   ├── Permission.cs
│   ├── RefreshToken.cs
│   └── AuditLog.cs
│
├── Data/
│   ├── SecurityDbContext.cs
│   └── Migrations/
│
├── Services/
│   ├── IJwtTokenService.cs
│   ├── JwtTokenService.cs
│   ├── IPasswordHasher.cs
│   └── PasswordHasher.cs
│
└── SecurityModule.cs (Registration)
```

**Frontend Structure:**

```typescript
packages/modules/security/
├── src/
│   ├── components/
│   │   ├── LoginForm.tsx
│   │   ├── RegisterForm.tsx
│   │   ├── PasswordReset.tsx
│   │   ├── MFASetup.tsx
│   │   └── ProfileSettings.tsx
│   │
│   ├── hooks/
│   │   ├── useAuth.ts
│   │   ├── usePermissions.ts
│   │   ├── useSession.ts
│   │   └── useLogin.ts
│   │
│   ├── api/
│   │   └── authClient.ts
│   │
│   ├── store/
│   │   └── authStore.ts (Zustand)
│   │
│   ├── types/
│   │   └── auth.types.ts
│   │
│   ├── contexts/
│   │   └── AuthContext.tsx
│   │
│   └── index.ts (Public API)
│
├── adapters/
│   ├── tailwind/
│   ├── material-ui/
│   └── ant-design/
│
├── vite.config.ts (Library Build)
└── package.json
```

#### 4.1.3 Integration Pattern

**Backend Integration (2 lines):**

```csharp
// Program.cs
builder.Services.AddSecurityModule(builder.Configuration);
app.UseSecurityModule();
```

**Frontend Integration (NPM Package):**

**Step 1: Install the module**

```bash
# Install from private npm registry or workspace
npm install @company/security-module

# Or using pnpm in monorepo
pnpm add @company/security-module
```

**Step 2: Import and use in your application**

```tsx
// App.tsx
import { AuthProvider, useAuth, LoginForm } from '@company/security-module';

function App() {
  return (
    <AuthProvider config={{ apiUrl: '/api/auth' }}>
      <YourAppContent />
    </AuthProvider>
  );
}

// In your login page
function LoginPage() {
  return <LoginForm onSuccess={() => navigate('/dashboard')} />;
}

// In any component
function ProtectedComponent() {
  const { user, isAuthenticated, logout } = useAuth();
  
  if (!isAuthenticated) {
    return <Navigate to="/login" />;
  }
  
  return (
    <div>
      <h1>Welcome, {user.fullName}</h1>
      <button onClick={logout}>Logout</button>
    </div>
  );
}
```

**Step 3: Optional - Lazy loading for code splitting**

```tsx
// For large modules, use React.lazy() for code splitting
import { lazy, Suspense } from 'react';

const SecurityModule = lazy(() => import('@company/security-module'));

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <SecurityModule />
    </Suspense>
  );
}
```

**Benefits of NPM Package Approach:**
- ✅ **Full Type Safety:** TypeScript types available at compile-time
- ✅ **IDE Support:** Autocomplete, go-to-definition, refactoring all work
- ✅ **Simple Setup:** No Module Federation configuration needed
- ✅ **Tree Shaking:** Unused code automatically removed
- ✅ **Easy Debugging:** Standard source maps and debugging flow
- ✅ **Version Control:** Package.json manages versions clearly

#### 4.1.4 Database Schema

```sql
-- security.users
CREATE TABLE security.users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    email_verified BOOLEAN DEFAULT FALSE,
    password_hash TEXT NOT NULL,
    full_name VARCHAR(255),
    phone_number VARCHAR(20),
    is_active BOOLEAN DEFAULT TRUE,
    is_locked BOOLEAN DEFAULT FALSE,
    failed_login_attempts INT DEFAULT 0,
    last_login_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- security.roles
CREATE TABLE security.roles (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) UNIQUE NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- security.permissions
CREATE TABLE security.permissions (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) UNIQUE NOT NULL,
    resource VARCHAR(100) NOT NULL,
    action VARCHAR(50) NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- security.user_roles (many-to-many)
CREATE TABLE security.user_roles (
    user_id INT REFERENCES security.users(id) ON DELETE CASCADE,
    role_id INT REFERENCES security.roles(id) ON DELETE CASCADE,
    assigned_at TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (user_id, role_id)
);

-- security.role_permissions (many-to-many)
CREATE TABLE security.role_permissions (
    role_id INT REFERENCES security.roles(id) ON DELETE CASCADE,
    permission_id INT REFERENCES security.permissions(id) ON DELETE CASCADE,
    granted_at TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (role_id, permission_id)
);

-- security.refresh_tokens
CREATE TABLE security.refresh_tokens (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES security.users(id) ON DELETE CASCADE,
    token TEXT UNIQUE NOT NULL,
    expires_at TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    revoked_at TIMESTAMP
);

-- security.audit_logs
CREATE TABLE security.audit_logs (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES security.users(id),
    action VARCHAR(100) NOT NULL,
    resource VARCHAR(100),
    details JSONB,
    ip_address VARCHAR(45),
    user_agent TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Indexes for performance
CREATE INDEX idx_users_email ON security.users(email);
CREATE INDEX idx_refresh_tokens_user_id ON security.refresh_tokens(user_id);
CREATE INDEX idx_refresh_tokens_token ON security.refresh_tokens(token);
CREATE INDEX idx_audit_logs_user_id ON security.audit_logs(user_id);
CREATE INDEX idx_audit_logs_created_at ON security.audit_logs(created_at);
```

#### 4.1.5 API Endpoints

```csharp
// Wolverine HTTP endpoints
public static class SecurityEndpoints
{
    public static void MapSecurityEndpoints(this WebApplication app)
    {
        var group = app.MapGroup("/api/auth").WithTags("Authentication");
        
        // POST /api/auth/login
        group.MapPost("/login", async (LoginCommand command, IMessageBus bus) =>
        {
            var result = await bus.InvokeAsync<LoginResult>(command);
            return Results.Ok(result);
        });
        
        // POST /api/auth/register
        group.MapPost("/register", async (RegisterCommand command, IMessageBus bus) =>
        {
            var result = await bus.InvokeAsync<RegisterResult>(command);
            return Results.Ok(result);
        });
        
        // POST /api/auth/refresh
        group.MapPost("/refresh", async (RefreshTokenCommand command, IMessageBus bus) =>
        {
            var result = await bus.InvokeAsync<LoginResult>(command);
            return Results.Ok(result);
        });
        
        // POST /api/auth/logout
        group.MapPost("/logout", async (LogoutCommand command, IMessageBus bus) =>
        {
            await bus.InvokeAsync(command);
            return Results.Ok();
        }).RequireAuthorization();
        
        // GET /api/auth/me
        group.MapGet("/me", async (HttpContext context, IMessageBus bus) =>
        {
            var userId = context.User.FindFirst("sub")?.Value;
            var user = await bus.InvokeAsync<UserDto>(new GetCurrentUserQuery(int.Parse(userId)));
            return Results.Ok(user);
        }).RequireAuthorization();
    }
}
```

---

### 4.2 Notifications Module

**Priority:** 🔴 Critical (Phase 1)  
**Dependencies:** Security Module (for user preferences)

#### 4.2.1 Module Responsibilities

- Multi-channel delivery (Email, SMS, Push, In-App)
- Template management with variable substitution
- Notification scheduling and queuing
- Delivery status tracking and reporting
- User notification preferences
- Rate limiting and throttling
- Retry logic with exponential backoff
- Provider abstraction (SendGrid, Twilio, Firebase, AWS SES, etc.)
- Batch notifications
- Notification history

#### 4.2.2 Backend Structure

```csharp
// Command example
public record SendNotificationCommand(
    string Channel, // "email", "sms", "push", "in-app"
    string Template,
    Dictionary<string, object> Variables,
    string[] Recipients
);

public class SendNotificationHandler
{
    private readonly INotificationProviderFactory _providerFactory;
    private readonly IMessageBus _bus;
    
    public async Task Handle(SendNotificationCommand command)
    {
        var provider = _providerFactory.GetProvider(command.Channel);
        
        foreach (var recipient in command.Recipients)
        {
            var result = await provider.SendAsync(new NotificationRequest
            {
                Template = command.Template,
                Variables = command.Variables,
                To = recipient
            });
            
            // Publish delivery event
            await _bus.PublishAsync(new NotificationSentEvent
            {
                Channel = command.Channel,
                Recipient = recipient,
                Status = result.Status,
                MessageId = result.MessageId
            });
        }
    }
}
```

#### 4.2.3 Provider Abstraction

```csharp
public interface INotificationProvider
{
    string Channel { get; }
    Task<DeliveryResult> SendAsync(NotificationRequest request);
}

// Email provider example
public class SendGridEmailProvider : INotificationProvider
{
    public string Channel => "email";
    
    public async Task<DeliveryResult> SendAsync(NotificationRequest request)
    {
        // SendGrid implementation
    }
}

public class SmtpEmailProvider : INotificationProvider
{
    public string Channel => "email";
    
    public async Task<DeliveryResult> SendAsync(NotificationRequest request)
    {
        // SMTP implementation
    }
}

// Configuration determines which provider to use
{
  "Notifications": {
    "Email": {
      "Provider": "SendGrid", // or "SMTP"
      "SendGrid": {
        "ApiKey": "..."
      },
      "SMTP": {
        "Host": "smtp.gmail.com",
        "Port": 587
      }
    }
  }
}
```

---

### 4.3 Chat Module (AI-Powered)

**Priority:** 🟠 High (Phase 2)  
**Dependencies:** Security, Media Content Service

#### 4.3.1 Module Responsibilities

- Real-time messaging (WebSocket/SignalR)
- One-on-one and group conversations
- AI chatbot integration (OpenAI, Claude, custom models)
- Message history with pagination
- File attachments
- Read receipts and typing indicators
- Message reactions (emoji)
- Message threading (replies)
- Full-text search
- Message encryption (optional)

#### 4.3.2 SignalR Hub

```csharp
public class ChatHub : Hub
{
    private readonly IMessageBus _bus;
    
    public async Task SendMessage(string roomId, string message)
    {
        var userId = Context.User.FindFirst("sub")?.Value;
        
        // Send command via Wolverine
        var result = await _bus.InvokeAsync<MessageSentResult>(
            new SendMessageCommand(roomId, userId, message));
        
        // Broadcast to room
        await Clients.Group(roomId).SendAsync("ReceiveMessage", result);
    }
    
    public async Task JoinRoom(string roomId)
    {
        await Groups.AddToGroupAsync(Context.ConnectionId, roomId);
    }
    
    public async Task Typing(string roomId)
    {
        var userId = Context.User.FindFirst("sub")?.Value;
        await Clients.OthersInGroup(roomId).SendAsync("UserTyping", userId);
    }
}
```

#### 4.3.3 AI Integration

```csharp
public interface IAIProvider
{
    Task<string> GenerateResponseAsync(string prompt, ChatContext context);
    IAsyncEnumerable<string> StreamResponseAsync(string prompt, ChatContext context);
}

public class OpenAIProvider : IAIProvider
{
    private readonly OpenAIClient _client;
    
    public async IAsyncEnumerable<string> StreamResponseAsync(
        string prompt, 
        ChatContext context)
    {
        var completion = _client.CompleteChatStreamingAsync(new ChatMessage[]
        {
            new SystemChatMessage(context.SystemPrompt),
            ...context.History,
            new UserChatMessage(prompt)
        });
        
        await foreach (var chunk in completion)
        {
            yield return chunk.ContentUpdate;
        }
    }
}
```

---

### 4.4 Workflow Module

**Priority:** 🟡 Medium (Phase 2)  
**Dependencies:** Security, Notifications, Form Builder

#### 4.4.1 Workflow Definition (JSON)

```json
{
  "id": "expense-approval",
  "name": "Expense Approval Workflow",
  "version": "1.0",
  "steps": [
    {
      "id": "submit",
      "type": "form",
      "form": "expense-form",
      "next": "manager-approval"
    },
    {
      "id": "manager-approval",
      "type": "approval",
      "approvers": ["role:manager"],
      "onApprove": "finance-review",
      "onReject": "rejected"
    },
    {
      "id": "finance-review",
      "type": "approval",
      "approvers": ["role:finance"],
      "condition": "amount > 1000",
      "onApprove": "approved",
      "onReject": "rejected"
    },
    {
      "id": "approved",
      "type": "terminal",
      "action": "send-notification"
    },
    {
      "id": "rejected",
      "type": "terminal",
      "action": "send-notification"
    }
  ]
}
```

---

## 5. IMPLEMENTATION STRATEGY

### 5.1 Project Structure

```
modular-component-library/
├── .github/
│   └── workflows/
│       ├── ci.yml
│       ├── test.yml
│       ├── publish-frontend.yml
│       └── publish-backend.yml
│
├── packages/                           # Frontend monorepo (Turborepo + pnpm)
│   ├── core/
│   │   ├── types/
│   │   ├── utils/
│   │   ├── constants/
│   │   └── module-registry/
│   │
│   ├── design-tokens/                  # Style Dictionary
│   │   ├── tokens/
│   │   │   ├── color.json
│   │   │   ├── typography.json
│   │   │   ├── spacing.json
│   │   │   └── shadows.json
│   │   ├── build.js
│   │   └── package.json
│   │
│   ├── ui-primitives/                  # Headless components (Radix-based)
│   │   ├── src/
│   │   │   ├── button/
│   │   │   ├── dialog/
│   │   │   ├── popover/
│   │   │   ├── select/
│   │   │   ├── tabs/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   └── vite.config.ts
│   │
│   ├── ui-adapters/                    # Framework-specific adapters
│   │   ├── tailwind/
│   │   │   ├── src/
│   │   │   ├── tailwind.config.js
│   │   │   └── package.json
│   │   ├── material-ui/
│   │   └── ant-design/
│   │
│   ├── modules/                        # Business modules
│   │   ├── security/
│   │   │   ├── src/
│   │   │   │   ├── components/
│   │   │   │   ├── hooks/
│   │   │   │   ├── api/
│   │   │   │   ├── store/
│   │   │   │   ├── types/
│   │   │   │   └── index.ts
│   │   │   ├── adapters/
│   │   │   │   ├── tailwind/
│   │   │   │   ├── material-ui/
│   │   │   │   └── ant-design/
│   │   │   ├── vite.config.ts
│   │   │   └── package.json
│   │   │
│   │   ├── notifications/
│   │   ├── chat/
│   │   ├── workflow/
│   │   ├── payment-gateway/
│   │   ├── media-content/
│   │   └── analytics/
│   │
│   └── examples/                       # Demo applications
│       ├── demo-app-tailwind/
│       ├── demo-app-mui/
│       └── demo-app-antd/
│
├── backends/                           # .NET Backend
│   ├── YourCompany.Modules.sln
│   │
│   ├── src/
│   │   ├── Core/
│   │   │   ├── YourCompany.Modules.Core/
│   │   │   │   ├── Interfaces/
│   │   │   │   ├── Models/
│   │   │   │   └── Extensions/
│   │   │   │
│   │   │   └── YourCompany.Modules.Core.Events/
│   │   │       ├── PaymentCompletedEvent.cs
│   │   │       ├── UserLoggedInEvent.cs
│   │   │       └── NotificationSentEvent.cs
│   │   │
│   │   ├── Modules/
│   │   │   ├── Security/
│   │   │   │   ├── YourCompany.Modules.Security/
│   │   │   │   │   ├── Commands/
│   │   │   │   │   ├── Queries/
│   │   │   │   │   ├── Handlers/
│   │   │   │   │   ├── Events/
│   │   │   │   │   ├── Entities/
│   │   │   │   │   ├── Data/
│   │   │   │   │   ├── Services/
│   │   │   │   │   └── SecurityModule.cs
│   │   │   │   │
│   │   │   │   └── YourCompany.Modules.Security.Tests/
│   │   │   │       ├── Unit/
│   │   │   │       └── Integration/
│   │   │   │
│   │   │   ├── Notifications/
│   │   │   ├── Chat/
│   │   │   ├── Workflow/
│   │   │   ├── PaymentGateway/
│   │   │   ├── MediaContent/
│   │   │   └── Analytics/
│   │   │
│   │   └── Gateway/
│   │       └── YourCompany.Gateway/
│   │           ├── Program.cs
│   │           ├── appsettings.json
│   │           └── yarp.json
│   │
│   └── tests/
│       ├── Integration.Tests/
│       └── E2E.Tests/
│
├── infrastructure/
│   ├── docker/
│   │   ├── Dockerfile.frontend
│   │   ├── Dockerfile.backend
│   │   ├── docker-compose.yml
│   │   └── docker-compose.dev.yml
│   │
│   └── terraform/                     # Optional: IaC
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
│
├── docs/
│   ├── architecture/
│   │   ├── overview.md
│   │   ├── modules.md
│   │   └── decisions.md (ADRs)
│   ├── api/
│   │   └── openapi.yml
│   ├── guides/
│   │   ├── getting-started.md
│   │   ├── creating-modules.md
│   │   └── integration.md
│   └── diagrams/
│
├── turbo.json                        # Turborepo configuration
├── pnpm-workspace.yaml               # pnpm workspace
├── .npmrc                            # npm/pnpm config
├── .gitignore
├── .editorconfig
├── README.md
└── LICENSE
```

---

### 5.2 Turborepo Configuration

```json
// turbo.json
{
  "$schema": "https://turbo.build/schema.json",
  "globalDependencies": ["**/.env.*local"],
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**", ".next/**", "build/**"]
    },
    "test": {
      "dependsOn": ["^build"],
      "cache": false
    },
    "lint": {
      "outputs": []
    },
    "dev": {
      "cache": false,
      "persistent": true
    },
    "type-check": {
      "dependsOn": ["^build"],
      "outputs": []
    }
  }
}
```

```yaml
# pnpm-workspace.yaml
packages:
  - 'packages/*'
  - 'packages/modules/*'
  - 'packages/ui-adapters/*'
  - 'packages/examples/*'
```

---

### 5.3 Module Package Configuration

#### 5.3.1 Frontend Module Package Setup

**Module Package.json Configuration:**

```json
// packages/modules/security/package.json
{
  "name": "@company/security-module",
  "version": "1.0.0",
  "type": "module",
  "main": "./dist/index.cjs",
  "module": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.cjs",
      "types": "./dist/index.d.ts"
    },
    "./styles": "./dist/style.css"
  },
  "files": [
    "dist"
  ],
  "scripts": {
    "build": "vite build && tsc --emitDeclarationOnly",
    "dev": "vite build --watch",
    "test": "vitest",
    "type-check": "tsc --noEmit"
  },
  "peerDependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0"
  },
  "dependencies": {
    "zustand": "^5.0.0",
    "react-hook-form": "^7.0.0",
    "zod": "^3.0.0",
    "@tanstack/react-query": "^5.0.0",
    "axios": "^1.0.0"
  },
  "devDependencies": {
    "@types/react": "^19.0.0",
    "@types/react-dom": "^19.0.0",
    "vite": "^6.0.0",
    "typescript": "^5.0.0"
  }
}
```

**Vite Build Configuration:**

```typescript
// packages/modules/security/vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import { resolve } from 'path';
import dts from 'vite-plugin-dts';

export default defineConfig({
  plugins: [
    react(),
    dts({
      insertTypesEntry: true,
    })
  ],
  build: {
    lib: {
      entry: resolve(__dirname, 'src/index.ts'),
      name: 'SecurityModule',
      formats: ['es', 'cjs'],
      fileName: (format) => `index.${format === 'es' ? 'js' : 'cjs'}`
    },
    rollupOptions: {
      // Externalize dependencies that shouldn't be bundled
      external: ['react', 'react-dom', 'react/jsx-runtime'],
      output: {
        globals: {
          react: 'React',
          'react-dom': 'ReactDOM'
        }
      }
    },
    sourcemap: true,
    minify: 'esbuild'
  }
});
```

**Module Entry Point:**

```typescript
// packages/modules/security/src/index.ts
// Export components
export { LoginForm } from './components/LoginForm';
export { RegisterForm } from './components/RegisterForm';
export { PasswordReset } from './components/PasswordReset';
export { ProfileSettings } from './components/ProfileSettings';

// Export hooks
export { useAuth } from './hooks/useAuth';
export { usePermissions } from './hooks/usePermissions';
export { useSession } from './hooks/useSession';
export { useLogin } from './hooks/useLogin';

// Export context/provider
export { AuthProvider, useAuthContext } from './contexts/AuthContext';

// Export types
export type {
  User,
  LoginCredentials,
  AuthConfig,
  AuthState,
  Permission,
  Role
} from './types/auth.types';

// Export API client (if needed)
export { authClient } from './api/authClient';
```

#### 5.3.2 Publishing Modules

**Option 1: Private NPM Registry (Recommended for Enterprise)**

```bash
# Using Verdaccio (self-hosted)
npm publish --registry http://your-registry.company.com

# Using GitHub Packages
npm publish --registry https://npm.pkg.github.com

# Using Azure Artifacts
npm publish --registry https://pkgs.dev.azure.com/your-org/_packaging/your-feed/npm/registry/
```

**Option 2: Monorepo Workspace (Development)**

```yaml
# pnpm-workspace.yaml
packages:
  - 'packages/*'
  - 'packages/modules/*'
  - 'packages/ui-adapters/*'
  - 'apps/*'  # Host applications
```

```json
// apps/web-app/package.json
{
  "name": "web-app",
  "dependencies": {
    "@company/security-module": "workspace:*",
    "@company/chat-module": "workspace:*",
    "@company/notifications-module": "workspace:*"
  }
}
```

#### 5.3.3 Host Application Setup

**Installing Modules:**

```bash
# From private registry
npm install @company/security-module @company/chat-module

# Or in monorepo (automatically linked)
pnpm install
```

**Using in Host Application:**

```typescript
// apps/web-app/src/main.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { AuthProvider } from '@company/security-module';
import { NotificationsProvider } from '@company/notifications-module';
import App from './App';
import '@company/security-module/styles';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <AuthProvider config={{ apiUrl: '/api/auth' }}>
      <NotificationsProvider config={{ apiUrl: '/api/notifications' }}>
        <App />
      </NotificationsProvider>
    </AuthProvider>
  </React.StrictMode>
);
```

**TypeScript Configuration:**

```json
// apps/web-app/tsconfig.json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "jsx": "react-jsx",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "strict": true,
    "paths": {
      "@company/*": ["../../packages/modules/*/src"]
    }
  },
  "include": ["src"],
  "references": [
    { "path": "../../packages/modules/security" },
    { "path": "../../packages/modules/chat" }
  ]
}
```

---

### 5.4 Module Federation (Deprecated - v2.0 Only)

**Note:** The following Module Federation approach was used in version 2.0 but has been replaced with NPM package distribution in v2.1 for improved simplicity and developer experience.

<details>
<summary>Click to view deprecated Module Federation configuration</summary>

```typescript
// packages/modules/security/vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import federation from '@originjs/vite-plugin-federation';

export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'security-module',
      filename: 'remoteEntry.js',
      exposes: {
        './Module': './src/index',
        './LoginForm': './src/components/LoginForm',
        './useAuth': './src/hooks/useAuth',
        './AuthProvider': './src/contexts/AuthContext'
      },
      shared: {
        'react': { singleton: true, requiredVersion: '^19.0.0' },
        'react-dom': { singleton: true, requiredVersion: '^19.0.0' },
        'zustand': { singleton: true },
        'react-router-dom': { singleton: true }
      }
    })
  ],
  build: {
    modulePreload: false,
    target: 'esnext',
    minify: false,
    cssCodeSplit: false
  }
});
```

```typescript
// Host application vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import federation from '@originjs/vite-plugin-federation';

export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'host-app',
      remotes: {
        'security-module': 'http://localhost:5001/assets/remoteEntry.js',
        'chat-module': 'http://localhost:5002/assets/remoteEntry.js',
        'notifications-module': 'http://localhost:5003/assets/remoteEntry.js'
      },
      shared: {
        'react': { singleton: true, requiredVersion: '^19.0.0' },
        'react-dom': { singleton: true, requiredVersion: '^19.0.0' }
      }
    })
  ]
});
```

</details>

**Why Module Federation Was Replaced:**

While Module Federation offers runtime module loading capabilities, we found that:
- ❌ Configuration complexity outweighed benefits for most use cases
- ❌ Type safety issues at runtime made development harder
- ❌ Debugging across module boundaries was challenging
- ❌ Network overhead from multiple module fetches
- ❌ Shared dependency version management was complex

The NPM package approach provides:
- ✅ Better developer experience with full type safety
- ✅ Simpler configuration and debugging
- ✅ Standard tooling support
- ✅ Better performance through tree-shaking
- ✅ Lazy loading still available via React.lazy() when needed

---

### 5.5 Design Token System

```json
// packages/design-tokens/tokens/color.json
{
  "color": {
    "brand": {
      "primary": { "value": "#0066CC" },
      "secondary": { "value": "#6B7280" }
    },
    "semantic": {
      "success": { "value": "#10B981" },
      "warning": { "value": "#F59E0B" },
      "error": { "value": "#EF4444" },
      "info": { "value": "#3B82F6" }
    },
    "neutral": {
      "50": { "value": "#F9FAFB" },
      "100": { "value": "#F3F4F6" },
      "900": { "value": "#111827" }
    }
  }
}
```

```javascript
// packages/design-tokens/build.js
const StyleDictionary = require('style-dictionary');

// CSS Variables
StyleDictionary.registerFormat({
  name: 'css/variables',
  formatter: function(dictionary) {
    return `:root {\n${dictionary.allProperties.map(prop => 
      `  --${prop.name}: ${prop.value};`
    ).join('\n')}\n}`;
  }
});

// Tailwind Config
StyleDictionary.registerFormat({
  name: 'tailwind/config',
  formatter: function(dictionary) {
    return `module.exports = ${JSON.stringify({
      theme: {
        extend: {
          colors: dictionary.properties.color
        }
      }
    }, null, 2)}`;
  }
});

// Material-UI Theme
StyleDictionary.registerFormat({
  name: 'mui/theme',
  formatter: function(dictionary) {
    return `export const muiTheme = ${JSON.stringify({
      palette: {
        primary: { main: dictionary.properties.color.brand.primary.value }
      }
    }, null, 2)}`;
  }
});

const sd = StyleDictionary.extend({
  source: ['tokens/**/*.json'],
  platforms: {
    css: {
      transformGroup: 'css',
      buildPath: 'build/css/',
      files: [{
        destination: 'variables.css',
        format: 'css/variables'
      }]
    },
    tailwind: {
      transformGroup: 'js',
      buildPath: 'build/tailwind/',
      files: [{
        destination: 'tailwind.config.js',
        format: 'tailwind/config'
      }]
    },
    mui: {
      transformGroup: 'js',
      buildPath: 'build/mui/',
      files: [{
        destination: 'theme.ts',
        format: 'mui/theme'
      }]
    }
  }
});

sd.buildAllPlatforms();
```

---

## 6. TESTING & QUALITY ASSURANCE

### 6.1 Testing Strategy

#### 6.1.1 Frontend Testing

**Unit Testing (Vitest + Testing Library)**

```typescript
// packages/modules/security/src/hooks/useAuth.test.ts
import { renderHook, act, waitFor } from '@testing-library/react';
import { describe, it, expect, beforeEach } from 'vitest';
import { useAuth } from './useAuth';
import { AuthProvider } from '../contexts/AuthContext';

describe('useAuth', () => {
  it('should login successfully', async () => {
    const { result } = renderHook(() => useAuth(), {
      wrapper: AuthProvider
    });
    
    await act(async () => {
      await result.current.login('test@example.com', 'password');
    });
    
    await waitFor(() => {
      expect(result.current.isAuthenticated).toBe(true);
      expect(result.current.user).toEqual({
        email: 'test@example.com'
      });
    });
  });
  
  it('should handle login failure', async () => {
    const { result } = renderHook(() => useAuth(), {
      wrapper: AuthProvider
    });
    
    await act(async () => {
      await expect(
        result.current.login('invalid@example.com', 'wrong')
      ).rejects.toThrow('Invalid credentials');
    });
    
    expect(result.current.isAuthenticated).toBe(false);
  });
});
```

**Component Testing**

```typescript
// packages/modules/security/src/components/LoginForm.test.tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { describe, it, expect, vi } from 'vitest';
import { LoginForm } from './LoginForm';

describe('LoginForm', () => {
  it('should render form fields', () => {
    render(<LoginForm onSubmit={vi.fn()} />);
    
    expect(screen.getByLabelText(/email/i)).toBeInTheDocument();
    expect(screen.getByLabelText(/password/i)).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /login/i })).toBeInTheDocument();
  });
  
  it('should validate email format', async () => {
    render(<LoginForm onSubmit={vi.fn()} />);
    
    const emailInput = screen.getByLabelText(/email/i);
    fireEvent.change(emailInput, { target: { value: 'invalid-email' } });
    fireEvent.blur(emailInput);
    
    await waitFor(() => {
      expect(screen.getByText(/valid email/i)).toBeInTheDocument();
    });
  });
  
  it('should call onSubmit with correct data', async () => {
    const onSubmit = vi.fn();
    render(<LoginForm onSubmit={onSubmit} />);
    
    fireEvent.change(screen.getByLabelText(/email/i), {
      target: { value: 'test@example.com' }
    });
    fireEvent.change(screen.getByLabelText(/password/i), {
      target: { value: 'password123' }
    });
    
    fireEvent.click(screen.getByRole('button', { name: /login/i }));
    
    await waitFor(() => {
      expect(onSubmit).toHaveBeenCalledWith({
        email: 'test@example.com',
        password: 'password123'
      });
    });
  });
});
```

**API Mocking (MSW)**

```typescript
// packages/modules/security/src/mocks/handlers.ts
import { http, HttpResponse } from 'msw';

export const handlers = [
  http.post('/api/auth/login', async ({ request }) => {
    const { email, password } = await request.json();
    
    if (email === 'test@example.com' && password === 'password') {
      return HttpResponse.json({
        token: 'mock-jwt-token',
        user: { id: 1, email, fullName: 'Test User' }
      });
    }
    
    return HttpResponse.json(
      { message: 'Invalid credentials' },
      { status: 401 }
    );
  }),
  
  http.get('/api/auth/me', ({ request }) => {
    const authHeader = request.headers.get('Authorization');
    
    if (!authHeader) {
      return HttpResponse.json(
        { message: 'Unauthorized' },
        { status: 401 }
      );
    }
    
    return HttpResponse.json({
      id: 1,
      email: 'test@example.com',
      fullName: 'Test User'
    });
  })
];
```

---

#### 6.1.2 Backend Testing

**Unit Testing (xUnit + NSubstitute)**

```csharp
// YourCompany.Modules.Security.Tests/Unit/LoginCommandHandlerTests.cs
using NSubstitute;
using Xunit;
using FluentAssertions;

public class LoginCommandHandlerTests
{
    private readonly SecurityDbContext _dbContext;
    private readonly IJwtTokenService _tokenService;
    private readonly IMessageBus _bus;
    private readonly LoginCommandHandler _handler;
    
    public LoginCommandHandlerTests()
    {
        _dbContext = CreateInMemoryDbContext();
        _tokenService = Substitute.For<IJwtTokenService>();
        _bus = Substitute.For<IMessageBus>();
        _handler = new LoginCommandHandler(_dbContext, _tokenService, _bus);
    }
    
    [Fact]
    public async Task Handle_ValidCredentials_ReturnsToken()
    {
        // Arrange
        var user = new User
        {
            Email = "test@example.com",
            PasswordHash = BCrypt.Net.BCrypt.HashPassword("password"),
            IsActive = true
        };
        _dbContext.Users.Add(user);
        await _dbContext.SaveChangesAsync();
        
        _tokenService.GenerateToken(user).Returns("mock-token");
        
        var command = new LoginCommand("test@example.com", "password");
        
        // Act
        var result = await _handler.Handle(command);
        
        // Assert
        result.Token.Should().Be("mock-token");
        result.User.Email.Should().Be("test@example.com");
        
        await _bus.Received(1).PublishAsync(
            Arg.Is<UserLoggedInEvent>(e => e.UserId == user.Id));
    }
    
    [Fact]
    public async Task Handle_InvalidPassword_ThrowsException()
    {
        // Arrange
        var user = new User
        {
            Email = "test@example.com",
            PasswordHash = BCrypt.Net.BCrypt.HashPassword("password")
        };
        _dbContext.Users.Add(user);
        await _dbContext.SaveChangesAsync();
        
        var command = new LoginCommand("test@example.com", "wrong-password");
        
        // Act & Assert
        await Assert.ThrowsAsync<InvalidCredentialsException>(
            () => _handler.Handle(command));
    }
}
```

**Integration Testing (Alba)**

```csharp
// YourCompany.Modules.Security.Tests/Integration/LoginEndpointTests.cs
using Alba;
using Xunit;

public class LoginEndpointTests : IAsyncLifetime
{
    private IAlbaHost _host;
    
    public async Task InitializeAsync()
    {
        _host = await AlbaHost.For<Program>(builder =>
        {
            builder.ConfigureServices(services =>
            {
                // Use test database
                services.AddDbContext<SecurityDbContext>(options =>
                    options.UseNpgsql("test-connection-string"));
            });
        });
    }
    
    [Fact]
    public async Task Login_ValidCredentials_Returns200WithToken()
    {
        // Arrange
        await SeedTestUser();
        
        // Act & Assert
        await _host.Scenario(_ =>
        {
            _.Post.Json(new
            {
                email = "test@example.com",
                password = "password123"
            }).ToUrl("/api/auth/login");
            
            _.StatusCodeShouldBeOk();
            _.ContentShouldContain("token");
        });
    }
    
    [Fact]
    public async Task Login_InvalidCredentials_Returns401()
    {
        await _host.Scenario(_ =>
        {
            _.Post.Json(new
            {
                email = "test@example.com",
                password = "wrong-password"
            }).ToUrl("/api/auth/login");
            
            _.StatusCodeShouldBe(401);
        });
    }
    
    public async Task DisposeAsync()
    {
        await _host.DisposeAsync();
    }
}
```

---

#### 6.1.3 E2E Testing (Playwright)

```typescript
// tests/e2e/security.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Authentication Flow', () => {
  test('should login successfully', async ({ page }) => {
    await page.goto('/login');
    
    await page.fill('[name="email"]', 'test@example.com');
    await page.fill('[name="password"]', 'password123');
    await page.click('button[type="submit"]');
    
    await expect(page).toHaveURL('/dashboard');
    await expect(page.locator('text=Welcome back')).toBeVisible();
  });
  
  test('should show error for invalid credentials', async ({ page }) => {
    await page.goto('/login');
    
    await page.fill('[name="email"]', 'test@example.com');
    await page.fill('[name="password"]', 'wrong-password');
    await page.click('button[type="submit"]');
    
    await expect(page.locator('text=Invalid credentials')).toBeVisible();
    await expect(page).toHaveURL('/login');
  });
  
  test('should logout successfully', async ({ page }) => {
    // Login first
    await page.goto('/login');
    await page.fill('[name="email"]', 'test@example.com');
    await page.fill('[name="password"]', 'password123');
    await page.click('button[type="submit"]');
    
    // Logout
    await page.click('[aria-label="User menu"]');
    await page.click('text=Logout');
    
    await expect(page).toHaveURL('/login');
  });
});
```

---

### 6.2 Code Quality Standards

#### Coverage Targets
- **Backend:** 80% minimum code coverage
- **Frontend:** 70% minimum code coverage
- **Critical paths:** 100% coverage (authentication, payment, security)

#### Linting & Formatting
```json
// .eslintrc.json (Frontend)
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended"
  ],
  "rules": {
    "no-console": "warn",
    "no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "error"
  }
}
```

```xml
<!-- .editorconfig (Backend) -->
root = true

[*.cs]
indent_style = space
indent_size = 4
dotnet_sort_system_directives_first = true
dotnet_style_qualification_for_field = false
```

---

## 7. DEPLOYMENT & OPERATIONS

### 7.1 Docker Configuration

```dockerfile
# infrastructure/docker/Dockerfile.frontend
FROM node:20-alpine AS builder

WORKDIR /app

# Copy workspace files
COPY pnpm-lock.yaml pnpm-workspace.yaml ./
COPY packages/package.json ./packages/
COPY turbo.json ./

# Install dependencies
RUN npm install -g pnpm
RUN pnpm install --frozen-lockfile

# Copy source
COPY packages ./packages

# Build all packages
RUN pnpm turbo build

# Production image
FROM nginx:alpine

COPY --from=builder /app/packages/modules/security/dist /usr/share/nginx/html/security
COPY --from=builder /app/packages/modules/chat/dist /usr/share/nginx/html/chat
COPY infrastructure/docker/nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

```dockerfile
# infrastructure/docker/Dockerfile.backend
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build

WORKDIR /src

# Copy solution and project files
COPY backends/YourCompany.Modules.sln ./
COPY backends/src/**/*.csproj ./

# Restore dependencies
RUN dotnet restore

# Copy source code
COPY backends/src ./src

# Build
RUN dotnet build -c Release -o /app/build

# Publish
RUN dotnet publish -c Release -o /app/publish

# Runtime image
FROM mcr.microsoft.com/dotnet/aspnet:10.0

WORKDIR /app
COPY --from=build /app/publish .

EXPOSE 8080
ENTRYPOINT ["dotnet", "YourCompany.Gateway.dll"]
```

```yaml
# infrastructure/docker/docker-compose.yml
version: '3.8'

services:
  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: modular_components
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
  
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
  
  backend:
    build:
      context: ../../
      dockerfile: infrastructure/docker/Dockerfile.backend
    environment:
      DATABASE_PROVIDER: PostgreSQL
      ConnectionStrings__Default: "Host=postgres;Database=modular_components;Username=admin;Password=password"
      Redis__ConnectionString: "redis:6379"
    ports:
      - "8080:8080"
    depends_on:
      - postgres
      - redis
  
  frontend:
    build:
      context: ../../
      dockerfile: infrastructure/docker/Dockerfile.frontend
    ports:
      - "3000:80"
    depends_on:
      - backend

volumes:
  postgres_data:
  redis_data:
```

---

### 7.2 CI/CD Pipeline

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  frontend-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: pnpm/action-setup@v2
        with:
          version: 9
      
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'pnpm'
      
      - name: Install dependencies
        run: pnpm install
      
      - name: Type check
        run: pnpm turbo type-check
      
      - name: Lint
        run: pnpm turbo lint
      
      - name: Test
        run: pnpm turbo test -- --coverage
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
  
  backend-test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_DB: test_db
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '10.0.x'
      
      - name: Restore dependencies
        run: dotnet restore backends/YourCompany.Modules.sln
      
      - name: Build
        run: dotnet build backends/YourCompany.Modules.sln -c Release
      
      - name: Test
        run: dotnet test backends/YourCompany.Modules.sln -c Release --collect:"XPlat Code Coverage"
        env:
          ConnectionStrings__Default: "Host=localhost;Database=test_db;Username=test;Password=test"
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
  
  e2e-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      
      - name: Install Playwright
        run: npx playwright install --with-deps
      
      - name: Run E2E tests
        run: npx playwright test
      
      - name: Upload test results
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: playwright-report
          path: playwright-report/
```

---

## 8. RISK MANAGEMENT

### 8.1 Risk Assessment Matrix

| Risk | Probability | Impact | Mitigation Strategy | Owner |
|------|-------------|--------|---------------------|-------|
| **Scope Creep** | High | High | Strict phase definitions, change control process | PM |
| **Technology Learning Curve** | Medium | Medium | Training sessions, documentation, pair programming | Tech Lead |
| **Integration Complexity** | Medium | High | Proof of concepts, early integration testing | Architects |
| **Performance Issues** | Medium | High | Load testing from Phase 1, monitoring | DevOps |
| **Security Vulnerabilities** | Low | Critical | Security audits, penetration testing, code reviews | Security Team |
| **Database Migration Issues** | Medium | High | Test on both DBs, automated migration tests | DBAs |
| **Module Dependencies** | Medium | Medium | Clear interfaces, event-driven architecture | Architects |
| **Resource Availability** | Medium | High | Cross-training, knowledge transfer, documentation | PM |

---

## 9. ROADMAP & TIMELINE

> **Note:** Development priorities have been established based on business requirements and technical dependencies. The roadmap follows a phased approach focusing on foundational modules first.

### 9.0 Development Priority Order

Based on business requirements and architectural dependencies, modules will be developed in the following order:

| Priority | Module | Rationale |
|----------|--------|-----------|
| **P1** | Security | Foundation for authentication, authorization, and audit - required by all modules |
| **P2** | Notifications | Cross-cutting concern for alerts, emails, SMS - needed by most modules |
| **P3** | Logging/Audit Trail | Critical for compliance and debugging - supports all modules |
| **P4** | Workflow | Core business process automation - enables complex scenarios |
| **P5** | Rule Engine | Business rules and validation - complements workflow |
| **P6** | Chat/Chatbot | Real-time communication with AI - enhances user experience |

**Remaining Modules (Post-Priority 6):**
- Payment Gateway, Tracking, DMS, Digital Signature
- Survey Management, CMS, Knowledge Base, Gamification
- Risk Management & Assessment, Form Builder, Monitoring Management
- BI/Reporting, Analytics, Chat Bot (advanced features)

---

### 9.1 Phase 1: Security & Core Infrastructure (Months 1-3)

**Focus:** Priority #1 - Security Module + Foundation

**Objectives:**
- Establish core monorepo infrastructure
- Deliver complete Security module with all authentication/authorization features
- Prove the modular architecture and NPM package distribution
- Set up CI/CD and development workflows

**Deliverables:**

**Month 1: Infrastructure Setup**
- ✅ Turborepo + pnpm monorepo setup
- ✅ Design token system (Style Dictionary)
- ✅ Headless UI component library foundation (Radix UI)
- ✅ Tailwind, MUI, and Ant Design adapters (initial set)
- ✅ .NET 10 solution structure with Wolverine
- ✅ Database setup (PostgreSQL + MS SQL Server schemas)
- ✅ CI/CD pipeline (GitHub Actions)
- ✅ NPM package publishing setup (private registry)
- ✅ NuGet package publishing setup

**Month 2: Security Module (Priority #1) - Backend**
- ✅ User Management (CRUD operations)
- ✅ Authentication System
  - Email/Password authentication
  - JWT token generation and validation
  - Refresh token mechanism
  - Password reset flow
  - Multi-factor authentication (MFA)
- ✅ Authorization System
  - Role-Based Access Control (RBAC)
  - Permission management
  - Claims-based authorization
  - Policy-based authorization
- ✅ Session Management
  - Active session tracking
  - Session timeout handling
  - Concurrent session control
- ✅ Active Directory Integration
  - LDAP authentication
  - AD group synchronization
  - SSO support

**Month 3: Security Module (Priority #1) - Frontend + Integration**
- ✅ Security Module Frontend Components
  - Login/Logout forms
  - Registration form
  - Password reset flow
  - MFA setup and validation
  - User profile management
  - Permission checking hooks
  - Protected route components
- ✅ Audit Trail System
  - Login/logout events
  - Permission changes
  - User action tracking
  - Audit log viewer component
- ✅ Security Module NPM & NuGet Packages
  - Package configuration and publishing
  - Version management
  - Documentation and examples
- ✅ Demo application (Security module with all 3 UI frameworks)
- ✅ Integration tests and E2E tests
- ✅ Security module documentation
- ✅ Phase 1 review and adjustments

**Success Criteria:**
- Security module fully functional and tested
- Sample application deployed with authentication and authorization
- Both databases tested and working
- All three UI frameworks functional with Security components
- Modules successfully published to private npm registry and NuGet
- Full type safety demonstrated across module boundaries
- 80%+ test coverage for Security module
- Security audit passed for P1 module
- Documentation complete with integration guides

---

### 9.2 Phase 2: Cross-Cutting Concerns (Months 4-6)

**Focus:** Priority #2 (Notifications) + Priority #3 (Logging/Audit Trail)

**Objectives:**
- Implement notification system for multi-channel communication
- Build comprehensive logging and audit trail infrastructure
- Integrate with Security module for complete audit capability

**Deliverables:**

**Month 4: Notifications Module (Priority #2)**
- ✅ Notifications Backend
  - Email notifications (SendGrid/SMTP)
  - SMS notifications (Twilio, local SMS gateway)
  - Push notifications (web push, mobile)
  - In-app notification system
  - Template engine with variable substitution
  - Notification scheduling and queuing
  - Delivery tracking and status
  - Failed notification retry mechanism
- ✅ Notifications Frontend
  - Notification center component
  - Toast/alert components
  - Notification preferences UI
  - Real-time notification updates (SignalR)

**Month 5: Logging/Audit Trail Module (Priority #3)**
- ✅ Logging Infrastructure
  - Structured logging (Serilog)
  - Log aggregation (OpenTelemetry)
  - Log levels and filtering
  - Performance logging
  - Error tracking and reporting
- ✅ Audit Trail System
  - Entity change tracking
  - User action auditing
  - API call logging
  - Security event logging
  - Compliance reporting
- ✅ Logging Frontend Components
  - Log viewer with filtering
  - Audit trail viewer
  - Activity timeline
  - Compliance reports dashboard

**Month 6: Integration & Enhancement**
- ✅ Media Content Service (supporting module)
  - File upload (drag-and-drop)
  - Cloud storage (Azure Blob, AWS S3)
  - Image processing
  - CDN integration
- ✅ Integration between Security, Notifications, and Logging
  - Security events trigger notifications
  - All actions logged and auditable
  - Unified user activity tracking
- ✅ Demo application with P1-P3 modules
- ✅ Phase 2 review

**Success Criteria:**
- Multi-channel notification system working
- Complete audit trail for all user actions
- Notifications triggered by security events
- NPM packages working seamlessly across modules
- 80%+ test coverage for new modules
- Performance benchmarks met
- Integration documentation complete

---

### 9.3 Phase 3: Business Process Automation (Months 7-9)

**Focus:** Priority #4 (Workflow) + Priority #5 (Rule Engine)

**Objectives:**
- Build workflow automation engine
- Implement rule engine for business logic
- Create visual designers for workflows and rules
- Enable complex business process scenarios

**Deliverables:**

**Month 7: Workflow Module (Priority #4) - Backend**
- ✅ Workflow Engine
  - State machine implementation
  - Workflow definition storage
  - Workflow execution engine
  - Step execution and transitions
  - Conditional branching
  - Parallel execution support
- ✅ Workflow Features
  - Approval workflows
  - Multi-step processes
  - Timeout and escalation
  - Workflow versioning
  - Workflow instance tracking
  - Workflow events and triggers

**Month 8: Workflow Module (Priority #4) - Frontend + Rule Engine (Priority #5)**
- ✅ Workflow Frontend
  - Visual workflow designer (drag-and-drop)
  - Workflow template library
  - Workflow monitoring dashboard
  - Task inbox for approvals
  - Workflow history viewer
- ✅ Rule Engine Module (Priority #5)
  - Business rule definition language
  - Rule evaluation engine
  - Rule versioning and management
  - Rule testing framework
  - Integration with Workflow module
- ✅ Rule Engine Frontend
  - Rule builder UI
  - Rule testing interface
  - Rule library and templates

**Month 9: Testing & Integration**
- ✅ Complex workflow scenarios
  - Approval workflows with notifications
  - Rule-based workflow routing
  - Integration with Security for authorization
  - Integration with Logging for audit
- ✅ Demo application with P1-P5 modules
- ✅ Performance optimization for workflow execution
- ✅ Load testing (workflow engine stress test)
- ✅ Phase 3 review

**Success Criteria:**
- Visual workflow designer functional
- Complex approval workflows working
- Rule engine integrated with workflows
- All P1-P5 modules working together
- Performance benchmarks met for workflow execution
- 80%+ test coverage maintained
- Documentation complete for workflow and rule engine

---

### 9.4 Phase 4: Communication & AI (Months 10-12)

**Focus:** Priority #6 (Chat/Chatbot) + AI Integration

**Objectives:**
- Implement real-time chat functionality
- Add AI-powered chatbot with intelligent responses
- Enable rich media messaging
- Complete the priority module development

**Deliverables:**

**Month 10: Chat Module (Priority #6) - Real-time Infrastructure**
- ✅ Chat Backend
  - SignalR real-time communication
  - One-on-one messaging
  - Group chat functionality
  - Message history and persistence
  - File attachments and media
  - Read receipts and typing indicators
  - Message search and filtering
- ✅ Chat Frontend
  - Chat UI components
  - Contact list and presence
  - Message composer with rich text
  - File upload and preview
  - Emoji and reaction support

**Month 11: Chatbot & AI Integration**
- ✅ AI Chatbot System
  - OpenAI/Claude integration
  - Streaming response support
  - Context management
  - Intent recognition
  - Multi-turn conversations
  - Bot training and customization
- ✅ Chatbot Frontend
  - Bot interface component
  - Suggested responses
  - Bot configuration UI
  - Analytics dashboard
- ✅ Integration with existing modules
  - Security for authentication
  - Notifications for alerts
  - Workflow for automated tasks
  - Rule Engine for bot logic

**Month 12: Polish & Production Readiness**
- ✅ WebRTC integration (audio/video chat)
- ✅ Advanced chat features
  - Message threading
  - Channel management
  - Broadcast messages
- ✅ Complete demo application with all P1-P6 modules
- ✅ Comprehensive integration testing
- ✅ Security audit for chat and AI features
- ✅ Performance optimization
- ✅ Phase 4 review and priority module completion

**Success Criteria:**
- Real-time chat working with presence
- AI chatbot providing intelligent responses
- WebRTC audio/video functional
- All P1-P6 priority modules integrated
- Complete demo showcasing all features
- Security audit passed
- Performance benchmarks met
- Documentation complete for all priority modules

---

### 9.5 Phase 5: Remaining Modules & Production (Months 13-18)

**Focus:** Non-priority modules + Production hardening

**Objectives:**
- Complete remaining modules based on business needs
- External system integrations
- Production hardening and optimization
- Final launch preparation

**Deliverables:**

**Month 13-15: Remaining Business Modules**
- ✅ Payment Gateway Module (Stripe, PayPal, PCI compliance)
- ✅ Tracking Module (shipment, events)
- ✅ DMS (Document Management System)
- ✅ Digital Signature (e-signature integration)
- ✅ Survey Management
- ✅ CMS (Content Management System)
- ✅ Knowledge Base
- ✅ Gamification
- ✅ Risk Management & Assessment
- ✅ Form Builder
- ✅ Monitoring Management

**Month 16-17: Analytics, Reporting & Integrations**
- ✅ BI/Reporting Module
  - Dashboard builder
  - Report generation
  - Data visualization
- ✅ Analytics Module
  - Event tracking
  - User analytics
  - Performance metrics
- ✅ External System Integrations
  - TDRA, ADM, AGPAY
  - Active Directory (advanced features)
  - SMS Gateway (multi-provider)
  - Emirates Post
  - Other third-party integrations

**Month 18: Production Hardening & Launch**
- ✅ Production hardening
  - Security audit & penetration testing (comprehensive)
  - Performance optimization (all modules)
  - Load testing (1000+ concurrent users)
  - Disaster recovery procedures
  - Monitoring and alerting (OpenTelemetry)
  - Database optimization and indexing
- ✅ Final documentation
  - API documentation (complete)
  - Integration guides (all modules)
  - Architecture documentation (updated)
  - Deployment guides
  - Troubleshooting guides
- ✅ Launch preparation
  - Production environment setup
  - Rollout plan
  - Support procedures

**Success Criteria:**
- All modules completed and tested
- External integrations working
- Complete application demo with all features
- Security audit passed (comprehensive)
- Performance benchmarks met (p95 < 500ms)
- Load testing passed (1000+ users)
- Complete documentation published
- Production environment ready
- Launch approval obtained

---

## 10. APPENDICES

### 10.1 Glossary

| Term | Definition |
|------|------------|
| **CQRS** | Command Query Responsibility Segregation - pattern separating read and write operations |
| **Wolverine** | .NET messaging and CQRS framework with durable messaging |
| **NPM Package** | Standard Node Package Manager distribution format for JavaScript/TypeScript modules |
| **Headless Components** | UI components with logic separated from presentation |
| **Vertical Slice** | Architecture where features contain all layers (UI, API, Data) |
| **Turborepo** | Monorepo build system with caching and task orchestration |
| **Radix UI** | Headless UI component library with accessibility built-in |
| **Style Dictionary** | Design token transformation system |
| **Alba** | Modern integration testing library for .NET |
| **MSW** | Mock Service Worker - API mocking for browser/Node.js |
| **Tree Shaking** | Dead code elimination during build process to reduce bundle size |
| **Peer Dependencies** | npm dependencies that must be provided by the consuming application |

---

### 10.2 Decision Log (ADRs)

**ADR-001: Use Wolverine instead of MediatR**
- **Date:** Feb 11, 2026
- **Status:** Accepted
- **Context:** Need CQRS framework with durable messaging
- **Decision:** Use Wolverine for built-in outbox pattern and performance
- **Consequences:** Team needs to learn new framework, but gains enterprise features

**ADR-002: Shared Database with Schema Isolation**
- **Date:** Feb 9, 2026
- **Status:** Accepted
- **Context:** Balance between module independence and operational simplicity
- **Decision:** Single database with separate schemas per module
- **Consequences:** Simplified operations, possible cross-module queries

**ADR-003: Use Radix UI as Foundation**
- **Date:** Feb 11, 2026
- **Status:** Accepted
- **Context:** Need accessible, headless components quickly
- **Decision:** Use Radix UI instead of building from scratch
- **Consequences:** Faster development, better accessibility, dependency on external library

**ADR-004: NPM Package Distribution (Replaced Module Federation)**
- **Date:** Feb 12, 2026 (Updated from Feb 9, 2026)
- **Status:** Accepted (Supersedes ADR-004-v2.0)
- **Context:** Balance between type safety, deployment flexibility, and developer experience
- **Previous Decision (v2.0):** Use Module Federation for runtime module loading
- **Current Decision (v2.1):** Use standard NPM package distribution
- **Rationale for Change:**
  - Module Federation added unnecessary complexity for our use case
  - NPM packages provide better type safety and IDE support
  - Standard tooling works out of the box
  - Tree-shaking and build optimization work naturally
  - React.lazy() provides code splitting when needed
  - Most applications don't need runtime module updates
- **Consequences:** 
  - ✅ Simpler setup and configuration
  - ✅ Better developer experience
  - ✅ Improved type safety and debugging
  - ⚠️ Requires host rebuild to update modules (acceptable trade-off)
  - ⚠️ All modules bundled at build time (mitigated with lazy loading)

**ADR-005: Monorepo with pnpm Workspaces**
- **Date:** Feb 12, 2026
- **Status:** Accepted
- **Context:** Need efficient development workflow for multiple modules
- **Decision:** Use pnpm workspaces for local development, publish to private registry for production
- **Consequences:** Fast local development, efficient disk usage, clear versioning

---

### 10.3 References & Resources

**Technology Documentation:**
- [React 19 Documentation](https://react.dev)
- [.NET 10 Documentation](https://learn.microsoft.com/en-us/dotnet/)
- [Wolverine Documentation](https://wolverine.netlify.app/)
- [Vite Documentation](https://vitejs.dev)
- [Turborepo Documentation](https://turbo.build)
- [Radix UI Documentation](https://www.radix-ui.com)
- [Style Dictionary Documentation](https://amzn.github.io/style-dictionary/)

**Architecture Patterns:**
- [Vertical Slice Architecture by Jimmy Bogard](https://www.jimmybogard.com/vertical-slice-architecture/)
- [Module Federation Best Practices](https://module-federation.io/)
- [Headless Component Patterns](https://www.patterns.dev/posts/headless-ui)

**Testing:**
- [Testing Library Best Practices](https://testing-library.com/)
- [Playwright Documentation](https://playwright.dev)
- [Alba Testing Documentation](https://jasperfx.github.io/alba/)

---

### 10.4 Contact & Support

**Project Team:**
- **Project Manager:** TBD
- **Technical Architect:** TBD
- **Frontend Lead:** TBD
- **Backend Lead:** TBD
- **DevOps Lead:** TBD

**Questions?** Contact: [team@company.com](mailto:team@company.com)

---

## Document Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Technical Architect | | | |
| Development Lead | | | |
| Security Lead | | | |
| CTO | | | |

---

**END OF DOCUMENT**

*This is a living document and will be updated as the project evolves.*
