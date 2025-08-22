# FoundationFirst Product Requirements Document (PRD)

**Document Date:** 2025-08-22  
**Author:** John (PM)  
**Version:** 1.0  

## Goals and Background Context

### Goals

- **Validate maintenance-first philosophy:** Prove that users prefer prioritizing maintenance habits over goal habits when both need attention, achieving 70%+ adoption of this approach
- **Demonstrate sustainable habit formation:** Show that maintenance-first users achieve 40% higher goal completion rates and report improved energy levels compared to traditional tracking patterns
- **Achieve rapid market validation:** Launch MVP within 6-8 weeks to 1,000 beta users, validating core hypothesis before competitors enter maintenance-first space
- **Establish foundation health concept:** Prove users will consistently engage with weekly foundation check-ins (65%+ completion rate) and find the concept valuable for habit prioritization
- **Create platform expansion readiness:** Build technical architecture and gather user feedback sufficient to support Phase 2 advanced features (foundation warnings, habit evaluations)
- **Validate business model potential:** Achieve 15%+ premium conversion rate proving users will pay for foundation health insights and habit guidance

### Background Context

FoundationFirst addresses a critical gap in the habit tracking market by recognizing that maintenance habits (sleep, stress management, social connection) serve as the foundation for successful goal achievement. Unlike existing apps that treat all habits equally, our research revealed that 92% of goal abandonment stems from "lack of energy" and "feeling overwhelmed" - symptoms of neglected foundational wellness.

The post-pandemic cultural shift toward sustainable productivity creates ideal market timing, with the wellness market growing 13% annually while traditional productivity tools remain saturated. Our maintenance-first approach bridges this divide, offering intelligent habit prioritization that prevents burnout while enabling sustainable goal achievement.

### Change Log

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 2025-08-22 | 1.0 | Initial PRD creation from comprehensive Project Brief | John (PM) |

## Requirements

### Functional

1. **FR1:** Users must be able to create habits and classify them as either "Goal" or "Maintenance" during creation process with clear definitions provided
2. **FR2:** The priority dashboard must display maintenance habits first, followed by goal habits, in the main "Today's Focus" view
3. **FR3:** Habits that are behind their weekly targets must display visual urgency indicators using traffic light colors (red/yellow/green)
4. **FR4:** Users must be able to log habit completion with simple tap-to-complete functionality including "mark complete," "skip today," and optional quick notes
5. **FR5:** The system must prompt users weekly (Sundays) with "How's your foundation this week?" using a 1-10 scale for foundation health check-ins
6. **FR6:** Foundation health trends must be visualized showing the last 4 weeks of foundation check-in data
7. **FR7:** Users must complete guided onboarding that explains maintenance vs goal habits with examples and creates 3-5 initial habits
8. **FR8:** The app must track daily habit logging status and calculate weekly completion rates for priority dashboard urgency indicators
9. **FR9:** Users must be able to edit existing habits including changing their classification between goal and maintenance
10. **FR10:** The system must send push notifications for daily habit reminders and weekly foundation check-ins (with user permission)

### Non Functional

1. **NFR1:** App launch time must be under 2 seconds on target devices (iOS 14+ and Android 8+)
2. **NFR2:** Habit logging completion action must respond within 1 second of user tap
3. **NFR3:** The application must support offline habit logging with automatic sync when connectivity returns
4. **NFR4:** The system must handle 1,000+ concurrent users with 99.5% uptime during MVP validation period
5. **NFR5:** All health data must be encrypted at rest and in transit following GDPR compliance requirements
6. **NFR6:** The mobile app must work consistently across iOS and Android platforms using React Native cross-platform development
7. **NFR7:** Database queries for priority dashboard must load within 500ms to maintain responsive user experience
8. **NFR8:** The system must support user data export functionality for privacy compliance and user data portability
9. **NFR9:** Supabase backend services must stay within reasonable usage limits during MVP phase with cost monitoring implemented
10. **NFR10:** The application must maintain user session persistence to avoid repeated login requirements during normal usage

## User Interface Design Goals

### Overall UX Vision

Create a calming, supportive interface that guides users toward sustainable habit formation through visual prioritization and gentle nudging. The app should feel more like a wellness companion than a productivity tracker, using visual hierarchy to naturally direct attention to maintenance habits first. The design reinforces the foundation metaphor through visual elements while maintaining simplicity to avoid overwhelming users.

### Key Interaction Paradigms

**Priority-First Navigation:** The main dashboard uses vertical hierarchy with maintenance habits visually elevated and goal habits positioned below, creating a natural reading flow that reinforces the foundation concept.

**Traffic Light Urgency System:** Red/yellow/green visual indicators provide immediate understanding of habit urgency without requiring explanation, following universal color conventions for status communication.

**One-Tap Completion:** Primary interaction is effortless habit logging with large, accessible tap targets that provide immediate visual feedback and subtle celebration animations.

**Progressive Disclosure:** Complex features (notes, editing, trends) are available but not prominent, maintaining focus on core logging behavior while supporting power users.

### Core Screens and Views

- **Priority Dashboard** - Main screen showing maintenance habits first, goal habits second, with urgency indicators and one-tap logging
- **Habit Creation Flow** - Simple guided process for classifying habits with clear examples of goal vs maintenance categories  
- **Foundation Health Trends** - Visual timeline showing weekly foundation check-in scores with correlation insights
- **Onboarding Tutorial** - Interactive walkthrough teaching maintenance-first concept through actual habit creation
- **Weekly Foundation Check-in** - Single-question modal with 1-10 scale and optional reflection notes
- **Habit Detail/Edit View** - Individual habit management including classification changes and completion history

### Accessibility: WCAG AA

Implement WCAG AA compliance to ensure broad user adoption including users with disabilities who may particularly benefit from sustainable habit formation approaches. This includes proper color contrast for urgency indicators, screen reader compatibility, and keyboard navigation support for React Native components.

### Branding

Clean, wellness-focused visual design that communicates stability and growth. Use natural color palettes (earth tones, greens, calming blues) to reinforce the foundation metaphor. Avoid aggressive productivity aesthetics in favor of supportive, nurturing design language that aligns with maintenance-first philosophy.

### Target Device and Platforms: Cross-Platform

React Native development targeting iOS 14+ and Android 8+ with consistent user experience across platforms. Mobile-first responsive design with consideration for various screen sizes. Future web dashboard planned for Phase 2 analytics but MVP focuses exclusively on mobile native app experience.

## Technical Assumptions

### Repository Structure: Monorepo

Single repository containing the React Native mobile application and shared utilities. This approach supports rapid development by a small team while maintaining clear separation between UI components, business logic, and API integration layers. Feature-based organization (habits, foundation, dashboard) enables focused development and future team scaling.

### Service Architecture

**Monolith Architecture:** Client-server model with Supabase handling backend services to minimize complexity during MVP validation phase. React Native client communicates with Supabase via REST API patterns with real-time subscriptions for habit updates and foundation check-in reminders. This approach avoids microservices overhead while maintaining scalability for Phase 2 expansion.

**Critical architectural decisions:**
- PostgreSQL database via Supabase with simple relational design (users, habits, habit_logs, foundation_checkins tables)
- Binary habit classification minimizes schema complexity identified as performance risk
- Real-time subscriptions for immediate UI updates during habit logging
- Authentication via Supabase Auth with social login options for user convenience

### Testing Requirements

**Unit + Integration Testing:** Comprehensive testing approach balancing development speed with quality assurance. Unit tests for business logic components (habit classification, priority algorithms, foundation health calculations) with integration tests for critical user workflows (onboarding, habit logging, foundation check-ins). Manual testing protocols for mobile-specific interactions and cross-platform consistency validation.

**Testing focus areas:**
- Priority dashboard algorithm correctness
- Cross-platform UI consistency (iOS vs Android)
- Offline functionality and data sync behavior
- Foundation health trend calculations
- Push notification delivery and handling

### Additional Technical Assumptions and Requests

- **Push Notification Services:** Firebase Cloud Messaging integration for habit reminders and foundation check-ins with iOS/Android platform-specific handling
- **Offline Capability:** Local SQLite storage for habit logging with background sync when connectivity returns, ensuring core functionality works without internet
- **Performance Monitoring:** Mixpanel or similar analytics integration for user behavior tracking and MVP validation metrics
- **Cost Management:** Supabase usage monitoring with alerts to prevent unexpected scaling costs during beta testing
- **App Store Deployment:** Expo Application Services (EAS) for streamlined iOS and Android app distribution and updates
- **Data Export:** User data portability features for GDPR compliance and user trust
- **Session Management:** Persistent authentication to minimize login friction during daily habit logging workflows

## Epic List

**Epic 1: Foundation Infrastructure & Basic Habit Management**
Establish project setup, user authentication, and core habit CRUD operations with simple daily logging functionality

**Epic 2: Maintenance-First Priority System** 
Implement habit classification (goal vs maintenance) and priority dashboard that surfaces maintenance habits first with urgency indicators

**Epic 3: Foundation Health Monitoring & MVP Completion**
Add weekly foundation check-ins, trend visualization, and onboarding flow to complete maintenance-first concept validation

## Epic 1: Foundation Infrastructure & Basic Habit Management

Establish the foundational project infrastructure while delivering immediate user value through core habit creation and logging functionality. This epic provides users with a working habit tracking app that validates basic user workflows before introducing the maintenance-first differentiation.

### Story 1.1: Project Setup and Development Environment
As a **developer**,
I want **a properly configured React Native project with Supabase integration**,
so that **I can begin building features on a stable, scalable foundation**.

#### Acceptance Criteria
1. React Native project created with Expo framework and proper folder structure organized by feature domains (habits, foundation, dashboard)
2. Supabase project configured with PostgreSQL database and authentication services connected
3. Git repository initialized with appropriate .gitignore for React Native and development environment files
4. Basic CI/CD pipeline established using Expo Application Services (EAS) for automated builds
5. Development environment documentation created for team onboarding
6. Health check endpoint implemented returning app status and database connectivity

### Story 1.2: User Authentication System
As a **new user**,
I want **to create an account and securely log into the app**,
so that **my habit data is private and persists across sessions**.

#### Acceptance Criteria
1. User registration flow with email/password and optional social login (Google) options
2. Secure login screen with form validation and error handling for invalid credentials
3. Session persistence implemented so users remain logged in across app restarts
4. Password reset functionality via email for account recovery
5. Basic user profile creation with name and optional preferences
6. Authentication state properly managed throughout app navigation

### Story 1.3: Basic Habit Creation
As a **user**,
I want **to create new habits with names and descriptions**,
so that **I can start tracking activities I want to develop**.

#### Acceptance Criteria
1. "Add Habit" button prominently displayed on main screen for easy access
2. Habit creation form with required name field and optional description
3. Form validation preventing empty habit names and duplicate habit creation
4. Habits saved to user's personal habit list in Supabase database
5. Immediate confirmation feedback when habit is successfully created
6. Ability to cancel habit creation and return to main screen

### Story 1.4: Habit Listing and Basic Management
As a **user**,
I want **to view all my created habits and manage them**,
so that **I can see my habit portfolio and make changes as needed**.

#### Acceptance Criteria
1. Main screen displays complete list of user's habits with names and descriptions
2. Habits sorted alphabetically for consistent, predictable ordering
3. Edit functionality allowing users to modify habit names and descriptions
4. Delete functionality with confirmation prompt to prevent accidental removal
5. Empty state message displayed when user has no habits created yet
6. Pull-to-refresh functionality to sync latest habit data from server

### Story 1.5: Daily Habit Logging
As a **user**,
I want **to mark habits as complete for each day**,
so that **I can track my daily progress and build consistency**.

#### Acceptance Criteria
1. Each habit displays clear completion status (complete/incomplete) for current day
2. Large, accessible tap targets for marking habits complete with immediate visual feedback
3. "Skip today" option available for planned rest days without breaking streaks
4. Optional quick notes field for logging context or observations about habit completion
5. Completion data persists immediately to database with offline capability and sync
6. Visual indicators show completion status clearly (checkmarks, colors, etc.)

## Epic 2: Maintenance-First Priority System

Implement the core product differentiator by enabling habit classification (goal vs maintenance) and creating the priority dashboard that surfaces maintenance habits first with visual urgency indicators. This epic validates the fundamental maintenance-first hypothesis that drives the entire product strategy.

### Story 2.1: Habit Classification System
As a **user**,
I want **to classify my habits as either "Goal" or "Maintenance" when creating them**,
so that **the app can understand which habits support my foundation versus specific achievements**.

#### Acceptance Criteria
1. Habit creation form includes required classification choice between "Goal" and "Maintenance" with clear definitions provided
2. Goal habits defined as "Activities that move you toward specific achievements or milestones" with examples (learn guitar, train for marathon, write book)
3. Maintenance habits defined as "Activities that maintain your mental, physical, or spiritual well-being" with examples (sleep 8 hours, meditate, call family)
4. Visual distinction between goal and maintenance habits throughout the app (icons, colors, or labels)
5. Classification stored in database and can be changed by editing existing habits
6. Default suggestion logic guides users toward appropriate classification based on common habit patterns

### Story 2.2: Priority Dashboard Implementation
As a **user**,
I want **to see my maintenance habits displayed first on the main screen**,
so that **I prioritize my foundational well-being before pursuing goals**.

#### Acceptance Criteria
1. Main dashboard restructured with "Today's Focus" section showing maintenance habits at top, goal habits below
2. Clear visual hierarchy established through spacing, typography, and sectioning that emphasizes maintenance priority
3. Section headers distinguish "Foundation (Maintenance)" and "Goals" areas of the dashboard
4. Maintenance habits remain at top regardless of completion status to reinforce priority philosophy
5. Dashboard layout is consistent and intuitive across iOS and Android platforms
6. Empty states handled gracefully when users have no habits in either category

### Story 2.3: Weekly Target Setting and Tracking
As a **user**,
I want **to set weekly targets for my habits and track progress toward them**,
so that **I can see which habits need attention and maintain consistent practice**.

#### Acceptance Criteria
1. Habit creation and edit flows include optional weekly target setting (number of times per week)
2. Default weekly targets suggested based on habit type and common patterns (maintenance habits: 5-7x/week, goal habits: 3-5x/week)
3. Dashboard displays current weekly progress as fraction (e.g., "3/5 this week") for each habit
4. Weekly progress resets every Sunday at midnight in user's local timezone
5. Progress calculation accurately counts completed habit instances within current week
6. Users can modify weekly targets at any time with changes applying to current and future weeks

### Story 2.4: Visual Urgency Indicator System
As a **user**,
I want **to see which habits are behind schedule with clear visual indicators**,
so that **I can quickly identify what needs attention without mental calculation**.

#### Acceptance Criteria
1. Traffic light color system implemented: Green (on track), Yellow (slightly behind), Red (significantly behind target)
2. Urgency calculation based on current weekly progress relative to target and days remaining in week
3. Visual indicators prominently displayed on each habit card (colored borders, icons, or backgrounds)
4. Maintenance habits "in the red" receive extra visual prominence to reinforce foundation-first priority
5. Color choices meet WCAG AA accessibility standards for color contrast and provide alternative indicators for color-blind users
6. Urgency indicators update in real-time as users complete habits throughout the day

### Story 2.5: Maintenance-First User Education
As a **new user**,
I want **to understand why maintenance habits appear first**,
so that **I can appreciate the foundation-first approach and use the app as intended**.

#### Acceptance Criteria
1. First-time user tooltip or brief overlay explains maintenance-first ordering when dashboard is initially displayed
2. Help section accessible from main menu explains the foundation metaphor and maintenance-first philosophy
3. Contextual hints appear during habit creation guiding users toward appropriate classification choices
4. Educational content emphasizes benefits without being preachy: "Build a strong foundation for sustainable success"
5. Option to dismiss educational elements for users who understand the concept
6. Educational messaging tested to ensure it enhances rather than impedes user adoption

## Epic 3: Foundation Health Monitoring & MVP Completion

Complete the MVP by implementing foundation health monitoring through weekly check-ins and trend visualization, plus comprehensive onboarding that teaches the maintenance-first concept. This epic validates user willingness to engage with foundation health concepts and provides data for the core hypothesis testing.

### Story 3.1: Weekly Foundation Health Check-ins
As a **user**,
I want **to reflect on my overall foundation health each week**,
so that **I can track my well-being trends and make informed decisions about habit priorities**.

#### Acceptance Criteria
1. Weekly prompt appears every Sunday evening asking "How's your foundation this week?" with 1-10 scale
2. Foundation check-in presented as modal dialog with clear scale labels (1="Struggling", 10="Thriving")
3. Optional text field allows users to add brief reflection notes about their week
4. Check-in data stored with timestamp and associated with user's weekly habit completion patterns
5. Gentle reminder notification sent if user hasn't completed weekly check-in by Monday evening
6. Users can complete missed check-ins for previous weeks through foundation trends screen

### Story 3.2: Foundation Health Trend Visualization
As a **user**,
I want **to see how my foundation health changes over time**,
so that **I can understand patterns and the relationship between my habits and well-being**.

#### Acceptance Criteria
1. Foundation trends screen displays line chart showing last 12 weeks of foundation health scores
2. Chart includes average trend line and highlights current week's score prominently
3. Tap on data points reveals week details: foundation score, habit completion rates, and optional user notes
4. Visual correlation hints show weeks with high maintenance habit completion alongside foundation scores
5. Empty state encourages users to complete more weekly check-ins to build meaningful trend data
6. Chart is responsive and accessible on different screen sizes with proper touch targets

### Story 3.3: Push Notification System
As a **user**,
I want **to receive gentle reminders for habits and foundation check-ins**,
so that **I maintain consistency without the app feeling demanding or intrusive**.

#### Acceptance Criteria
1. Permission request for notifications presented during onboarding with clear value explanation
2. Daily habit reminders sent at user-specified time (default 9 AM) listing maintenance habits first
3. Weekly foundation check-in reminder sent Sunday at 7 PM with option to complete directly from notification
4. Notification content positive and supportive: "Time for your foundation check-in" rather than demanding
5. Users can customize notification timing and disable specific notification types in settings
6. Notification deep-linking opens relevant app screen (dashboard for habit reminders, check-in for foundation)

### Story 3.4: Comprehensive User Onboarding Flow
As a **new user**,
I want **to understand the app's concept and create my first habits**,
so that **I can start using the maintenance-first approach effectively from day one**.

#### Acceptance Criteria
1. Welcome screen introduces FoundationFirst concept with simple foundation metaphor illustration
2. Interactive tutorial walks through habit classification with real examples user can relate to
3. Guided habit creation helps user create 2-3 maintenance habits and 1-2 goal habits with suggestions
4. Priority dashboard explanation shows maintenance-first ordering and explains urgency indicators
5. Foundation check-in concept introduced with sample question and explanation of weekly rhythm
6. Onboarding skippable for returning users and resumable if interrupted

### Story 3.5: Settings and User Preferences
As a **user**,
I want **to customize notification timing and manage my account settings**,
so that **the app integrates smoothly with my daily routine and preferences**.

#### Acceptance Criteria
1. Settings screen accessible from main menu with clear organization of preference categories
2. Notification preferences allow customization of habit reminder timing and foundation check-in day/time
3. Account management includes profile editing, password change, and data export functionality
4. Privacy settings explain data usage and provide options for minimal data collection
5. App information includes version number, feedback contact, and privacy policy links
6. Logout functionality with confirmation prompt and secure session termination

## Checklist Results Report

### PRD Validation Summary - FoundationFirst

**Executive Summary:**
- **Overall PRD completeness:** 92%
- **MVP scope appropriateness:** Just Right - Focused on core hypothesis validation
- **Readiness for architecture phase:** Ready with minor clarifications needed
- **Most critical gap:** Technical risk areas need architect investigation flags

**Category Analysis:**

| Category                         | Status | Critical Issues |
| -------------------------------- | ------ | --------------- |
| 1. Problem Definition & Context  | PASS   | None - Well documented from Project Brief |
| 2. MVP Scope Definition          | PASS   | Clear boundaries, appropriate size |
| 3. User Experience Requirements  | PASS   | Comprehensive UX vision with accessibility |
| 4. Functional Requirements       | PASS   | 10 testable FRs covering core features |
| 5. Non-Functional Requirements   | PASS   | Performance, security, scalability covered |
| 6. Epic & Story Structure        | PASS   | Sequential, deliverable, appropriately sized |
| 7. Technical Guidance            | PARTIAL| Need architect investigation areas flagged |
| 8. Cross-Functional Requirements | PASS   | Data, integration, operational needs covered |
| 9. Clarity & Communication       | PASS   | Clear, consistent, well-structured |

**Key Findings:**
- PRD successfully translates Project Brief strategic vision into actionable development requirements
- 3 epics with 15 stories appropriately sized for 6-8 week MVP timeline
- Each epic delivers standalone, testable value supporting core hypothesis validation
- Technical constraints well-defined with React Native + Supabase stack

**Areas for Architect Focus:**
- Real-time sync complexity with offline capability
- Priority dashboard algorithm performance at scale
- Cross-platform UI consistency approaches
- Push notification implementation across platforms

**Final Assessment:** **READY FOR ARCHITECT** - The PRD provides sufficient detail and structure for architectural design phase.

## Next Steps

### UX Expert Prompt

Review the FoundationFirst PRD focusing on User Interface Design Goals section. Create design system and visual mockups for the priority dashboard that emphasizes maintenance-first philosophy through visual hierarchy. Prioritize mobile-first responsive design with WCAG AA accessibility compliance and traffic light urgency indicators.

### Architect Prompt

Review the complete FoundationFirst PRD and create technical architecture using the documented React Native + Supabase stack. Focus on database schema design for habit classification system, real-time sync with offline capability, and cross-platform consistency. Address identified technical complexity areas: priority algorithms, push notifications, and performance at scale.