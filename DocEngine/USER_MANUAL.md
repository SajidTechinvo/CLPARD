# DocEngine — Comprehensive User Manual

> **Version:** 1.0
> **Last Updated:** March 2026
> **Platform:** Web Application (Desktop & Mobile Responsive)

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Getting Started](#2-getting-started)
   - 2.1 [System Requirements](#21-system-requirements)
   - 2.2 [Creating an Account (Signup)](#22-creating-an-account-signup)
   - 2.3 [Email Verification](#23-email-verification)
   - 2.4 [Logging In](#24-logging-in)
   - 2.5 [Forgot Password](#25-forgot-password)
   - 2.6 [Resetting Your Password](#26-resetting-your-password)
3. [Navigating the Interface](#3-navigating-the-interface)
   - 3.1 [Sidebar Navigation](#31-sidebar-navigation)
   - 3.2 [Top Header Bar](#32-top-header-bar)
   - 3.3 [Workspace Switcher](#33-workspace-switcher)
   - 3.4 [Dark Mode](#34-dark-mode)
   - 3.5 [Language & Localization](#35-language--localization)
4. [Dashboard](#4-dashboard)
   - 4.1 [Admin Dashboard](#41-admin-dashboard)
   - 4.2 [Manager Dashboard](#42-manager-dashboard)
   - 4.3 [Member Dashboard](#43-member-dashboard)
5. [Organization Management](#5-organization-management)
   - 5.1 [Organization Overview](#51-organization-overview)
   - 5.2 [Updating Organization Settings](#52-updating-organization-settings)
   - 5.3 [Organization Logo](#53-organization-logo)
6. [Workspace Management](#6-workspace-management)
   - 6.1 [Understanding Workspaces](#61-understanding-workspaces)
   - 6.2 [Workspace Settings](#62-workspace-settings)
   - 6.3 [Workspace Members & Invitations](#63-workspace-members--invitations)
   - 6.4 [Accepting a Workspace Invitation](#64-accepting-a-workspace-invitation)
   - 6.5 [Workspace Roles & Permissions](#65-workspace-roles--permissions)
7. [Project Management](#7-project-management)
   - 7.1 [Projects Overview](#71-projects-overview)
   - 7.2 [Creating a New Project](#72-creating-a-new-project)
   - 7.3 [Editing a Project](#73-editing-a-project)
   - 7.4 [Project Detail Page](#74-project-detail-page)
   - 7.5 [Managing Project Members](#75-managing-project-members)
   - 7.6 [Duplicating a Project](#76-duplicating-a-project)
   - 7.7 [Deleting a Project](#77-deleting-a-project)
   - 7.8 [Project Statuses](#78-project-statuses)
   - 7.9 [Project Views](#79-project-views)
8. [Document Management](#8-document-management)
   - 8.1 [Document Library](#81-document-library)
   - 8.2 [Uploading Documents](#82-uploading-documents)
   - 8.3 [Searching & Filtering Documents](#83-searching--filtering-documents)
   - 8.4 [Viewing Documents](#84-viewing-documents)
   - 8.5 [Editing Documents](#85-editing-documents)
   - 8.6 [Document Version History](#86-document-version-history)
   - 8.7 [Restoring a Previous Version](#87-restoring-a-previous-version)
   - 8.8 [Comparing Document Versions](#88-comparing-document-versions)
   - 8.9 [Document Collaborators](#89-document-collaborators)
   - 8.10 [Sharing Documents via Link](#810-sharing-documents-via-link)
   - 8.11 [Downloading Documents](#811-downloading-documents)
   - 8.12 [Duplicating Documents](#812-duplicating-documents)
   - 8.13 [Deleting Documents](#813-deleting-documents)
   - 8.14 [Bulk Document Operations](#814-bulk-document-operations)
9. [AI Document Generation (AI Brain)](#9-ai-document-generation-ai-brain)
   - 9.1 [Overview of AI Generation](#91-overview-of-ai-generation)
   - 9.2 [Generation Pipeline](#92-generation-pipeline)
   - 9.3 [Starting a New Generation Session](#93-starting-a-new-generation-session)
   - 9.4 [Quality Analysis & Quality Gates](#94-quality-analysis--quality-gates)
   - 9.5 [Monitoring Generation Progress](#95-monitoring-generation-progress)
   - 9.6 [Tech Stack Configuration](#96-tech-stack-configuration)
   - 9.7 [Multi-Document Upload & Generation](#97-multi-document-upload--generation)
   - 9.8 [Selective Document Generation](#98-selective-document-generation)
   - 9.9 [Generation Sessions & History](#99-generation-sessions--history)
   - 9.10 [Checkpoint Recovery](#910-checkpoint-recovery)
   - 9.11 [Generation Budget & Cost Tracking](#911-generation-budget--cost-tracking)
10. [Reviewing & Approving AI-Generated Documents](#10-reviewing--approving-ai-generated-documents)
    - 10.1 [Document Review Workflow](#101-document-review-workflow)
    - 10.2 [Approving a Document](#102-approving-a-document)
    - 10.3 [Rejecting a Document](#103-rejecting-a-document)
    - 10.4 [Requesting Changes](#104-requesting-changes)
    - 10.5 [Regenerating Document Sections](#105-regenerating-document-sections)
    - 10.6 [Full Document Regeneration](#106-full-document-regeneration)
    - 10.7 [Document Traceability](#107-document-traceability)
11. [Document Editing & Collaboration](#11-document-editing--collaboration)
    - 11.1 [The Document Editor](#111-the-document-editor)
    - 11.2 [Section-Level Editing](#112-section-level-editing)
    - 11.3 [Real-Time Collaboration](#113-real-time-collaboration)
    - 11.4 [Comments & Annotations](#114-comments--annotations)
    - 11.5 [Exporting Documents](#115-exporting-documents)
12. [Document Conversion](#12-document-conversion)
    - 12.1 [Converting Epics to Milestones](#121-converting-epics-to-milestones)
    - 12.2 [Converting User Stories to Tasks](#122-converting-user-stories-to-tasks)
    - 12.3 [Format Conversions](#123-format-conversions)
13. [GraphRAG & Knowledge Graph](#13-graphrag--knowledge-graph)
    - 13.1 [What is GraphRAG?](#131-what-is-graphrag)
    - 13.2 [Building a Knowledge Graph](#132-building-a-knowledge-graph)
    - 13.3 [Querying the Knowledge Graph](#133-querying-the-knowledge-graph)
    - 13.4 [Cross-Project Intelligence](#134-cross-project-intelligence)
14. [Search & Discovery](#14-search--discovery)
    - 14.1 [System-Wide Search](#141-system-wide-search)
    - 14.2 [Search Suggestions](#142-search-suggestions)
    - 14.3 [Recent Activity](#143-recent-activity)
15. [Notifications](#15-notifications)
    - 15.1 [Notification Center](#151-notification-center)
    - 15.2 [Real-Time Notifications](#152-real-time-notifications)
    - 15.3 [Email Notifications](#153-email-notifications)
    - 15.4 [Notification Preferences](#154-notification-preferences)
    - 15.5 [Managing Notifications](#155-managing-notifications)
16. [User Profile](#16-user-profile)
    - 16.1 [Viewing Your Profile](#161-viewing-your-profile)
    - 16.2 [Editing Your Profile](#162-editing-your-profile)
    - 16.3 [Profile Photo](#163-profile-photo)
    - 16.4 [Profile Visibility](#164-profile-visibility)
    - 16.5 [Changing Your Password](#165-changing-your-password)
17. [Settings & Configuration](#17-settings--configuration)
    - 17.1 [Settings Hub Overview](#171-settings-hub-overview)
    - 17.2 [General Settings](#172-general-settings)
    - 17.3 [Organization Settings](#173-organization-settings)
    - 17.4 [Security Settings](#174-security-settings)
    - 17.5 [AI Usage & Analytics](#175-ai-usage--analytics)
    - 17.6 [LLM Provider Configuration](#176-llm-provider-configuration)
    - 17.7 [Admin Controls for AI](#177-admin-controls-for-ai)
    - 17.8 [Audit Logs](#178-audit-logs)
18. [Subscription & Billing](#18-subscription--billing)
    - 18.1 [Subscription Plans Overview](#181-subscription-plans-overview)
    - 18.2 [Viewing Your Current Subscription](#182-viewing-your-current-subscription)
    - 18.3 [Upgrading Your Plan](#183-upgrading-your-plan)
    - 18.4 [Billing Cycles](#184-billing-cycles)
    - 18.5 [Usage Monitoring](#185-usage-monitoring)
    - 18.6 [Payment & Invoices](#186-payment--invoices)
19. [Permissions & Access Control](#19-permissions--access-control)
    - 19.1 [Role Hierarchy](#191-role-hierarchy)
    - 19.2 [Organization Roles](#192-organization-roles)
    - 19.3 [Workspace Roles](#193-workspace-roles)
    - 19.4 [Project Roles](#194-project-roles)
    - 19.5 [Document Access Levels](#195-document-access-levels)
    - 19.6 [Custom Roles](#196-custom-roles)
20. [Frequently Asked Questions (FAQ)](#20-frequently-asked-questions-faq)
21. [Glossary](#21-glossary)

---

## 1. Introduction

**DocEngine** is an enterprise-grade, AI-powered document generation and project management platform. It enables teams to automatically generate comprehensive software project documentation from a single Business Requirements Document (BRD), leveraging advanced AI models to produce a full suite of technical documents.

### What DocEngine Does

- **AI-Powered Document Generation**: Upload a BRD and automatically generate a complete document suite — SRD (System Requirements Document), FRS (Functional Requirements Specification), Epics, User Stories, and Tasks.
- **Document Management**: Upload, organize, version-control, and collaborate on documents with full audit trails.
- **Quality Analysis**: Automatically analyze BRD quality before generation, with configurable quality gates.
- **Real-Time Collaboration**: Work simultaneously with team members on documents with live cursor tracking and instant updates.
- **Multi-Tenant Architecture**: Manage multiple organizations, workspaces, and projects with granular role-based access control.
- **Subscription Management**: Choose from flexible pricing tiers with integrated Stripe billing.
- **Knowledge Graphs**: Build and query knowledge graphs from generated documents using GraphRAG technology.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Organization** | The top-level tenant. Each user belongs to at least one organization. Subscriptions are managed at the organization level. |
| **Workspace** | A container within an organization. Projects and members are organized within workspaces. |
| **Project** | A logical grouping for documents and AI generation sessions. Contains its own members, settings, and document library. |
| **Generation Session** | A single AI generation run that takes input documents and produces output documents through the pipeline. |
| **Quality Gate** | An automated quality check performed on input documents before AI generation proceeds. |

---

## 2. Getting Started

### 2.1 System Requirements

DocEngine is a web-based application accessible from any modern browser:

| Browser | Minimum Version |
|---------|----------------|
| Google Chrome | 90+ |
| Mozilla Firefox | 90+ |
| Microsoft Edge | 90+ |
| Safari | 14+ |

- **Screen Resolution**: 1280 x 720 or higher recommended
- **Internet Connection**: Stable broadband connection required
- **JavaScript**: Must be enabled

### 2.2 Creating an Account (Signup)

DocEngine uses a guided 3-step signup process:

#### Step 1 — Email Verification

1. Navigate to the DocEngine login page.
2. Click **"Sign Up"** or **"Get Started"**.
3. Enter your **email address**.
4. Click **"Send Verification Code"**.
5. A one-time password (OTP) will be sent to your email.
6. Enter the **6-digit OTP** code in the verification field.
7. Click **"Verify"** to proceed.

> **Note:** The verification code expires after a set period. If it expires, click **"Resend Code"** to receive a new one. You have a limited number of attempts before a cooldown period is enforced.

#### Step 2 — Organization & Workspace Setup

1. Enter your **Organization Name** — this is the top-level entity for your account (e.g., your company name).
2. Enter a **Workspace Name** — the default workspace where your projects will live.
3. Both names can be changed later in Settings.

#### Step 3 — Profile Completion

1. Enter your **Full Name** (required).
2. Create a **Password** that meets security requirements:
   - Minimum 8 characters
   - At least one uppercase letter
   - At least one lowercase letter
   - At least one number
   - At least one special character
3. Confirm your password.
4. Click **"Complete Signup"**.

Upon successful signup:
- Your organization is created automatically.
- A default workspace is created within that organization.
- You are assigned the **Owner** role for both the organization and workspace.
- A **Starter plan** (free tier) subscription is automatically assigned to your organization.
- You are logged in and redirected to the Dashboard.

### 2.3 Email Verification

Email verification is a security measure to ensure that the email address you provide is valid and belongs to you.

- **During Signup**: You must verify your email before completing registration.
- **OTP Code**: A 6-digit one-time password is sent to your email.
- **Expiration**: OTP codes expire after a defined time period (typically 10 minutes).
- **Resend**: You can request a new OTP if the previous one expires.
- **Attempt Limits**: After multiple failed attempts, you may need to wait before trying again.

### 2.4 Logging In

1. Navigate to the DocEngine login page.
2. Enter your **Email** address.
3. Enter your **Password**.
4. Click **"Sign In"**.

Upon successful login:
- You receive a JWT access token (used for API authentication).
- A refresh token is issued for maintaining your session.
- Your session information (IP address, device, browser) is recorded for security.
- You are redirected to the **Dashboard**.

**Session Management:**
- Sessions remain active for a configured period.
- When your access token expires, it is automatically refreshed using your refresh token.
- You can view and manage active sessions from the Security settings.

### 2.5 Forgot Password

If you've forgotten your password:

1. On the login page, click **"Forgot Password?"**.
2. Enter the **email address** associated with your account.
3. Click **"Send Reset Link"**.
4. Check your email for a password reset link.
5. Click the link in the email to proceed to the reset page.

### 2.6 Resetting Your Password

1. After clicking the reset link from your email, you'll be taken to the reset password page.
2. Enter your **new password** (must meet the security requirements listed above).
3. Confirm the new password.
4. Click **"Reset Password"**.
5. Upon success, you'll be redirected to the login page where you can sign in with your new password.

> **For Invited Users**: If you were invited to a workspace and haven't set a password yet, you'll be prompted to create one when you first access the system. Use the dedicated invited-user password setup flow.

---

## 3. Navigating the Interface

### 3.1 Sidebar Navigation

The sidebar is the primary navigation element and appears on the left side of the screen.

**Sidebar Features:**
- **Collapsible**: Click the collapse button to minimize the sidebar to icon-only mode, giving more screen space for content.
- **Resizable**: Drag the right edge of the sidebar to adjust its width (200px to 400px).
- **Workspace Selector**: Located at the top of the sidebar, shows the current workspace name. Click to switch between workspaces.

**Navigation Items:**

| Icon | Label | Description |
|------|-------|-------------|
| Layout | **Dashboard** | Your personalized home page with statistics and quick actions |
| Folder | **Projects** | View and manage all projects in the current workspace |
| File | **Documents** | Access the global document library |
| Settings | **Settings** | Access workspace, organization, and system settings |

**Sidebar Footer:**
- **User Avatar**: Shows your profile picture and name.
- **Profile Link**: Click your name to go to your profile page.
- **Logout**: Sign out of DocEngine.

### 3.2 Top Header Bar

The header bar runs along the top of every page and includes:

- **Search Bar**: A system-wide search input that searches across documents, projects, users, and workspaces. Press `/` to quickly focus the search bar.
- **Notification Bell**: Shows a badge with the count of unread notifications. Click to open the notification center.
- **User Menu**: Access profile settings, account management, and sign out.

### 3.3 Workspace Switcher

If you belong to multiple workspaces (across one or more organizations), you can switch between them:

1. Click the **workspace name** in the top area of the sidebar.
2. A dropdown appears listing all workspaces you belong to.
3. Click the desired workspace to switch.
4. The dashboard, projects, and documents will update to reflect the selected workspace.

### 3.4 Dark Mode

DocEngine supports both light and dark themes:

1. Navigate to **Settings > General**.
2. Toggle the **Theme** option between **Light** and **Dark**.
3. Alternatively, DocEngine can detect your system preference automatically.
4. The theme preference is saved and persists across sessions.

### 3.5 Language & Localization

DocEngine supports multiple languages:

| Language | Code |
|----------|------|
| English | en |
| Arabic | ar |
| Spanish | es |
| French | fr |
| German | de |

To change the language:

1. Navigate to **Settings > General** or your **Profile** page.
2. Select your preferred **Language** from the dropdown.
3. The interface will update immediately.

> **RTL Support**: When Arabic is selected, the entire interface switches to right-to-left (RTL) layout automatically.

---

## 4. Dashboard

The Dashboard is your home page after logging in. It provides an at-a-glance overview of your workspace activity, statistics, and quick actions. The dashboard content varies based on your role.

### 4.1 Admin Dashboard

Visible to users with **Owner** or **Admin** roles in the workspace.

**Statistics Cards:**
- **Total Projects**: Number of active projects in the workspace.
- **AI Generations**: Total AI generation sessions completed this month.
- **Documents**: Total documents across all projects.
- **Active Members**: Number of active workspace members.

**Subscription Status:**
- Current subscription tier (Starter, Professional, Business, Enterprise).
- Current billing period and renewal date.
- Usage progress bars (AI generations used vs. limit, projects created vs. limit).

**AI Budget Overview:**
- Current month's AI spending.
- Budget utilization percentage.
- Quick link to budget settings.

**Recent AI Generation Sessions:**
- A list of recent generation sessions across all projects.
- Each entry shows: project name, session status (Completed, Failed, In Progress), time elapsed, and document count.
- Click a session to view details.

**Recent Documents:**
- Latest documents uploaded or generated across the workspace.
- Shows document name, type, project, and modification date.

**Team Activity Feed:**
- Recent actions by team members (document uploads, project creation, generation sessions, etc.).

**Quick Actions:**
- **Create New Project** — opens the project creation stepper.
- **Start AI Generation** — navigate to AI generation page.
- **Manage Team** — go to workspace member settings.
- **View Analytics** — open AI usage analytics.

### 4.2 Manager Dashboard

Visible to users with **Manager** role.

**Includes:**
- Team member activity and workload overview.
- Project status summary across managed projects.
- Recent documents modified by team.
- Generation activity for the team.
- Quick action buttons for common operations.

### 4.3 Member Dashboard

Visible to users with **Member** role.

**Includes:**
- Personal activity summary.
- Recent projects you've accessed.
- Documents you've recently worked on.
- Your generation sessions and their statuses.
- Personal statistics (documents reviewed, generations initiated, etc.).

---

## 5. Organization Management

### 5.1 Organization Overview

An **Organization** is the highest-level entity in DocEngine. It represents your company or team and contains:

- One or more **Workspaces**.
- A **Subscription Plan** (billing is at the organization level).
- **Members** with organization-level roles.

Each user who signs up automatically creates an organization. Additional organizations can be joined through invitations.

### 5.2 Updating Organization Settings

To update your organization settings:

1. Navigate to **Settings > Organization Settings** (accessible to Admins and Owners).
2. You can modify:
   - **Organization Name**: The display name for your organization.
   - **Description**: A brief description of your organization.
   - **Website**: Your organization's website URL.
   - **Logo**: Upload or change the organization logo.

3. Click **"Save Changes"** to apply.

### 5.3 Organization Logo

To set or change your organization logo:

1. In Organization Settings, click the **logo upload area**.
2. Select an image file (JPEG, PNG, or SVG recommended).
3. The image is uploaded and stored as base64.
4. The new logo appears throughout the application wherever your organization is referenced.

---

## 6. Workspace Management

### 6.1 Understanding Workspaces

A **Workspace** is a collaborative environment within an organization. Think of it as a department or team space.

**Key properties of a workspace:**
- **Name**: Display name for the workspace.
- **Type**: Either **Team** (shared) or **Personal** (individual use).
- **Projects**: Each workspace contains its own set of projects.
- **Members**: Users are invited to workspaces with specific roles.
- **Settings**: Each workspace has its own timezone, date format, notification preferences, and logo.

### 6.2 Workspace Settings

To configure workspace settings:

1. Navigate to **Settings > General Settings**.
2. Available settings include:

| Setting | Description |
|---------|-------------|
| **Workspace Name** | Display name for the workspace |
| **Description** | Brief description of the workspace's purpose |
| **Timezone** | Default timezone for dates and times |
| **Date Format** | How dates are displayed (e.g., MM/DD/YYYY, DD/MM/YYYY) |
| **Week Starts On** | Which day the week begins (Sunday or Monday) |
| **Logo** | Workspace-specific logo |
| **Email Notifications** | Enable/disable email notifications for workspace events |
| **Browser Notifications** | Enable/disable browser push notifications |
| **Weekly Digest** | Receive a weekly summary email |

3. Click **"Save Changes"** to apply your modifications.

### 6.3 Workspace Members & Invitations

**Inviting New Members:**

1. Navigate to **Settings > Members** (or the team management section).
2. Click **"Invite Member"**.
3. Enter the invitee's **email address**.
4. Select the **role** to assign (Member, Manager, Admin).
5. Optionally add a **personal message** (up to 500 characters).
6. Click **"Send Invitation"**.

The invitee will receive an email with a unique invitation link containing a token.

**Invitation Statuses:**

| Status | Description |
|--------|-------------|
| **Pending** | Invitation sent, awaiting response |
| **Accepted** | Invitee has joined the workspace |
| **Rejected** | Invitee declined the invitation |
| **Expired** | Invitation link has expired |
| **Cancelled** | Invitation was withdrawn by the sender |

### 6.4 Accepting a Workspace Invitation

**For Existing Users:**
1. Click the invitation link in your email.
2. If you're already logged in, you'll see the invitation details (workspace name, inviter, role).
3. Click **"Accept"** to join the workspace, or **"Reject"** to decline.
4. Once accepted, the new workspace appears in your workspace switcher.

**For New Users:**
1. Click the invitation link in your email.
2. You'll be taken to a signup page pre-filled with your email.
3. Complete the signup process (set your name and password).
4. Upon completion, you'll automatically join the workspace with the assigned role.

### 6.5 Workspace Roles & Permissions

| Role | Description | Key Permissions |
|------|-------------|-----------------|
| **Owner** | Full control over the workspace | All permissions, cannot be removed |
| **Admin** | Administrative access | Manage members, settings, projects, documents |
| **Manager** | Team management | Create projects, manage team members, review documents |
| **Member** | Standard access | View projects, upload documents, participate in generation |
| **Custom** | Tailored permissions | Configured by Admin with specific permission sets |

---

## 7. Project Management

### 7.1 Projects Overview

Projects are the primary organizational unit within a workspace. Each project groups related documents, team members, and AI generation sessions together.

**Accessing the Projects Page:**
1. Click **"Projects"** in the sidebar navigation.
2. You'll see all projects in the current workspace.

**View Modes:**
- **Grid View**: Projects displayed as cards in a responsive grid. Each card shows the project logo, name, description, status badge, and action menu.
- **List View**: Projects displayed in a table format with sortable columns.

Toggle between views using the view mode buttons in the top-right corner of the projects page. The view preference is saved in the URL and persists during navigation.

### 7.2 Creating a New Project

To create a new project:

1. On the Projects page, click **"Create Project"**.
2. Complete the 3-step creation wizard:

#### Step 1 — Project Basics

| Field | Required | Description |
|-------|----------|-------------|
| **Project Name** | Yes | A descriptive name for your project (max 200 characters) |
| **Project Code** | No | A short identifier code (max 20 characters, e.g., "PROJ-001") |
| **Project Type** | Yes | Agile, Waterfall, Hybrid, or Custom |
| **Visibility** | Yes | Private (only members), Team (all workspace members), or Public |
| **Logo** | No | Upload a project logo (drag-and-drop or file selector) |

#### Step 2 — Project Details

| Field | Required | Description |
|-------|----------|-------------|
| **Default View** | No | Preferred view for the project: List, Kanban, or Report |
| **Milestone View** | No | Timeline granularity: Week, Month, or Quarter |
| **Budget** | No | Allocated budget amount |
| **Currency** | No | Budget currency (USD, EUR, GBP, AED, SAR, CAD, AUD, JPY) |
| **Tags** | No | Custom labels for categorization |
| **Custom Fields** | No | Additional metadata fields specific to your needs |

#### Step 3 — Project Charter (Optional)

| Field | Max Length | Description |
|-------|-----------|-------------|
| **Problem Statement** | 2000 chars | What problem does this project solve? |
| **Project Objectives** | 2000 chars | What are the measurable goals? |
| **Expected Benefits** | 2000 chars | What value will this project deliver? |
| **Scope Details** | 2000 chars | What is in scope and out of scope? |
| **Project Deliverables** | 2000 chars | What tangible outputs will be produced? |
| **Mission** | 1000 chars | The project's mission statement |
| **Vision** | 1000 chars | The long-term vision |
| **Goals** | 1000 chars | Specific, achievable goals |

3. Review your entries and click **"Create Project"**.

> **Subscription Limits**: The number of projects you can create is limited by your subscription plan. The system checks your plan limits before allowing creation. If you've reached your limit, you'll receive a notification prompting you to upgrade.

### 7.3 Editing a Project

1. On the Projects page, locate the project you want to edit.
2. Click the **action menu** (three dots) on the project card/row.
3. Select **"Edit"**.
4. The same 3-step wizard opens, pre-filled with the project's current data.
5. Make your changes and click **"Save"**.

Alternatively, from the Project Detail page, click the **"Edit"** button in the project header.

### 7.4 Project Detail Page

Click on any project name to open the Project Detail page. This serves as the hub for all project-related activities.

**Project Header:**
- Project logo, name, and code.
- Status badge (Draft, Planning, Active, On Hold, Completed, Cancelled).
- Description.
- Progress indicator.
- Team member count.
- Edit button.

**Main Tabs:**

| Tab | Description |
|-----|-------------|
| **AI Generation** | Start and manage AI document generation sessions. View generation history, quality queue, and settings. |
| **Documents** | Project-specific document library. Upload, view, edit, and manage project documents. |

### 7.5 Managing Project Members

**Adding Members:**
1. Navigate to the Project Detail page.
2. Open the team management section.
3. Click **"Add Members"**.
4. You'll see a list of workspace members not yet added to the project.
5. Select one or more members.
6. Optionally enable **"Notify Users"** to send them a notification.
7. Click **"Add"**.

**Project Member Roles:**

| Role | Description |
|------|-------------|
| **Admin** | Full project control — manage members, settings, and all documents |
| **Manager** | Manage project workflow and team assignments |
| **Lead** | Lead specific areas of the project |
| **Member** | Standard access — view and contribute to documents |

**Removing Members:**
1. In the team management section, find the member to remove.
2. Click **"Remove"**.
3. A preview dialog shows the impact of removal (affected tasks, assignments).
4. Optionally add a **reason** for removal and toggle **"Notify User"**.
5. Confirm the removal.

> **Note:** Removing a member performs a soft removal — their historical contributions are preserved, but they lose access to the project going forward.

### 7.6 Duplicating a Project

1. On the Projects page, click the action menu on the project card.
2. Select **"Duplicate"**.
3. Enter a **new name** for the duplicated project.
4. Click **"Duplicate"**.
5. A copy of the project is created with the same settings and charter, but without documents or generation sessions.

### 7.7 Deleting a Project

1. Click the action menu on the project card/row.
2. Select **"Delete"**.
3. A confirmation dialog appears warning that this action will soft-delete the project and all associated data.
4. Click **"Delete"** to confirm.

> **Important:** Deletion is a soft delete — the project is marked as deleted and hidden from view but can be recovered by an administrator if needed. All associated documents and generation sessions are also soft-deleted.

**Bulk Delete:**
1. Switch to **List View** on the Projects page.
2. Select multiple projects using the checkboxes.
3. Click the **"Delete Selected"** button in the bulk actions bar.
4. Confirm the deletion.

### 7.8 Project Statuses

| Status | Description |
|--------|-------------|
| **Draft** | Project has been created but not yet started |
| **Planning** | Project is in the planning phase |
| **Active** | Project is actively being worked on |
| **On Hold** | Project is temporarily paused |
| **Completed** | Project work is finished |
| **Cancelled** | Project has been cancelled |

### 7.9 Project Views

Projects support multiple view preferences:

- **List View**: Traditional table layout with sortable columns.
- **Kanban View**: Visual board with columns representing statuses.
- **Report View**: Analytical view with charts and statistics.

The default view is set during project creation and can be changed in project settings.

---

## 8. Document Management

### 8.1 Document Library

The Document Library is the central place for managing all documents in DocEngine. Access it in two ways:

- **Global Document Library**: Click **"Documents"** in the sidebar to see all documents across the current workspace.
- **Project Document Library**: Navigate to a project's **Documents** tab to see only that project's documents.

**Document Types Supported:**

| Type | Extensions | Icon |
|------|-----------|------|
| **PDF** | .pdf | File icon |
| **Word** | .doc, .docx | Word icon |
| **Excel** | .xls, .xlsx | Spreadsheet icon |
| **Image** | .png, .jpg, .gif, .svg | Image icon |
| **Text** | .txt, .md | Text icon |
| **Other** | Various | Generic file icon |

### 8.2 Uploading Documents

**Single File Upload:**
1. In the Document Library, click **"Upload"**.
2. A dialog appears with a drag-and-drop zone.
3. Either **drag files** into the zone or click **"Browse"** to select files.
4. The upload begins immediately with a progress indicator.
5. Upon completion, the document appears in the library.

**Multi-File Upload:**
1. Select multiple files at once (up to the upload limit).
2. Each file shows individual progress.
3. Files are validated before upload (type checking, size limits).
4. Maximum file size: **500 MB** per upload batch.

**Validation:**
- File type compatibility is checked before upload.
- File size limits are enforced based on your subscription tier.
- Duplicate file detection may warn you if a similar document already exists.

### 8.3 Searching & Filtering Documents

**Search:**
- Use the **search bar** at the top of the Document Library to search by document name or description.
- Search is case-insensitive and matches partial text.
- Results update as you type.

**Filters:**

| Filter | Options |
|--------|---------|
| **File Type** | All, PDF, Word, Excel, Image, Text |
| **AI Generated** | All, AI Generated Only, Manual Only |

**Sorting:**

| Sort By | Description |
|---------|-------------|
| **Last Modified** | Most recently modified first (default) |
| **Name** | Alphabetical order |
| **Created Date** | Newest first |
| **File Size** | Largest first |

Click the **sort order toggle** to switch between ascending and descending.

### 8.4 Viewing Documents

To view a document:

1. Click on a document name or thumbnail in the library.
2. Depending on the file type:
   - **PDF files**: Open in the built-in PDF viewer with page navigation.
   - **Images**: Open in the image viewer with zoom and pan.
   - **Markdown files**: Rendered with formatting, code highlighting, and table support.
   - **Word/Excel**: Preview available or download to view locally.
   - **Other types**: Download prompt.

**Document Viewer Features:**
- **Zoom**: Adjust zoom level for detailed viewing.
- **Page Navigation**: For multi-page documents, navigate between pages.
- **Full Screen**: Toggle full-screen mode for distraction-free viewing.
- **Download**: Download the current version from the viewer.

### 8.5 Editing Documents

To edit a document:

1. In the Document Library, click the action menu on a document.
2. Select **"Edit"**.
3. The Document Editor opens (see [Section 11](#11-document-editing--collaboration) for detailed editor documentation).

### 8.6 Document Version History

Every document in DocEngine maintains a complete version history. Each time a new version is uploaded or the document is modified, a new version is created.

**Viewing Version History:**
1. Click the action menu on a document.
2. Select **"View Versions"**.
3. A dialog shows all versions with:
   - **Version Number** (sequential, starting from 1).
   - **Upload/Modification Date**.
   - **Changed By** (user who made the change).
   - **Change Description** (optional notes about what changed).
   - **File Size**.
   - **Checksum** (for integrity verification).

### 8.7 Restoring a Previous Version

1. Open the Version History dialog for a document.
2. Find the version you want to restore.
3. Click **"Restore"** on that version.
4. Confirm the restoration.
5. The selected version becomes the current version, and a new version entry is created to maintain the audit trail.

### 8.8 Comparing Document Versions

1. In the Version History, select two versions to compare.
2. Click **"Compare"**.
3. A side-by-side diff view shows the differences between the two versions.
4. Added content is highlighted in green, removed content in red.

### 8.9 Document Collaborators

Collaborators are users who have explicit access to a specific document beyond their default workspace permissions.

**Adding Collaborators:**
1. Open a document.
2. Click the **"Collaborators"** button (or find it in the document settings).
3. Click **"Add Collaborator"**.
4. Select a user from the workspace.
5. Choose the **access level**:

| Access Level | Permissions |
|-------------|-------------|
| **Owner** | Full control — edit, delete, share, manage collaborators |
| **Editor** | Can view and edit document content |
| **Commenter** | Can view and add comments, but cannot edit |
| **Viewer** | Read-only access |

6. Click **"Add"**.

**Removing Collaborators:**
1. In the collaborators list, find the user.
2. Click **"Remove"**.
3. Confirm the removal.

**Updating Access Level:**
1. In the collaborators list, click the access level dropdown for a user.
2. Select the new access level.
3. Changes take effect immediately.

### 8.10 Sharing Documents via Link

You can create a public share link for any document, allowing external users to view it without logging in.

**Creating a Share Link:**
1. Open a document.
2. Click the **"Share"** button.
3. Configure share settings:
   - **Access Level**: Viewer (default) or Commenter.
   - **Expiration**: Set an expiry date (optional).
   - **Password Protection**: Add a password for extra security (optional).
   - **Allow Download**: Toggle whether the recipient can download the file.
   - **Allow Comments**: Toggle whether the recipient can add comments.
   - **Message**: Add a personal note visible to the recipient (optional, up to 1000 characters).
4. Click **"Create Link"**.
5. Copy the generated link and share it.

**Managing Share Links:**
- View all active share links for a document.
- See access statistics (view count, last accessed, IP address).
- Revoke a share link at any time.

**Viewing a Shared Document:**
- Recipients access the document at `/shared/{token}`.
- No login is required.
- The document renders with full formatting (Markdown, code highlighting, wireframes).
- If password-protected, the recipient must enter the password first.
- If expired, an appropriate message is shown.

### 8.11 Downloading Documents

- **From the Library**: Click the action menu on a document and select **"Download"**.
- **From the Viewer**: Click the download button in the document viewer toolbar.
- **Specific Version**: In the Version History, click **"Download"** on any version to download that specific version.

### 8.12 Duplicating Documents

1. Click the action menu on a document.
2. Select **"Duplicate"**.
3. Choose the target workspace and/or project (optional — defaults to the same location).
4. Enter a new name (defaults to "Copy of [Original Name]").
5. Click **"Duplicate"**.
6. A complete copy of the document is created, including its content but starting with Version 1.

### 8.13 Deleting Documents

1. Click the action menu on a document.
2. Select **"Delete"**.
3. Confirm the deletion in the dialog.

> **Note:** Deletion is a soft delete. The document is hidden from view but remains in the database and can be recovered by an administrator. All associated versions, comments, and collaborators are also soft-deleted.

### 8.14 Bulk Document Operations

**Selecting Multiple Documents:**
1. In the Document Library, hover over documents to reveal the selection checkbox.
2. Check the boxes for the documents you want to act on.
3. Alternatively, click **"Select All"** to select all visible documents.

**Available Bulk Actions:**
- **Delete Selected**: Soft-deletes all selected documents after confirmation.

**Selection Indicator:**
- A badge in the toolbar shows the number of selected documents.
- Click **"Deselect All"** to clear the selection.

---

## 9. AI Document Generation (AI Brain)

### 9.1 Overview of AI Generation

DocEngine's AI Brain is the core feature that distinguishes it from traditional document management systems. It uses advanced Large Language Models (LLMs) to automatically generate comprehensive software project documentation from a single Business Requirements Document (BRD).

**What AI Generation Produces:**

Starting from a BRD, the AI pipeline generates:

| Document Type | Abbreviation | Description |
|--------------|-------------|-------------|
| **System Requirements Document** | SRD | Technical system requirements derived from business requirements |
| **Functional Requirements Specification** | FRS | Detailed functional specifications with use cases and acceptance criteria |
| **Epics** | EPIC | High-level feature groupings for Agile project management |
| **User Stories** | USER_STORY | Individual user stories with acceptance criteria, derived from Epics |
| **Tasks** | TASK | Granular development tasks broken down from User Stories |

### 9.2 Generation Pipeline

The AI generation follows a sequential pipeline, where each document type builds upon the previous:

```
BRD (Input)
  └─→ SRD (System Requirements)
        └─→ FRS (Functional Requirements)
              └─→ Epics (Feature Groups)
                    └─→ User Stories (Individual Stories)
                          └─→ Tasks (Development Tasks)
```

**Pipeline Characteristics:**
- **Sequential Processing**: Each stage uses the output of the previous stage as context.
- **Chunked Processing**: Large documents are split into manageable chunks for processing.
- **Checkpoint Recovery**: The system saves checkpoints at each stage, allowing recovery from failures.
- **Cost Tracking**: Token usage and costs are tracked at every step.
- **Quality Validation**: Each generated document undergoes quality checks.
- **Traceability**: Links between source and generated content are maintained.

### 9.3 Starting a New Generation Session

1. Navigate to your **Project Detail page**.
2. Select the **AI Generation** tab.
3. Click **"Start New Generation"** or the relevant quick action card.

**Generation Setup:**

| Field | Description |
|-------|-------------|
| **BRD Document** | Upload your BRD file (PDF, Word, or Text format) or provide a URL to the document |
| **Generation Type** | Select which documents to generate (full pipeline or specific types) |
| **Tech Stack** | Optionally configure the technology stack for more contextual generation |

4. Click **"Start Generation"**.

**What Happens Next:**
1. **Quality Analysis**: The system first analyzes the quality of your BRD (see [Section 9.4](#94-quality-analysis--quality-gates)).
2. **Quality Gate Check**: If the quality score meets the threshold, generation proceeds automatically. If not, you may be asked to override or improve the BRD.
3. **Document Processing**: The BRD is parsed, chunked, and processed through the AI pipeline.
4. **Real-Time Progress**: Track progress via the progress indicator (see [Section 9.5](#95-monitoring-generation-progress)).
5. **Completion**: Generated documents appear in the project's document library for review.

> **Subscription Check**: Before starting, the system verifies your organization has remaining AI generation credits for the current billing period. If you've exhausted your quota, you'll be prompted to upgrade your plan.

### 9.4 Quality Analysis & Quality Gates

Before AI generation begins, your BRD undergoes automated quality analysis.

**Quality Scores:**

The analysis evaluates three dimensions:

| Dimension | What It Measures |
|-----------|-----------------|
| **Structure Completeness** | Does the BRD have all expected sections? (Executive summary, scope, requirements, etc.) |
| **Content Quality** | Is the content clear, specific, and free of ambiguity? |
| **Requirements Quality** | Are requirements measurable, testable, and properly categorized? |

**Overall Quality Grade:**

| Grade | Score Range | Meaning |
|-------|-----------|---------|
| A | 90-100 | Excellent — ready for generation |
| B | 80-89 | Good — minor improvements possible |
| C | 70-79 | Adequate — meets minimum threshold |
| D | 60-69 | Below threshold — improvements recommended |
| F | Below 60 | Poor — significant improvements needed |

**Quality Gate Behavior:**
- If the quality score **meets or exceeds** the configured minimum threshold (default: 70), generation proceeds automatically.
- If the quality score **falls below** the threshold:
  - You receive a detailed analysis with recommendations.
  - If quality gate override is enabled (configurable by admins), you can choose to proceed anyway.
  - Otherwise, you must improve the BRD and re-submit.

**Viewing Quality Reports:**
1. After analysis, a quality report is available for each generation session.
2. The report includes:
   - Overall score and grade.
   - Per-dimension scores.
   - Specific recommendations for improvement.
   - Top recommendations prioritized by impact.

**Quality Gate Configuration (Admin):**

Administrators can configure quality gates per project:

| Setting | Default | Description |
|---------|---------|-------------|
| **Enabled** | Yes | Whether quality gates are active |
| **Minimum Score** | 70.0 | The threshold score for automatic approval |
| **Require User Override** | Yes | Whether users can override a failed quality gate |
| **Allow Low Quality Generation** | No | Whether generation can proceed with below-threshold scores |
| **Analysis Timeout** | 120 sec | Maximum time for quality analysis |
| **Cache Results** | Yes | Cache analysis results to avoid re-processing |
| **Cache Expiration** | 60 min | How long cached results remain valid |

### 9.5 Monitoring Generation Progress

Once generation starts, you can monitor progress in real-time:

**Progress Indicators:**
- **Step Counter**: Shows "Step X of Y" (e.g., "Step 2 of 6").
- **Progress Bar**: Visual percentage indicator.
- **Current Step Description**: What the AI is currently working on (e.g., "Generating System Requirements Document").
- **Elapsed Time**: How long the generation has been running.
- **Estimated Remaining Time**: Approximate time to completion.

**Session Status Badges:**

| Status | Color | Description |
|--------|-------|-------------|
| **Initializing** | Blue | Session is being set up |
| **Processing** | Blue (animated) | AI is actively generating content |
| **Analyzing** | Blue | Quality analysis in progress |
| **Paused** | Yellow | Generation is paused (can be resumed) |
| **Completed** | Green | All documents generated successfully |
| **Failed** | Red | An error occurred during generation |
| **Cancelled** | Gray | Generation was cancelled by user |

**Real-Time Updates:**
DocEngine uses **SignalR** (WebSocket) connections to push real-time progress updates to your browser. You'll see progress changes without needing to refresh the page.

**Metrics Tracked:**
- Total tokens consumed (input + output).
- Total cost in USD.
- API call count (successful and failed).
- Average response time.

### 9.6 Tech Stack Configuration

Before starting generation, you can configure the technology stack for your project. This helps the AI generate more contextually relevant and technically accurate documents.

**Extracting Tech Stack from Document:**
1. Upload a Systems Requirements Document (SRD) or similar technical document.
2. Click **"Extract from Document"**.
3. The AI analyzes the document and extracts technology references.
4. Review and adjust the extracted technologies.

**Manual Configuration:**
1. Navigate to the Tech Stack section of your project.
2. Add technologies by category (Frontend, Backend, Database, Cloud, DevOps, etc.).
3. Save the configuration.

**AI Suggestions:**
- Click **"Get AI Suggestions"** to receive AI-generated technical notes and recommendations based on your configured stack.
- Suggestions take into account industry best practices and the project's requirements.

### 9.7 Multi-Document Upload & Generation

For complex projects, you can upload multiple source documents to enrich the generation context:

1. Click **"Upload Multiple Documents"** in the AI Generation tab.
2. Select multiple files (BRDs, SRDs, reference documents).
3. The system validates each document for:
   - File type compatibility.
   - Content readability.
   - Document type identification.
4. Documents are analyzed for compatibility with each other.
5. A **generation plan** is created showing which document types will be generated and from which sources.

### 9.8 Selective Document Generation

Instead of generating the full document suite, you can select specific document types:

1. After uploading your source documents, review the **Generation Plan**.
2. The plan shows which documents can be generated based on available inputs.
3. Select or deselect specific document types.
4. Click **"Generate Selected"** to start generation for only the chosen types.

**Use Cases:**
- Already have an SRD? Skip SRD generation and go directly to FRS.
- Only need Epics and User Stories? Deselect FRS and Tasks.
- Want to regenerate only the Tasks? Select Tasks only.

### 9.9 Generation Sessions & History

**Viewing Session History:**
1. Navigate to the **AI Generation** tab in your project.
2. You'll see a list of all generation sessions for the project.
3. Each session shows:
   - Session ID and creation date.
   - Status (Completed, Failed, In Progress, etc.).
   - Number of documents generated.
   - Quality gate status and score.
   - Total cost.
   - Time elapsed.

**Session Details:**
Click on any session to view:
- Complete list of generated documents with status.
- Step-by-step progress log.
- Checkpoint history.
- Cost breakdown.
- Error log (if any errors occurred).
- Token usage statistics.

### 9.10 Checkpoint Recovery

DocEngine automatically creates checkpoints during generation, allowing recovery from failures without restarting the entire process.

**How Checkpoints Work:**
- Checkpoints are created automatically at each major step of the pipeline.
- Each checkpoint saves:
  - The current generation state.
  - All content generated so far.
  - Context and conversation history.
  - Token counts and costs.

**Recovering from a Failure:**
1. If a generation session fails, it enters the **Failed** state.
2. The system checks if the session can be recovered from a checkpoint.
3. If recovery is possible, a **"Resume"** button appears.
4. Click **"Resume"** to continue generation from the last successful checkpoint.
5. The AI picks up where it left off, preserving all previously generated content.

**Manual Recovery:**
- Administrators can view all checkpoints for a session.
- Specific checkpoints can be selected for recovery.
- Checkpoint integrity is validated before restoration.

### 9.11 Generation Budget & Cost Tracking

AI generation consumes tokens from LLM providers, which incur costs. DocEngine provides comprehensive budget management.

**Budget Overview:**
- Navigate to **Settings > AI Usage** or the project's AI Generation tab.
- View current month's spending vs. budget.
- See per-project cost breakdowns.

**Budget Settings:**

| Setting | Description |
|---------|-------------|
| **Monthly Budget (USD)** | Maximum monthly AI spending |
| **Per-Project Budget** | Optional budget cap per project |
| **Alert Threshold** | Percentage at which to trigger budget alerts (e.g., 80%) |
| **Hard Limit** | Whether to block generation when budget is exceeded |

**Cost Tracking:**
- Each generation session tracks:
  - Input tokens (prompt tokens).
  - Output tokens (completion tokens).
  - Total cost in USD.
  - Cost per document type.
- Historical usage data is available for trend analysis.

**Budget Alerts:**
- When spending reaches the alert threshold, you receive a notification.
- If hard limits are enabled, generation is blocked when the budget is exhausted.
- Budget usage resets automatically at the beginning of each billing period.

---

## 10. Reviewing & Approving AI-Generated Documents

### 10.1 Document Review Workflow

After AI generation completes, all generated documents start in **Draft** status and go through a review workflow:

```
Draft → Under Review → Approved / Rejected / Changes Requested
```

**Document Statuses in the Review Flow:**

| Status | Description |
|--------|-------------|
| **Draft** | Newly generated, awaiting initial review |
| **Under Review** | Currently being reviewed by a team member |
| **Approved** | Document has been approved and finalized |
| **Rejected** | Document has been rejected with a reason |
| **Changes Requested** | Specific changes have been requested |
| **Pending Regeneration** | Document is queued for regeneration |
| **Invalidated** | Document has been invalidated (e.g., source document changed) |
| **Archived** | Document has been archived for record-keeping |

### 10.2 Approving a Document

1. Open an AI-generated document from the project's document list or the generation session.
2. Review the document content carefully.
3. Click **"Approve"**.
4. Optionally add **approval comments** explaining your decision.
5. Click **"Confirm Approval"**.

**What Happens:**
- The document status changes to **Approved**.
- Your name and the approval timestamp are recorded.
- Relevant team members are notified.
- The document is locked from further editing (unless a change request is made).

### 10.3 Rejecting a Document

1. Open the AI-generated document.
2. After review, click **"Reject"**.
3. Provide a **rejection reason** (required) explaining why the document doesn't meet requirements.
4. Click **"Confirm Rejection"**.

**What Happens:**
- The document status changes to **Rejected**.
- The rejection reason is recorded for audit purposes.
- The document can be regenerated or manually edited and re-submitted.

### 10.4 Requesting Changes

Instead of a full rejection, you can request specific changes:

1. Open the AI-generated document.
2. Click **"Request Changes"**.
3. Provide a **change request reason** describing what needs to be modified.
4. Click **"Submit Change Request"**.

**What Happens:**
- The document status changes to **Changes Requested**.
- The change request details are recorded.
- Team members can then manually edit the document or regenerate specific sections.

### 10.5 Regenerating Document Sections

If only specific sections of a generated document need improvement:

1. Open the document.
2. Navigate to the section that needs regeneration.
3. Click **"Regenerate Section"** on that section.
4. The AI regenerates only that section while preserving all other content.
5. The regenerated section appears for review.
6. A version history is maintained for the section.

**Batch Section Regeneration:**
1. Select multiple sections that need regeneration.
2. Click **"Regenerate Selected Sections"**.
3. All selected sections are regenerated in one operation.

### 10.6 Full Document Regeneration

To regenerate an entire document:

1. Open the document from the generation session.
2. Click **"Regenerate"**.
3. Provide a **regeneration reason** (optional).
4. Click **"Confirm"**.

**What Happens:**
- A new version of the document is generated.
- The original document is preserved (marked as the source of regeneration).
- The new document links back to the original via `RegeneratedFromDocumentId`.
- A `RegenerationNumber` tracks how many times the document has been regenerated.
- The old version is archived, not deleted.

### 10.7 Document Traceability

DocEngine maintains comprehensive traceability between all documents in the generation pipeline.

**Traceability Features:**
- Every generated document section includes **source attribution** indicating which BRD requirements it derives from.
- **Traceability links** connect:
  - BRD requirements → SRD specifications
  - SRD specifications → FRS functional requirements
  - FRS requirements → Epics
  - Epics → User Stories
  - User Stories → Tasks

**Viewing Traceability:**
1. Open any AI-generated document.
2. Click **"View Traceability"** (or the traceability tab).
3. A visual map shows the document's source connections.
4. Click any link to navigate to the source content.

**Traceability Labels:**
- **BRD Source**: Content directly derived from the original BRD.
- **Inferred**: Content inferred by the AI based on context and best practices.
- **Best Practice**: Industry best practice recommendations added by the AI.
- **Flagged/Invented**: Content that the AI generated without a direct BRD source (flagged for review).

---

## 11. Document Editing & Collaboration

### 11.1 The Document Editor

DocEngine includes a rich document editor for modifying both uploaded and AI-generated documents.

**Accessing the Editor:**
1. Navigate to a document in the Document Library or generation session.
2. Click **"Edit"** from the action menu.
3. The editor opens with the document content.

**Editor Features:**
- **Rich Text Editing**: Full formatting support (headings, bold, italic, lists, tables, code blocks).
- **Markdown Support**: Toggle between rich text and raw Markdown editing.
- **Auto-Save**: Changes are saved automatically with a "Last saved" timestamp indicator.
- **Save Status**: Visual indicator showing whether changes are saved or pending.
- **Full Screen Mode**: Toggle full-screen for distraction-free editing.
- **Preview Mode**: Switch between Edit and Preview modes to see the rendered output.

**Editor Toolbar:**
- Save button (manual save).
- Share button.
- Collaborators button.
- Comments toggle.
- Settings menu.
- View mode toggle (Edit/Preview).
- Full screen toggle.

### 11.2 Section-Level Editing

AI-generated documents are organized into hierarchical sections. Each section can be edited independently:

1. Open the document in the editor.
2. Click on any section to select it.
3. Modify the section content directly.
4. Changes are tracked at the section level with version history.

**Section Properties:**
- **Section Number**: Hierarchical numbering (e.g., "1.0", "1.1", "2.3.1").
- **Title**: Section heading.
- **Content**: Section body in Markdown format.
- **Level**: Nesting depth (1 = top-level, 2 = subsection, etc.).
- **Metadata**: Additional metadata as JSON.

**Section Version History:**
- Each section maintains its own version history.
- Changes are categorized as: CREATED, EDITED, REGENERATED, INSERTED, or REMOVED.
- You can view and restore previous versions of individual sections.

**Section Dependencies:**
- Sections can have dependencies on other sections.
- Dependency types: REFERENCES, DEPENDS_ON, RELATED_TO.
- When editing a section, you can see which other sections reference it.

### 11.3 Real-Time Collaboration

DocEngine supports real-time collaborative editing via SignalR WebSocket connections.

**How It Works:**
- When multiple users open the same document, they enter a collaborative editing session.
- Each user's cursor position is visible to others (with color coding).
- Changes made by one user appear in real-time for all other editors.
- A user list shows all active collaborators and their current position in the document.

**Collaboration Features:**
- **Live Cursor Tracking**: See where other users are editing in real-time.
- **Presence Indicators**: Online/offline status for each collaborator.
- **Conflict Resolution**: If two users edit the same section simultaneously, a conflict resolution dialog appears allowing you to choose which version to keep or merge changes.
- **Viewport State**: The system tracks each user's viewport position.

### 11.4 Comments & Annotations

**Adding Comments:**
1. In the document editor, click the **"Comments"** button in the toolbar.
2. The comments sidebar opens.
3. Click **"Add Comment"** or select text and click the comment icon.
4. Enter your comment (up to 2000 characters).
5. Optionally specify a **line number** for inline comments.
6. Click **"Post"**.

**Managing Comments:**
- View all comments in the sidebar.
- Reply to comments (threaded discussions).
- **Resolve** comments when the issue is addressed.
- Delete your own comments.

**Comment Features:**
- Comments can be attached to specific document versions.
- Inline comments reference specific line numbers.
- Resolved comments are hidden by default (toggle to show).

### 11.5 Exporting Documents

Generated documents can be exported in various formats:

1. Open the document.
2. Click **"Export"** from the action menu or toolbar.
3. Select the export format:

| Format | Description |
|--------|-------------|
| **PDF** | Portable Document Format — preserves formatting |
| **Markdown** | Raw Markdown text |
| **DOCX** | Microsoft Word format |

4. The file is generated and downloaded to your device.

---

## 12. Document Conversion

### 12.1 Converting Epics to Milestones

AI-generated Epics can be converted into project milestones for project planning:

1. Navigate to the generation session that produced Epics.
2. Click **"Convert to Milestones"**.
3. A **preview** appears showing:
   - Number of Epics to convert.
   - Proposed milestone structure.
   - Estimated timeline.
   - Milestone assignments.
4. Review and adjust the proposed milestones.
5. Click **"Convert"** to create the milestones.

**What's Created:**
- Project milestones linked to the source Epics.
- Traceability links from milestones back to Epics and BRD requirements.
- Timeline estimates based on Epic scope.

### 12.2 Converting User Stories to Tasks

AI-generated User Stories can be converted into actionable development tasks:

1. Navigate to the generation session with User Stories.
2. Click **"Convert to Tasks"**.
3. A **preview** appears showing:
   - Number of stories to convert.
   - Proposed task breakdown.
   - Sprint distribution suggestions.
   - Resource assignment suggestions.
4. Review and adjust the proposed tasks.
5. Click **"Convert"** to create the tasks.

**What's Created:**
- Development tasks linked to source User Stories.
- Sprint-level distribution.
- Traceability links throughout the hierarchy.

### 12.3 Format Conversions

DocEngine supports converting between document formats:

| Conversion | Description |
|-----------|-------------|
| **Markdown → PDF** | Convert Markdown content to formatted PDF |
| **PDF → Markdown** | Extract text from PDF and convert to Markdown |
| **DOCX → Markdown** | Convert Word documents to Markdown format |

**How to Convert:**
1. Open the document or navigate to the conversion tool.
2. Select the source format and target format.
3. Upload or select the source file.
4. Click **"Convert"**.
5. Download the converted file.

---

## 13. GraphRAG & Knowledge Graph

### 13.1 What is GraphRAG?

**GraphRAG** (Graph-based Retrieval-Augmented Generation) is an advanced feature that builds a knowledge graph from your project documents. It enables:

- **Entity Extraction**: Automatically identify actors, systems, requirements, and other entities in your documents.
- **Relationship Mapping**: Discover and visualize relationships between entities.
- **Cross-Document Intelligence**: Find connections and dependencies across multiple documents and projects.
- **Impact Analysis**: Understand how changes in one document affect others.

### 13.2 Building a Knowledge Graph

1. Navigate to the **GraphRAG** section in the AI Generation tab.
2. Click **"Build Graph"** for a generation session.
3. The system processes all generated documents:
   - Extracts named entities (actors, systems, components, etc.).
   - Maps relationships between entities.
   - Creates traceability links.
4. Progress is tracked in real-time.
5. Once complete, the knowledge graph is available for querying.

### 13.3 Querying the Knowledge Graph

After a graph is built, you can query it:

1. Open the GraphRAG section.
2. Enter a natural language query (e.g., "What systems depend on the payment gateway?").
3. The system searches the graph and returns relevant entities, relationships, and document references.
4. Results include:
   - Matching entities with metadata.
   - Relationship paths between entities.
   - Source document references.

### 13.4 Cross-Project Intelligence

For organizations with multiple projects, GraphRAG enables cross-project analysis:

- **Shared Entities**: Identify entities (systems, actors, components) that appear across multiple projects.
- **Impact Analysis**: Understand how a change in one project might affect others.
- **Reuse Opportunities**: Discover similar requirements or components across projects.
- **Dependency Mapping**: Visualize cross-project dependencies.

---

## 14. Search & Discovery

### 14.1 System-Wide Search

The search bar in the top header provides system-wide search capabilities:

1. Click the **search bar** or press the `/` key to focus it.
2. Type your search query.
3. Results are grouped by category:
   - **Documents**: Matching documents by name or content.
   - **Projects**: Matching projects by name or description.
   - **Users**: Matching team members by name or email.
   - **Workspaces**: Matching workspaces by name.
4. Click any result to navigate directly to it.

### 14.2 Search Suggestions

As you type in the search bar, DocEngine provides real-time suggestions:

- **Auto-complete**: Suggestions based on existing document and project names.
- **Recent Searches**: Your recent search queries.
- **Popular**: Frequently searched items in your workspace.

### 14.3 Recent Activity

Access recent activity from the search:

1. Click the search bar without typing.
2. A dropdown shows your **recent activity**:
   - Recently viewed documents.
   - Recently accessed projects.
   - Recent generation sessions.
3. Click any item to navigate to it.

---

## 15. Notifications

### 15.1 Notification Center

The notification center is accessible from the **bell icon** in the top header bar.

**Notification Display:**
- A **badge** on the bell icon shows the count of unread notifications.
- Click the bell to open the notification dropdown.
- Each notification shows:
  - **Title**: Brief description of the event.
  - **Message**: Detailed notification text.
  - **Time**: When the notification was created (relative time, e.g., "5 minutes ago").
  - **Priority Badge**: Visual indicator for high-priority notifications.

### 15.2 Real-Time Notifications

DocEngine uses SignalR WebSocket connections to deliver notifications in real-time:

- **Instant Delivery**: Notifications appear immediately without page refresh.
- **Desktop Notifications**: Browser push notifications for important events (if enabled).
- **Sound Alerts**: Optional audio alerts for high-priority notifications.

### 15.3 Email Notifications

For critical events, DocEngine sends email notifications:

- **Workspace Invitations**: Email with invitation link and details.
- **Document Approvals**: Notification when a document is approved or rejected.
- **Generation Completion**: Email when AI generation finishes.
- **Password Reset**: Password reset link emails.
- **Welcome Email**: Sent after successful signup.

### 15.4 Notification Preferences

Configure notification preferences in **Settings > General**:

| Setting | Description |
|---------|-------------|
| **Email Notifications** | Enable/disable email notifications |
| **Browser Notifications** | Enable/disable browser push notifications |
| **Weekly Digest** | Receive a weekly summary email |

### 15.5 Managing Notifications

**Marking as Read:**
- Click on a notification to mark it as read.
- Click **"Mark All as Read"** to clear all unread notifications.

**Deleting Notifications:**
- Click the delete button on individual notifications to remove them.

**Notification Statistics:**
- View notification statistics (total, unread, by category) in the notification settings.

**Notification Categories:**

| Category | Examples |
|----------|---------|
| **Tasks** | Task assigned, task completed, task overdue |
| **Milestones** | Milestone approaching, milestone completed |
| **Team** | Member added, member removed, role changed |
| **Approvals** | Document approval requested, approved, rejected |
| **Mentions** | You were mentioned in a comment or document |
| **Files** | Document uploaded, document shared, version created |
| **System** | Subscription changes, security alerts, budget warnings |

**Priority Levels:**
- **Low**: Informational (gray).
- **Normal**: Standard events (blue).
- **High**: Important actions needed (orange).
- **Urgent**: Critical issues requiring immediate attention (red).

---

## 16. User Profile

### 16.1 Viewing Your Profile

1. Click your **avatar** or **name** in the sidebar footer.
2. Select **"Profile"**.
3. Your profile page shows all your personal information.

### 16.2 Editing Your Profile

1. On your profile page, click **"Edit Profile"** in the top-right corner.
2. Editable fields include:

| Field | Description |
|-------|-------------|
| **Full Name** | Your display name (required) |
| **Job Title** | Your position or title |
| **Department** | Your team or department |
| **Phone Number** | Contact phone number |
| **Time Zone** | Your local timezone (for accurate date/time display) |
| **Language** | Interface language preference |
| **Bio** | A brief bio or description (max 500 characters) |
| **Skills** | Your skills and expertise (stored as tags) |
| **Hourly Rate** | Your hourly billing rate (optional) |
| **Hourly Rate Currency** | Currency for your hourly rate |

3. Click **"Save Changes"** to apply.
4. Click **"Cancel"** to discard changes.

### 16.3 Profile Photo

**Uploading a Profile Photo:**
1. While in edit mode on your profile page, click the **camera icon** on your avatar.
2. Select an image file from your device.
3. The photo is uploaded and immediately displayed.

**Removing a Profile Photo:**
1. Navigate to your profile.
2. Click the **remove photo** option.
3. Your avatar reverts to the default (initials-based).

### 16.4 Profile Visibility

Control who can see your profile information:

| Visibility | Description |
|-----------|-------------|
| **Public** | Anyone in the organization can view your full profile |
| **Team Only** | Only members of your workspaces can see your profile (default) |
| **Private** | Only you can see your full profile details |

To change visibility:
1. Edit your profile.
2. Select the desired **Profile Visibility** option.
3. Save changes.

### 16.5 Changing Your Password

1. Navigate to **Settings > Security** or your profile page.
2. Click **"Change Password"**.
3. Enter your **current password**.
4. Enter your **new password** (must meet security requirements).
5. Confirm the new password.
6. Click **"Update Password"**.

---

## 17. Settings & Configuration

### 17.1 Settings Hub Overview

The Settings page is organized into multiple tabs, accessible from the sidebar navigation. Available tabs depend on your role and permissions.

**Accessing Settings:**
1. Click **"Settings"** in the sidebar.
2. Use the horizontal tab navigation to switch between settings categories.
3. Scroll left/right if there are more tabs than fit on screen (arrow buttons provided).

### 17.2 General Settings

Available to all workspace members.

| Setting | Description |
|---------|-------------|
| **Workspace Name** | Display name for the workspace |
| **Description** | Brief workspace description |
| **Timezone** | Default timezone for the workspace |
| **Date Format** | How dates are displayed |
| **Week Starts On** | First day of the week (Sunday or Monday) |
| **Logo** | Workspace logo |
| **Theme** | Light or Dark mode |
| **Language** | Interface language |
| **Notifications** | Email and browser notification toggles |
| **Weekly Digest** | Weekly summary email toggle |

### 17.3 Organization Settings

Available to Organization Admins and Owners.

| Setting | Description |
|---------|-------------|
| **Organization Name** | Top-level entity name |
| **Description** | Organization description |
| **Website** | Company website URL |
| **Logo** | Organization logo (displayed in shared documents and branding) |

### 17.4 Security Settings

Manage account and workspace security.

| Feature | Description |
|---------|-------------|
| **Password Management** | Change password, enforce password policies |
| **Session Management** | View active sessions, IP addresses, and devices |
| **Login History** | See recent login activity with timestamps and locations |
| **Device Management** | Review and revoke device access |

### 17.5 AI Usage & Analytics

View comprehensive analytics about AI generation usage.

**Analytics Dashboard:**
- **Usage Charts**: Visual representation of AI generation activity over time (using Recharts).
- **Cost Breakdown**: Per-project and per-operation cost analysis.
- **Token Usage**: Input vs. output token consumption trends.

**Breakdown Views:**

| View | Description |
|------|-------------|
| **By User** | AI usage per team member |
| **By Project** | AI costs per project |
| **By Use Case** | Usage by document type (BRD analysis, SRD generation, etc.) |
| **By Operation** | Usage by operation type |
| **By Model** | Usage by LLM model |

**Additional Features:**
- **Date Filtering**: Filter analytics by date range.
- **Export**: Export usage data for external analysis.
- **Provider Breakdown**: See which LLM provider was used for each operation.
- **Detailed Logging**: View individual API call logs with token counts and costs.

### 17.6 LLM Provider Configuration

Configure the Large Language Model providers used for AI generation.

**Provider Management:**
1. Navigate to **Settings > LLM Providers**.
2. View all configured providers.

**Adding a Provider:**
1. Click **"Add Provider"**.
2. Configure:

| Field | Description |
|-------|-------------|
| **Provider Name** | Display name |
| **Provider Type** | OpenRouter, OpenAI, Azure OpenAI, Anthropic, or Custom |
| **API Endpoint** | The provider's API URL |
| **API Key** | Your API key (stored encrypted) |
| **Model Name** | The specific model to use (e.g., "gpt-4", "claude-3") |
| **Max Tokens** | Maximum token context window |
| **Max Output Tokens** | Maximum tokens per response |
| **Temperature** | Creativity parameter (0.0 = deterministic, 1.0 = creative) |
| **Cost per 1K Input Tokens** | Pricing for input tokens |
| **Cost per 1K Output Tokens** | Pricing for output tokens |
| **Rate Limits** | Requests per minute, tokens per minute, daily token limit |
| **Priority** | Provider priority order (lower = higher priority) |

3. Click **"Test Connection"** to verify the provider works.
4. Click **"Save"** to add the provider.

**Provider Health:**
- Each provider has a health status (Healthy/Unhealthy).
- Automatic health checks run periodically.
- Unhealthy providers are skipped in favor of healthy alternatives.
- Fallback providers are used when the primary provider fails.

**Load Balancing:**
DocEngine intelligently distributes AI requests across providers based on:
- Provider priority.
- Current health status.
- Token capacity.
- Cost optimization.
- Rate limit availability.

### 17.7 Admin Controls for AI

Organization administrators can configure AI behavior:

| Control | Default | Description |
|---------|---------|-------------|
| **Allow Quality Gate Override** | No | Whether users can bypass quality gates |
| **Require Admin Approval for Overrides** | No | Whether overrides need admin approval |
| **Document Approval Workflow** | Single | Approval type: "single" or "multi" |
| **Log Quality Gate Decisions** | Yes | Track all quality gate pass/fail decisions |
| **Log Generation Sessions** | Yes | Track all generation session details |
| **Log Cost Transactions** | Yes | Track all AI-related costs |

### 17.8 Audit Logs

View a comprehensive audit trail of all activities in the workspace.

**Audit Log Table:**
- **Action**: What was done (create, update, delete, login, etc.).
- **Entity Type**: What was affected (Project, Document, User, etc.).
- **User**: Who performed the action.
- **Timestamp**: When it occurred.
- **Details**: Additional metadata (IP address, user agent, etc.).

**Filtering:**
- Filter by user.
- Filter by action type.
- Filter by date range.
- Filter by entity type.

**Export:**
- Export audit logs for compliance or analysis.

---

## 18. Subscription & Billing

### 18.1 Subscription Plans Overview

DocEngine offers four subscription tiers, managed at the **Organization** level:

| Feature | Starter (Free) | Professional ($39/mo) | Business ($99/mo) | Enterprise ($299/mo) |
|---------|---------------|----------------------|-------------------|---------------------|
| **Projects** | 3 | 10 | 50 | Unlimited |
| **AI Generations/Month** | 3 | 25 | 100 | Unlimited |
| **Workspaces** | 1 | Multiple | Multiple | Unlimited |
| **Team Members** | Limited | Extended | Large teams | Unlimited |
| **Quality Gates** | Basic | Advanced | Full | Full + Custom |
| **Support** | Community | Email | Priority | Dedicated |
| **Analytics** | Basic | Standard | Advanced | Custom |

**Annual Billing Discount:**
Choosing annual billing saves approximately **17%** — you pay for 10 months and get 12 months of service.

### 18.2 Viewing Your Current Subscription

1. Navigate to **Settings > Subscription**.
2. View your current plan details:
   - **Plan Name & Tier**: Your active subscription plan.
   - **Status**: Active, Past Due, Cancelled, or Expired.
   - **Billing Cycle**: Monthly or Annual.
   - **Current Period**: Start and end dates of the current billing period.
   - **Usage**: AI generations used vs. limit, projects created vs. limit.

### 18.3 Upgrading Your Plan

1. Navigate to **Settings > Subscription** or the **Pricing** page.
2. Review available plans.
3. Click **"Upgrade"** on the desired plan.
4. Select your **billing cycle** (Monthly or Annual).
5. You'll be redirected to **Stripe Checkout** to complete payment:
   - Enter credit card information.
   - Review the charge amount.
   - Click **"Subscribe"**.
6. Upon successful payment, you're redirected to a **success page**.
7. Your plan is immediately activated with the new limits.

> **Note:** When upgrading, you immediately gain access to the higher plan's features and limits. The billing is prorated for the remainder of the current period.

### 18.4 Billing Cycles

| Cycle | Description |
|-------|-------------|
| **Monthly** | Billed every month on the subscription start date |
| **Annual** | Billed once per year with ~17% discount |

**Billing Events:**
- **checkout.session.completed**: Initial subscription activation.
- **invoice.payment_succeeded**: Successful recurring payment.
- **invoice.payment_failed**: Failed payment (account marked as Past Due).
- **customer.subscription.updated**: Plan change (upgrade/downgrade).
- **customer.subscription.deleted**: Subscription cancellation (reverts to Starter).

### 18.5 Usage Monitoring

Monitor your subscription usage in real-time:

**AI Generations:**
- Current month's usage vs. plan limit.
- Progress bar showing utilization percentage.
- Warning when approaching the limit (80% threshold).
- Block notification when limit is reached.

**Projects:**
- Active project count vs. plan limit.
- Warning when approaching the limit.

**Viewing Usage:**
1. Navigate to **Settings > Subscription > Usage**.
2. See detailed usage for the current billing period:
   - AI generations used / limit.
   - Projects count / limit.
   - Period start and end dates.
   - Whether limits are unlimited (Enterprise plan).

### 18.6 Payment & Invoices

All payments are processed securely through **Stripe**:

- **Payment Methods**: Credit cards (Visa, MasterCard, American Express, etc.).
- **Invoices**: Generated automatically for each payment.
- **Payment History**: View all past transactions with amounts, dates, and statuses.
- **Payment Statuses**: Pending, Completed, Failed, Refunded.

**If Payment Fails:**
1. Your account is marked as **Past Due**.
2. You receive a notification about the failed payment.
3. You have a grace period to update your payment method.
4. If payment remains unresolved, the subscription may be downgraded to the Starter plan.

---

## 19. Permissions & Access Control

### 19.1 Role Hierarchy

DocEngine implements a multi-level role-based access control (RBAC) system:

```
Organization Level
  └─ Workspace Level
       └─ Project Level
            └─ Document Level
```

Permissions are inherited downward, meaning organization-level roles grant baseline access that can be augmented at lower levels.

### 19.2 Organization Roles

| Role | Description |
|------|-------------|
| **Owner** | Full control over the organization. Can manage subscriptions, all workspaces, and all members. Cannot be removed. |
| **Admin** | Can manage organization settings, workspaces, and members. Cannot modify the Owner role. |
| **Member** | Basic organization membership. Access is primarily controlled at the workspace level. |

### 19.3 Workspace Roles

| Role | Description | Key Capabilities |
|------|-------------|-----------------|
| **Owner** | Full workspace control | All permissions, member management, settings, deletion |
| **Admin** | Administrative access | Member management, settings, project creation, AI generation |
| **Manager** | Team management | Project management, document review, member oversight |
| **Member** | Standard access | View projects, upload documents, basic AI operations |
| **Custom** | Tailored access | Specific permission set defined by Admin |

### 19.4 Project Roles

| Role | Description |
|------|-------------|
| **Admin** | Full project control — settings, members, documents, AI generation |
| **Manager** | Manage project workflow, review documents, coordinate team |
| **Lead** | Lead specific project areas, approve documents |
| **Member** | View project content, contribute documents, participate in generation |

### 19.5 Document Access Levels

| Level | Permissions |
|-------|-------------|
| **Owner** | Full control — edit, delete, share, manage collaborators, manage versions |
| **Editor** | View and edit document content, add comments, upload new versions |
| **Commenter** | View content and add comments (no editing) |
| **Viewer** | Read-only access (no comments or editing) |

### 19.6 Custom Roles

Administrators can create custom roles with specific permission sets:

1. Navigate to **Settings > Roles & Permissions** (Admin access required).
2. Click **"Create Custom Role"**.
3. Enter a **role name** and **description**.
4. Select the permissions to include from the available permission categories:
   - **Project Permissions**: Create, read, update, delete projects.
   - **Document Permissions**: Upload, edit, delete, share, approve documents.
   - **Member Permissions**: Invite, remove, change roles.
   - **AI Permissions**: Start generation, configure providers, manage budgets.
   - **Settings Permissions**: Modify workspace and organization settings.
5. Click **"Save"**.
6. Assign the custom role to users.

---

## 20. Frequently Asked Questions (FAQ)

### Account & Access

**Q: Can I belong to multiple organizations?**
A: Yes. You can be invited to join additional organizations by other users. Use the workspace switcher to navigate between them.

**Q: What happens if I forget my password?**
A: Use the "Forgot Password" link on the login page. A reset link will be sent to your registered email address.

**Q: Can I change my email address?**
A: Email addresses are currently read-only after signup, as they serve as your unique identifier. Contact your administrator if you need to change your email.

**Q: What happens when my session expires?**
A: DocEngine automatically refreshes your session using refresh tokens. If both tokens expire (after extended inactivity), you'll be redirected to the login page.

### Projects

**Q: What's the difference between project visibility options?**
A: **Private** projects are only visible to assigned project members. **Team** projects are visible to all workspace members. **Public** projects are visible to all organization members.

**Q: Can I move a project between workspaces?**
A: Projects belong to a specific workspace. To move a project, you would need to create a new project in the target workspace and transfer the documents.

**Q: What happens when I reach my project limit?**
A: You'll receive a notification and won't be able to create new projects until you either delete existing ones or upgrade your subscription plan.

### AI Generation

**Q: How long does AI generation take?**
A: Generation time depends on the BRD size and complexity. A typical BRD produces a full document suite in 5-15 minutes. Larger documents may take longer.

**Q: What happens if generation fails mid-way?**
A: DocEngine creates checkpoints at each stage. If a failure occurs, you can resume from the last successful checkpoint without losing progress or re-spending tokens.

**Q: Can I use my own LLM provider?**
A: Yes. DocEngine supports multiple LLM providers including OpenRouter, OpenAI, Azure OpenAI, and Anthropic. You can configure custom providers in Settings > LLM Providers.

**Q: How much does each generation cost?**
A: The average cost is approximately $0.75 per full BRD generation. Actual costs vary based on document size, the LLM provider/model used, and the number of documents generated. All costs are tracked in real-time.

**Q: What file formats can I upload as a BRD?**
A: DocEngine accepts PDF, Word (.doc, .docx), and plain text (.txt) formats for BRD upload. You can also provide a URL to the document.

### Documents

**Q: Are deleted documents permanently removed?**
A: No. DocEngine uses soft deletion — deleted documents are hidden from view but retained in the database. Administrators can recover soft-deleted documents if needed.

**Q: How many versions of a document are kept?**
A: All versions are retained indefinitely. There is no automatic version cleanup. You can view and restore any previous version at any time.

**Q: Can external users view shared documents?**
A: Yes. When you create a share link, anyone with the link can view the document without logging in. You can optionally add password protection and set an expiration date.

### Billing

**Q: When does the Starter plan's free generation count reset?**
A: Free generation credits reset at the beginning of each calendar month.

**Q: What happens if my payment fails?**
A: Your account is marked as "Past Due" and you receive a notification. You'll have a grace period to update your payment method. If unresolved, your subscription may be downgraded to the free Starter plan.

**Q: Can I downgrade my plan?**
A: Plan changes take effect at the next billing cycle. If your current usage exceeds the lower plan's limits, you'll need to reduce your usage before the downgrade takes effect.

---

## 21. Glossary

| Term | Definition |
|------|-----------|
| **BRD** | Business Requirements Document — the primary input document for AI generation |
| **SRD** | System Requirements Document — technical system requirements derived from the BRD |
| **FRS** | Functional Requirements Specification — detailed functional specifications with use cases |
| **Epic** | A large body of work that can be broken down into smaller User Stories |
| **User Story** | A description of a feature from an end-user perspective |
| **Task** | A specific, actionable piece of development work |
| **Generation Session** | A single run of the AI document generation pipeline |
| **Quality Gate** | An automated quality check that must pass before generation proceeds |
| **Checkpoint** | A saved state during generation that allows recovery from failures |
| **GraphRAG** | Graph-based Retrieval-Augmented Generation — uses knowledge graphs for enhanced AI context |
| **Traceability** | The ability to trace requirements from BRD through all generated documents |
| **LLM** | Large Language Model — the AI model used for document generation |
| **Token** | The unit of text processing used by LLMs (roughly 4 characters = 1 token) |
| **Soft Delete** | A deletion method where records are marked as deleted but not physically removed |
| **RBAC** | Role-Based Access Control — permission system based on user roles |
| **SignalR** | Real-time communication technology for live updates and collaboration |
| **Multi-Tenant** | Architecture supporting multiple isolated organizations on a shared platform |
| **OTP** | One-Time Password — a temporary code used for email verification |
| **JWT** | JSON Web Token — the authentication token format used for API access |
| **Workspace** | A collaborative space within an organization for organizing projects and teams |
| **Organization** | The top-level entity representing a company or team |

---

*This user manual covers DocEngine version as of March 2026. Features and interfaces may evolve with future updates. For the latest information, please contact your system administrator or refer to the in-app help resources.*
