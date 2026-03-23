# DocEngine Platform - Video Narration Script
### Complete Product Walkthrough

---

## SCENE 1: Sign Up
**Image:** `signup.PNG`

> Welcome to DocEngine, the AI-powered document generation platform that transforms how teams create and manage project documentation.
>
> Getting started is simple. On the Sign Up page, you will see a clean, three-step registration process. In Step One, you enter your professional email address. DocEngine will send you a secure six-digit verification code to confirm your identity. The platform supports language selection with a language dropdown at the top right corner. You will also notice a helpful illustration showcasing DocEngine's AI-powered capabilities right on the sign-up screen.
>
> If you already have an account, you can click "Sign In" at the bottom to go directly to the login page.

---

## SCENE 2: Login
**Image:** `login.PNG`

> Once your account is created, you will land on the Sign In page. Here, you enter your email address and password to access your workspace. DocEngine also offers convenient single sign-on options with Google and Microsoft, making it easy for enterprise teams to get started without creating separate credentials.
>
> You will also see the tagline "Transform Documents with AI" alongside an illustration that highlights DocEngine's core value proposition — effortlessly generate, edit, and manage documents using our intelligent AI-powered document generation platform.
>
> If you have forgotten your password, simply click the "Forgot Password" link to recover your account.

---

## SCENE 3: Forgot Password
**Image:** `forgot-password.PNG`

> In case you forget your password, DocEngine provides a secure password recovery process. Simply enter your registered email address, and the platform will send you a secure reset link. This link expires within one hour for security purposes. The page reassures you with the message "Your Security is Our Priority," emphasizing that your data and account remain fully protected throughout the recovery process.

---

## SCENE 4: User Profile
**Image:** `User Profile.PNG`

> After logging in, you can manage your personal information through the User Profile page. Here, you can view and edit your profile details including your full name, email address, job title, department, phone number, time zone, preferred language, hourly rate, and a personal bio.
>
> Your profile picture is displayed prominently at the top, and you can update it at any time. The "Edit Profile" button in the top-right corner allows you to make changes to your information. Note that your email address is locked after registration and cannot be changed, ensuring account security.

---

## SCENE 5: Subscriptions
**Image:** `Subscriptions.PNG`

> DocEngine offers flexible subscription plans to suit teams of every size. The Subscription page, located within Workspace Settings, displays your current plan along with real-time usage tracking.
>
> Here you can see the current plan is "Starter," which is free forever. The usage section shows AI generation credits and project limits with visual progress bars. When you reach your limits, the bar turns red, signaling that it is time to upgrade.
>
> Below, you will find the available upgrade options: Professional at thirty-nine dollars per month, Business at ninety-nine dollars per month, and Enterprise at two hundred and ninety-nine dollars per month. Each plan includes PDF and DOCX export, community support, and all export formats. You can also click "View full pricing details" for a comprehensive feature comparison.

---

## SCENE 6: LLM Providers
**Image:** `LLM Providers.PNG`

> The LLM Providers page, also within Workspace Settings, is where you configure the AI language models that power your document generation. DocEngine integrates with multiple AI providers through OpenRouter, giving you access to a wide range of models.
>
> At the top, you can see summary cards showing the total number of providers available, how many are currently enabled, and their health status. In this example, there are thirty-five total providers, six are enabled, and all thirty-five are healthy.
>
> Each provider card displays the model name, source platform, maximum token capacity, temperature setting, cost per thousand tokens, health status, and the last health check timestamp. You can enable or disable providers with a simple toggle switch, and add new providers using the "Add Provider" button.

---

## SCENE 7: Project Listing
**Image:** `Project Listing.PNG`

> Now let us move to the core of DocEngine — project management. The Projects page displays all your projects within the current workspace. You can see the workspace name and organization in the breadcrumb navigation at the top.
>
> Projects are displayed in a clean card-based grid view, but you can switch to a list view if you prefer. Each project card shows the project name, unique project code, and its current status indicated by a green "Active" badge. The colored progress bar at the bottom of each card provides a quick visual indicator.
>
> At the top, you have a search bar to quickly find projects, along with filter dropdowns for status and priority. The "Create Project" button in the top-right corner lets you start a new project instantly.

---

## SCENE 8: Create New Project
**Image:** `Create New Project.PNG`

> Creating a new project is straightforward. The "Basic Information" form asks for just the essentials: a project name and a unique project code. You can also upload a project logo by dragging and dropping an image file or clicking to browse. The platform supports PNG, JPG, and GIF formats up to five megabytes.
>
> Once you have filled in the details, simply click "Create Project" to get started. The Back button at the top lets you return to the project listing if needed.

---

## SCENE 9: AI Brain Dashboard
**Image:** `AI Brain Dashboard.PNG`

> Inside each project, you will find the AI Brain Orchestration Layer — the intelligent engine that powers document generation. The Dashboard tab provides a comprehensive overview of your project's AI capabilities.
>
> At the top, summary cards display key metrics: active sessions, generated documents this month, BRD quality score, uploaded documents from multi-document sessions, and the number of configured LLM providers.
>
> The Quick Actions section provides one-click access to the most common tasks: Configure LLM Providers, Start Document Generation, Multi-Document Upload, Manage Generated Documents, and the Knowledge Hub. Each action card includes a brief description and status indicators like "Required," "Active," or "New."
>
> Below the quick actions, you will find sections for BRD Quality Validation — which includes quality score analysis, AI enhancement, and quality gate enforcement — as well as Multi-Document Support for document compatibility, selective generation, and cost optimization.

---

## SCENE 10: Choose Generation Method
**Image:** `StartGeneration.PNG`

> When you are ready to generate documents, DocEngine presents you with two generation methods.
>
> The first option is Traditional Generation, which is the most popular choice. With this method, you upload a single Business Requirements Document, and DocEngine generates the complete document suite — including SRDs, FRDs, epics, user stories, and tasks — all in one go. This is best for new projects that need comprehensive documentation.
>
> The second option is Multi-Document Upload, a newer feature designed for teams that already have some documentation in place. This method lets you upload multiple existing documents and selectively generate only what is missing. It supports multiple document types, offers selective generation, and provides cost savings by skipping documents you already have.
>
> A helpful comparison at the bottom clearly explains the difference between the two approaches.

---

## SCENE 11: Multi-Document — Select Documents
**Image:** `MultiDocumentStep.PNG`

> If you choose the Multi-Document Upload method, the process begins with Step One: Select Documents. This guided wizard walks you through six steps: Select Documents, Upload Documents, Validation, Tech Stack, Choose Generation, and Review and Start.
>
> On this screen, you select which documents you already have from the full document hierarchy: Business Requirements Document, Software Requirements Document, Functional Requirements Specification, Epics, User Stories, and Tasks. The BRD is marked as "Required" since it serves as the foundation for all other documents.
>
> A "Show Quick-Start Templates" button is available for common workflow scenarios. At the bottom, the "What happens next?" section clearly outlines the remaining steps in the process.

---

## SCENE 12: Multi-Document — Upload Documents
**Image:** `BRDUpload.PNG`

> In Step Two, you upload your documents. The upload area supports drag-and-drop as well as click-to-browse functionality. DocEngine accepts PDF, DOCX, Markdown, and plain text files, with a generous maximum file size of one hundred megabytes.
>
> Each document is validated automatically upon upload with built-in validation features including format compatibility check, content structure analysis, cross-document consistency validation, and quality assessment with recommendations. Files are processed securely and stored temporarily during the validation phase. Once your documents are uploaded, click "Review Validation" to proceed.

---

## SCENE 13: Generation Templates
**Image:** `Generation Templates.PNG`

> DocEngine offers Quick-Start Templates to streamline your workflow. These pre-configured templates match common project scenarios so you can get started faster.
>
> You can choose from several options: "Upload BRD, Generate Everything" for complete documentation suites, "Upload BRD plus SRD, Generate Remaining" for teams that already have requirements documents, "Upload All Documents, Generate Stories and Tasks Only" for iterative development, and "Upload Epics, Generate Stories and Tasks" for agile teams. Each template card shows which documents are uploaded, which are generated, the document count, estimated cost, and who it is best suited for.
>
> Simply click "Use This Template" on your preferred option to apply it instantly.

---

## SCENE 14: Tech Stack Configuration
**Image:** `TechStack Selection.PNG`

> Step Three is Technology Stack Configuration. This is a powerful feature that ensures your generated documents include accurate technical specifications tailored to your project's technology choices.
>
> You can configure your tech stack manually or start with a predefined template. The manual configuration is organized into four categories: Frontend, Backend, Database, and Infrastructure. For the Frontend tab, you can specify your framework and version, UI library, styling approach, state management solution, and build tool.
>
> There is also an "Additional Technical Notes" field where you can add specific patterns, requirements, or guidelines for the AI to follow — for example, "Use repository pattern" or "All APIs must be RESTful." The AI Suggest button can auto-generate technical recommendations for you.

---

## SCENE 15: Tech Stack — AI Suggestions
**Image:** `TechStack Selection Technical additional Suggestion.PNG`

> Here we see the Infrastructure tab of the Technology Stack Configuration, where you can specify your cloud provider, containerization strategy, CI/CD pipeline, and monitoring solution.
>
> Notice the AI Suggestions section at the bottom — this is one of DocEngine's standout features. Based on your selected technology stack, the AI automatically generates relevant technical recommendations. In this example, the AI suggests implementing CQRS with MediatR, using Entity Framework Core with read/write separation, Redis distributed caching, TanStack Query for real-time updates, SignalR for live notifications, FluentValidation for constraints, Azure Service Bus for asynchronous workflows, and Temporal Tables for audit trails.
>
> You can click any suggestion to add it to your technical notes, or dismiss suggestions that are not relevant.

---

## SCENE 16: Select What to Generate
**Image:** `Select Generation options for Documents.PNG`

> In Step Four, you select which documents to generate. This screen shows the document status clearly — your uploaded BRD is marked as "Uploaded and ready."
>
> Below, the Generation Options section lists all available document types: Software Requirements Document, Functional Requirements Specification, Epics, User Stories, and Tasks. Each document type shows its description and dependency chain. For example, FRS requires the SRD, Epics require FRS, User Stories require Epics, and Tasks require User Stories. Status badges indicate whether a document "Will Generate," is "Skipped," or has a "Dependency Missing."
>
> The Generation Summary at the bottom shows a clear count: documents uploaded, documents to generate, and total documents. Click "Review and Start Generation" when you are ready.

---

## SCENE 17: Review and Start Generation
**Image:** `Review and Start Generation.PNG`

> The final step before generation is the Review Generation Plan. This comprehensive review screen shows everything that will happen during the generation process.
>
> The Generation Plan lists all documents with their status: uploaded, will generate, or skipped. Summary cards show the total count of documents uploaded, documents to generate, and documents skipped.
>
> One of the most valuable features is the Cost Estimate section. DocEngine provides full transparency by showing the estimated total cost, total tokens to be consumed, and the estimated processing time. In this example, generating one Software Requirements Document from a BRD costs approximately fifty-six cents, uses around nineteen thousand tokens, and takes about eleven minutes. It even shows savings from uploaded documents.
>
> When you are satisfied with the plan, click "Start Generation" to begin.

---

## SCENE 18: Generation In Progress
**Image:** `Generation Overview.PNG`

> Once generation begins, you are taken to the Generation tab where you can monitor real-time progress. The Active Generation Session panel shows a live connection status, confirming you are connected to real-time progress updates via SignalR.
>
> The Document Generation Progress section displays the session ID, an overall progress bar showing steps completed, and the current step being processed. You have full control with Pause, Cancel, and Close buttons.
>
> Below, the Generation Steps section provides granular detail for each step in the pipeline: BRD Analysis, System Requirements generation, and Document Saving. Each step shows its own status — pending, in progress, or complete — along with a percentage completion bar.

---

## SCENE 19: Generation Tab Overview
**Image:** `GenerationTab.PNG`

> The Generation tab also provides an overview of all your generation activity. Summary cards at the top display total sessions, completed sessions, currently in-progress sessions, and any failed sessions.
>
> Sub-tabs allow you to switch between Overview, Generation History, and Settings. The Recent Sessions section lists all your document generation sessions with their dates, completion status, and quick action buttons to view details or delete a session. You can also start a new generation session at any time using the "Start New Generation" button.

---

## SCENE 20: Generation Completed
**Image:** `Generation Completed.PNG`

> When generation completes successfully, the overview updates to reflect the results. Here you can see that one session has been completed with zero in progress and zero failed. The recent session entry shows the completion date, a green "Completed" badge, and options to "View Details" or delete the session.
>
> From here, you can navigate to the Documents tab to view your newly generated documents, or start another generation session.

---

## SCENE 21: Documents Tab — Empty State
**Image:** `DocumentsGenerated.PNG`

> The Documents tab within the AI Brain Orchestration Layer is your central hub for managing all AI-generated project documentation. When no documents have been generated yet, you will see a clean empty state with summary cards showing zero for Total Documents, Approved, and Pending Approval.
>
> The tab provides powerful filtering capabilities: search by name, content, or session ID, and filter by document type, approval status, generation status, and date range. You can toggle between Documents and Traceability views, and switch between Grid and List display modes.

---

## SCENE 22: Generated Documents
**Image:** `Generated Documents.PNG`

> After generation completes, the Documents tab populates with your AI-generated documentation. Summary cards now show the total document count and approval status. In this view, you can see two documents: a System Requirements Document and a Business Requirements Document.
>
> Each document card displays the document name, type with a color-coded label, generation status, approval status marked as "Draft," the date it was created, and file size. Action icons at the bottom of each card let you quickly view, download, approve, reject, or edit any document. The colored bar at the bottom of each card distinguishes document types at a glance.

---

## SCENE 23: Generated Documents — Traceability
**Image:** `Generated Documents Traceability.PNG`

> Switching to the Traceability tab reveals a visual representation of document relationships and dependencies. This is a powerful feature that shows how your documents are connected and derived from one another.
>
> The traceability view displays key metrics: total links, high-confidence links, uploaded documents, AI-generated documents, missing documents, and average quality score. You can switch between Graph View for a visual representation and export data to Excel or CSV for external analysis.
>
> The interactive graph shows documents as nodes with connecting lines indicating derivation relationships and confidence levels. In this example, you can see the BRD connected to the SRD with a "Derived From" relationship at one hundred percent confidence.

---

## SCENE 24: Generated Document — Preview
**Image:** `Generated Document View.PNG`

> Clicking on any document opens the full document viewer. At the top, you see the document name, type label, file size, version number, and creation date. Action buttons let you set the document status as Draft, Download it, Share it with team members, Reject it, or Approve it.
>
> The viewer offers two modes: Preview and Sections. The Preview mode displays the full document content with a collapsible Table of Contents on the left side for easy navigation. The table of contents shows a hierarchical structure with sections and subsections — in this SRD example, you can see sections like Document Information, Executive Summary, System Architecture with sub-sections for High-Level Architecture, Component Diagram, and Data Flow, followed by Database Design with detailed entity definitions.
>
> The document header shows key metadata: total word count of over twenty-six thousand words across one hundred and nine sections. You can also search within the document, print it, edit it, and zoom in or out.

---

## SCENE 25: Generated Document — Sections View
**Image:** `Generated Document Section Heading View.PNG`

> The Sections view provides an alternative way to navigate and manage your document. Instead of a continuous preview, it displays the document as a structured hierarchy of sections with numbered headings.
>
> You can filter sections by level — L1 for top-level headings, L2 for sub-sections, L3 and L4 for deeper levels — or view all sections at once. A search bar allows you to quickly find sections by title, number, or content. The "Show Removed" option lets you toggle visibility of any sections that have been removed during editing. Each section is clickable, expanding to show its full content.

---

## SCENE 26: Download Generated Document
**Image:** `Download Generated Document.PNG`

> When you are ready to export your document, the Download dialog offers multiple format options. You can choose PDF, which is recommended for sharing and printing, or DOCX for an editable Word document format. Simply click your preferred format, and the document will be downloaded instantly.

---

## SCENE 27: Document Queue
**Image:** `DocumentQueue.PNG`

> The Queue tab provides visibility into the Quality Analysis Queue, where you can monitor and manage quality analysis jobs for your project. Summary cards show the count of pending jobs, currently processing jobs, completed analyses, failed jobs, and the average processing time.
>
> The Job History section below lists all quality analysis jobs with their details, allowing you to track the status of every analysis that has been performed on your project documentation.

---

## SCENE 28: Knowledge Hub — Overview
**Image:** `Knowledge Hub for Generated Document.PNG`

> The Knowledge Hub is one of DocEngine's most powerful features. It is an intelligent knowledge graph system that automatically creates connections between requirements, features, and implementation artifacts, enabling powerful traceability, impact analysis, and cross-project intelligence.
>
> The overview displays key metrics: Total Nodes in the knowledge graph, Relationships between entities, Traceability percentage, and Coverage percentage. Below, the Knowledge Graph Structure Visualization provides an interactive graph view where you can drag nodes to rearrange, zoom with the mouse wheel, and click on nodes to explore connections. You can switch between Graph Visualization, a Legend and Stats view, and other display options.

---

## SCENE 29: Knowledge Hub — Features
**Image:** `knowledge Hub Features.PNG`

> The Knowledge Hub Features page outlines the key benefits and available analysis tools. At the top, Key Benefits cards highlight Automatic Traceability, Impact Analysis, Coverage Analysis, Cross-Project Search, and Knowledge Reuse.
>
> The Available Features section provides access to six powerful tools: Traceability Analysis for forward and backward traceability links, Impact Analysis to understand the ripple effect of changes, Semantic Search for natural language queries, Coverage Analysis to identify gaps, Cross-Project Intelligence for discovering similar entities across projects, and Graph Statistics for comprehensive knowledge graph metrics.
>
> Below, the page explains what a Knowledge Graph is, its key concepts, and the different entity types it captures including Requirements, Components, Features, Activities, and Constraints.

---

## SCENE 30: Knowledge Hub — Detailed Information
**Image:** `Knowledge Hub More info.PNG`

> This page provides in-depth documentation about the Knowledge Hub's relationship types and how entities are connected. It explains the five relationship types: Requires, Implements, Derives From, Depends On, and Influences.
>
> An example graph structure illustrates how a BRD requirement flows through to features, components, and implementation artifacts. The page also details the automatic graph-building process: Document Generation triggers entity extraction, which leads to relationship detection and traceability linking.
>
> A comprehensive reference table shows what gets extracted from each document type, and the Automatic Traceability Links section maps how different document types trace to one another.

---

## SCENE 31: Coverage Analysis
**Image:** `Coverage Analysis.PNG`

> The Coverage Analysis tool helps identify uncovered requirements and orphaned items in your project to ensure complete implementation. You can configure the analysis by selecting the analysis type — whether to analyze the entire project or a specific document — and set options like including detailed breakdowns by entity type, orphaned items with no relationships, and coverage gaps.
>
> Step-by-step instructions guide you through the process: select the analysis type, provide a project or document ID, configure options, click "Analyze Coverage," and review the results. You can export results as JSON or CSV for further analysis and reporting.

---

## SCENE 32: Cross-Project Intelligence
**Image:** `Cross Project Analysis.PNG`

> Cross-Project Intelligence is a unique feature that lets you discover similar entities across all your projects, get actionable recommendations, and find knowledge reuse opportunities. You can enter an entity ID or select an entity from your project documents.
>
> Configuration options include "Include recommendations" for actionable suggestions based on similar entities, and "Include knowledge reuse suggestions" to discover reusable artifacts, templates, and patterns from other projects.
>
> The How to Use section explains each capability: entity selection, viewing similar entities with similarity scores, getting recommendations, and discovering knowledge reuse opportunities like templates and patterns that can be adapted for your current project.

---

## SCENE 33: Impact Analysis
**Image:** `Impact Analysis.PNG`

> Impact Analysis is essential for understanding the ripple effects of changes before they are made. This tool analyzes the impact of requirement or feature changes on dependent entities, helping teams make informed decisions about modifications.
>
> You configure the analysis by entering an entity ID, selecting the change type — such as Update, Delete, or Add — choosing the analysis type, and providing an optional change description. Advanced options include generating a visualization graph and calculating impact severity.
>
> You can select entities directly from your project documents, with each document clearly labeled by type. This ensures you always analyze the right component before making changes.

---

## SCENE 34: Semantic Search
**Image:** `Semantic Search.PNG`

> Semantic Search enables you to search for requirements and features using natural language queries. Unlike keyword-based search, semantic search understands the meaning behind your query, finding related entities even when exact keywords do not match.
>
> You configure your search by entering a natural language query — for example, "user authentication requirements" — selecting the search type, and setting a results limit. Options to include recommendations and knowledge reuse suggestions are available when providing an Entity ID.
>
> The How to Use section explains three search modes: Semantic Search for meaning-based queries, Cross-Project Search for finding similar entities across projects, and Similarity Search for finding entities similar to a specific one. Tips are provided to help you get the most relevant results.

---

## SCENE 35: Traceability Analysis
**Image:** `Traceability Analysis.PNG`

> Traceability Analysis provides forward and backward traceability links between requirements, features, components, and implementation artifacts. This ensures full visibility into how high-level requirements flow down to implementation details.
>
> Configuration options include the analysis type — entity-level or document-level — traceability direction (forward, backward, or bidirectional), and maximum depth up to five levels. The "Include visualization graph" option generates an interactive visual representation of the traceability chain.
>
> You can select entities from your project to analyze, with each entity showing its name, ID, and description. This makes it easy to trace any requirement or feature through the entire documentation hierarchy.

---

## SCENE 36: Graph Statistics
**Image:** `Graph Statistics.PNG`

> The Graph Statistics page provides comprehensive analytics about your knowledge graph structure. Summary cards display the total number of nodes, total relationships, traceability completeness percentage, and coverage percentage.
>
> The detailed breakdown section offers three views: Nodes by Type, Relationships by Type, and Overview. The Nodes by Type view shows a complete breakdown of every entity type in your knowledge graph — from Actors and Functional Requirements to Components, Features, and specific entities like Role-Based Access Control or User Profile Management. Each entry shows the count and percentage with a visual bar chart.
>
> You can refresh the statistics at any time, and export the data in JSON or CSV format for external reporting. The last updated timestamp ensures you always know how current your data is.

---

## CLOSING

> That concludes our complete walkthrough of the DocEngine platform. From secure sign-up and flexible subscription plans, through intelligent AI-powered document generation with full technology stack awareness, to the advanced Knowledge Hub with its traceability analysis, impact assessment, semantic search, and cross-project intelligence — DocEngine transforms the way teams create, manage, and understand their project documentation.
>
> Get started today and experience the future of AI-powered document generation.

---

*Script prepared for DocEngine Platform Demo Video*
*Total Scenes: 36 + Closing*
