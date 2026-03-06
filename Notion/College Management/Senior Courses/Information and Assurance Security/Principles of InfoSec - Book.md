
[[Principles of InfoSec-Module1]]
[[Principles of InfoSec-Module2]]
[[Principles of InfoSec-Module3]]

[[Principles of InfoSec-Module5]]
[[Principles of InfoSec-Module8]]
[[Principles of InfoSec-Module9]]
[[Principles of InfoSec-Module10]]
[[Principles of InfoSec-Module11]]
[[Principles of InfoSec-Module12]]


# Project Documentation: ESPTFA ARIMA System

This document outlines the technical architecture, page structure, and the software development methodology utilized to build the ESPTFA ARIMA Educational Analysis System.

---

## 1. Technologies and Tech Stack

The application is built using a modern, decoupled client-server architecture, prioritizing performance, user experience, and scalable data analysis.

### Frontend

- **Core Library**: React (v19)
- **Build Tool / Bundler**: Vite (for rapid development and optimized production builds)
- **Language**: TypeScript (for type safety and enhanced developer experience)
- **Styling**: Tailwind CSS (v4) for utility-first styling and a custom "Refined Editorial" aesthetic.
- **UI Components**: `shadcn/ui` (Radix UI primitives combined with Tailwind for accessible, customizable components).
- **State Management**: Zustand (for lightweight, global state management like user authentication).
- **Routing**: React Router DOM (v7) for client-side navigation and protected routes.
- **Data Visualization**: Recharts (for rendering dynamic performance trajectory and prediction charts).
- **Animations**: Framer Motion (for staggered reveals, micro-interactions, and smooth UI transitions).
- **Form Handling & Validation**: React Hook Form combined with Zod/Yup for robust client-side validation.
- **Icons**: Lucide React.
- **HTTP Client**: Axios.

### Backend (Based on ecosystem)

- **Framework**: Django (Python)
- **API Standard**: Django REST Framework (DRF) to serve JSON APIs to the React frontend.
- **Machine Learning / Analytics**: ARIMA (AutoRegressive Integrated Moving Average) integration for predicting student performance.
- **Authentication**: JWT or Token-based authentication (managed via custom permissions and views in Django).

---

## 2. Site Map and Pages

The application is heavily role-based (Admin, Teacher, Student), routing users to specific dashboard views immediately upon login.

### Authentication Pages

- **Login Page (`/login`)**: Entry point for all users.
- **Register Page (`/register`)**: Account creation logic.

### Dashboards (Role-Dispatched: `/dashboard`)

- **Admin Dashboard**: Overview of system-wide metrics, user statistics, and global data management.
- **Teacher Dashboard**: Central hub for teachers to view their classes, recent assessments, and quick links to analytics.
- **Student Dashboard**: Personalized view for students to see their latest assessments, overall standing, and predicted outcomes.

### Analysis & Assessment Pages

- **Create Analysis (`/dashboard/create-analysis`)**: Workflow for teachers to initiate a new set of test analyses.
- **Assessment Editor (`/dashboard/editor/:draftId`)**: Interface to build or edit formative assessments and map curriculum topics.
- **Drafts (`/dashboard/drafts`)**: Repository of saved, incomplete assessment configurations.
- **All Analyses (`/dashboard/analysis`)**: Master list of all completed document analyses.
- **Analysis Detail (`/dashboard/analysis/:docId`)**: The teacher's view of a specific document's analysis, showing class averages, topic breakdowns, and a roster of students.
- **Student Analysis Detail (`/dashboard/analysis/:docId/student/:lrn`)**: The student-centric deep dive. Features a "Mastery Profile" gauge, "Curriculum Outcomes", "Performance Trajectory" charts against class averages, an "ARIMA Prediction" for post-tests, and a personalized "Growth Guide".

### Administrative & Management Pages

- **User Management (`/dashboard/users`)**: Admin-only page to manage system accounts, reset passwords, or alter roles.
- **Admin Data Management (`/dashboard/data-management`)**: Interface for configuring core system data (Subjects, Quarters, Sections, etc.).
- **Teacher Assignments (`/dashboard/assignments`)**: Maps teachers to specific sections and subjects.
- **Student Import (`/dashboard/import-students`)**: Utility to bulk-load student data into the system via CSV or similar formats.
- **Settings (`/dashboard/settings`)**: User-specific profile settings.

---

## 3. Agile Methodology

The development of the ESPTFA ARIMA system strictly adhered to Agile software development principles to ensure rapid iteration, continuous feedback, and alignment with end-user (Teacher/Student) needs.

### Standard Agile Process Used

1. **Requirements Gathering & User Stories**:
    - Requirements were broken down into user stories from different perspectives. For example: _"As a student, I want to see a clear visualization of my mastery so I know what topics to focus on."_ or _"As a teacher, I need to see the highest and lowest performing students per topic."_
2. **Sprint Planning & Prioritization**:
    - Features were grouped into manageable sprints. Foundational backend APIs and core UI components were prioritized first, followed by complex analytics and finally, UI/UX polish.
3. **Iterative Development & Execution**:
    - Work was executed in continuous loops. For instance, the **Student Analysis Detail page** underwent multiple iterations. It started as a functional data dump, and in a subsequent sprint, it was iteratively refined into a "Refined Editorial" design incorporating `Framer Motion` animations, a 
        
        MasteryGauge, and a 
        
        PersonalizedStudyGuide.
4. **Continuous Code Review & Testing**:
    - Software Engineers utilized automated TypeScript checks (`npm run build`), ESLint, and manual UI verification to ensure code quality before merging changes.
5. **Feedback Integration (Retrospectives)**:
    - "Act like a teacher" or "Act like a student" mindset simulations were used to evaluate the UI critically. This immediately fed back into the development loop (e.g., realizing scores lower than 60 were demoralizing and shifting to percentage-based metrics relative to class pacing).

### Key Agile Wins

- **Adaptability**: When the initial "Performance Metrics" felt too stark, the agile process allowed immediate pivoting to create the "Your Mastery Profile" and "Growth Guide", drastically improving the student UX without railroading the timeline.
- **Component Reusability**: By iteratively building out components (`Card`, 
    
    Badge, 
    
    Tooltip), the team created a robust design system that made subsequent features faster to deploy.