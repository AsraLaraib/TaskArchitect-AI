# Project Title: TaskArchitect AI — AI-Powered Project Planning and Execution Assistant

## 1. Abstract

Independent software development projects and academic capstones often stall during the initial planning phase. Students and solo developers frequently possess clear project ideas but struggle to break them down into smaller actionable steps, identify technical dependencies, and structure a logical workflow. While traditional project management tools require manual setup and general-purpose chat tools only provide static text advice, an execution gap remains between ideation and tracking.

This project presents **TaskArchitect AI**, a web-based application designed to bridge this gap by transforming raw project descriptions into structured, trackable execution roadmaps. The system allows users to input their project goals, which are then analyzed by a Large Language Model to generate organized milestones, actionable tasks, and basic task dependencies. The validated data is stored in a relational database and presented through a clean dashboard where users can monitor progress, update task statuses, and track overall completion metrics.

Additionally, TaskArchitect AI includes a simple contextual assistant that leverages the active project state to answer implementation questions and provide targeted guidance. The role of artificial intelligence is focused strictly on structured data generation and contextual support rather than autonomous project management. By combining automated roadmap structuring with simple progress tracking, TaskArchitect AI offers a realistic, technically sound implementation that minimizes project overwhelm and provides a practical workspace for academic evaluation and real-world utility.

---

# 2. Project Overview

## 2.1 Introduction and Background

When students and solo developers begin a software project, they often face a steep planning curve. Moving from a broad conceptual idea to concrete development steps requires defining technical milestones, organizing task sequences, and understanding dependencies. Without a structured starting plan, individuals frequently encounter delays, scope creep, and disorganization.

TaskArchitect AI is designed to simplify this process by offering an automated planning and tracking environment tailored for individual builders.

## 2.2 Problem Statement

The primary challenges faced by solo creators include:

- Difficulty breaking down a large project concept into logically ordered milestones and smaller actionable tasks.
- Uncertainty regarding task dependencies, such as knowing what must be built before database or API integration can begin.
- The limitation of existing tools: traditional project management software requires manual configuration from scratch, while standard AI text generators do not connect to interactive workspaces or progress dashboards.

## 2.3 Proposed Solution

TaskArchitect AI provides a centralized web application that automates the transition from a text-based project idea to an interactive milestone-and-task workflow.

By integrating language model processing with structured database management, the system generates an organized roadmap with prerequisites, displays it on a progress dashboard, and allows users to actively update task statuses and track their progress.

## 2.4 Target Users

- **University Students:** Particularly those undertaking final year capstone projects who need help organizing their software development process.
- **Solo Developers and Indie Creators:** Individuals building independent applications who need a streamlined way to manage personal development tasks without the overhead of heavy enterprise tools.

## 2.5 Project Objectives

- To design and implement a secure, user-authenticated web platform for project management.
- To integrate a server-side Large Language Model to parse project descriptions and produce structured, schema-compliant JSON data.
- To implement basic dependency handling where the AI identifies prerequisites and the system warns users if a prerequisite task remains incomplete.
- To develop an interactive dashboard that calculates and displays automatically updated project progress metrics.
- To build a simple context-aware assistant capable of answering task-related questions based on the active user project.

## 2.6 Main Features

- **User Authentication:** Secure registration and login workflows to isolate individual user data.
- **Project Creation Workspace:** A form allowing users to submit project titles and descriptive goals.
- **AI Roadmap Generator:** Backend logic that parses input text into organized milestones, tasks, and dependencies.
- **Relational Roadmap Display:** An organized interface showing project milestones and structured tasks.
- **Task Dependency Handling:** The AI identifies basic prerequisites between tasks, and the system displays these relationships and warns users when a prerequisite task has not been completed.
- **Progress Tracking Dashboard:** Interactive status controls coupled with automatically updated completion metrics.
- **Simple Contextual Assistant:** A built-in helper tool providing targeted clarifications or recommendations based on current project data.

## 2.7 How AI Is Used

Artificial intelligence is utilized in two specific, controlled capacities:

### 1. Roadmap Generation

The LLM receives project descriptions and is restricted through schema validation libraries such as Zod to return clean, structured JSON data containing milestones, tasks, and dependencies rather than unstructured paragraphs.

### 2. Contextual Assistance

The assistant reviews the user's current project tasks and status to provide relevant answers when prompted, helping prevent generalized or off-topic responses.

## 2.8 High-Level System Architecture

**User → Next.js Frontend → Backend / API Layer → LLM Service → JSON Validation → PostgreSQL / Supabase → Roadmap Dashboard**

The user first creates a project and provides a description of their goal. The backend sends the project information to the LLM, which generates structured milestones, tasks, and basic dependencies. The generated response is validated using a schema validation layer such as Zod.

After successful validation, the roadmap is stored in PostgreSQL through Supabase and displayed on the user's dashboard. Users can update task statuses and view their automatically updated project progress. The contextual assistant can access the relevant project information to provide task-related guidance.

## 2.9 Proposed Technology Stack

- **Frontend & Backend Framework:** Next.js (React) using TypeScript for type-safe and unified full-stack development.
- **Styling:** Tailwind CSS for a responsive and modern user interface.
- **Database & Authentication:** PostgreSQL hosted through Supabase, using Supabase Auth for user authentication.
- **AI Integration:** LLM API, such as OpenAI or an equivalent service, combined with Zod for structured output validation.
- **Version Control:** Git and GitHub for source code management.
- **Deployment:** Vercel for deploying the web application.

## 2.10 Expected Outcomes

The expected outcome is a fully functional web application deployed for academic review.

The system will allow students or solo developers to:

- Enter a project description.
- Generate a structured development plan.
- View milestones and tasks.
- Understand basic task dependencies.
- Update task statuses.
- Track project progress.
- Ask the contextual assistant for project-related guidance.

## 2.11 Project Scope and Limitations

### What is Included (In Scope)

- User accounts
- Project creation
- AI-powered JSON roadmap generation
- Basic task dependency warnings
- Interactive task status updates
- Automatically updated progress dashboards
- Simple project-aware assistant

### What is Excluded (Out of Scope / Future Work)

- Multi-agent autonomous frameworks
- Automatic code analysis
- GitHub repository webhooks
- Real-time multi-user team collaboration
- Automatic schedule restructuring upon delays
- Gantt chart visualizers
- Advanced autonomous project management
- Automatic project restructuring

These advanced elements are intentionally omitted to maintain a realistic implementation timeline for a single-semester undergraduate project.
