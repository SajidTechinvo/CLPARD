# SECURITY MODULE - DETAILED IMPLEMENTATION PLAN
## Priority #1 Module - Complete Implementation Guide
### Version 1.0

**Created:** February 12, 2026  
**Priority:** P1 (Highest)  
**Timeline:** Months 2-3 of Phase 1  
**Status:** Ready for Development

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Module Overview](#2-module-overview)
3. [Technical Architecture](#3-technical-architecture)
4. [Backend Implementation](#4-backend-implementation)
5. [Frontend Implementation](#5-frontend-implementation)
6. [Database Schema](#6-database-schema)
7. [API Specification](#7-api-specification)
8. [Security Considerations](#8-security-considerations)
9. [Testing Strategy](#9-testing-strategy)
10. [Deployment & Packaging](#10-deployment--packaging)
11. [Integration Guide](#11-integration-guide)
12. [Timeline & Milestones](#12-timeline--milestones)

---

## 1. EXECUTIVE SUMMARY

### 1.1 Purpose

The Security Module is the foundational module of the enterprise component library, providing comprehensive authentication, authorization, and audit trail capabilities. As Priority #1, all other modules will depend on this for user management and access control.

### 1.2 Key Features

- ✅ **User Management:** Complete CRUD operations for users, roles, and permissions
- ✅ **Authentication:** Email/password, JWT tokens, refresh tokens, MFA
- ✅ **Authorization:** RBAC, claims-based, policy-based authorization
- ✅ **Session Management:** Active session tracking, timeout, concurrent session control
- ✅ **Active Directory Integration:** LDAP, AD group sync, SSO
- ✅ **Audit Trail:** Comprehensive logging of all security events
- ✅ **Password Management:** Reset flow, complexity requirements, history
- ✅ **Security UI Components:** Login, registration, profile management, MFA setup

### 1.3 Success Metrics

- 80%+ test coverage (unit, integration, E2E)
- Sub-200ms response time for authentication operations
- Security audit compliance
- Support for 1000+ concurrent users
- Zero critical security vulnerabilities

---

## 2. MODULE OVERVIEW

### 2.1 Module Structure

```
packages/
└── security-module/
    ├── backend/                           # .NET Backend (4 projects) ✅
    │   ├── ReachDigital.Security.Api/      # API Layer (NuGet Package) ✅
    │   ├── ReachDigital.Security.Core/     # Business Logic ✅
    │   ├── ReachDigital.Security.Data/     # Data Access ✅
    │   └── ReachDigital.Security.Tests/    # Backend Tests ✅
    │
    └── frontend/                          # React Frontend (1 project) ✅
        ├── src/
        │   ├── components/                # UI Components ✅
        │   ├── hooks/                     # Custom React hooks ✅
        │   ├── services/                  # API service layer ✅
        │   ├── types/                     # TypeScript types ✅
        │   ├── adapters/                  # Multi-framework adapters (ready)
        │   └── index.ts                   # Public exports ✅
        ├── tests/                         # Frontend tests (ready)
        ├── package.json                   # NPM package config ✅
        └── vite.config.ts                 # Build configuration ✅
```

**Status:** ✅ All 5 projects created at `D:\WorkCode\Component-Library\packages\security-module\`

### 2.2 Technology Stack

**Backend:**
- .NET 10 (C#)
- Wolverine (CQRS, messaging)
- Entity Framework Core 10
- PostgreSQL / MS SQL Server
- JWT Bearer Authentication
- System.DirectoryServices (AD integration)
- BCrypt.Net (password hashing)
- FluentValidation (input validation)
- Serilog (logging)

**Frontend:**
- React 19
- TypeScript 5.x
- Radix UI (headless components)
- Tailwind CSS (default styling)
- TanStack Query (data fetching)
- React Hook Form (form management)
- Zod (validation)
- Axios (HTTP client)

### 2.3 Dependencies

**Backend Dependencies:**
- None (foundational module)

**Frontend Dependencies:**
- Design Token System (for theming)
- Base UI Component Library (Radix UI primitives)

**Modules that depend on Security:**
- All other modules (for authentication/authorization)

---

## 3. TECHNICAL ARCHITECTURE

### 3.1 Backend Architecture (Clean Architecture)

```
┌─────────────────────────────────────────────────────┐
│            CompanyName.Security.Api                  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Controllers (REST Endpoints)                 │  │
│  │  - AuthController                             │  │
│  │  - UserController                             │  │
│  │  - RoleController                             │  │
│  │  - PermissionController                       │  │
│  │  - AuditController                            │  │
│  └──────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Middleware                                   │  │
│  │  - JWT Authentication Middleware              │  │
│  │  - Authorization Middleware                   │  │
│  │  - Audit Logging Middleware                   │  │
│  │  - Exception Handling Middleware              │  │
│  └──────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│           CompanyName.Security.Core                  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Commands (Wolverine)                         │  │
│  │  - RegisterUser, LoginUser, LogoutUser        │  │
│  │  - CreateRole, AssignPermission               │  │
│  │  - ResetPassword, EnableMFA                   │  │
│  └──────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Queries (Wolverine)                          │  │
│  │  - GetUserById, GetUserPermissions            │  │
│  │  - GetRoles, GetAuditLog                      │  │
│  └──────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Handlers                                     │  │
│  │  - Command/Query Handlers (Wolverine)         │  │
│  │  - Business Logic Implementation              │  │
│  └──────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Services                                     │  │
│  │  - TokenService (JWT generation/validation)   │  │
│  │  - PasswordService (hashing, validation)      │  │
│  │  - MfaService (TOTP generation/validation)    │  │
│  │  - ActiveDirectoryService (LDAP)              │  │
│  └──────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Domain Models                                │  │
│  │  - User, Role, Permission, Session            │  │
│  │  - AuditLog, UserClaim                        │  │
│  └──────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│           CompanyName.Security.Data                  │
│  ┌──────────────────────────────────────────────┐  │
│  │  DbContext (EF Core)                          │  │
│  │  - SecurityDbContext                          │  │
│  └──────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Repositories                                 │  │
│  │  - UserRepository                             │  │
│  │  - RoleRepository                             │  │
│  │  - PermissionRepository                       │  │
│  │  - AuditLogRepository                         │  │
│  └──────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Migrations                                   │  │
│  │  - EF Core Database Migrations                │  │
│  └──────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
                         ↓
              ┌──────────────────────┐
              │  PostgreSQL / MSSQL  │
              │  security schema     │
              └──────────────────────┘
```

### 3.2 Frontend Architecture (Component-Based)

```
┌─────────────────────────────────────────────────────┐
│         @company/security-module (NPM Package)       │
│                                                       │
│  ┌──────────────────────────────────────────────┐  │
│  │  Public API (index.ts exports)                │  │
│  │  - Components (LoginForm, RegisterForm, etc.) │  │
│  │  - Hooks (useAuth, usePermissions, etc.)      │  │
│  │  - Services (authService, userService)        │  │
│  │  - Types (User, Role, Permission, etc.)       │  │
│  └──────────────────────────────────────────────┘  │
│                                                       │
│  ┌──────────────────────────────────────────────┐  │
│  │  Components (src/components/)                 │  │
│  │  ┌────────────────────────────────────────┐  │  │
│  │  │  Authentication                         │  │  │
│  │  │  - LoginForm, LogoutButton              │  │  │
│  │  │  - RegisterForm, ForgotPasswordForm     │  │  │
│  │  │  - ResetPasswordForm                    │  │  │
│  │  └────────────────────────────────────────┘  │  │
│  │  ┌────────────────────────────────────────┐  │  │
│  │  │  MFA (Multi-Factor Authentication)      │  │  │
│  │  │  - MfaSetup, MfaVerify                  │  │  │
│  │  │  - QRCodeDisplay                        │  │  │
│  │  └────────────────────────────────────────┘  │  │
│  │  ┌────────────────────────────────────────┐  │  │
│  │  │  User Management                        │  │  │
│  │  │  - UserProfile, UserSettings            │  │  │
│  │  │  - UserList, UserForm                   │  │  │
│  │  │  - ChangePasswordForm                   │  │  │
│  │  └────────────────────────────────────────┘  │  │
│  │  ┌────────────────────────────────────────┐  │  │
│  │  │  Authorization                          │  │  │
│  │  │  - ProtectedRoute, PermissionGuard      │  │  │
│  │  │  - RoleGuard, RequireAuth               │  │  │
│  │  └────────────────────────────────────────┘  │  │
│  │  ┌────────────────────────────────────────┐  │  │
│  │  │  Role & Permission Management           │  │  │
│  │  │  - RoleList, RoleForm                   │  │  │
│  │  │  - PermissionMatrix                     │  │  │
│  │  └────────────────────────────────────────┘  │  │
│  │  ┌────────────────────────────────────────┐  │  │
│  │  │  Audit & Session                        │  │  │
│  │  │  - AuditLogViewer, SessionList          │  │  │
│  │  │  - ActivityTimeline                     │  │  │
│  │  └────────────────────────────────────────┘  │  │
│  └──────────────────────────────────────────────┘  │
│                                                       │
│  ┌──────────────────────────────────────────────┐  │
│  │  Hooks (src/hooks/)                           │  │
│  │  - useAuth() - Current user & auth state      │  │
│  │  - usePermissions() - Check user permissions  │  │
│  │  - useRoles() - Check user roles              │  │
│  │  - useSession() - Session management          │  │
│  │  - useAuditLog() - Audit log queries          │  │
│  └──────────────────────────────────────────────┘  │
│                                                       │
│  ┌──────────────────────────────────────────────┐  │
│  │  Services (src/services/)                     │  │
│  │  - authService (login, logout, refresh)       │  │
│  │  - userService (CRUD operations)              │  │
│  │  - roleService (role management)              │  │
│  │  - permissionService (permission checks)      │  │
│  │  - auditService (audit log queries)           │  │
│  └──────────────────────────────────────────────┘  │
│                                                       │
│  ┌──────────────────────────────────────────────┐  │
│  │  Providers (src/providers/)                   │  │
│  │  - AuthProvider (React Context)               │  │
│  │  - PermissionProvider                         │  │
│  └──────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

---

## 4. BACKEND IMPLEMENTATION

### 4.1 Project Setup

#### 4.1.1 Solution Structure

```bash
# Create the solution and projects
dotnet new sln -n CompanyName.Security

# API Project
dotnet new webapi -n CompanyName.Security.Api -o backend/CompanyName.Security.Api
dotnet sln add backend/CompanyName.Security.Api

# Core Project (Business Logic)
dotnet new classlib -n CompanyName.Security.Core -o backend/CompanyName.Security.Core
dotnet sln add backend/CompanyName.Security.Core

# Data Project (EF Core)
dotnet new classlib -n CompanyName.Security.Data -o backend/CompanyName.Security.Data
dotnet sln add backend/CompanyName.Security.Data

# Test Project
dotnet new xunit -n CompanyName.Security.Tests -o backend/CompanyName.Security.Tests
dotnet sln add backend/CompanyName.Security.Tests

# Add project references
dotnet add backend/CompanyName.Security.Api reference backend/CompanyName.Security.Core
dotnet add backend/CompanyName.Security.Api reference backend/CompanyName.Security.Data
dotnet add backend/CompanyName.Security.Core reference backend/CompanyName.Security.Data
dotnet add backend/CompanyName.Security.Tests reference backend/CompanyName.Security.Api
dotnet add backend/CompanyName.Security.Tests reference backend/CompanyName.Security.Core
```

#### 4.1.2 NuGet Packages

> **Updated Feb 12, 2026:** Package names and versions corrected to match actual NuGet registry.
> Key corrections: `Wolverine` → `WolverineFx`, `Wolverine.Http` → `WolverineFx.Http`, `OtpNet` → `Otp.NET`,
> `FluentValidation.AspNetCore` capped at 11.3.1 (deprecated after that), `Swashbuckle` replaced by `Microsoft.AspNetCore.OpenApi`.

**ReachDigital.Security.Api:** ✅ Installed
```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="10.0.3" />
<PackageReference Include="WolverineFx.Http" Version="5.15.0" />
<PackageReference Include="Microsoft.AspNetCore.OpenApi" Version="10.0.2" />
<PackageReference Include="Serilog.AspNetCore" Version="10.0.0" />
<PackageReference Include="FluentValidation.AspNetCore" Version="11.3.1" />
```

**ReachDigital.Security.Core:** ✅ Installed
```xml
<PackageReference Include="WolverineFx" Version="5.15.0" />
<PackageReference Include="BCrypt.Net-Next" Version="4.0.3" />
<PackageReference Include="System.IdentityModel.Tokens.Jwt" Version="8.15.0" />
<PackageReference Include="FluentValidation" Version="12.1.1" />
<PackageReference Include="System.DirectoryServices" Version="10.0.3" />
<PackageReference Include="System.DirectoryServices.AccountManagement" Version="10.0.3" />
<PackageReference Include="Otp.NET" Version="1.4.1" />
```

**ReachDigital.Security.Data:** ✅ Installed
```xml
<PackageReference Include="Microsoft.EntityFrameworkCore" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="10.0.3" />
<PackageReference Include="Npgsql.EntityFrameworkCore.PostgreSQL" Version="10.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="10.0.3" />
```

**ReachDigital.Security.Tests:** ✅ Installed
```xml
<PackageReference Include="xunit" Version="2.9.3" />
<PackageReference Include="xunit.runner.visualstudio" Version="3.1.4" />
<PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.14.1" />
<PackageReference Include="coverlet.collector" Version="6.0.4" />
<PackageReference Include="Moq" Version="4.20.72" />
<PackageReference Include="FluentAssertions" Version="8.8.0" />
<PackageReference Include="Alba" Version="8.4.0" />
<PackageReference Include="Testcontainers.PostgreSql" Version="4.10.0" />
```

### 4.2 Domain Models (CompanyName.Security.Core/Models)

#### 4.2.1 User.cs

```csharp
namespace CompanyName.Security.Core.Models;

public class User
{
    public Guid Id { get; set; }
    public string Email { get; set; } = string.Empty;
    public string Username { get; set; } = string.Empty;
    public string PasswordHash { get; set; } = string.Empty;
    public string? FirstName { get; set; }
    public string? LastName { get; set; }
    public string? PhoneNumber { get; set; }
    public bool IsActive { get; set; } = true;
    public bool EmailConfirmed { get; set; }
    public bool PhoneConfirmed { get; set; }
    public bool MfaEnabled { get; set; }
    public string? MfaSecret { get; set; }
    public DateTime? LastLoginAt { get; set; }
    public DateTime? PasswordChangedAt { get; set; }
    public int FailedLoginAttempts { get; set; }
    public DateTime? LockoutEndAt { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime? UpdatedAt { get; set; }
    public string? CreatedBy { get; set; }
    public string? UpdatedBy { get; set; }

    // Navigation properties
    public ICollection<UserRole> UserRoles { get; set; } = new List<UserRole>();
    public ICollection<UserClaim> UserClaims { get; set; } = new List<UserClaim>();
    public ICollection<UserSession> Sessions { get; set; } = new List<UserSession>();
    public ICollection<PasswordHistory> PasswordHistory { get; set; } = new List<PasswordHistory>();

    // Computed properties
    public bool IsLockedOut => LockoutEndAt.HasValue && LockoutEndAt > DateTime.UtcNow;
    public string FullName => $"{FirstName} {LastName}".Trim();
}
```

#### 4.2.2 Role.cs

```csharp
namespace CompanyName.Security.Core.Models;

public class Role
{
    public Guid Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public string? Description { get; set; }
    public bool IsSystemRole { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime? UpdatedAt { get; set; }

    // Navigation properties
    public ICollection<UserRole> UserRoles { get; set; } = new List<UserRole>();
    public ICollection<RolePermission> RolePermissions { get; set; } = new List<RolePermission>();
}
```

#### 4.2.3 Permission.cs

```csharp
namespace CompanyName.Security.Core.Models;

public class Permission
{
    public Guid Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public string Resource { get; set; } = string.Empty;
    public string Action { get; set; } = string.Empty;
    public string? Description { get; set; }
    public DateTime CreatedAt { get; set; }

    // Navigation properties
    public ICollection<RolePermission> RolePermissions { get; set; } = new List<RolePermission>();

    // Computed property
    public string FullPermission => $"{Resource}:{Action}";
}
```

#### 4.2.4 UserSession.cs

```csharp
namespace CompanyName.Security.Core.Models;

public class UserSession
{
    public Guid Id { get; set; }
    public Guid UserId { get; set; }
    public string RefreshToken { get; set; } = string.Empty;
    public string? IpAddress { get; set; }
    public string? UserAgent { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime ExpiresAt { get; set; }
    public DateTime? RevokedAt { get; set; }
    public string? RevokedByIp { get; set; }
    public string? ReplacedByToken { get; set; }

    // Navigation properties
    public User User { get; set; } = null!;

    // Computed properties
    public bool IsExpired => DateTime.UtcNow >= ExpiresAt;
    public bool IsRevoked => RevokedAt != null;
    public bool IsActive => !IsRevoked && !IsExpired;
}
```

#### 4.2.5 AuditLog.cs

```csharp
namespace CompanyName.Security.Core.Models;

public class AuditLog
{
    public Guid Id { get; set; }
    public Guid? UserId { get; set; }
    public string Action { get; set; } = string.Empty;
    public string Resource { get; set; } = string.Empty;
    public string? Details { get; set; }
    public string? IpAddress { get; set; }
    public string? UserAgent { get; set; }
    public DateTime CreatedAt { get; set; }
    public bool IsSuccess { get; set; }
    public string? ErrorMessage { get; set; }

    // Navigation properties
    public User? User { get; set; }
}
```

#### 4.2.6 Junction Tables

```csharp
namespace CompanyName.Security.Core.Models;

public class UserRole
{
    public Guid UserId { get; set; }
    public Guid RoleId { get; set; }
    public DateTime AssignedAt { get; set; }
    public string? AssignedBy { get; set; }

    public User User { get; set; } = null!;
    public Role Role { get; set; } = null!;
}

public class RolePermission
{
    public Guid RoleId { get; set; }
    public Guid PermissionId { get; set; }
    public DateTime AssignedAt { get; set; }

    public Role Role { get; set; } = null!;
    public Permission Permission { get; set; } = null!;
}

public class UserClaim
{
    public Guid Id { get; set; }
    public Guid UserId { get; set; }
    public string ClaimType { get; set; } = string.Empty;
    public string ClaimValue { get; set; } = string.Empty;
    public DateTime CreatedAt { get; set; }

    public User User { get; set; } = null!;
}

public class PasswordHistory
{
    public Guid Id { get; set; }
    public Guid UserId { get; set; }
    public string PasswordHash { get; set; } = string.Empty;
    public DateTime CreatedAt { get; set; }

    public User User { get; set; } = null!;
}
```

### 4.3 Database Context (CompanyName.Security.Data/SecurityDbContext.cs)

```csharp
using Microsoft.EntityFrameworkCore;
using CompanyName.Security.Core.Models;

namespace CompanyName.Security.Data;

public class SecurityDbContext : DbContext
{
    public SecurityDbContext(DbContextOptions<SecurityDbContext> options) : base(options)
    {
    }

    public DbSet<User> Users => Set<User>();
    public DbSet<Role> Roles => Set<Role>();
    public DbSet<Permission> Permissions => Set<Permission>();
    public DbSet<UserRole> UserRoles => Set<UserRole>();
    public DbSet<RolePermission> RolePermissions => Set<RolePermission>();
    public DbSet<UserClaim> UserClaims => Set<UserClaim>();
    public DbSet<UserSession> UserSessions => Set<UserSession>();
    public DbSet<AuditLog> AuditLogs => Set<AuditLog>();
    public DbSet<PasswordHistory> PasswordHistories => Set<PasswordHistory>();

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        // Set default schema
        modelBuilder.HasDefaultSchema("security");

        // User entity
        modelBuilder.Entity<User>(entity =>
        {
            entity.ToTable("users");
            entity.HasKey(e => e.Id);
            entity.HasIndex(e => e.Email).IsUnique();
            entity.HasIndex(e => e.Username).IsUnique();
            entity.Property(e => e.Email).IsRequired().HasMaxLength(255);
            entity.Property(e => e.Username).IsRequired().HasMaxLength(100);
            entity.Property(e => e.PasswordHash).IsRequired();
            entity.Property(e => e.FirstName).HasMaxLength(100);
            entity.Property(e => e.LastName).HasMaxLength(100);
            entity.Property(e => e.PhoneNumber).HasMaxLength(20);
        });

        // Role entity
        modelBuilder.Entity<Role>(entity =>
        {
            entity.ToTable("roles");
            entity.HasKey(e => e.Id);
            entity.HasIndex(e => e.Name).IsUnique();
            entity.Property(e => e.Name).IsRequired().HasMaxLength(100);
            entity.Property(e => e.Description).HasMaxLength(500);
        });

        // Permission entity
        modelBuilder.Entity<Permission>(entity =>
        {
            entity.ToTable("permissions");
            entity.HasKey(e => e.Id);
            entity.HasIndex(e => new { e.Resource, e.Action }).IsUnique();
            entity.Property(e => e.Name).IsRequired().HasMaxLength(100);
            entity.Property(e => e.Resource).IsRequired().HasMaxLength(100);
            entity.Property(e => e.Action).IsRequired().HasMaxLength(50);
            entity.Property(e => e.Description).HasMaxLength(500);
        });

        // UserRole junction table
        modelBuilder.Entity<UserRole>(entity =>
        {
            entity.ToTable("user_roles");
            entity.HasKey(e => new { e.UserId, e.RoleId });
            entity.HasOne(e => e.User)
                .WithMany(u => u.UserRoles)
                .HasForeignKey(e => e.UserId)
                .OnDelete(DeleteBehavior.Cascade);
            entity.HasOne(e => e.Role)
                .WithMany(r => r.UserRoles)
                .HasForeignKey(e => e.RoleId)
                .OnDelete(DeleteBehavior.Cascade);
        });

        // RolePermission junction table
        modelBuilder.Entity<RolePermission>(entity =>
        {
            entity.ToTable("role_permissions");
            entity.HasKey(e => new { e.RoleId, e.PermissionId });
            entity.HasOne(e => e.Role)
                .WithMany(r => r.RolePermissions)
                .HasForeignKey(e => e.RoleId)
                .OnDelete(DeleteBehavior.Cascade);
            entity.HasOne(e => e.Permission)
                .WithMany(p => p.RolePermissions)
                .HasForeignKey(e => e.PermissionId)
                .OnDelete(DeleteBehavior.Cascade);
        });

        // UserClaim entity
        modelBuilder.Entity<UserClaim>(entity =>
        {
            entity.ToTable("user_claims");
            entity.HasKey(e => e.Id);
            entity.HasOne(e => e.User)
                .WithMany(u => u.UserClaims)
                .HasForeignKey(e => e.UserId)
                .OnDelete(DeleteBehavior.Cascade);
            entity.Property(e => e.ClaimType).IsRequired().HasMaxLength(100);
            entity.Property(e => e.ClaimValue).IsRequired().HasMaxLength(500);
        });

        // UserSession entity
        modelBuilder.Entity<UserSession>(entity =>
        {
            entity.ToTable("user_sessions");
            entity.HasKey(e => e.Id);
            entity.HasIndex(e => e.RefreshToken).IsUnique();
            entity.HasOne(e => e.User)
                .WithMany(u => u.Sessions)
                .HasForeignKey(e => e.UserId)
                .OnDelete(DeleteBehavior.Cascade);
            entity.Property(e => e.RefreshToken).IsRequired();
            entity.Property(e => e.IpAddress).HasMaxLength(50);
        });

        // AuditLog entity
        modelBuilder.Entity<AuditLog>(entity =>
        {
            entity.ToTable("audit_logs");
            entity.HasKey(e => e.Id);
            entity.HasIndex(e => e.CreatedAt);
            entity.HasIndex(e => e.UserId);
            entity.HasOne(e => e.User)
                .WithMany()
                .HasForeignKey(e => e.UserId)
                .OnDelete(DeleteBehavior.SetNull);
            entity.Property(e => e.Action).IsRequired().HasMaxLength(100);
            entity.Property(e => e.Resource).IsRequired().HasMaxLength(100);
            entity.Property(e => e.IpAddress).HasMaxLength(50);
        });

        // PasswordHistory entity
        modelBuilder.Entity<PasswordHistory>(entity =>
        {
            entity.ToTable("password_histories");
            entity.HasKey(e => e.Id);
            entity.HasIndex(e => new { e.UserId, e.CreatedAt });
            entity.HasOne(e => e.User)
                .WithMany(u => u.PasswordHistory)
                .HasForeignKey(e => e.UserId)
                .OnDelete(DeleteBehavior.Cascade);
            entity.Property(e => e.PasswordHash).IsRequired();
        });

        // Seed default roles and permissions
        SeedData(modelBuilder);
    }

    private void SeedData(ModelBuilder modelBuilder)
    {
        // Default Roles
        var adminRoleId = Guid.Parse("11111111-1111-1111-1111-111111111111");
        var userRoleId = Guid.Parse("22222222-2222-2222-2222-222222222222");

        modelBuilder.Entity<Role>().HasData(
            new Role
            {
                Id = adminRoleId,
                Name = "Administrator",
                Description = "Full system access",
                IsSystemRole = true,
                CreatedAt = DateTime.UtcNow
            },
            new Role
            {
                Id = userRoleId,
                Name = "User",
                Description = "Standard user access",
                IsSystemRole = true,
                CreatedAt = DateTime.UtcNow
            }
        );

        // Default Permissions
        var permissionId = 1;
        var permissions = new List<Permission>
        {
            // User permissions
            new() { Id = Guid.NewGuid(), Name = "View Users", Resource = "users", Action = "read", CreatedAt = DateTime.UtcNow },
            new() { Id = Guid.NewGuid(), Name = "Create Users", Resource = "users", Action = "create", CreatedAt = DateTime.UtcNow },
            new() { Id = Guid.NewGuid(), Name = "Update Users", Resource = "users", Action = "update", CreatedAt = DateTime.UtcNow },
            new() { Id = Guid.NewGuid(), Name = "Delete Users", Resource = "users", Action = "delete", CreatedAt = DateTime.UtcNow },

            // Role permissions
            new() { Id = Guid.NewGuid(), Name = "View Roles", Resource = "roles", Action = "read", CreatedAt = DateTime.UtcNow },
            new() { Id = Guid.NewGuid(), Name = "Manage Roles", Resource = "roles", Action = "manage", CreatedAt = DateTime.UtcNow },

            // Permission permissions
            new() { Id = Guid.NewGuid(), Name = "View Permissions", Resource = "permissions", Action = "read", CreatedAt = DateTime.UtcNow },
            new() { Id = Guid.NewGuid(), Name = "Manage Permissions", Resource = "permissions", Action = "manage", CreatedAt = DateTime.UtcNow },

            // Audit permissions
            new() { Id = Guid.NewGuid(), Name = "View Audit Logs", Resource = "audit", Action = "read", CreatedAt = DateTime.UtcNow },
        };

        modelBuilder.Entity<Permission>().HasData(permissions);

        // Assign all permissions to Administrator role
        var rolePermissions = permissions.Select(p => new RolePermission
        {
            RoleId = adminRoleId,
            PermissionId = p.Id,
            AssignedAt = DateTime.UtcNow
        }).ToList();

        modelBuilder.Entity<RolePermission>().HasData(rolePermissions);
    }
}
```

### 4.4 Core Services (CompanyName.Security.Core/Services)

#### 4.4.1 ITokenService.cs & TokenService.cs

```csharp
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using System.Security.Cryptography;
using Microsoft.IdentityModel.Tokens;
using System.Text;
using CompanyName.Security.Core.Models;

namespace CompanyName.Security.Core.Services;

public interface ITokenService
{
    string GenerateAccessToken(User user, IEnumerable<string> roles, IEnumerable<string> permissions);
    string GenerateRefreshToken();
    ClaimsPrincipal? GetPrincipalFromExpiredToken(string token);
}

public class TokenService : ITokenService
{
    private readonly string _secret;
    private readonly string _issuer;
    private readonly string _audience;
    private readonly int _accessTokenExpirationMinutes;

    public TokenService(IConfiguration configuration)
    {
        _secret = configuration["Jwt:Secret"] ?? throw new ArgumentNullException("Jwt:Secret");
        _issuer = configuration["Jwt:Issuer"] ?? throw new ArgumentNullException("Jwt:Issuer");
        _audience = configuration["Jwt:Audience"] ?? throw new ArgumentNullException("Jwt:Audience");
        _accessTokenExpirationMinutes = int.Parse(configuration["Jwt:AccessTokenExpirationMinutes"] ?? "30");
    }

    public string GenerateAccessToken(User user, IEnumerable<string> roles, IEnumerable<string> permissions)
    {
        var claims = new List<Claim>
        {
            new(JwtRegisteredClaimNames.Sub, user.Id.ToString()),
            new(JwtRegisteredClaimNames.Email, user.Email),
            new(JwtRegisteredClaimNames.UniqueName, user.Username),
            new(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
            new("full_name", user.FullName)
        };

        // Add roles
        claims.AddRange(roles.Select(role => new Claim(ClaimTypes.Role, role)));

        // Add permissions
        claims.AddRange(permissions.Select(permission => new Claim("permission", permission)));

        var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_secret));
        var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

        var token = new JwtSecurityToken(
            issuer: _issuer,
            audience: _audience,
            claims: claims,
            expires: DateTime.UtcNow.AddMinutes(_accessTokenExpirationMinutes),
            signingCredentials: creds
        );

        return new JwtSecurityTokenHandler().WriteToken(token);
    }

    public string GenerateRefreshToken()
    {
        var randomNumber = new byte[64];
        using var rng = RandomNumberGenerator.Create();
        rng.GetBytes(randomNumber);
        return Convert.ToBase64String(randomNumber);
    }

    public ClaimsPrincipal? GetPrincipalFromExpiredToken(string token)
    {
        var tokenValidationParameters = new TokenValidationParameters
        {
            ValidateAudience = true,
            ValidAudience = _audience,
            ValidateIssuer = true,
            ValidIssuer = _issuer,
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_secret)),
            ValidateLifetime = false // Don't validate lifetime here
        };

        var tokenHandler = new JwtSecurityTokenHandler();
        var principal = tokenHandler.ValidateToken(token, tokenValidationParameters, out var securityToken);

        if (securityToken is not JwtSecurityToken jwtSecurityToken ||
            !jwtSecurityToken.Header.Alg.Equals(SecurityAlgorithms.HmacSha256, StringComparison.InvariantCultureIgnoreCase))
        {
            return null;
        }

        return principal;
    }
}
```

#### 4.4.2 IPasswordService.cs & PasswordService.cs

```csharp
using BCrypt.Net;

namespace CompanyName.Security.Core.Services;

public interface IPasswordService
{
    string HashPassword(string password);
    bool VerifyPassword(string password, string hash);
    bool ValidatePasswordComplexity(string password);
    string GenerateRandomPassword(int length = 12);
}

public class PasswordService : IPasswordService
{
    private const int MinLength = 8;
    private const int MaxLength = 128;

    public string HashPassword(string password)
    {
        return BCrypt.Net.BCrypt.HashPassword(password, BCrypt.Net.BCrypt.GenerateSalt(12));
    }

    public bool VerifyPassword(string password, string hash)
    {
        try
        {
            return BCrypt.Net.BCrypt.Verify(password, hash);
        }
        catch
        {
            return false;
        }
    }

    public bool ValidatePasswordComplexity(string password)
    {
        if (string.IsNullOrWhiteSpace(password))
            return false;

        if (password.Length < MinLength || password.Length > MaxLength)
            return false;

        var hasUpperCase = password.Any(char.IsUpper);
        var hasLowerCase = password.Any(char.IsLower);
        var hasDigit = password.Any(char.IsDigit);
        var hasSpecialChar = password.Any(c => !char.IsLetterOrDigit(c));

        return hasUpperCase && hasLowerCase && hasDigit && hasSpecialChar;
    }

    public string GenerateRandomPassword(int length = 12)
    {
        const string validChars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890!@#$%^&*";
        var random = new Random();
        return new string(Enumerable.Range(0, length)
            .Select(_ => validChars[random.Next(validChars.Length)])
            .ToArray());
    }
}
```

#### 4.4.3 IMfaService.cs & MfaService.cs

```csharp
using OtpNet;
using System.Text;

namespace CompanyName.Security.Core.Services;

public interface IMfaService
{
    string GenerateSecret();
    string GenerateQrCodeUri(string email, string secret, string issuer = "CompanyName");
    bool ValidateCode(string secret, string code);
}

public class MfaService : IMfaService
{
    public string GenerateSecret()
    {
        var key = KeyGeneration.GenerateRandomKey(20);
        return Base32Encoding.ToString(key);
    }

    public string GenerateQrCodeUri(string email, string secret, string issuer = "CompanyName")
    {
        return $"otpauth://totp/{issuer}:{email}?secret={secret}&issuer={issuer}";
    }

    public bool ValidateCode(string secret, string code)
    {
        var secretBytes = Base32Encoding.ToBytes(secret);
        var totp = new Totp(secretBytes);
        
        // Allow 1 time step before and after current time (30 second window each)
        return totp.VerifyTotp(code, out _, new VerificationWindow(1, 1));
    }
}
```

### 4.5 CQRS Commands & Handlers (Wolverine)

#### 4.5.1 RegisterUserCommand.cs

```csharp
namespace CompanyName.Security.Core.Commands;

public record RegisterUserCommand(
    string Email,
    string Username,
    string Password,
    string? FirstName,
    string? LastName,
    string? PhoneNumber
);

public record RegisterUserResult(Guid UserId, string Message);

public class RegisterUserHandler
{
    private readonly SecurityDbContext _context;
    private readonly IPasswordService _passwordService;
    private readonly ILogger<RegisterUserHandler> _logger;

    public RegisterUserHandler(
        SecurityDbContext context,
        IPasswordService passwordService,
        ILogger<RegisterUserHandler> logger)
    {
        _context = context;
        _passwordService = passwordService;
        _logger = logger;
    }

    public async Task<RegisterUserResult> Handle(RegisterUserCommand command)
    {
        // Validate email uniqueness
        if (await _context.Users.AnyAsync(u => u.Email == command.Email))
            throw new InvalidOperationException("Email already exists");

        // Validate username uniqueness
        if (await _context.Users.AnyAsync(u => u.Username == command.Username))
            throw new InvalidOperationException("Username already exists");

        // Validate password complexity
        if (!_passwordService.ValidatePasswordComplexity(command.Password))
            throw new InvalidOperationException("Password does not meet complexity requirements");

        var user = new User
        {
            Id = Guid.NewGuid(),
            Email = command.Email,
            Username = command.Username,
            PasswordHash = _passwordService.HashPassword(command.Password),
            FirstName = command.FirstName,
            LastName = command.LastName,
            PhoneNumber = command.PhoneNumber,
            CreatedAt = DateTime.UtcNow,
            IsActive = true,
            EmailConfirmed = false
        };

        _context.Users.Add(user);

        // Assign default "User" role
        var userRole = await _context.Roles.FirstOrDefaultAsync(r => r.Name == "User");
        if (userRole != null)
        {
            _context.UserRoles.Add(new UserRole
            {
                UserId = user.Id,
                RoleId = userRole.Id,
                AssignedAt = DateTime.UtcNow
            });
        }

        // Add to password history
        _context.PasswordHistories.Add(new PasswordHistory
        {
            Id = Guid.NewGuid(),
            UserId = user.Id,
            PasswordHash = user.PasswordHash,
            CreatedAt = DateTime.UtcNow
        });

        // Audit log
        _context.AuditLogs.Add(new AuditLog
        {
            Id = Guid.NewGuid(),
            UserId = user.Id,
            Action = "register",
            Resource = "user",
            Details = $"User {user.Email} registered",
            CreatedAt = DateTime.UtcNow,
            IsSuccess = true
        });

        await _context.SaveChangesAsync();

        _logger.LogInformation("User {Email} registered successfully with ID {UserId}", user.Email, user.Id);

        return new RegisterUserResult(user.Id, "User registered successfully");
    }
}
```

#### 4.5.2 LoginCommand.cs

```csharp
namespace CompanyName.Security.Core.Commands;

public record LoginCommand(
    string EmailOrUsername,
    string Password,
    string? IpAddress,
    string? UserAgent,
    string? MfaCode
);

public record LoginResult(
    string AccessToken,
    string RefreshToken,
    UserDto User,
    bool RequiresMfa
);

public record UserDto(
    Guid Id,
    string Email,
    string Username,
    string? FirstName,
    string? LastName,
    bool MfaEnabled,
    IEnumerable<string> Roles,
    IEnumerable<string> Permissions
);

public class LoginHandler
{
    private readonly SecurityDbContext _context;
    private readonly IPasswordService _passwordService;
    private readonly ITokenService _tokenService;
    private readonly IMfaService _mfaService;
    private readonly ILogger<LoginHandler> _logger;

    public LoginHandler(
        SecurityDbContext context,
        IPasswordService passwordService,
        ITokenService tokenService,
        IMfaService mfaService,
        ILogger<LoginHandler> logger)
    {
        _context = context;
        _passwordService = passwordService;
        _tokenService = tokenService;
        _mfaService = mfaService;
        _logger = logger;
    }

    public async Task<LoginResult> Handle(LoginCommand command)
    {
        // Find user
        var user = await _context.Users
            .Include(u => u.UserRoles)
                .ThenInclude(ur => ur.Role)
                    .ThenInclude(r => r.RolePermissions)
                        .ThenInclude(rp => rp.Permission)
            .FirstOrDefaultAsync(u => 
                u.Email == command.EmailOrUsername || 
                u.Username == command.EmailOrUsername);

        if (user == null)
        {
            await LogFailedLogin(command.EmailOrUsername, "User not found", command.IpAddress);
            throw new UnauthorizedAccessException("Invalid credentials");
        }

        // Check if user is locked out
        if (user.IsLockedOut)
        {
            await LogFailedLogin(user.Email, "Account locked", command.IpAddress);
            throw new UnauthorizedAccessException("Account is locked");
        }

        // Check if user is active
        if (!user.IsActive)
        {
            await LogFailedLogin(user.Email, "Account inactive", command.IpAddress);
            throw new UnauthorizedAccessException("Account is inactive");
        }

        // Verify password
        if (!_passwordService.VerifyPassword(command.Password, user.PasswordHash))
        {
            user.FailedLoginAttempts++;
            
            // Lock account after 5 failed attempts
            if (user.FailedLoginAttempts >= 5)
            {
                user.LockoutEndAt = DateTime.UtcNow.AddMinutes(30);
                _logger.LogWarning("User {Email} locked out after {Attempts} failed attempts", 
                    user.Email, user.FailedLoginAttempts);
            }

            await _context.SaveChangesAsync();
            await LogFailedLogin(user.Email, "Invalid password", command.IpAddress);
            throw new UnauthorizedAccessException("Invalid credentials");
        }

        // Check MFA if enabled
        if (user.MfaEnabled)
        {
            if (string.IsNullOrEmpty(command.MfaCode))
            {
                return new LoginResult(
                    string.Empty,
                    string.Empty,
                    MapToUserDto(user),
                    RequiresMfa: true
                );
            }

            if (!_mfaService.ValidateCode(user.MfaSecret!, command.MfaCode))
            {
                await LogFailedLogin(user.Email, "Invalid MFA code", command.IpAddress);
                throw new UnauthorizedAccessException("Invalid MFA code");
            }
        }

        // Reset failed login attempts
        user.FailedLoginAttempts = 0;
        user.LastLoginAt = DateTime.UtcNow;

        // Generate tokens
        var roles = user.UserRoles.Select(ur => ur.Role.Name).ToList();
        var permissions = user.UserRoles
            .SelectMany(ur => ur.Role.RolePermissions)
            .Select(rp => rp.Permission.FullPermission)
            .Distinct()
            .ToList();

        var accessToken = _tokenService.GenerateAccessToken(user, roles, permissions);
        var refreshToken = _tokenService.GenerateRefreshToken();

        // Create session
        var session = new UserSession
        {
            Id = Guid.NewGuid(),
            UserId = user.Id,
            RefreshToken = refreshToken,
            IpAddress = command.IpAddress,
            UserAgent = command.UserAgent,
            CreatedAt = DateTime.UtcNow,
            ExpiresAt = DateTime.UtcNow.AddDays(7)
        };

        _context.UserSessions.Add(session);

        // Audit log
        _context.AuditLogs.Add(new AuditLog
        {
            Id = Guid.NewGuid(),
            UserId = user.Id,
            Action = "login",
            Resource = "auth",
            Details = "User logged in successfully",
            IpAddress = command.IpAddress,
            UserAgent = command.UserAgent,
            CreatedAt = DateTime.UtcNow,
            IsSuccess = true
        });

        await _context.SaveChangesAsync();

        _logger.LogInformation("User {Email} logged in successfully", user.Email);

        return new LoginResult(
            accessToken,
            refreshToken,
            MapToUserDto(user),
            RequiresMfa: false
        );
    }

    private async Task LogFailedLogin(string identifier, string reason, string? ipAddress)
    {
        _context.AuditLogs.Add(new AuditLog
        {
            Id = Guid.NewGuid(),
            Action = "login",
            Resource = "auth",
            Details = $"Failed login attempt for {identifier}: {reason}",
            IpAddress = ipAddress,
            CreatedAt = DateTime.UtcNow,
            IsSuccess = false,
            ErrorMessage = reason
        });

        await _context.SaveChangesAsync();
        _logger.LogWarning("Failed login attempt for {Identifier}: {Reason}", identifier, reason);
    }

    private UserDto MapToUserDto(User user)
    {
        var roles = user.UserRoles.Select(ur => ur.Role.Name).ToList();
        var permissions = user.UserRoles
            .SelectMany(ur => ur.Role.RolePermissions)
            .Select(rp => rp.Permission.FullPermission)
            .Distinct()
            .ToList();

        return new UserDto(
            user.Id,
            user.Email,
            user.Username,
            user.FirstName,
            user.LastName,
            user.MfaEnabled,
            roles,
            permissions
        );
    }
}
```

### 4.6 API Controllers (CompanyName.Security.Api/Controllers)

#### 4.6.1 AuthController.cs

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Authorization;
using Wolverine;
using CompanyName.Security.Core.Commands;

namespace CompanyName.Security.Api.Controllers;

[ApiController]
[Route("api/[controller]")]
public class AuthController : ControllerBase
{
    private readonly IMessageBus _bus;
    private readonly ILogger<AuthController> _logger;

    public AuthController(IMessageBus bus, ILogger<AuthController> logger)
    {
        _bus = bus;
        _logger = logger;
    }

    [HttpPost("register")]
    [ProducesResponseType(typeof(RegisterUserResult), StatusCodes.Status201Created)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    public async Task<IActionResult> Register([FromBody] RegisterUserCommand command)
    {
        try
        {
            var result = await _bus.InvokeAsync<RegisterUserResult>(command);
            return CreatedAtAction(nameof(Register), new { id = result.UserId }, result);
        }
        catch (InvalidOperationException ex)
        {
            return BadRequest(new { error = ex.Message });
        }
    }

    [HttpPost("login")]
    [ProducesResponseType(typeof(LoginResult), StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status401Unauthorized)]
    public async Task<IActionResult> Login([FromBody] LoginCommand command)
    {
        try
        {
            var ipAddress = HttpContext.Connection.RemoteIpAddress?.ToString();
            var userAgent = HttpContext.Request.Headers["User-Agent"].ToString();
            
            var loginCommand = command with { IpAddress = ipAddress, UserAgent = userAgent };
            var result = await _bus.InvokeAsync<LoginResult>(loginCommand);
            
            return Ok(result);
        }
        catch (UnauthorizedAccessException ex)
        {
            return Unauthorized(new { error = ex.Message });
        }
    }

    [HttpPost("refresh")]
    [ProducesResponseType(typeof(RefreshTokenResult), StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status401Unauthorized)]
    public async Task<IActionResult> RefreshToken([FromBody] RefreshTokenCommand command)
    {
        try
        {
            var result = await _bus.InvokeAsync<RefreshTokenResult>(command);
            return Ok(result);
        }
        catch (UnauthorizedAccessException ex)
        {
            return Unauthorized(new { error = ex.Message });
        }
    }

    [Authorize]
    [HttpPost("logout")]
    [ProducesResponseType(StatusCodes.Status204NoContent)]
    public async Task<IActionResult> Logout()
    {
        var userId = User.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
        if (userId == null)
            return Unauthorized();

        var command = new LogoutCommand(Guid.Parse(userId));
        await _bus.InvokeAsync(command);
        
        return NoContent();
    }

    [Authorize]
    [HttpGet("me")]
    [ProducesResponseType(typeof(UserDto), StatusCodes.Status200OK)]
    public async Task<IActionResult> GetCurrentUser()
    {
        var userId = User.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
        if (userId == null)
            return Unauthorized();

        var query = new GetUserByIdQuery(Guid.Parse(userId));
        var result = await _bus.InvokeAsync<UserDto>(query);
        
        return Ok(result);
    }
}
```

---

## 5. FRONTEND IMPLEMENTATION

### 5.1 Project Setup

#### 5.1.1 Initialize Frontend Package

```bash
cd packages/security-module
mkdir frontend
cd frontend

# Initialize package.json
pnpm init
```

#### 5.1.2 package.json

```json
{
  "name": "@company/security-module",
  "version": "1.0.0",
  "description": "Security module with authentication, authorization, and audit trail",
  "type": "module",
  "main": "./dist/index.cjs",
  "module": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.cjs",
      "types": "./dist/index.d.ts"
    }
  },
  "files": [
    "dist"
  ],
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "lint": "eslint . --ext ts,tsx",
    "test": "vitest",
    "test:ui": "vitest --ui"
  },
  "peerDependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0"
  },
  "dependencies": {
    "@radix-ui/react-dialog": "^1.1.0",
    "@radix-ui/react-dropdown-menu": "^2.1.0",
    "@radix-ui/react-label": "^2.1.0",
    "@radix-ui/react-popover": "^1.1.0",
    "@radix-ui/react-select": "^2.1.0",
    "@radix-ui/react-switch": "^1.1.0",
    "@radix-ui/react-tabs": "^1.1.0",
    "@tanstack/react-query": "^5.28.0",
    "axios": "^1.6.7",
    "clsx": "^2.1.0",
    "react-hook-form": "^7.51.0",
    "zod": "^3.22.4",
    "@hookform/resolvers": "^3.3.4",
    "qrcode": "^1.5.3"
  },
  "devDependencies": {
    "@types/node": "^20.11.24",
    "@types/react": "^19.0.0",
    "@types/react-dom": "^19.0.0",
    "@types/qrcode": "^1.5.5",
    "@vitejs/plugin-react": "^4.2.1",
    "typescript": "^5.4.2",
    "vite": "^5.1.5",
    "vite-plugin-dts": "^3.7.3",
    "vitest": "^1.3.1",
    "@testing-library/react": "^14.2.1",
    "@testing-library/user-event": "^14.5.2",
    "@testing-library/jest-dom": "^6.4.2",
    "eslint": "^8.57.0",
    "tailwindcss": "^3.4.1",
    "autoprefixer": "^10.4.18",
    "postcss": "^8.4.35"
  }
}
```

#### 5.1.3 vite.config.ts

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import dts from 'vite-plugin-dts';
import { resolve } from 'path';

export default defineConfig({
  plugins: [
    react(),
    dts({
      insertTypesEntry: true,
      include: ['src'],
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

#### 5.1.4 tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "declaration": true,
    "declarationMap": true,
    "outDir": "./dist"
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

### 5.2 Core Types (src/types/index.ts)

```typescript
export interface User {
  id: string;
  email: string;
  username: string;
  firstName?: string;
  lastName?: string;
  phoneNumber?: string;
  isActive: boolean;
  emailConfirmed: boolean;
  mfaEnabled: boolean;
  lastLoginAt?: string;
  createdAt: string;
  roles: string[];
  permissions: string[];
}

export interface LoginRequest {
  emailOrUsername: string;
  password: string;
  mfaCode?: string;
}

export interface LoginResponse {
  accessToken: string;
  refreshToken: string;
  user: User;
  requiresMfa: boolean;
}

export interface RegisterRequest {
  email: string;
  username: string;
  password: string;
  firstName?: string;
  lastName?: string;
  phoneNumber?: string;
}

export interface RegisterResponse {
  userId: string;
  message: string;
}

export interface RefreshTokenRequest {
  refreshToken: string;
}

export interface RefreshTokenResponse {
  accessToken: string;
  refreshToken: string;
}

export interface ResetPasswordRequest {
  email: string;
}

export interface ChangePasswordRequest {
  currentPassword: string;
  newPassword: string;
}

export interface MfaSetupResponse {
  secret: string;
  qrCodeUri: string;
}

export interface MfaVerifyRequest {
  code: string;
}

export interface Role {
  id: string;
  name: string;
  description?: string;
  isSystemRole: boolean;
  createdAt: string;
}

export interface Permission {
  id: string;
  name: string;
  resource: string;
  action: string;
  description?: string;
}

export interface AuditLog {
  id: string;
  userId?: string;
  action: string;
  resource: string;
  details?: string;
  ipAddress?: string;
  userAgent?: string;
  createdAt: string;
  isSuccess: boolean;
  errorMessage?: string;
}

export interface Session {
  id: string;
  userId: string;
  ipAddress?: string;
  userAgent?: string;
  createdAt: string;
  expiresAt: string;
  isActive: boolean;
}
```

### 5.3 API Service (src/services/authService.ts)

```typescript
import axios, { AxiosInstance } from 'axios';
import type {
  LoginRequest,
  LoginResponse,
  RegisterRequest,
  RegisterResponse,
  RefreshTokenRequest,
  RefreshTokenResponse,
  ResetPasswordRequest,
  ChangePasswordRequest,
  MfaSetupResponse,
  MfaVerifyRequest,
  User
} from '../types';

export class AuthService {
  private api: AxiosInstance;
  private tokenKey = 'security_access_token';
  private refreshTokenKey = 'security_refresh_token';

  constructor(baseURL: string) {
    this.api = axios.create({
      baseURL,
      headers: {
        'Content-Type': 'application/json',
      },
    });

    // Request interceptor to add token
    this.api.interceptors.request.use(
      (config) => {
        const token = this.getAccessToken();
        if (token) {
          config.headers.Authorization = `Bearer ${token}`;
        }
        return config;
      },
      (error) => Promise.reject(error)
    );

    // Response interceptor to handle token refresh
    this.api.interceptors.response.use(
      (response) => response,
      async (error) => {
        const originalRequest = error.config;

        if (error.response?.status === 401 && !originalRequest._retry) {
          originalRequest._retry = true;

          try {
            const refreshToken = this.getRefreshToken();
            if (refreshToken) {
              const response = await this.refreshToken({ refreshToken });
              this.setTokens(response.accessToken, response.refreshToken);
              originalRequest.headers.Authorization = `Bearer ${response.accessToken}`;
              return this.api(originalRequest);
            }
          } catch (refreshError) {
            this.clearTokens();
            window.location.href = '/login';
            return Promise.reject(refreshError);
          }
        }

        return Promise.reject(error);
      }
    );
  }

  async login(request: LoginRequest): Promise<LoginResponse> {
    const response = await this.api.post<LoginResponse>('/auth/login', request);
    if (!response.data.requiresMfa) {
      this.setTokens(response.data.accessToken, response.data.refreshToken);
    }
    return response.data;
  }

  async register(request: RegisterRequest): Promise<RegisterResponse> {
    const response = await this.api.post<RegisterResponse>('/auth/register', request);
    return response.data;
  }

  async logout(): Promise<void> {
    await this.api.post('/auth/logout');
    this.clearTokens();
  }

  async refreshToken(request: RefreshTokenRequest): Promise<RefreshTokenResponse> {
    const response = await this.api.post<RefreshTokenResponse>('/auth/refresh', request);
    return response.data;
  }

  async getCurrentUser(): Promise<User> {
    const response = await this.api.get<User>('/auth/me');
    return response.data;
  }

  async resetPassword(request: ResetPasswordRequest): Promise<void> {
    await this.api.post('/auth/reset-password', request);
  }

  async changePassword(request: ChangePasswordRequest): Promise<void> {
    await this.api.post('/auth/change-password', request);
  }

  async setupMfa(): Promise<MfaSetupResponse> {
    const response = await this.api.post<MfaSetupResponse>('/auth/mfa/setup');
    return response.data;
  }

  async verifyMfa(request: MfaVerifyRequest): Promise<void> {
    await this.api.post('/auth/mfa/verify', request);
  }

  async disableMfa(): Promise<void> {
    await this.api.post('/auth/mfa/disable');
  }

  // Token management
  private setTokens(accessToken: string, refreshToken: string): void {
    localStorage.setItem(this.tokenKey, accessToken);
    localStorage.setItem(this.refreshTokenKey, refreshToken);
  }

  private getAccessToken(): string | null {
    return localStorage.getItem(this.tokenKey);
  }

  private getRefreshToken(): string | null {
    return localStorage.getItem(this.refreshTokenKey);
  }

  private clearTokens(): void {
    localStorage.removeItem(this.tokenKey);
    localStorage.removeItem(this.refreshTokenKey);
  }

  isAuthenticated(): boolean {
    return !!this.getAccessToken();
  }
}
```

### 5.4 Auth Context & Hook (src/hooks/useAuth.tsx)

```typescript
import React, { createContext, useContext, useState, useEffect, ReactNode } from 'react';
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { AuthService } from '../services/authService';
import type { User, LoginRequest, RegisterRequest } from '../types';

interface AuthContextType {
  user: User | null;
  isLoading: boolean;
  isAuthenticated: boolean;
  login: (request: LoginRequest) => Promise<void>;
  register: (request: RegisterRequest) => Promise<void>;
  logout: () => Promise<void>;
  hasPermission: (permission: string) => boolean;
  hasRole: (role: string) => boolean;
  hasAnyRole: (roles: string[]) => boolean;
  hasAllRoles: (roles: string[]) => boolean;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

interface AuthProviderProps {
  children: ReactNode;
  authService: AuthService;
}

export function AuthProvider({ children, authService }: AuthProviderProps) {
  const queryClient = useQueryClient();

  // Fetch current user
  const { data: user, isLoading } = useQuery({
    queryKey: ['currentUser'],
    queryFn: () => authService.getCurrentUser(),
    enabled: authService.isAuthenticated(),
    retry: false,
    staleTime: 5 * 60 * 1000, // 5 minutes
  });

  // Login mutation
  const loginMutation = useMutation({
    mutationFn: (request: LoginRequest) => authService.login(request),
    onSuccess: (data) => {
      if (!data.requiresMfa) {
        queryClient.setQueryData(['currentUser'], data.user);
      }
    },
  });

  // Register mutation
  const registerMutation = useMutation({
    mutationFn: (request: RegisterRequest) => authService.register(request),
  });

  // Logout mutation
  const logoutMutation = useMutation({
    mutationFn: () => authService.logout(),
    onSuccess: () => {
      queryClient.setQueryData(['currentUser'], null);
      queryClient.clear();
    },
  });

  const hasPermission = (permission: string): boolean => {
    return user?.permissions.includes(permission) ?? false;
  };

  const hasRole = (role: string): boolean => {
    return user?.roles.includes(role) ?? false;
  };

  const hasAnyRole = (roles: string[]): boolean => {
    return roles.some(role => user?.roles.includes(role)) ?? false;
  };

  const hasAllRoles = (roles: string[]): boolean => {
    return roles.every(role => user?.roles.includes(role)) ?? false;
  };

  const value: AuthContextType = {
    user: user ?? null,
    isLoading,
    isAuthenticated: !!user,
    login: async (request) => {
      const result = await loginMutation.mutateAsync(request);
      if (result.requiresMfa) {
        throw new Error('MFA_REQUIRED');
      }
    },
    register: (request) => registerMutation.mutateAsync(request),
    logout: () => logoutMutation.mutateAsync(),
    hasPermission,
    hasRole,
    hasAnyRole,
    hasAllRoles,
  };

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

export function useAuth(): AuthContextType {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
}
```

### 5.5 Login Component (src/components/LoginForm.tsx)

```typescript
import React, { useState } from 'react';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { useAuth } from '../hooks/useAuth';

const loginSchema = z.object({
  emailOrUsername: z.string().min(1, 'Email or username is required'),
  password: z.string().min(1, 'Password is required'),
  mfaCode: z.string().optional(),
});

type LoginFormData = z.infer<typeof loginSchema>;

interface LoginFormProps {
  onSuccess?: () => void;
  onForgotPassword?: () => void;
  onRegister?: () => void;
}

export function LoginForm({ onSuccess, onForgotPassword, onRegister }: LoginFormProps) {
  const { login } = useAuth();
  const [requiresMfa, setRequiresMfa] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<LoginFormData>({
    resolver: zodResolver(loginSchema),
  });

  const onSubmit = async (data: LoginFormData) => {
    try {
      setError(null);
      await login(data);
      onSuccess?.();
    } catch (err: any) {
      if (err.message === 'MFA_REQUIRED') {
        setRequiresMfa(true);
      } else {
        setError(err.response?.data?.error || 'Login failed');
      }
    }
  };

  return (
    <div className="w-full max-w-md mx-auto p-6 bg-white rounded-lg shadow-md">
      <h2 className="text-2xl font-bold mb-6 text-center">Sign In</h2>
      
      {error && (
        <div className="mb-4 p-3 bg-red-100 border border-red-400 text-red-700 rounded">
          {error}
        </div>
      )}

      <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
        <div>
          <label htmlFor="emailOrUsername" className="block text-sm font-medium mb-1">
            Email or Username
          </label>
          <input
            id="emailOrUsername"
            type="text"
            {...register('emailOrUsername')}
            className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
            disabled={isSubmitting}
          />
          {errors.emailOrUsername && (
            <p className="mt-1 text-sm text-red-600">{errors.emailOrUsername.message}</p>
          )}
        </div>

        <div>
          <label htmlFor="password" className="block text-sm font-medium mb-1">
            Password
          </label>
          <input
            id="password"
            type="password"
            {...register('password')}
            className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
            disabled={isSubmitting}
          />
          {errors.password && (
            <p className="mt-1 text-sm text-red-600">{errors.password.message}</p>
          )}
        </div>

        {requiresMfa && (
          <div>
            <label htmlFor="mfaCode" className="block text-sm font-medium mb-1">
              MFA Code
            </label>
            <input
              id="mfaCode"
              type="text"
              {...register('mfaCode')}
              className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter 6-digit code"
              disabled={isSubmitting}
            />
            {errors.mfaCode && (
              <p className="mt-1 text-sm text-red-600">{errors.mfaCode.message}</p>
            )}
          </div>
        )}

        <button
          type="submit"
          disabled={isSubmitting}
          className="w-full bg-blue-600 text-white py-2 px-4 rounded-md hover:bg-blue-700 disabled:bg-gray-400 disabled:cursor-not-allowed"
        >
          {isSubmitting ? 'Signing in...' : 'Sign In'}
        </button>
      </form>

      <div className="mt-4 flex justify-between text-sm">
        {onForgotPassword && (
          <button
            onClick={onForgotPassword}
            className="text-blue-600 hover:underline"
          >
            Forgot password?
          </button>
        )}
        {onRegister && (
          <button
            onClick={onRegister}
            className="text-blue-600 hover:underline"
          >
            Create account
          </button>
        )}
      </div>
    </div>
  );
}
```

### 5.6 Protected Route Component (src/components/ProtectedRoute.tsx)

```typescript
import React, { ReactNode } from 'react';
import { Navigate } from 'react-router-dom';
import { useAuth } from '../hooks/useAuth';

interface ProtectedRouteProps {
  children: ReactNode;
  requiredPermission?: string;
  requiredRole?: string;
  requiredAnyRole?: string[];
  redirectTo?: string;
  fallback?: ReactNode;
}

export function ProtectedRoute({
  children,
  requiredPermission,
  requiredRole,
  requiredAnyRole,
  redirectTo = '/login',
  fallback,
}: ProtectedRouteProps) {
  const { isAuthenticated, isLoading, hasPermission, hasRole, hasAnyRole } = useAuth();

  if (isLoading) {
    return fallback || <div>Loading...</div>;
  }

  if (!isAuthenticated) {
    return <Navigate to={redirectTo} replace />;
  }

  if (requiredPermission && !hasPermission(requiredPermission)) {
    return <Navigate to="/unauthorized" replace />;
  }

  if (requiredRole && !hasRole(requiredRole)) {
    return <Navigate to="/unauthorized" replace />;
  }

  if (requiredAnyRole && !hasAnyRole(requiredAnyRole)) {
    return <Navigate to="/unauthorized" replace />;
  }

  return <>{children}</>;
}
```

### 5.7 Public API (src/index.ts)

```typescript
// Components
export { LoginForm } from './components/LoginForm';
export { RegisterForm } from './components/RegisterForm';
export { ProtectedRoute } from './components/ProtectedRoute';
export { UserProfile } from './components/UserProfile';
export { MfaSetup } from './components/MfaSetup';

// Hooks
export { AuthProvider, useAuth } from './hooks/useAuth';
export { usePermissions } from './hooks/usePermissions';
export { useRoles } from './hooks/useRoles';

// Services
export { AuthService } from './services/authService';
export { UserService } from './services/userService';

// Types
export type {
  User,
  LoginRequest,
  LoginResponse,
  RegisterRequest,
  RegisterResponse,
  Role,
  Permission,
  AuditLog,
  Session,
} from './types';
```

---

### 5.8 Multi-Framework Adapter Architecture (CSS-Only Approach)

**Important Architectural Decision:** The Security Module uses a **CSS-only adapter pattern** to support multiple UI frameworks without adding any framework dependencies to the module itself.

#### 5.8.1 Core Principle: Framework Agnostic Module

The Security Module has **ZERO UI framework dependencies** in its `package.json`:

```json
{
  "name": "@reachdigital/security-module",
  "version": "1.0.0",
  "dependencies": {
    "@radix-ui/react-dialog": "^1.0.0",
    "@radix-ui/react-label": "^2.0.0",
    "@radix-ui/react-tabs": "^2.0.0",
    "react-hook-form": "^7.50.0",
    "zod": "^3.22.0",
    "axios": "^1.6.7",
    "clsx": "^2.1.0",
    "qrcode": "^1.5.3"
  },
  "peerDependencies": {
    "react": "^18.0.0 || ^19.0.0",
    "react-dom": "^18.0.0 || ^19.0.0"
  }
  // ❌ NO tailwindcss
  // ❌ NO @mui/material
  // ❌ NO antd
}
```

**Key Benefits:**
- ✅ **Tiny bundle size** - Module itself is ~50-80 KB
- ✅ **Framework flexibility** - Host app chooses UI framework
- ✅ **Zero conflicts** - No version mismatches with host app's UI framework
- ✅ **Production-ready** - This is how professional component libraries work

#### 5.8.2 Adapter Structure

**File Structure:**
```
packages/security-module/frontend/src/
├── adapters/
│   ├── index.ts              # Adapter selection logic
│   ├── tailwind.ts           # Tailwind CSS class strings
│   ├── mui.ts                # Material-UI CSS class strings
│   └── ant-design.ts         # Ant Design CSS class strings
└── components/
    └── LoginForm.tsx         # Uses adapters
```

**Adapter Interface:**

```typescript
// src/adapters/index.ts
export interface ComponentStyles {
  // Common elements
  button: string;
  buttonPrimary: string;
  buttonSecondary: string;
  buttonDanger: string;
  input: string;
  inputError: string;
  label: string;
  
  // Layout
  container: string;
  card: string;
  
  // Dialog/Modal
  overlay: string;
  dialogContent: string;
  dialogTitle: string;
  dialogDescription: string;
  
  // Form
  formGroup: string;
  errorMessage: string;
  successMessage: string;
  
  // Status badges
  badgeSuccess: string;
  badgeError: string;
  badgeWarning: string;
  badgeInfo: string;
}

export type FrameworkType = 'tailwind' | 'mui' | 'ant';

export function getFrameworkStyles(framework: FrameworkType): ComponentStyles {
  switch (framework) {
    case 'tailwind':
      return tailwindStyles;
    case 'mui':
      return muiStyles;
    case 'ant':
      return antStyles;
    default:
      return tailwindStyles; // Default fallback
  }
}
```

**Tailwind Adapter (CSS Classes Only):**

```typescript
// src/adapters/tailwind.ts
import { ComponentStyles } from './index';

export const tailwindStyles: ComponentStyles = {
  // Buttons
  button: 'px-4 py-2 rounded-md font-medium transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2',
  buttonPrimary: 'bg-blue-600 text-white hover:bg-blue-700 focus:ring-blue-500',
  buttonSecondary: 'bg-gray-100 text-gray-700 hover:bg-gray-200 focus:ring-gray-500',
  buttonDanger: 'bg-red-600 text-white hover:bg-red-700 focus:ring-red-500',
  
  // Inputs
  input: 'w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
  inputError: 'border-red-400 focus:ring-red-500',
  label: 'block text-sm font-medium text-gray-700',
  
  // Layout
  container: 'w-full max-w-md mx-auto',
  card: 'bg-white rounded-lg shadow-md p-6',
  
  // Dialog
  overlay: 'fixed inset-0 bg-black/50',
  dialogContent: 'fixed top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white rounded-lg shadow-xl p-6 max-w-md w-full',
  dialogTitle: 'text-xl font-bold text-gray-900',
  dialogDescription: 'text-sm text-gray-500 mt-2',
  
  // Form
  formGroup: 'space-y-1',
  errorMessage: 'text-sm text-red-600',
  successMessage: 'text-sm text-green-600',
  
  // Badges
  badgeSuccess: 'inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-green-100 text-green-800',
  badgeError: 'inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-red-100 text-red-800',
  badgeWarning: 'inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-yellow-100 text-yellow-800',
  badgeInfo: 'inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-blue-100 text-blue-800',
};
```

**Material-UI Adapter (CSS Classes Only):**

```typescript
// src/adapters/mui.ts
import { ComponentStyles } from './index';

export const muiStyles: ComponentStyles = {
  // Buttons - Using MUI CSS class names
  button: 'MuiButton-root MuiButton-text',
  buttonPrimary: 'MuiButton-contained MuiButton-containedPrimary',
  buttonSecondary: 'MuiButton-outlined MuiButton-outlinedSecondary',
  buttonDanger: 'MuiButton-contained MuiButton-containedError',
  
  // Inputs
  input: 'MuiInput-root MuiInput-underline',
  inputError: 'Mui-error',
  label: 'MuiFormLabel-root',
  
  // Layout
  container: 'MuiContainer-root MuiContainer-maxWidthSm',
  card: 'MuiPaper-root MuiPaper-elevation1 MuiPaper-rounded',
  
  // Dialog
  overlay: 'MuiBackdrop-root',
  dialogContent: 'MuiDialog-paper MuiDialog-paperScrollPaper',
  dialogTitle: 'MuiDialogTitle-root',
  dialogDescription: 'MuiDialogContent-root',
  
  // Form
  formGroup: 'MuiFormControl-root',
  errorMessage: 'MuiFormHelperText-root Mui-error',
  successMessage: 'MuiFormHelperText-root MuiFormHelperText-filled',
  
  // Badges
  badgeSuccess: 'MuiChip-root MuiChip-filled MuiChip-colorSuccess MuiChip-sizeSmall',
  badgeError: 'MuiChip-root MuiChip-filled MuiChip-colorError MuiChip-sizeSmall',
  badgeWarning: 'MuiChip-root MuiChip-filled MuiChip-colorWarning MuiChip-sizeSmall',
  badgeInfo: 'MuiChip-root MuiChip-filled MuiChip-colorInfo MuiChip-sizeSmall',
};
```

**Ant Design Adapter (CSS Classes Only):**

```typescript
// src/adapters/ant-design.ts
import { ComponentStyles } from './index';

export const antStyles: ComponentStyles = {
  // Buttons
  button: 'ant-btn',
  buttonPrimary: 'ant-btn ant-btn-primary',
  buttonSecondary: 'ant-btn ant-btn-default',
  buttonDanger: 'ant-btn ant-btn-dangerous',
  
  // Inputs
  input: 'ant-input',
  inputError: 'ant-input-status-error',
  label: 'ant-form-item-label',
  
  // Layout
  container: 'ant-layout-content',
  card: 'ant-card ant-card-bordered',
  
  // Dialog
  overlay: 'ant-modal-mask',
  dialogContent: 'ant-modal-content',
  dialogTitle: 'ant-modal-title',
  dialogDescription: 'ant-modal-body',
  
  // Form
  formGroup: 'ant-form-item',
  errorMessage: 'ant-form-item-explain-error',
  successMessage: 'ant-form-item-explain-success',
  
  // Badges
  badgeSuccess: 'ant-badge ant-badge-success',
  badgeError: 'ant-badge ant-badge-error',
  badgeWarning: 'ant-badge ant-badge-warning',
  badgeInfo: 'ant-badge ant-badge-processing',
};
```

#### 5.8.3 Component Usage of Adapters

**Example: LoginForm Component**

```typescript
// src/components/LoginForm.tsx
import { useState } from 'react';
import { useForm } from 'react-hook-form';
import * as Label from '@radix-ui/react-label';
import { useAuth } from '../hooks/useAuth';
import { getFrameworkStyles, FrameworkType } from '../adapters';
import { clsx } from 'clsx';

export interface LoginFormProps {
  onSuccess?: () => void;
  onForgotPassword?: () => void;
  onRegister?: () => void;
  className?: string;
  framework?: FrameworkType; // 👈 Framework selection prop
}

export function LoginForm({
  onSuccess,
  onForgotPassword,
  onRegister,
  className,
  framework = 'tailwind', // 👈 Default to Tailwind
}: LoginFormProps) {
  const { login } = useAuth();
  const [error, setError] = useState<string | null>(null);
  
  // 👇 Get framework-specific styles
  const styles = getFrameworkStyles(framework);
  
  const { register, handleSubmit, formState: { errors, isSubmitting } } = useForm();

  const onSubmit = async (data: any) => {
    try {
      await login(data);
      onSuccess?.();
    } catch (err) {
      setError('Login failed');
    }
  };

  return (
    <div className={clsx(styles.container, styles.card, className)}>
      <h2 className={styles.dialogTitle}>Sign In</h2>

      {error && (
        <div className={styles.errorMessage} role="alert">
          {error}
        </div>
      )}

      <form onSubmit={handleSubmit(onSubmit)}>
        {/* Email Field */}
        <div className={styles.formGroup}>
          <Label.Root className={styles.label}>
            Email or Username
          </Label.Root>
          <input
            type="text"
            {...register('emailOrUsername')}
            className={clsx(styles.input, errors.emailOrUsername && styles.inputError)}
            disabled={isSubmitting}
          />
          {errors.emailOrUsername && (
            <p className={styles.errorMessage}>
              {errors.emailOrUsername.message}
            </p>
          )}
        </div>

        {/* Password Field */}
        <div className={styles.formGroup}>
          <Label.Root className={styles.label}>
            Password
          </Label.Root>
          <input
            type="password"
            {...register('password')}
            className={clsx(styles.input, errors.password && styles.inputError)}
            disabled={isSubmitting}
          />
        </div>

        {/* Submit Button */}
        <button
          type="submit"
          disabled={isSubmitting}
          className={clsx(styles.button, styles.buttonPrimary)}
        >
          {isSubmitting ? 'Signing in...' : 'Sign In'}
        </button>
      </form>
    </div>
  );
}
```

#### 5.8.4 How Host Applications Use It

**Host App with Tailwind CSS:**

```json
// host-app-tailwind/package.json
{
  "dependencies": {
    "@reachdigital/security-module": "^1.0.0",
    "tailwindcss": "^3.4.1",
    "react": "^19.0.0"
  }
}
```

```typescript
// host-app-tailwind/src/App.tsx
import { LoginForm } from '@reachdigital/security-module';

function App() {
  // Tailwind CSS in host app styles the Tailwind classes
  return <LoginForm framework="tailwind" />;
}
```

**Host App with Material-UI:**

```json
// host-app-mui/package.json
{
  "dependencies": {
    "@reachdigital/security-module": "^1.0.0",
    "@mui/material": "^5.15.0",
    "@emotion/react": "^11.11.0",
    "@emotion/styled": "^11.11.0",
    "react": "^19.0.0"
  }
}
```

```typescript
// host-app-mui/src/App.tsx
import { LoginForm } from '@reachdigital/security-module';
import { ThemeProvider, createTheme } from '@mui/material';

const theme = createTheme();

function App() {
  return (
    <ThemeProvider theme={theme}>
      {/* MUI ThemeProvider styles the MUI classes */}
      <LoginForm framework="mui" />
    </ThemeProvider>
  );
}
```

**Host App with Ant Design:**

```json
// host-app-ant/package.json
{
  "dependencies": {
    "@reachdigital/security-module": "^1.0.0",
    "antd": "^5.15.0",
    "react": "^19.0.0"
  }
}
```

```typescript
// host-app-ant/src/App.tsx
import { LoginForm } from '@reachdigital/security-module';
import { ConfigProvider } from 'antd';

function App() {
  return (
    <ConfigProvider>
      {/* Ant Design ConfigProvider styles the Ant classes */}
      <LoginForm framework="ant" />
    </ConfigProvider>
  );
}
```

#### 5.8.5 Bundle Size Comparison

| App Configuration | Security Module | UI Framework | Total Bundle |
|-------------------|-----------------|--------------|--------------|
| **Tailwind only** | ~80 KB | ~50 KB | ~130 KB |
| **MUI only** | ~80 KB | ~300 KB | ~380 KB |
| **Ant Design only** | ~80 KB | ~400 KB | ~480 KB |
| **All three (❌ not recommended)** | ~80 KB | ~750 KB | ~830 KB |

**Key Insight:** Each host app only installs ONE UI framework, keeping bundle sizes optimal!

#### 5.8.6 Migration Path for Existing Components

All existing components in the Security Module need to be updated to support the framework prop:

**Components to Update:**
1. ✅ LoginForm.tsx
2. ✅ RegisterForm.tsx
3. ✅ ProtectedRoute.tsx (minimal styling)
4. ✅ UserProfile.tsx
5. ✅ MfaSetup.tsx
6. ✅ ChangePassword.tsx
7. ✅ PasswordResetRequest.tsx
8. ✅ RoleManagement.tsx
9. ✅ AuditLogViewer.tsx
10. ✅ SessionManagement.tsx

**Update Pattern:**
```typescript
// Before (Week 1-10 implementation)
export function SomeComponent({ ...props }) {
  return <div className="bg-white p-6 rounded-lg">...</div>;
}

// After (Week 12 update)
export interface SomeComponentProps {
  framework?: FrameworkType;
  // ... other props
}

export function SomeComponent({ framework = 'tailwind', ...props }: SomeComponentProps) {
  const styles = getFrameworkStyles(framework);
  return <div className={clsx(styles.card)}>...</div>;
}
```

---

## 6. DATABASE SCHEMA

### 6.1 PostgreSQL Schema

```sql
-- Create schema
CREATE SCHEMA IF NOT EXISTS security;

-- Users table
CREATE TABLE security.users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) NOT NULL UNIQUE,
    username VARCHAR(100) NOT NULL UNIQUE,
    password_hash TEXT NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    phone_number VARCHAR(20),
    is_active BOOLEAN DEFAULT true,
    email_confirmed BOOLEAN DEFAULT false,
    phone_confirmed BOOLEAN DEFAULT false,
    mfa_enabled BOOLEAN DEFAULT false,
    mfa_secret VARCHAR(255),
    last_login_at TIMESTAMP,
    password_changed_at TIMESTAMP,
    failed_login_attempts INT DEFAULT 0,
    lockout_end_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP,
    created_by VARCHAR(255),
    updated_by VARCHAR(255)
);

-- Roles table
CREATE TABLE security.roles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL UNIQUE,
    description VARCHAR(500),
    is_system_role BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP
);

-- Permissions table
CREATE TABLE security.permissions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL,
    resource VARCHAR(100) NOT NULL,
    action VARCHAR(50) NOT NULL,
    description VARCHAR(500),
    created_at TIMESTAMP DEFAULT NOW(),
    UNIQUE(resource, action)
);

-- User-Role junction table
CREATE TABLE security.user_roles (
    user_id UUID NOT NULL REFERENCES security.users(id) ON DELETE CASCADE,
    role_id UUID NOT NULL REFERENCES security.roles(id) ON DELETE CASCADE,
    assigned_at TIMESTAMP DEFAULT NOW(),
    assigned_by VARCHAR(255),
    PRIMARY KEY (user_id, role_id)
);

-- Role-Permission junction table
CREATE TABLE security.role_permissions (
    role_id UUID NOT NULL REFERENCES security.roles(id) ON DELETE CASCADE,
    permission_id UUID NOT NULL REFERENCES security.permissions(id) ON DELETE CASCADE,
    assigned_at TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (role_id, permission_id)
);

-- User claims table
CREATE TABLE security.user_claims (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES security.users(id) ON DELETE CASCADE,
    claim_type VARCHAR(100) NOT NULL,
    claim_value VARCHAR(500) NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

-- User sessions table
CREATE TABLE security.user_sessions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES security.users(id) ON DELETE CASCADE,
    refresh_token TEXT NOT NULL UNIQUE,
    ip_address VARCHAR(50),
    user_agent TEXT,
    created_at TIMESTAMP DEFAULT NOW(),
    expires_at TIMESTAMP NOT NULL,
    revoked_at TIMESTAMP,
    revoked_by_ip VARCHAR(50),
    replaced_by_token TEXT
);

-- Audit logs table
CREATE TABLE security.audit_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES security.users(id) ON DELETE SET NULL,
    action VARCHAR(100) NOT NULL,
    resource VARCHAR(100) NOT NULL,
    details TEXT,
    ip_address VARCHAR(50),
    user_agent TEXT,
    created_at TIMESTAMP DEFAULT NOW(),
    is_success BOOLEAN DEFAULT true,
    error_message TEXT
);

-- Password history table
CREATE TABLE security.password_histories (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES security.users(id) ON DELETE CASCADE,
    password_hash TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Indexes for performance
CREATE INDEX idx_users_email ON security.users(email);
CREATE INDEX idx_users_username ON security.users(username);
CREATE INDEX idx_users_last_login ON security.users(last_login_at);
CREATE INDEX idx_user_sessions_refresh_token ON security.user_sessions(refresh_token);
CREATE INDEX idx_user_sessions_user_id ON security.user_sessions(user_id);
CREATE INDEX idx_audit_logs_user_id ON security.audit_logs(user_id);
CREATE INDEX idx_audit_logs_created_at ON security.audit_logs(created_at);
CREATE INDEX idx_audit_logs_resource_action ON security.audit_logs(resource, action);
CREATE INDEX idx_password_histories_user_id_created ON security.password_histories(user_id, created_at);
```

---

## 7. API SPECIFICATION

### 7.1 Authentication Endpoints

#### POST /api/auth/register
Register a new user

**Request:**
```json
{
  "email": "user@example.com",
  "username": "johndoe",
  "password": "SecurePass123!",
  "firstName": "John",
  "lastName": "Doe",
  "phoneNumber": "+1234567890"
}
```

**Response:** 201 Created
```json
{
  "userId": "uuid",
  "message": "User registered successfully"
}
```

#### POST /api/auth/login
Authenticate user and get tokens

**Request:**
```json
{
  "emailOrUsername": "user@example.com",
  "password": "SecurePass123!",
  "mfaCode": "123456"
}
```

**Response:** 200 OK
```json
{
  "accessToken": "eyJhbGc...",
  "refreshToken": "base64string",
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "username": "johndoe",
    "firstName": "John",
    "lastName": "Doe",
    "mfaEnabled": false,
    "roles": ["User"],
    "permissions": ["users:read"]
  },
  "requiresMfa": false
}
```

#### POST /api/auth/refresh
Refresh access token

**Request:**
```json
{
  "refreshToken": "base64string"
}
```

**Response:** 200 OK
```json
{
  "accessToken": "eyJhbGc...",
  "refreshToken": "newbase64string"
}
```

#### POST /api/auth/logout
Logout user (requires authentication)

**Headers:** `Authorization: Bearer {token}`

**Response:** 204 No Content

#### GET /api/auth/me
Get current authenticated user

**Headers:** `Authorization: Bearer {token}`

**Response:** 200 OK
```json
{
  "id": "uuid",
  "email": "user@example.com",
  "username": "johndoe",
  "firstName": "John",
  "lastName": "Doe",
  "roles": ["User"],
  "permissions": ["users:read"],
  "mfaEnabled": false,
  "lastLoginAt": "2026-02-12T10:30:00Z"
}
```

---

## 8. SECURITY CONSIDERATIONS

### 8.1 Password Security

- **Hashing:** BCrypt with work factor 12
- **Complexity Requirements:**
  - Minimum 8 characters
  - At least one uppercase letter
  - At least one lowercase letter
  - At least one digit
  - At least one special character
- **Password History:** Store last 5 passwords, prevent reuse
- **Reset Flow:** Secure token-based password reset with expiration

### 8.2 Authentication Security

- **JWT Tokens:**
  - Short-lived access tokens (30 minutes)
  - Long-lived refresh tokens (7 days)
  - Secure token storage
  - Token rotation on refresh
- **MFA:** TOTP-based (Time-based One-Time Password)
- **Session Management:**
  - Track active sessions
  - Allow users to revoke sessions
  - Automatic session expiration

### 8.3 Authorization Security

- **RBAC:** Role-Based Access Control
- **Claims-based:** Additional granular claims
- **Policy-based:** Custom authorization policies
- **Principle of Least Privilege:** Users get minimum necessary permissions

### 8.4 Additional Security Measures

- **Rate Limiting:** Prevent brute force attacks
- **Account Lockout:** 5 failed attempts = 30-minute lockout
- **Audit Logging:** Log all security events
- **HTTPS Only:** Enforce HTTPS in production
- **CORS:** Configure allowed origins
- **SQL Injection Protection:** Parameterized queries
- **XSS Protection:** Input sanitization, output encoding
- **CSRF Protection:** CSRF tokens for state-changing operations

---

## 9. TESTING STRATEGY

### 9.1 Backend Testing

#### Unit Tests (xUnit + Moq)
- Service layer tests (TokenService, PasswordService, MfaService)
- Handler tests (Command/Query handlers)
- Domain model tests

#### Integration Tests (Alba)
- API endpoint tests
- Database integration tests
- Authentication middleware tests

#### Test Coverage Target: 80%+

#### Example Test:

```csharp
[Fact]
public async Task RegisterUser_WithValidData_ReturnsSuccess()
{
    // Arrange
    var command = new RegisterUserCommand(
        "test@example.com",
        "testuser",
        "SecurePass123!",
        "Test",
        "User",
        null
    );

    // Act
    var result = await _handler.Handle(command);

    // Assert
    result.Should().NotBeNull();
    result.UserId.Should().NotBeEmpty();
    result.Message.Should().Be("User registered successfully");
}
```

### 9.2 Frontend Testing

#### Unit Tests (Vitest)
- Utility function tests
- Service layer tests
- Hook tests

#### Component Tests (Testing Library)
- LoginForm component
- RegisterForm component
- ProtectedRoute component
- All other UI components

#### E2E Tests (Playwright)
- Complete login flow
- Registration flow
- MFA setup flow
- Password reset flow

#### Test Coverage Target: 80%+

#### Example Test:

```typescript
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { LoginForm } from './LoginForm';
import { AuthProvider } from '../hooks/useAuth';

describe('LoginForm', () => {
  it('should submit login form with valid credentials', async () => {
    const onSuccess = vi.fn();
    
    render(
      <AuthProvider authService={mockAuthService}>
        <LoginForm onSuccess={onSuccess} />
      </AuthProvider>
    );

    fireEvent.change(screen.getByLabelText(/email or username/i), {
      target: { value: 'test@example.com' },
    });
    fireEvent.change(screen.getByLabelText(/password/i), {
      target: { value: 'SecurePass123!' },
    });

    fireEvent.click(screen.getByRole('button', { name: /sign in/i }));

    await waitFor(() => {
      expect(onSuccess).toHaveBeenCalled();
    });
  });
});
```

---

## 10. DEPLOYMENT & PACKAGING

### 10.1 Backend NuGet Package

#### Build & Publish

```bash
# Build the project
dotnet build backend/CompanyName.Security.Api -c Release

# Pack as NuGet package
dotnet pack backend/CompanyName.Security.Api -c Release -o ./nupkg

# Publish to private NuGet feed
dotnet nuget push ./nupkg/CompanyName.Security.Api.1.0.0.nupkg \
  --source https://your-private-nuget-feed.com/v3/index.json \
  --api-key YOUR_API_KEY
```

#### .csproj Configuration

```xml
<PropertyGroup>
  <PackageId>CompanyName.Security.Api</PackageId>
  <Version>1.0.0</Version>
  <Authors>Your Company</Authors>
  <Description>Security module for authentication, authorization, and audit trail</Description>
  <PackageTags>security;authentication;authorization;jwt;rbac</PackageTags>
  <RepositoryUrl>https://github.com/yourcompany/component-library</RepositoryUrl>
  <PackageLicenseExpression>MIT</PackageLicenseExpression>
</PropertyGroup>
```

### 10.2 Frontend NPM Package

#### Build & Publish

```bash
# Build the package
cd packages/security-module/frontend
pnpm build

# Publish to private NPM registry
pnpm publish --registry https://your-private-npm-registry.com
```

#### Publishing to Verdaccio (Private Registry)

```bash
# Login to registry
npm login --registry https://your-private-npm-registry.com

# Publish
npm publish --registry https://your-private-npm-registry.com
```

---

## 11. INTEGRATION GUIDE - HOW TO USE IN HOST WEBSITE

This section provides complete step-by-step instructions for integrating the Security Module into your host application (website/web app).

### 11.1 Backend Integration in Host API

#### Step 1: Install NuGet Package

```bash
# Navigate to your host API project
cd YourCompany.HostApp.Api

# Install the Security Module NuGet package
dotnet add package CompanyName.Security.Api --version 1.0.0
dotnet add package CompanyName.Security.Data --version 1.0.0
```

#### Step 2: Configure in Program.cs

```csharp
using CompanyName.Security.Api;
using CompanyName.Security.Data;
using Microsoft.EntityFrameworkCore;
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.IdentityModel.Tokens;
using System.Text;

var builder = WebApplication.CreateBuilder(args);

// ============================================
// STEP 1: Add Security Module Database
// ============================================
builder.Services.AddDbContext<SecurityDbContext>(options =>
{
    // Use PostgreSQL
    options.UseNpgsql(
        builder.Configuration.GetConnectionString("DefaultConnection"),
        b => b.MigrationsAssembly("CompanyName.Security.Data")
    );
    
    // OR use SQL Server
    // options.UseSqlServer(
    //     builder.Configuration.GetConnectionString("DefaultConnection"),
    //     b => b.MigrationsAssembly("CompanyName.Security.Data")
    // );
});

// ============================================
// STEP 2: Add JWT Authentication
// ============================================
var jwtSettings = builder.Configuration.GetSection("Jwt");
var secretKey = jwtSettings["Secret"] ?? throw new InvalidOperationException("JWT Secret not configured");

builder.Services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
})
.AddJwtBearer(options =>
{
    options.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuer = true,
        ValidateAudience = true,
        ValidateLifetime = true,
        ValidateIssuerSigningKey = true,
        ValidIssuer = jwtSettings["Issuer"],
        ValidAudience = jwtSettings["Audience"],
        IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(secretKey))
    };
});

// ============================================
// STEP 3: Add Authorization
// ============================================
builder.Services.AddAuthorization(options =>
{
    // Add custom policies
    options.AddPolicy("RequireAdminRole", policy => 
        policy.RequireRole("Administrator"));
    
    options.AddPolicy("RequireUserRole", policy => 
        policy.RequireRole("User"));
    
    // Permission-based policies
    options.AddPolicy("CanManageUsers", policy => 
        policy.RequireClaim("permission", "users:manage"));
});

// ============================================
// STEP 4: Add Security Module Services
// ============================================
builder.Services.AddSecurityModule(builder.Configuration);

// ============================================
// STEP 5: Add Wolverine for CQRS
// ============================================
builder.Host.UseWolverine();

// ============================================
// STEP 6: Add CORS (for frontend)
// ============================================
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowFrontend", policy =>
    {
        policy.WithOrigins("http://localhost:3000", "https://your-production-domain.com")
              .AllowAnyHeader()
              .AllowAnyMethod()
              .AllowCredentials();
    });
});

// Add controllers
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// ============================================
// STEP 7: Run Database Migrations on Startup
// ============================================
using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService<SecurityDbContext>();
    await db.Database.MigrateAsync();
}

// ============================================
// STEP 8: Configure Middleware Pipeline
// ============================================
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseCors("AllowFrontend");

// IMPORTANT: Order matters!
app.UseAuthentication();  // Must come before UseAuthorization
app.UseAuthorization();

app.MapControllers();

app.Run();
```

#### Step 3: Configure appsettings.json

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Database=myapp;Username=postgres;Password=postgres;Port=5432"
  },
  "Jwt": {
    "Secret": "your-super-secret-key-must-be-at-least-32-characters-long",
    "Issuer": "YourCompanyName",
    "Audience": "YourAppUsers",
    "AccessTokenExpirationMinutes": 30
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning",
      "CompanyName.Security": "Debug"
    }
  }
}
```

#### Step 4: Use Security in Your Controllers

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;

namespace YourCompany.HostApp.Api.Controllers;

[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    // Public endpoint - no authentication required
    [HttpGet("public")]
    public IActionResult GetPublicProducts()
    {
        return Ok(new { message = "Public products" });
    }

    // Requires authentication (any logged-in user)
    [Authorize]
    [HttpGet]
    public IActionResult GetProducts()
    {
        var userId = User.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
        var email = User.FindFirst(System.Security.Claims.ClaimTypes.Email)?.Value;
        
        return Ok(new { message = $"Products for {email}" });
    }

    // Requires specific role
    [Authorize(Roles = "Administrator")]
    [HttpPost]
    public IActionResult CreateProduct()
    {
        return Ok(new { message = "Product created by admin" });
    }

    // Requires specific permission
    [Authorize(Policy = "CanManageUsers")]
    [HttpDelete("{id}")]
    public IActionResult DeleteProduct(int id)
    {
        return Ok(new { message = $"Product {id} deleted" });
    }

    // Requires multiple roles
    [Authorize(Roles = "Administrator,Manager")]
    [HttpPut("{id}")]
    public IActionResult UpdateProduct(int id)
    {
        return Ok(new { message = $"Product {id} updated" });
    }
}
```

---

### 11.2 Frontend Integration in Host Website

#### Step 1: Install Required Packages

```bash
# Navigate to your host frontend project
cd your-company-website

# Install the Security Module
pnpm add @company/security-module

# Install peer dependencies
pnpm add @tanstack/react-query axios
```

#### Step 2: Setup Auth Provider in Root Component

```typescript
// src/main.tsx or src/App.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { AuthProvider, AuthService } from '@company/security-module';
import App from './App';
import './index.css';

// ============================================
// STEP 1: Create Query Client
// ============================================
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: 1,
      refetchOnWindowFocus: false,
      staleTime: 5 * 60 * 1000, // 5 minutes
    },
  },
});

// ============================================
// STEP 2: Create Auth Service Instance
// ============================================
const authService = new AuthService(
  import.meta.env.VITE_API_URL || 'http://localhost:5000/api'
);

// ============================================
// STEP 3: Wrap App with Providers
// ============================================
ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <BrowserRouter>
      <QueryClientProvider client={queryClient}>
        <AuthProvider authService={authService}>
          <App />
        </AuthProvider>
      </QueryClientProvider>
    </BrowserRouter>
  </React.StrictMode>
);
```

#### Step 3: Create Login Page

```typescript
// src/pages/LoginPage.tsx
import React from 'react';
import { useNavigate } from 'react-router-dom';
import { LoginForm } from '@company/security-module';

export function LoginPage() {
  const navigate = useNavigate();

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-50">
      <div className="max-w-md w-full">
        <LoginForm
          onSuccess={() => {
            console.log('Login successful');
            navigate('/dashboard');
          }}
          onForgotPassword={() => {
            navigate('/forgot-password');
          }}
          onRegister={() => {
            navigate('/register');
          }}
        />
      </div>
    </div>
  );
}
```

#### Step 4: Create Protected Dashboard

```typescript
// src/pages/Dashboard.tsx
import React from 'react';
import { ProtectedRoute, useAuth } from '@company/security-module';

export function Dashboard() {
  const { user, logout, hasPermission, hasRole } = useAuth();

  return (
    <ProtectedRoute requiredRole="User">
      <div className="min-h-screen bg-gray-100">
        <nav className="bg-white shadow-sm">
          <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div className="flex justify-between h-16">
              <div className="flex items-center">
                <h1 className="text-xl font-bold">My Dashboard</h1>
              </div>
              <div className="flex items-center space-x-4">
                <span>Welcome, {user?.firstName || user?.email}!</span>
                <button
                  onClick={() => logout()}
                  className="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600"
                >
                  Logout
                </button>
              </div>
            </div>
          </div>
        </nav>

        <main className="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
          <div className="px-4 py-6 sm:px-0">
            <div className="bg-white rounded-lg shadow p-6">
              <h2 className="text-2xl font-bold mb-4">User Information</h2>
              <dl className="space-y-2">
                <div>
                  <dt className="font-semibold">Email:</dt>
                  <dd>{user?.email}</dd>
                </div>
                <div>
                  <dt className="font-semibold">Username:</dt>
                  <dd>{user?.username}</dd>
                </div>
                <div>
                  <dt className="font-semibold">Roles:</dt>
                  <dd>{user?.roles.join(', ')}</dd>
                </div>
                <div>
                  <dt className="font-semibold">MFA Enabled:</dt>
                  <dd>{user?.mfaEnabled ? 'Yes' : 'No'}</dd>
                </div>
              </dl>

              {/* Conditional rendering based on permissions */}
              {hasPermission('users:manage') && (
                <div className="mt-6">
                  <a
                    href="/admin/users"
                    className="text-blue-600 hover:underline"
                  >
                    Manage Users →
                  </a>
                </div>
              )}

              {/* Conditional rendering based on role */}
              {hasRole('Administrator') && (
                <div className="mt-4">
                  <a
                    href="/admin"
                    className="text-blue-600 hover:underline"
                  >
                    Admin Panel →
                  </a>
                </div>
              )}
            </div>
          </div>
        </main>
      </div>
    </ProtectedRoute>
  );
}
```

#### Step 5: Setup Routes

```typescript
// src/App.tsx
import React from 'react';
import { Routes, Route, Navigate } from 'react-router-dom';
import { useAuth } from '@company/security-module';
import { LoginPage } from './pages/LoginPage';
import { RegisterPage } from './pages/RegisterPage';
import { Dashboard } from './pages/Dashboard';
import { AdminPanel } from './pages/AdminPanel';

function App() {
  const { isAuthenticated, isLoading } = useAuth();

  if (isLoading) {
    return (
      <div className="min-h-screen flex items-center justify-center">
        <div className="text-xl">Loading...</div>
      </div>
    );
  }

  return (
    <Routes>
      {/* Public routes */}
      <Route
        path="/login"
        element={
          isAuthenticated ? <Navigate to="/dashboard" replace /> : <LoginPage />
        }
      />
      <Route path="/register" element={<RegisterPage />} />
      <Route path="/forgot-password" element={<ForgotPasswordPage />} />

      {/* Protected routes */}
      <Route path="/dashboard" element={<Dashboard />} />
      <Route path="/profile" element={<ProfilePage />} />
      
      {/* Admin routes (requires Administrator role) */}
      <Route path="/admin/*" element={<AdminPanel />} />

      {/* Redirect root to dashboard or login */}
      <Route
        path="/"
        element={
          <Navigate to={isAuthenticated ? '/dashboard' : '/login'} replace />
        }
      />

      {/* 404 route */}
      <Route path="*" element={<NotFoundPage />} />
    </Routes>
  );
}

export default App;
```

#### Step 6: Use Auth Hook in Any Component

```typescript
// src/components/UserMenu.tsx
import React from 'react';
import { useAuth } from '@company/security-module';
import { Menu } from '@headlessui/react';

export function UserMenu() {
  const { user, logout, hasRole } = useAuth();

  if (!user) return null;

  return (
    <Menu as="div" className="relative">
      <Menu.Button className="flex items-center space-x-2">
        <img
          src={user.avatar || '/default-avatar.png'}
          alt={user.fullName}
          className="h-8 w-8 rounded-full"
        />
        <span>{user.firstName}</span>
      </Menu.Button>

      <Menu.Items className="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg">
        <Menu.Item>
          {({ active }) => (
            <a
              href="/profile"
              className={`block px-4 py-2 ${active ? 'bg-gray-100' : ''}`}
            >
              Profile
            </a>
          )}
        </Menu.Item>
        
        <Menu.Item>
          {({ active }) => (
            <a
              href="/settings"
              className={`block px-4 py-2 ${active ? 'bg-gray-100' : ''}`}
            >
              Settings
            </a>
          )}
        </Menu.Item>

        {hasRole('Administrator') && (
          <Menu.Item>
            {({ active }) => (
              <a
                href="/admin"
                className={`block px-4 py-2 ${active ? 'bg-gray-100' : ''}`}
              >
                Admin Panel
              </a>
            )}
          </Menu.Item>
        )}

        <Menu.Item>
          {({ active }) => (
            <button
              onClick={() => logout()}
              className={`block w-full text-left px-4 py-2 ${active ? 'bg-gray-100' : ''}`}
            >
              Logout
            </button>
          )}
        </Menu.Item>
      </Menu.Items>
    </Menu>
  );
}
```

#### Step 7: Environment Variables

```bash
# .env.development
VITE_API_URL=http://localhost:5000/api

# .env.production
VITE_API_URL=https://api.yourcompany.com/api
```

---

### 11.3 Complete Integration Example

Here's a complete example showing how all pieces work together:

```typescript
// src/pages/AdminUsersPage.tsx
import React from 'react';
import { ProtectedRoute, useAuth } from '@company/security-module';
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import axios from 'axios';

export function AdminUsersPage() {
  const { hasPermission } = useAuth();
  const queryClient = useQueryClient();

  // Fetch users
  const { data: users, isLoading } = useQuery({
    queryKey: ['users'],
    queryFn: async () => {
      const response = await axios.get('/api/users');
      return response.data;
    },
  });

  // Delete user mutation
  const deleteMutation = useMutation({
    mutationFn: (userId: string) => axios.delete(`/api/users/${userId}`),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });

  return (
    <ProtectedRoute 
      requiredPermission="users:manage"
      fallback={<div>Access Denied</div>}
    >
      <div className="max-w-7xl mx-auto py-6">
        <h1 className="text-3xl font-bold mb-6">User Management</h1>

        {isLoading ? (
          <div>Loading users...</div>
        ) : (
          <div className="bg-white shadow rounded-lg">
            <table className="min-w-full">
              <thead className="bg-gray-50">
                <tr>
                  <th className="px-6 py-3 text-left">Email</th>
                  <th className="px-6 py-3 text-left">Username</th>
                  <th className="px-6 py-3 text-left">Roles</th>
                  <th className="px-6 py-3 text-left">Status</th>
                  {hasPermission('users:delete') && (
                    <th className="px-6 py-3 text-left">Actions</th>
                  )}
                </tr>
              </thead>
              <tbody className="divide-y divide-gray-200">
                {users?.map((user: any) => (
                  <tr key={user.id}>
                    <td className="px-6 py-4">{user.email}</td>
                    <td className="px-6 py-4">{user.username}</td>
                    <td className="px-6 py-4">{user.roles.join(', ')}</td>
                    <td className="px-6 py-4">
                      <span
                        className={`px-2 py-1 rounded ${
                          user.isActive
                            ? 'bg-green-100 text-green-800'
                            : 'bg-red-100 text-red-800'
                        }`}
                      >
                        {user.isActive ? 'Active' : 'Inactive'}
                      </span>
                    </td>
                    {hasPermission('users:delete') && (
                      <td className="px-6 py-4">
                        <button
                          onClick={() => deleteMutation.mutate(user.id)}
                          className="text-red-600 hover:text-red-800"
                        >
                          Delete
                        </button>
                      </td>
                    )}
                  </tr>
                ))}
              </tbody>
            </table>
          </div>
        )}
      </div>
    </ProtectedRoute>
  );
}
```

---

### 11.4 Quick Start Summary

**For Backend Developers:**
1. Install NuGet package: `CompanyName.Security.Api`
2. Configure JWT authentication in `Program.cs`
3. Add `[Authorize]` attribute to protected endpoints
4. Use `User.FindFirst()` to get current user info

**For Frontend Developers:**
1. Install NPM package: `@company/security-module`
2. Wrap app with `<AuthProvider>`
3. Use `<LoginForm>` for login page
4. Use `<ProtectedRoute>` to guard routes
5. Use `useAuth()` hook to access user info and permissions

**Key Benefits:**
- ✅ Drop-in authentication/authorization for any app
- ✅ Fully typed with TypeScript
- ✅ Role and permission-based access control
- ✅ Works with Tailwind, MUI, or Ant Design
- ✅ Production-ready security features (MFA, audit trail, etc.)

---

### 11.5 Demo Applications (Three Separate Apps)

To demonstrate the Security Module's framework-agnostic capabilities, three separate demo applications are created, each using a different UI framework.

#### 11.5.1 Demo Application Structure

```
apps/
├── demo-tailwind/           # Tailwind CSS demo
│   ├── src/
│   │   ├── pages/
│   │   │   ├── LoginPage.tsx
│   │   │   ├── RegisterPage.tsx
│   │   │   ├── DashboardPage.tsx
│   │   │   ├── ProfilePage.tsx
│   │   │   └── AdminPage.tsx
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── package.json         # Only Tailwind CSS installed
│   ├── tailwind.config.js
│   ├── vite.config.ts
│   └── README.md
│
├── demo-mui/                # Material-UI demo
│   ├── src/
│   │   ├── pages/
│   │   │   ├── LoginPage.tsx
│   │   │   ├── RegisterPage.tsx
│   │   │   ├── DashboardPage.tsx
│   │   │   ├── ProfilePage.tsx
│   │   │   └── AdminPage.tsx
│   │   ├── theme.ts         # MUI theme customization
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── package.json         # Only MUI installed
│   ├── vite.config.ts
│   └── README.md
│
├── demo-ant-design/         # Ant Design demo
│   ├── src/
│   │   ├── pages/
│   │   │   ├── LoginPage.tsx
│   │   │   ├── RegisterPage.tsx
│   │   │   ├── DashboardPage.tsx
│   │   │   ├── ProfilePage.tsx
│   │   │   └── AdminPage.tsx
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── package.json         # Only Ant Design installed
│   ├── vite.config.ts
│   └── README.md
│
└── demo-showcase/           # Landing page (optional)
    ├── src/
    │   ├── App.tsx          # Links to all three demos
    │   └── main.tsx
    ├── package.json
    └── README.md
```

#### 11.5.2 Demo Tailwind (apps/demo-tailwind)

**package.json:**
```json
{
  "name": "@reachdigital/demo-tailwind",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "react-router-dom": "^7.1.0",
    "@tanstack/react-query": "^5.28.0",
    "@reachdigital/security-module": "workspace:*",
    "tailwindcss": "^3.4.1",
    "autoprefixer": "^10.4.18",
    "postcss": "^8.4.35"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.2.1",
    "typescript": "^5.4.2",
    "vite": "^5.1.5"
  },
  "scripts": {
    "dev": "vite --port 3001",
    "build": "tsc && vite build",
    "preview": "vite preview"
  }
}
```

**src/App.tsx:**
```typescript
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { AuthProvider, AuthService } from '@reachdigital/security-module';
import { LoginPage } from './pages/LoginPage';
import { RegisterPage } from './pages/RegisterPage';
import { DashboardPage } from './pages/DashboardPage';
import { ProfilePage } from './pages/ProfilePage';
import { AdminPage } from './pages/AdminPage';
import './index.css';

const queryClient = new QueryClient();
const authService = new AuthService(import.meta.env.VITE_API_URL);

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <AuthProvider authService={authService}>
        <BrowserRouter>
          <Routes>
            <Route path="/login" element={<LoginPage />} />
            <Route path="/register" element={<RegisterPage />} />
            <Route path="/dashboard" element={<DashboardPage />} />
            <Route path="/profile" element={<ProfilePage />} />
            <Route path="/admin" element={<AdminPage />} />
            <Route path="/" element={<Navigate to="/dashboard" replace />} />
          </Routes>
        </BrowserRouter>
      </AuthProvider>
    </QueryClientProvider>
  );
}
```

**src/pages/LoginPage.tsx:**
```typescript
import { useNavigate } from 'react-router-dom';
import { LoginForm } from '@reachdigital/security-module';

export function LoginPage() {
  const navigate = useNavigate();

  return (
    <div className="min-h-screen bg-gray-50 flex items-center justify-center py-12 px-4">
      <div className="w-full max-w-md">
        <div className="text-center mb-8">
          <h1 className="text-3xl font-bold text-gray-900">Security Module Demo</h1>
          <p className="text-gray-600 mt-2">Powered by Tailwind CSS</p>
        </div>
        
        <LoginForm
          framework="tailwind"
          onSuccess={() => navigate('/dashboard')}
          onRegister={() => navigate('/register')}
        />
      </div>
    </div>
  );
}
```

**Expected Bundle Size:** ~150 KB (gzipped)

#### 11.5.3 Demo Material-UI (apps/demo-mui)

**package.json:**
```json
{
  "name": "@reachdigital/demo-mui",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "react-router-dom": "^7.1.0",
    "@tanstack/react-query": "^5.28.0",
    "@reachdigital/security-module": "workspace:*",
    "@mui/material": "^5.15.0",
    "@mui/icons-material": "^5.15.0",
    "@emotion/react": "^11.11.0",
    "@emotion/styled": "^11.11.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.2.1",
    "typescript": "^5.4.2",
    "vite": "^5.1.5"
  },
  "scripts": {
    "dev": "vite --port 3002",
    "build": "tsc && vite build",
    "preview": "vite preview"
  }
}
```

**src/theme.ts:**
```typescript
import { createTheme } from '@mui/material/styles';

export const theme = createTheme({
  palette: {
    primary: {
      main: '#1976d2',
    },
    secondary: {
      main: '#dc004e',
    },
  },
  typography: {
    fontFamily: '"Roboto", "Helvetica", "Arial", sans-serif',
  },
});
```

**src/App.tsx:**
```typescript
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ThemeProvider } from '@mui/material/styles';
import { CssBaseline } from '@mui/material';
import { AuthProvider, AuthService } from '@reachdigital/security-module';
import { theme } from './theme';
import { LoginPage } from './pages/LoginPage';
// ... other imports

const queryClient = new QueryClient();
const authService = new AuthService(import.meta.env.VITE_API_URL);

export default function App() {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      <QueryClientProvider client={queryClient}>
        <AuthProvider authService={authService}>
          <BrowserRouter>
            <Routes>
              <Route path="/login" element={<LoginPage />} />
              {/* ... other routes */}
            </Routes>
          </BrowserRouter>
        </AuthProvider>
      </QueryClientProvider>
    </ThemeProvider>
  );
}
```

**src/pages/LoginPage.tsx:**
```typescript
import { useNavigate } from 'react-router-dom';
import { Container, Box, Typography } from '@mui/material';
import { LoginForm } from '@reachdigital/security-module';

export function LoginPage() {
  const navigate = useNavigate();

  return (
    <Container maxWidth="sm">
      <Box sx={{ minHeight: '100vh', display: 'flex', flexDirection: 'column', justifyContent: 'center', py: 6 }}>
        <Box sx={{ textAlign: 'center', mb: 4 }}>
          <Typography variant="h3" component="h1" gutterBottom>
            Security Module Demo
          </Typography>
          <Typography variant="body1" color="text.secondary">
            Powered by Material-UI
          </Typography>
        </Box>
        
        <LoginForm
          framework="mui"
          onSuccess={() => navigate('/dashboard')}
          onRegister={() => navigate('/register')}
        />
      </Box>
    </Container>
  );
}
```

**Expected Bundle Size:** ~350 KB (gzipped)

#### 11.5.4 Demo Ant Design (apps/demo-ant-design)

**package.json:**
```json
{
  "name": "@reachdigital/demo-ant-design",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "react-router-dom": "^7.1.0",
    "@tanstack/react-query": "^5.28.0",
    "@reachdigital/security-module": "workspace:*",
    "antd": "^5.15.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.2.1",
    "typescript": "^5.4.2",
    "vite": "^5.1.5"
  },
  "scripts": {
    "dev": "vite --port 3003",
    "build": "tsc && vite build",
    "preview": "vite preview"
  }
}
```

**src/App.tsx:**
```typescript
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ConfigProvider } from 'antd';
import { AuthProvider, AuthService } from '@reachdigital/security-module';
import { LoginPage } from './pages/LoginPage';
// ... other imports

const queryClient = new QueryClient();
const authService = new AuthService(import.meta.env.VITE_API_URL);

export default function App() {
  return (
    <ConfigProvider theme={{ token: { colorPrimary: '#1890ff' } }}>
      <QueryClientProvider client={queryClient}>
        <AuthProvider authService={authService}>
          <BrowserRouter>
            <Routes>
              <Route path="/login" element={<LoginPage />} />
              {/* ... other routes */}
            </Routes>
          </BrowserRouter>
        </AuthProvider>
      </QueryClientProvider>
    </ConfigProvider>
  );
}
```

**src/pages/LoginPage.tsx:**
```typescript
import { useNavigate } from 'react-router-dom';
import { Layout, Typography } from 'antd';
import { LoginForm } from '@reachdigital/security-module';

const { Content } = Layout;
const { Title, Paragraph } = Typography;

export function LoginPage() {
  const navigate = useNavigate();

  return (
    <Layout style={{ minHeight: '100vh', background: '#f0f2f5' }}>
      <Content style={{ display: 'flex', alignItems: 'center', justifyContent: 'center', padding: '50px' }}>
        <div style={{ maxWidth: '500px', width: '100%' }}>
          <div style={{ textAlign: 'center', marginBottom: '32px' }}>
            <Title level={2}>Security Module Demo</Title>
            <Paragraph type="secondary">Powered by Ant Design</Paragraph>
          </div>
          
          <LoginForm
            framework="ant"
            onSuccess={() => navigate('/dashboard')}
            onRegister={() => navigate('/register')}
          />
        </div>
      </Content>
    </Layout>
  );
}
```

**Expected Bundle Size:** ~450 KB (gzipped)

#### 11.5.5 Showcase Website (apps/demo-showcase) - Optional

A simple landing page that links to all three demos:

```typescript
// src/App.tsx
export default function App() {
  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-500 to-purple-600 flex items-center justify-center p-8">
      <div className="max-w-4xl w-full bg-white rounded-2xl shadow-2xl p-12">
        <div className="text-center mb-12">
          <h1 className="text-5xl font-bold text-gray-900 mb-4">
            Security Module Demos
          </h1>
          <p className="text-xl text-gray-600">
            See the same module working with three different UI frameworks
          </p>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-3 gap-8 mb-12">
          {/* Tailwind Demo */}
          <a href="http://localhost:3001" target="_blank" className="block p-8 bg-blue-50 rounded-xl hover:shadow-lg transition">
            <div className="text-4xl mb-4">🎨</div>
            <h3 className="text-2xl font-bold text-gray-900 mb-2">Tailwind CSS</h3>
            <p className="text-gray-600 mb-4">Utility-first CSS framework</p>
            <p className="text-sm text-blue-600 font-semibold">Bundle: ~150 KB</p>
          </a>

          {/* MUI Demo */}
          <a href="http://localhost:3002" target="_blank" className="block p-8 bg-purple-50 rounded-xl hover:shadow-lg transition">
            <div className="text-4xl mb-4">⚛️</div>
            <h3 className="text-2xl font-bold text-gray-900 mb-2">Material-UI</h3>
            <p className="text-gray-600 mb-4">Material Design components</p>
            <p className="text-sm text-purple-600 font-semibold">Bundle: ~350 KB</p>
          </a>

          {/* Ant Design Demo */}
          <a href="http://localhost:3003" target="_blank" className="block p-8 bg-green-50 rounded-xl hover:shadow-lg transition">
            <div className="text-4xl mb-4">🐜</div>
            <h3 className="text-2xl font-bold text-gray-900 mb-2">Ant Design</h3>
            <p className="text-gray-600 mb-4">Enterprise-class UI design</p>
            <p className="text-sm text-green-600 font-semibold">Bundle: ~450 KB</p>
          </a>
        </div>

        <div className="border-t pt-8">
          <h3 className="text-2xl font-bold text-gray-900 mb-4">Key Features</h3>
          <ul className="grid grid-cols-1 md:grid-cols-2 gap-4 text-gray-700">
            <li className="flex items-start">
              <span className="text-green-600 mr-2">✓</span>
              One Security Module, three frameworks
            </li>
            <li className="flex items-start">
              <span className="text-green-600 mr-2">✓</span>
              No framework dependencies in module
            </li>
            <li className="flex items-start">
              <span className="text-green-600 mr-2">✓</span>
              Optimized bundle sizes
            </li>
            <li className="flex items-start">
              <span className="text-green-600 mr-2">✓</span>
              CSS-only adapter pattern
            </li>
          </ul>
        </div>
      </div>
    </div>
  );
}
```

#### 11.5.6 Monorepo Scripts

Update root `package.json`:

```json
{
  "scripts": {
    "demo:tailwind": "pnpm --filter @reachdigital/demo-tailwind dev",
    "demo:mui": "pnpm --filter @reachdigital/demo-mui dev",
    "demo:ant": "pnpm --filter @reachdigital/demo-ant-design dev",
    "demo:showcase": "pnpm --filter @reachdigital/demo-showcase dev",
    "demo:all": "concurrently \"pnpm demo:tailwind\" \"pnpm demo:mui\" \"pnpm demo:ant\"",
    "build:demos": "pnpm --filter './apps/demo-*' build"
  }
}
```

#### 11.5.7 Deployment Strategy

Each demo should be deployed separately:

| Demo | URL | Platform |
|------|-----|----------|
| Tailwind | https://demo-tailwind.yourcompany.com | Vercel/Netlify |
| Material-UI | https://demo-mui.yourcompany.com | Vercel/Netlify |
| Ant Design | https://demo-ant.yourcompany.com | Vercel/Netlify |
| Showcase | https://demos.yourcompany.com | Vercel/Netlify |

**Vercel Deployment (per demo):**
```bash
cd apps/demo-tailwind
vercel --prod
```

---

## 12. TIMELINE & MILESTONES

### 12.1 Week-by-Week Breakdown

#### **Week 1-2: Backend Foundation (Month 2, Week 1-2)**
- ✅ Day 1-2: Project setup, solution structure, NuGet packages
- ✅ Day 3-4: Domain models (User, Role, Permission, Session, etc.)
- ✅ Day 5-7: Database context, migrations, seed data
- ✅ Day 8-10: Core services (TokenService, PasswordService, MfaService)

#### **Week 3-4: Backend Core Features (Month 2, Week 3-4)**
- ✅ Day 11-13: User registration (command, handler, validation)
- ✅ Day 14-16: User login (authentication, JWT generation, MFA)
- ✅ Day 17-18: Token refresh mechanism
- ✅ Day 19-20: Password reset flow
- ✅ Day 21-22: Active Directory integration

#### **Week 5-6: Backend Authorization & Audit (Month 3, Week 1-2)**
- ✅ Day 23-25: Role management (CRUD operations)
- ✅ Day 26-28: Permission management
- ✅ Day 29-30: Authorization middleware (policy-based)
- ✅ Day 31-32: Session management
- ✅ Day 33-34: Audit logging system

#### **Week 7-8: Frontend Setup & Core Components (Month 3, Week 3-4)**
- ✅ Day 35-36: Frontend project setup, package.json, build config
- ✅ Day 37-39: TypeScript types, API service layer
- ✅ Day 40-42: Auth context, hooks (useAuth, usePermissions)
- ✅ Day 43-45: LoginForm component
- ✅ Day 46-48: RegisterForm component
- ✅ Day 49-50: ProtectedRoute component

#### **Week 9-10: Frontend Advanced Features (Month 3, Week 5-6)**
- ✅ Day 51-53: UserProfile component
- ✅ Day 54-56: MFA setup component (QR code, verification)
- ✅ Day 57-59: Password reset flow components
- ✅ Day 60-61: Role & permission management UI
- ✅ Day 62-63: Audit log viewer component
- ✅ Day 64: Session management UI

#### **Week 11: Testing (Month 3, Week 7)**
- ✅ Day 65-67: Backend unit tests (services, handlers)
- ✅ Day 68-69: Backend integration tests (API endpoints)
- ✅ Day 70-71: Frontend unit tests (hooks, utils)

#### **Week 12: Testing & Documentation (Month 3, Week 8)**
- ✅ Day 72-74: Frontend component tests
- ✅ Day 75-77: E2E tests (Playwright)
- ✅ Day 78-79: API documentation (Swagger/OpenAPI)
- ✅ Day 80-81: Integration guide documentation
- ✅ Day 82-84: Demo applications (all 3 UI frameworks) - **UPDATED APPROACH**

**Day 82-84: Multi-Framework Demo Implementation**

This section has been updated to reflect the optimal architecture: **CSS-only adapters + Three separate demo applications**.

**Day 82: Framework Adapters (CSS-Only)**
- Create `packages/security-module/frontend/src/adapters/index.ts`
  - Export adapter selection function
  - Type definitions for framework prop
- Create `packages/security-module/frontend/src/adapters/tailwind.ts`
  - CSS class mappings for all components (button, input, overlay, dialog, etc.)
  - No Tailwind runtime dependency - just class strings
- Create `packages/security-module/frontend/src/adapters/mui.ts`
  - Material-UI CSS class mappings
  - MuiButton-root, MuiInput-root, etc. (CSS classes only)
- Create `packages/security-module/frontend/src/adapters/ant-design.ts`
  - Ant Design CSS class mappings
  - ant-btn, ant-input, etc. (CSS classes only)
- Update all components to accept `framework?: 'tailwind' | 'mui' | 'ant'` prop
- Ensure Security Module has ZERO UI framework dependencies in package.json

**Day 83: Tailwind & MUI Demo Applications**
- Create `apps/demo-tailwind/`
  - Initialize Vite + React + TypeScript project
  - Install `tailwindcss` and `@reachdigital/security-module`
  - Configure Tailwind CSS (tailwind.config.js, postcss.config.js)
  - Create demo pages: Login, Register, Dashboard, Profile, Admin
  - Use all Security Module components with `framework="tailwind"`
  - Add navigation, layout, and styling showcase
  - Bundle size target: ~150 KB
- Create `apps/demo-mui/`
  - Initialize Vite + React + TypeScript project
  - Install `@mui/material`, `@emotion/react`, `@emotion/styled`
  - Install `@reachdigital/security-module`
  - Configure MUI ThemeProvider with custom theme
  - Create demo pages: Login, Register, Dashboard, Profile, Admin
  - Use all Security Module components with `framework="mui"`
  - Add MUI-styled navigation and layout
  - Bundle size target: ~350 KB

**Day 84: Ant Design Demo + Showcase Website**
- Create `apps/demo-ant-design/`
  - Initialize Vite + React + TypeScript project
  - Install `antd` and `@reachdigital/security-module`
  - Configure Ant Design theming (customize less/css variables)
  - Create demo pages: Login, Register, Dashboard, Profile, Admin
  - Use all Security Module components with `framework="ant"`
  - Add Ant Design navigation and layout
  - Bundle size target: ~450 KB
- Create `apps/demo-showcase/` (Optional but recommended)
  - Simple landing page with 3 CTA buttons
  - "View Tailwind Demo" → demo-tailwind URL
  - "View Material-UI Demo" → demo-mui URL
  - "View Ant Design Demo" → demo-ant-design URL
  - Side-by-side screenshots/videos of all three demos
  - Architecture explanation and integration code examples
- Update monorepo scripts in root `package.json`:
  - `pnpm demo:tailwind` - Run Tailwind demo
  - `pnpm demo:mui` - Run MUI demo
  - `pnpm demo:ant` - Run Ant Design demo
  - `pnpm demo:all` - Run all demos concurrently
- Document deployment strategy (3 separate Vercel/Netlify deployments)

### 12.2 Key Milestones

| Milestone | Target Date | Deliverables |
|-----------|-------------|--------------|
| **M1: Backend Foundation Complete** | End of Week 2 | Domain models, database, core services |
| **M2: Authentication Working** | End of Week 4 | Login, register, token refresh, MFA |
| **M3: Authorization & Audit Complete** | End of Week 6 | RBAC, permissions, audit trail |
| **M4: Frontend Core Ready** | End of Week 8 | Auth context, login/register components |
| **M5: Frontend Advanced Features** | End of Week 10 | MFA, profile, role management UI |
| **M6: Testing Complete** | End of Week 11 | 80%+ test coverage achieved |
| **M7: Security Module Launch** | End of Week 12 | Packages published, docs complete, demo ready |

### 12.3 Success Criteria Checklist

**Backend:**
- [ ] All API endpoints implemented and documented
- [ ] 80%+ test coverage
- [ ] Security audit passed
- [ ] Performance benchmarks met (< 200ms response time)
- [ ] NuGet package published

**Frontend:**
- [ ] All components implemented with proper styling
- [ ] 80%+ test coverage
- [ ] Accessibility standards met (WCAG 2.1 AA)
- [ ] NPM package published
- [ ] Framework adapters created (Tailwind, MUI, Ant Design) - CSS-only, no runtime dependencies
- [ ] Security Module has ZERO UI framework dependencies (truly framework-agnostic)

**Integration:**
- [ ] Three separate demo applications working (demo-tailwind, demo-mui, demo-ant-design)
- [ ] Each demo uses only ONE UI framework (optimized bundle sizes)
- [ ] Bundle size targets met: Tailwind ~150KB, MUI ~350KB, Ant Design ~450KB
- [ ] Showcase website created with links to all three demos
- [ ] Documentation complete (API docs, integration guide, framework adapter guide)
- [ ] Developer training conducted
- [ ] Successful integration examples for each framework

---

## NEXT STEPS

1. **Review & Approve** this implementation plan
2. **Set up development environment** (repositories, CI/CD, registries)
3. **Assign development resources** (backend developer, frontend developer)
4. **Begin Week 1** with backend foundation setup
5. **Schedule weekly sync meetings** to track progress

---

**Document Version:** 1.0  
**Last Updated:** February 12, 2026  
**Next Review:** Weekly during development

