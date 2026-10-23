TaskArchitect AI
An AI-Assisted Project Planning and Adaptive Replanning System with Deadline-Aware Scope Optimization
TaskArchitect AI is a web application that helps students and solo developers turn a software idea into a realistic, dependency-aware plan. It combines AI-assisted task generation with deterministic planning algorithms that analyze dependencies, estimate deadline feasibility, recommend a deliverable minimum viable product (MVP), and replan when progress changes.
Core idea: The AI proposes a plan; the deterministic planning engine analyzes it and makes scope-optimization decisions. Users review and control the final plan.

Table of Contents
- Problem
- Proposed Solution
- Target Users
- Objectives
- Key Features
- How It Works
- Core Technical Contribution
- Technology Stack
- High-Level Architecture
- Proposed Data Model
- Testing and Evaluation
- Development Roadmap
- Scope and Limitations
- Risks and Mitigation
- Expected Deliverables
- Project Status
Problem
Students and solo developers often have a project idea but struggle to turn it into a plan they can realistically finish. Common challenges include:
- Breaking a broad idea into manageable tasks and milestones.
- Identifying task dependencies and the correct build order.
- Estimating effort and checking whether a deadline is realistic.
- Choosing which features belong in the first usable version.
- Adjusting scope when actual progress differs from estimates.
Traditional task-management tools help users record work, while AI plan generators can produce an initial task list. However, a generated list does not necessarily fit the user's available time, and a static plan may become unrealistic as work slips.
Proposed Solution
TaskArchitect AI combines three layers:
1. AI-assisted plan generation: Proposes milestones, tasks, effort estimates, priorities, and dependencies from a project description.
2. Deterministic planning engine: Validates the plan, analyzes dependencies and the critical path, checks deadline feasibility, and recommends a high-value feature scope that fits the available capacity.
3. Progress tracking and adaptive replanning: Uses recorded progress and changed estimates to recalculate the remaining plan, explaining what changed and why.
The AI-generated plan is reviewable and editable. The application must also support manual plan entry if the AI service is unavailable.
Target Users
The first version focuses on:
- Computer science students, including FYP teams.
- Solo developers and beginners building software projects.
- Small student teams.
The primary planning assumption is a single developer. An optional parallel-workers setting may support small teams, but complex multi-person resource scheduling is outside the initial scope.
Objectives
- Let users enter a project description, deadline, weekly working hours, and desired features.
- Generate milestones, tasks, estimates, priorities, and dependencies with an LLM.
- Validate AI output against a strict schema before saving.
- Visualize plans through a dashboard, timeline, and interactive dependency graph.
- Detect dependency cycles and calculate a valid build order and critical path.
- Assess deadline risk using available capacity.
- Recommend a valuable, dependency-valid feature set that fits the time budget.
- Track task status and actual effort.
- Replan when deadlines, estimates, or progress change.
- Evaluate algorithm quality, AI output reliability, replanning behavior, and usability.
Key Features
1. Project Setup
Users enter the project description, deadline, available hours per week, and desired features. Generated data remains editable.
2. AI-Assisted Planning
An LLM proposes milestones, tasks, effort estimates, priorities, and dependencies in a defined JSON structure. The application validates the response and labels AI-suggested fields for user review.
3. Project Dashboard and Roadmap
The dashboard summarizes the plan, while timeline and graph views show task order and dependencies.
4. Dependency and Critical-Path Analysis
The planning engine detects cycles, computes a valid topological order, calculates the critical path, and highlights deadline risk.
5. Deadline-Aware Scope Optimization
The optimizer recommends which features to include, postpone, or exclude so the selected scope fits the available time and respects prerequisites. Each recommendation has an explanation.
6. Progress Tracking
Users update task statuses and record actual effort. This information informs later replanning.
7. Adaptive Replanning
When progress falls behind or project constraints change, the system recalculates the remaining plan and shows a comparison with the previous plan.
8. Export and Scenario Comparison
A lower-priority extension may allow users to compare plan scenarios and export plans as JSON or Markdown.
How It Works
1. Describe the project: Enter the idea, deadline, weekly hours, and desired features.
2. Generate a draft: The server asks the LLM for a plan that follows the application's JSON schema.
3. Validate the plan: Check schema correctness, unique task IDs, valid dependency targets, cycles, positive effort estimates, and task-count limits.
4. Review and edit: The user corrects the proposed plan before saving it.
5. Analyze the plan: The deterministic engine calculates dependencies, build order, critical path, capacity, and deadline risk.
6. Optimize scope: The optimizer selects a high-value set of features that fits the time budget while including required prerequisites.
7. Track progress: Users record task status and actual effort.
8. Replan when needed: The system recalculates the remaining scope and schedule, then explains changes from the previous plan.
If validation fails, the system can retry with validation errors as feedback. After a bounded number of attempts, it should allow manual repair rather than relying indefinitely on the LLM.
Core Technical Contribution
AI plan generation alone is not the claimed research contribution. The proposal centers on two testable components.
A. Dependency-Aware Scope Optimization
The system selects a set of features that maximizes total feature value within the available effort budget. A selected feature must include all its prerequisites.

This is a form of the precedence-constrained knapsack problem, which is NP-hard in general.

The planned implementation compares:
- Greedy heuristic: Ranks candidate features using the value of their full prerequisite closure relative to its effort.
- Exact solver: Uses branch-and-bound or bitmask search for small instances to provide a comparison baseline.
Each feature receives a reason for its outcome, such as:
- Selected for the recommended scope.
- Included as a prerequisite of another selected feature.
- Postponed because it does not fit the available capacity.
- Excluded because a prerequisite is excluded.
B. Adaptive Replanning
Replanning is triggered by changes such as a deadline update, revised estimates, or falling behind schedule. The system:
- Preserves completed work.
- Uses remaining effort for work already in progress.
- Adjusts estimates using observed actual-versus-estimated effort, with smoothing across completed tasks.
- Re-runs optimization against remaining capacity.
- Applies a penalty for changing previously selected features unnecessarily.
- Shows a plan diff explaining what moved in or out and why.
Supporting Algorithms
- Cycle detection: Detects circular dependencies and reports the cycle.
- Topological sorting: Produces a valid task order when the dependency graph is acyclic.
- Critical Path Method (CPM): Uses forward and backward passes to calculate earliest/latest times and slack; a zero-slack chain forms the critical path.
- Deadline-risk analysis: Compares required effort or critical-path duration with available capacity, depending on the planning mode.
Technology Stack
Frontend
- Next.js
- React
- TypeScript
- Tailwind CSS
- React Flow for the interactive dependency graph
Backend
- Next.js API routes using Node.js
- A framework-independent TypeScript planning-engine library
Database and Authentication
- PostgreSQL through Supabase
- Supabase Auth
AI and Validation
- LLM API called server-side
- Structured JSON output
- Zod (or an equivalent schema validator)
- Bounded retry/repair logic
Security note: The LLM API key must remain on the server and must never be exposed to the browser.
High-Level Architecture
User
 |
 v
Next.js / React UI
 |-- Project setup and plan review
 |-- Dashboard and timeline
 |-- Dependency graph
 |-- Progress and plan-diff views
 |
 v
Next.js API routes
 |-- Authentication and request validation
 |-- Server-side LLM integration
 |-- Plan schema validation and repair
 |
 +--------------------------+
 |                          |
 v                          v
Planning Engine          Supabase / PostgreSQL
(TypeScript)              |-- Users
 |                        |-- Projects
 |-- Cycle detection      |-- Tasks
 |-- Topological order    |-- Dependencies
 |-- Critical path        |-- Plan versions
 |-- Deadline risk        |-- Progress logs
 |-- Scope optimizer
 |-- Adaptive replanner
The planning engine should be independently testable without a web framework, network access, or an AI API key. Core algorithms are built and tested using manually entered data before AI integration.
Proposed Data Model
The initial data model includes:
- Project: Project details, deadline, and available capacity.
- Task: Title, type, estimate, value, status, actual effort, and whether the task came from the user or AI.
- Dependency: Relationship between prerequisite and dependent tasks.
- PlanVersion: Timestamp, replanning trigger, selected tasks, capacity, and velocity factor.
- ProgressLog: Task, status change, and recorded hours.
The exact fields and relationships should be finalized during design.
Testing and Evaluation
The proposal evaluates the system using algorithm tests, simulations, AI-output checks, usability testing, and case studies.
Evaluation	Method	Metrics
Greedy optimizer quality	Compare greedy results with an exact solver on random dependency graphs with 10–20 features	Optimality gap, exact-match rate
Scalability	Run graph tests with 20–500 tasks	Runtime relative to tasks and edges
AI plan reliability	Test 15–20 diverse project descriptions	First-try schema-valid rate, cycle rate, retries, human reasonableness rating, estimate error on five real projects
Replanning benefit	Simulate 500 projects with effort deviations and compare static plans with adaptive replanning	Value delivered by deadline, must-have features completed, plan churn
Usability	8–10 participants plan the same sample project with TaskArchitect AI and a spreadsheet or Trello baseline	Task completion time, System Usability Scale (SUS), supervisor-rated plan quality
Real-project applicability	Three case studies: this FYP, an open-source project, and a classmate's project	Qualitative findings and examples


The replanning simulation is the main evidence for the research question. Results should be reported honestly, including cases where adaptive replanning does not improve outcomes.
Testing Strategy
- Unit tests for chains, diamond graphs, disconnected graphs, and cyclic graphs.
- Property tests to ensure recommended scope respects capacity and includes prerequisites.
- Tests to ensure completed work is not incorrectly selected as unfinished during replanning.
- Stubbed malformed LLM responses to test validation and repair without live API calls.
- Component tests for the graph editor and plan-diff view.
Development Roadmap
The proposal outlines a 16-week schedule. It is a planning target and may change after supervisor feedback.
Week	Planned work	Expected output
1	Finalize proposal, literature review, requirements, and assumptions	Approved proposal
2	Wireframes, architecture, data model, repository, Supabase, and authentication	Design document and running skeleton
3–4	Project/task CRUD, dependency graph editor, persistence	Editable plan saved to database
5	Cycle detection, topological order, critical path, deadline-risk logic, unit tests	Analysis engine v1
6	Dashboard, timeline, and risk indicators	Visual roadmap
7	Stabilization and supervisor demo	Manual-input core planner
8–9	Greedy optimizer, capacity input, scope explanations	Optimizer v1
10	Exact solver, benchmark harness, first optimizer experiments	Optimizer evaluation
11	LLM integration, JSON schema, validation/repair, review-before-save	AI planner
12	Progress tracking and actual-effort logging	Planner, optimizer, AI, and tracking integrated
13	Adaptive replanning, velocity factor, churn penalty, plan-diff view	Replanning v1
14	Replanning simulation and AI-quality evaluation	Experimental results
15	Usability study, case studies, and fixes	User-study findings
16	Final testing, deployment, report, and demo rehearsal	Final delivery


Build Order
The core engine comes first; AI is added as an input layer. This reduces the risk that the project becomes blocked by API access, cost, or unreliable model output.
Contingency Plan
If the schedule slips, lower-priority items may be reduced in this order:
1. Scenario comparison and export.
2. Parallel-workers option.
3. Smoothed velocity weighting (use a simple average).
4. Usability study size (reduce to five participants).
The proposal identifies the analysis engine, optimizer, validated AI planner, and replanning simulation as essential components.
Scope and Limitations
Included in the Initial Version
- Web application for students and solo developers.
- AI-assisted plan generation with user review.
- Deterministic dependency and critical-path analysis.
- Deadline-aware scope optimization with explanations.
- Progress tracking and adaptive replanning.
- Tests and evaluation experiments.
Out of Scope
- Resource-constrained scheduling across many people.
- Real-time collaboration.
- Native mobile applications.
- Jira or GitHub integrations.
- Training a custom machine-learning model.
Assumptions and Limitations
- Effort is measured in hours.
- Available capacity is based on hours per week until the deadline.
- The primary mode assumes one developer; parallel workers are optional.
- Feature value scores and effort estimates are subjective. Users can override them, and any mapping from priority to numeric value should be transparent.
- The initial related-work review is preliminary. A deeper academic literature review is required, especially for critical-path methods, precedence-constrained knapsack, release planning, effort estimation, and usability evaluation.
Risks and Mitigation
Risk	Planned mitigation
Scope too large for 16 weeks	Build core functionality first; prioritize features and apply the contingency plan
Project appears to be another AI planner	Center implementation and evaluation on the optimizer and replanner
LLM returns invalid JSON, cycles, or implausible estimates	Schema validation, bounded repair attempts, user review, and honest reliability reporting
Feature values and estimates are subjective	Allow user overrides and explain value scoring transparently
Replanning behavior is difficult to define or evaluate	Fix rules early and evaluate them through simulation
API cost or rate limits	Cache results where appropriate, bound plan size, use a low-cost model, and retain manual entry
Difficulty recruiting participants	Recruit early and reduce study size if necessary, reporting the limitation


Expected Deliverables
- Deployed web application and source repository.
- Planning-engine library with unit tests, benchmarks, and simulation scripts.
- Dataset of sample projects and AI-generated plans used in evaluation.
- FYP report covering literature, design, implementation, and evaluation.
- Demonstration script.
Success Criteria
- All eight objectives are demonstrated in the deployed application.
- Property tests report zero constraint violations for the tested cases.
- Optimizer gap against the exact solver is measured and reported.
- AI plan validity and estimate error are measured with limitations documented.
- Static and adaptive plans are compared in simulation, with results reported regardless of direction.
- Usability results and baseline comparisons are documented.
Preliminary References
- Instagantt AI Assistant
- Planitude
- Agilien — Visual Paradigm
- PlanA
- Mouc
Additional academic references are still required for the Critical Path Method, precedence-constrained knapsack, software release planning, LLM-based effort estimation, and the System Usability Scale (including Brooke, 1996).
Project Status
Status: Proposal / planning stage. Update this section as implementation progresses.
Suggested Next Steps
- [ ] Confirm the project scope and assumptions with the supervisor.
- [ ] Complete the academic literature review and related-work comparison.
- [ ] Finalize requirements and data model.
- [ ] Create wireframes and the initial repository structure.
- [ ] Implement and test the deterministic planning engine with manual data.
- [ ] Add AI plan generation only after the core engine is testable.
This README summarizes the proposed system. Features, algorithms, timeline, and evaluation methods are planned work unless explicitly marked as implemented.
