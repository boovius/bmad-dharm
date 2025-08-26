# FoundationFirst Fullstack Architecture Document

**Document Date:** 2025-08-26  
**Author:** Winston (Architect)  
**Version:** 1.0  

## Introduction

This document outlines the complete fullstack architecture for FoundationFirst, including backend systems, frontend implementation, and their integration. It serves as the single source of truth for AI-driven development, ensuring consistency across the entire technology stack.

This unified approach combines what would traditionally be separate backend and frontend architecture documents, streamlining the development process for modern fullstack applications where these concerns are increasingly intertwined.

### Starter Template or Existing Project

**Selected Approach:** Create T3 Turbo (T3 Stack + Turborepo monorepo)

The T3 Turbo stack provides the optimal foundation for FoundationFirst's requirements:
- **Type Safety:** End-to-end TypeScript from database to mobile UI
- **Rapid Development:** Pre-configured tRPC + Prisma + NextAuth integration
- **Cross-Platform Ready:** Shared API logic between React Native mobile app and future Next.js web dashboard
- **Monorepo Benefits:** Code sharing for maintenance-first business logic
- **AI-Agent Friendly:** Consistent patterns and clear interfaces throughout

### Change Log

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 2025-08-26 | 1.0 | Initial fullstack architecture creation | Winston (Architect) |

## High Level Architecture

### Technical Summary

FoundationFirst employs a modern full-stack TypeScript architecture using the T3 stack, emphasizing type safety and code reuse across mobile and future web platforms. The React Native mobile app communicates with tRPC API endpoints that abstract Supabase backend services, enabling offline-first habit logging with real-time synchronization. The monorepo structure supports shared business logic for maintenance-first prioritization algorithms while maintaining platform-specific UI optimizations. This architecture achieves PRD performance goals through efficient database queries, client-side caching, and progressive loading patterns that prioritize core habit logging workflows over secondary features.

### Platform and Infrastructure Choice

**Platform:** Vercel + Supabase (T3 stack optimized)
**Key Services:** Supabase (PostgreSQL, Auth, Real-time), Vercel (deployment, edge functions), Expo EAS (mobile builds)
**Deployment Host and Regions:** Vercel Edge Network (global), Supabase US-East-1 (primary) with read replicas as needed

### Repository Structure

**Structure:** Turborepo monorepo with T3 stack organization
**Monorepo Tool:** Turborepo (optimized for T3 stack builds and caching)
**Package Organization:** Apps (expo mobile, nextjs web), packages (shared API, database, auth, UI components, validators)

### High Level Architecture Diagram

```mermaid
graph TD
    A[Mobile App - React Native/Expo] --> B[tRPC API Layer]
    C[Web Dashboard - Next.js] --> B
    B --> D[Supabase PostgreSQL]
    B --> E[Supabase Auth]
    B --> F[Supabase Realtime]
    
    G[Push Notifications] --> A
    H[Offline Storage - SQLite] --> A
    
    I[Vercel Edge Functions] --> B
    J[CDN - Static Assets] --> A
    J --> C
    
    K[External APIs] --> B
    L[Analytics - Mixpanel] --> A
    L --> C
    
    B --> M[Background Jobs]
    M --> N[Foundation Health Calculations]
    M --> O[Weekly Check-in Reminders]
```

### Architectural Patterns

- **T3 Stack Architecture:** TypeScript-first full-stack with tRPC for type-safe APIs - _Rationale:_ Eliminates API documentation overhead and runtime type errors critical for maintenance-first logic complexity
- **Offline-First Mobile Pattern:** Local SQLite cache with optimistic updates and background sync - _Rationale:_ Ensures core habit logging works without connectivity, addressing PRD requirement NFR3
- **Monorepo Code Sharing:** Shared business logic between mobile and web via packages - _Rationale:_ Maintenance-first algorithms need consistency across platforms for Phase 2 web dashboard
- **Event-Driven Real-time:** Supabase subscriptions for habit completion notifications - _Rationale:_ Immediate UI feedback supports PRD goal of <1 second habit logging response time
- **Progressive Loading:** Priority dashboard loads cached data first, then fetches updates - _Rationale:_ Supports PRD requirement of <2 second app launch while maintaining data freshness

## Tech Stack

### Technology Stack Table

| Category | Technology | Version | Purpose | Rationale |
|----------|------------|---------|---------|-----------|
| Frontend Language | TypeScript | 5.2+ | Type-safe mobile and web development | Eliminates runtime type errors critical for maintenance-first logic consistency |
| Frontend Framework | React Native (Expo) | 0.74+ | Cross-platform mobile application | Proven for rapid deployment with EAS, supports offline-first requirements |
| UI Component Library | NativeWind + Expo Components | Latest | Tailwind CSS for React Native styling | Consistent design system across mobile/web, rapid prototyping capability |
| State Management | Zustand + tRPC React Query | 4.4+ / 10.0+ | Client state and server state management | Lightweight local state + optimistic updates for habit logging performance |
| Backend Language | TypeScript | 5.2+ | Server-side logic and API development | Full-stack type safety from database to UI components |
| Backend Framework | tRPC | 10.0+ | Type-safe API layer | End-to-end type safety eliminates API documentation and integration bugs |
| API Style | tRPC (TypeScript RPC) | 10.0+ | Type-safe remote procedure calls | Auto-generated client bindings, shared types between mobile/web |
| Database | PostgreSQL (via Supabase) | 15+ | Primary data storage | Relational model supports habit relationships, real-time subscriptions |
| Cache | Redis (via Supabase) | 7.0+ | Session storage and API caching | Improves dashboard load times, supports 1000+ concurrent users |
| File Storage | Supabase Storage | Latest | Profile images and app assets | Integrated with auth system, CDN distribution |
| Authentication | Supabase Auth + NextAuth.js | Latest / 4.0+ | User authentication and authorization | Social login support, secure session management |
| Frontend Testing | Jest + React Native Testing Library | 29+ / 12+ | Unit and integration testing | Component testing aligned with React Native patterns |
| Backend Testing | Vitest + Supertest | 1.0+ / 6.3+ | API and business logic testing | Fast test execution, tRPC integration testing |
| E2E Testing | Detox | 20.0+ | End-to-end mobile testing | React Native optimized, habit logging workflow validation |
| Build Tool | Turborepo | 1.10+ | Monorepo build orchestration | Optimized caching for T3 stack, parallel builds |
| Bundler | Metro (React Native) + Vite | Latest / 5.0+ | Mobile and web bundling | React Native optimized + fast web builds |
| IaC Tool | Terraform (optional) | 1.5+ | Infrastructure as code | Optional for advanced Supabase configuration |
| CI/CD | GitHub Actions + EAS | Latest | Automated testing and deployment | Native Expo integration, mobile app store deployment |
| Monitoring | Sentry | 7.0+ | Error tracking and performance | React Native SDK, tRPC error boundary integration |
| Logging | Flipper + Console (dev) | Latest | Development debugging | React Native inspection, network request debugging |
| CSS Framework | NativeWind | 4.0+ | Tailwind CSS for React Native | Design system consistency, rapid UI development |
| Analytics | Mixpanel | Latest | User behavior tracking and MVP validation | Advanced segmentation for maintenance-first adoption metrics |

## Data Models

### User
**Purpose:** Central user entity supporting authentication, onboarding progress, and foundation health tracking

**Key Attributes:**
- id: string (CUID) - Unique identifier
- email: string - Authentication and communication
- name: string? - Display name for personalization
- onboardingCompleted: boolean - Gates access to main app vs onboarding flow
- onboardingStep: number? - Tracks progress through guided setup
- createdAt: DateTime - Account creation timestamp

#### TypeScript Interface
```typescript
interface User {
  id: string;
  email: string;
  name: string | null;
  onboardingCompleted: boolean;
  onboardingStep: number | null;
  createdAt: Date;
  updatedAt: Date;
  
  // Relations
  habits: Habit[];
  foundationCheckins: FoundationCheckin[];
  habitLogs: HabitLog[];
}
```

#### Relationships
- One-to-many with Habit (user owns multiple habits)
- One-to-many with FoundationCheckin (weekly foundation health scores)
- One-to-many with HabitLog (user's daily habit completion records)

### Habit
**Purpose:** Core business entity representing both maintenance and goal habits with weekly targets and traffic light urgency

**Key Attributes:**
- id: string (CUID) - Unique identifier
- name: string - User-defined habit name
- description: string? - Optional habit details
- classification: HabitType - MAINTENANCE or GOAL enum
- weeklyTarget: number - Expected completions per week (1-7)
- currentStreak: number - Consecutive days completed
- isActive: boolean - Allows soft deletion/archiving
- createdAt: DateTime - Habit creation timestamp

#### TypeScript Interface
```typescript
interface Habit {
  id: string;
  name: string;
  description: string | null;
  classification: 'MAINTENANCE' | 'GOAL';
  weeklyTarget: number;
  currentStreak: number;
  isActive: boolean;
  createdAt: Date;
  updatedAt: Date;
  userId: string;
  
  // Relations
  user: User;
  logs: HabitLog[];
}
```

#### Relationships
- Many-to-one with User (habit belongs to specific user)
- One-to-many with HabitLog (habit has multiple daily completion records)

### HabitLog
**Purpose:** Daily habit completion tracking with optional context for habit logging workflow

**Key Attributes:**
- id: string (CUID) - Unique identifier
- completed: boolean - Whether habit was completed this day
- notes: string? - Optional user notes about completion
- loggedAt: DateTime - When the habit completion was logged
- habitId: string - Foreign key to habit
- userId: string - Foreign key to user for efficient querying

#### TypeScript Interface
```typescript
interface HabitLog {
  id: string;
  completed: boolean;
  notes: string | null;
  loggedAt: Date;
  habitId: string;
  userId: string;
  
  // Relations
  habit: Habit;
  user: User;
}
```

#### Relationships
- Many-to-one with Habit (log entry belongs to specific habit)
- Many-to-one with User (log entry belongs to specific user)

### FoundationCheckin
**Purpose:** Weekly foundation health scoring system that drives maintenance-first prioritization insights

**Key Attributes:**
- id: string (CUID) - Unique identifier
- score: number - Foundation health score 1-10
- notes: string? - Optional reflection notes
- weekOf: DateTime - Start date of the week being scored
- createdAt: DateTime - When check-in was completed
- userId: string - Foreign key to user

#### TypeScript Interface
```typescript
interface FoundationCheckin {
  id: string;
  score: number;
  notes: string | null;
  weekOf: Date;
  createdAt: Date;
  userId: string;
  
  // Relations
  user: User;
}
```

#### Relationships
- Many-to-one with User (check-in belongs to specific user)
- Unique constraint on (userId, weekOf) to prevent duplicate weekly check-ins

## API Specification

Based on the tRPC API style selected in the Tech Stack, here are the router definitions that provide type-safe remote procedure calls for FoundationFirst:

### tRPC Router Definitions

```typescript
// packages/api/src/root.ts
import { createTRPCRouter } from "./trpc";
import { authRouter } from "./router/auth";
import { habitRouter } from "./router/habit";
import { foundationRouter } from "./router/foundation";
import { userRouter } from "./router/user";

export const appRouter = createTRPCRouter({
  auth: authRouter,
  habit: habitRouter,
  foundation: foundationRouter,
  user: userRouter,
});

export type AppRouter = typeof appRouter;

// packages/api/src/router/habit.ts
import { z } from "zod";
import { createTRPCRouter, protectedProcedure, publicProcedure } from "../trpc";
import { prisma } from "@foundationfirst/db";

export const habitRouter = createTRPCRouter({
  // Get priority-ordered habits for dashboard
  getPriorityOrder: protectedProcedure
    .query(async ({ ctx }) => {
      const habits = await prisma.habit.findMany({
        where: { 
          userId: ctx.user.id,
          isActive: true 
        },
        include: {
          logs: {
            where: {
              loggedAt: {
                gte: startOfWeek(new Date()),
                lt: endOfWeek(new Date())
              }
            }
          }
        }
      });

      // Maintenance-first prioritization algorithm
      const maintenanceHabits = habits
        .filter(h => h.classification === 'MAINTENANCE')
        .map(habit => ({
          ...habit,
          weeklyProgress: habit.logs.length / habit.weeklyTarget,
          urgencyLevel: calculateUrgency(habit.logs.length, habit.weeklyTarget),
          trafficLightColor: getTrafficLightColor(habit.logs.length, habit.weeklyTarget)
        }))
        .sort(sortByUrgency);

      const goalHabits = habits
        .filter(h => h.classification === 'GOAL')
        .map(habit => ({
          ...habit,
          weeklyProgress: habit.logs.length / habit.weeklyTarget,
          urgencyLevel: calculateUrgency(habit.logs.length, habit.weeklyTarget),
          trafficLightColor: getTrafficLightColor(habit.logs.length, habit.weeklyTarget)
        }))
        .sort(sortByUrgency);

      return {
        maintenance: maintenanceHabits,
        goals: goalHabits,
        totalHabits: habits.length,
        weeklyCompletionRate: calculateWeeklyCompletionRate(habits)
      };
    }),

  // Create new habit with classification
  create: protectedProcedure
    .input(z.object({
      name: z.string().min(1, "Name is required").max(100),
      description: z.string().max(500).optional(),
      classification: z.enum(['MAINTENANCE', 'GOAL']),
      weeklyTarget: z.number().min(1).max(7)
    }))
    .mutation(async ({ input, ctx }) => {
      const habit = await prisma.habit.create({
        data: {
          ...input,
          userId: ctx.user.id
        }
      });

      return habit;
    }),

  // Log habit completion with offline sync support
  logCompletion: protectedProcedure
    .input(z.object({
      habitId: z.string(),
      completed: z.boolean(),
      notes: z.string().max(200).optional(),
      loggedAt: z.date().optional() // Support backdated logging
    }))
    .mutation(async ({ input, ctx }) => {
      const loggedAt = input.loggedAt ?? new Date();

      // Check for existing log on this day
      const existingLog = await prisma.habitLog.findFirst({
        where: {
          habitId: input.habitId,
          userId: ctx.user.id,
          loggedAt: {
            gte: startOfDay(loggedAt),
            lt: endOfDay(loggedAt)
          }
        }
      });

      if (existingLog) {
        // Update existing log
        const updatedLog = await prisma.habitLog.update({
          where: { id: existingLog.id },
          data: {
            completed: input.completed,
            notes: input.notes
          }
        });
        return updatedLog;
      } else {
        // Create new log
        const habitLog = await prisma.habitLog.create({
          data: {
            habitId: input.habitId,
            userId: ctx.user.id,
            completed: input.completed,
            notes: input.notes,
            loggedAt
          }
        });
        
        // Update streak if completed
        if (input.completed) {
          await updateHabitStreak(input.habitId);
        }
        
        return habitLog;
      }
    }),
});
```

## Components

Based on the architectural patterns, tech stack, and data models, here are the major logical components across the fullstack system:

### Authentication Service
**Responsibility:** User authentication, session management, and onboarding state tracking using Supabase Auth + NextAuth.js integration

**Key Interfaces:**
- `signUp(email, password) → User | AuthError`
- `signIn(email, password) → Session | AuthError`
- `getSession() → Session | null`
- `updateOnboardingStep(step: number) → User`

**Dependencies:** Supabase Auth, NextAuth.js, User data model

**Technology Stack:** NextAuth.js providers, Supabase Auth SDK, JWT tokens, secure session cookies

### Habit Management Service
**Responsibility:** Core habit CRUD operations, classification logic, and maintenance-first prioritization algorithms

**Key Interfaces:**
- `createHabit(data: CreateHabitInput) → Habit`
- `getPriorityOrderedHabits(userId: string) → { maintenance: Habit[], goals: Habit[] }`
- `calculateUrgencyLevel(logs: HabitLog[], target: number) → UrgencyLevel`
- `updateHabitClassification(habitId: string, classification: HabitType) → Habit`

**Dependencies:** Prisma ORM, HabitLog service, traffic light calculation utilities

**Technology Stack:** tRPC procedures, Prisma database client, TypeScript enums for classification

### Component Diagrams

```mermaid
graph TD
    subgraph "Mobile App Layer"
        A[Priority Dashboard UI]
        B[Habit Creation UI]
        C[Foundation Check-in UI]
        D[Onboarding Flow UI]
    end
    
    subgraph "Service Layer"
        E[Priority Dashboard Service]
        F[Habit Management Service]
        G[Foundation Health Service]
        H[Habit Logging Service]
        I[Onboarding Flow Service]
    end
    
    subgraph "Infrastructure Layer"
        J[Authentication Service]
        K[Push Notification Service]
        L[Offline Sync Service]
        M[Analytics Service]
    end
    
    subgraph "Data Layer"
        N[tRPC API]
        O[Prisma ORM]
        P[Supabase PostgreSQL]
        Q[Local SQLite]
    end
    
    A --> E
    B --> F
    C --> G
    D --> I
    
    E --> F
    E --> G
    E --> H
    F --> H
    G --> F
    
    E --> N
    F --> N
    G --> N
    H --> L
    H --> Q
    I --> J
    
    J --> N
    K --> M
    L --> N
    M --> N
    
    N --> O
    O --> P
```

## External APIs

FoundationFirst requires minimal external API integrations for the MVP, focusing on essential services for authentication, notifications, and analytics:

### Mixpanel Analytics API
- **Purpose:** Advanced user behavior tracking and MVP validation metrics with cohort analysis
- **Documentation:** https://developer.mixpanel.com/reference/overview
- **Base URL(s):** https://api.mixpanel.com
- **Authentication:** Project token and API secret
- **Rate Limits:** 1,000 requests per hour for free tier, 60,000 for paid

**Key Endpoints Used:**
- `POST /track` - Track habit completion events, foundation check-ins, and maintenance-first behavior patterns
- `POST /engage` - Update user profiles with onboarding progress and habit classification preferences  
- `GET /export` - Export event data for MVP validation analysis and cohort comparisons
- `GET /segmentation` - Advanced segmentation for maintenance-first adoption analysis
- `GET /retention` - Weekly foundation check-in retention tracking

**Integration Notes:** Essential for measuring maintenance-first philosophy adoption rates (70% target), goal completion comparisons (40% higher target), and foundation health engagement (65% target). Custom events track habit classification decisions, weekly check-in patterns, and traffic light urgency responses for comprehensive user behavior analysis.

### Firebase Cloud Messaging API
- **Purpose:** Push notifications for habit reminders and weekly foundation check-ins
- **Documentation:** https://firebase.google.com/docs/cloud-messaging
- **Base URL(s):** https://fcm.googleapis.com/v1/projects/{project-id}/messages:send
- **Authentication:** Firebase service account JSON with OAuth 2.0
- **Rate Limits:** 600,000 messages per minute per project

**Key Endpoints Used:**
- `POST /v1/projects/{project-id}/messages:send` - Send individual push notifications
- `POST /v1/projects/{project-id}/messages:sendMulticast` - Batch notifications for habit reminders

**Integration Notes:** Integrated via Expo push notifications service for cross-platform delivery. Handles both habit reminder scheduling and foundation check-in prompts with deep linking to specific app screens.

### Expo Push Notifications API
- **Purpose:** Cross-platform push notification delivery wrapper for FCM/APNS
- **Documentation:** https://docs.expo.dev/push-notifications/overview/
- **Base URL(s):** https://exp.host/--/api/v2/push/send
- **Authentication:** Expo access token
- **Rate Limits:** 600 notifications per second

**Key Endpoints Used:**
- `POST /--/api/v2/push/send` - Send push notifications via Expo service
- `POST /--/api/v2/push/getReceipts` - Check delivery status of sent notifications

**Integration Notes:** Simplifies cross-platform push notifications by handling both iOS APNS and Android FCM routing. Supports batch sending for daily habit reminders across user base.

## Core Workflows

Here are the critical system workflows using sequence diagrams to show component interactions:

### Daily Habit Logging Workflow

```mermaid
sequenceDiagram
    participant U as User
    participant MA as Mobile App
    participant PS as Priority Service
    participant HL as Habit Logging
    participant OS as Offline Sync
    participant API as tRPC API
    participant DB as Supabase DB
    participant MX as Mixpanel
    participant PN as Push Notifications

    U->>MA: Opens app / Tap habit reminder
    MA->>PS: getDashboardData(userId)
    PS->>API: habit.getPriorityOrder()
    API->>DB: Query habits + weekly logs
    DB-->>API: Habits with completion data
    API-->>PS: Maintenance-first ordered habits
    PS-->>MA: Dashboard with traffic light status

    U->>MA: Tap "Complete" on habit
    MA->>HL: logHabitCompletion(habitId, true)
    
    alt Online Mode
        HL->>API: habit.logCompletion(data)
        API->>DB: Insert/update HabitLog
        DB-->>API: Success confirmation
        API->>MX: Track "Habit Completed" event
        API-->>HL: Log created successfully
        HL-->>MA: Optimistic UI update + success
    else Offline Mode
        HL->>OS: persistOfflineLog(data)
        OS-->>HL: Stored locally
        HL-->>MA: Optimistic UI update
        Note over OS: Background sync when online
        OS->>API: batchLogCompletion(pendingLogs)
        API->>DB: Batch upsert operations
        API->>MX: Track batch completion events
    end

    MA->>U: Visual feedback + celebration
    
    alt Streak Achievement
        API->>PN: celebrateStreak(habitId, streakLength)
        PN-->>U: Push notification celebration
    end

    Note over MA,MX: Real-time dashboard update via Supabase subscriptions
```

## Database Schema

Transforming the conceptual data models into concrete PostgreSQL schema optimized for Supabase and maintenance-first query patterns:

### Database Schema (PostgreSQL/Prisma)

```sql
-- Users table with onboarding tracking
CREATE TABLE users (
  id TEXT PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT UNIQUE NOT NULL,
  name TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  
  -- Onboarding state management
  onboarding_completed BOOLEAN DEFAULT FALSE,
  onboarding_step INTEGER,
  
  -- Soft deletion support
  is_active BOOLEAN DEFAULT TRUE
);

-- Habit classification enum
CREATE TYPE habit_type AS ENUM ('MAINTENANCE', 'GOAL');

-- Core habits table with maintenance-first optimization
CREATE TABLE habits (
  id TEXT PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL CHECK (length(name) BETWEEN 1 AND 100),
  description TEXT CHECK (length(description) <= 500),
  classification habit_type NOT NULL,
  weekly_target INTEGER NOT NULL CHECK (weekly_target BETWEEN 1 AND 7),
  current_streak INTEGER DEFAULT 0,
  is_active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  
  -- Foreign key
  user_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE
);

-- Daily habit completion logs
CREATE TABLE habit_logs (
  id TEXT PRIMARY KEY DEFAULT gen_random_uuid(),
  completed BOOLEAN NOT NULL,
  notes TEXT CHECK (length(notes) <= 200),
  logged_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  
  -- Foreign keys with denormalized user_id for query optimization
  habit_id TEXT NOT NULL REFERENCES habits(id) ON DELETE CASCADE,
  user_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE
);

-- Weekly foundation health check-ins
CREATE TABLE foundation_checkins (
  id TEXT PRIMARY KEY DEFAULT gen_random_uuid(),
  score INTEGER NOT NULL CHECK (score BETWEEN 1 AND 10),
  notes TEXT CHECK (length(notes) <= 500),
  week_of DATE NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  
  -- Foreign key
  user_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  
  -- Ensure one check-in per user per week
  UNIQUE(user_id, week_of)
);

-- Indexes for performance optimization

-- Priority dashboard query optimization (most critical)
CREATE INDEX idx_habits_user_classification_active 
ON habits(user_id, classification, is_active) 
WHERE is_active = TRUE;

-- Weekly habit logs for dashboard urgency calculation
CREATE INDEX idx_habit_logs_user_week 
ON habit_logs(user_id, logged_at DESC) 
WHERE logged_at >= DATE_TRUNC('week', CURRENT_DATE);
```

## Frontend Architecture

### Component Architecture

#### Component Organization
```
apps/expo/src/
├── components/
│   ├── habits/
│   │   ├── HabitCard.tsx              # Maintenance vs Goal styled cards
│   │   ├── HabitClassificationSelector.tsx
│   │   ├── TrafficLightIndicator.tsx
│   │   └── HabitCompletionButton.tsx
│   ├── foundation/
│   │   ├── FoundationHealthScale.tsx  # 1-10 rating component
│   │   ├── FoundationTrendsChart.tsx
│   │   └── WeeklyCheckinModal.tsx
│   ├── dashboard/
│   │   ├── PriorityDashboard.tsx      # Main maintenance-first view
│   │   ├── MaintenanceSection.tsx
│   │   └── GoalSection.tsx
│   ├── onboarding/
│   │   ├── OnboardingFlow.tsx
│   │   ├── FoundationMetaphorScreen.tsx
│   │   └── InteractiveClassificationTutorial.tsx
│   └── ui/                            # Shared design system components
│       ├── Button.tsx
│       ├── Card.tsx
│       ├── Typography.tsx
│       └── Layout.tsx
```

### State Management Architecture

#### State Structure
```typescript
// stores/habitStore.ts
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';
import AsyncStorage from '@react-native-async-storage/async-storage';
import { Habit, HabitLog } from '@foundationfirst/db';

interface HabitState {
  // Priority dashboard data
  priorityHabits: {
    maintenance: (Habit & { weeklyProgress: number; urgencyLevel: string })[];
    goals: (Habit & { weeklyProgress: number; urgencyLevel: string })[];
  };
  
  // Offline sync queue
  pendingLogs: OfflineHabitLog[];
  syncStatus: 'idle' | 'syncing' | 'error';
  lastSyncAt: Date | null;
  
  // UI state
  isRefreshing: boolean;
  selectedHabit: Habit | null;
  
  // Actions
  setPriorityHabits: (data: HabitState['priorityHabits']) => void;
  addPendingLog: (log: OfflineHabitLog) => void;
  clearPendingLogs: () => void;
  setSyncStatus: (status: HabitState['syncStatus']) => void;
  setRefreshing: (refreshing: boolean) => void;
}

export const useHabitStore = create<HabitState>()(
  persist(
    (set, get) => ({
      priorityHabits: { maintenance: [], goals: [] },
      pendingLogs: [],
      syncStatus: 'idle',
      lastSyncAt: null,
      isRefreshing: false,
      selectedHabit: null,

      setPriorityHabits: (data) => 
        set({ priorityHabits: data }),
      
      addPendingLog: (log) => 
        set(state => ({ 
          pendingLogs: [...state.pendingLogs, log] 
        })),
      
      clearPendingLogs: () => 
        set({ pendingLogs: [], lastSyncAt: new Date() }),
      
      setSyncStatus: (status) => 
        set({ syncStatus: status }),
      
      setRefreshing: (refreshing) => 
        set({ isRefreshing: refreshing })
    }),
    {
      name: 'habit-storage',
      storage: createJSONStorage(() => AsyncStorage),
      partialize: (state) => ({
        pendingLogs: state.pendingLogs,
        lastSyncAt: state.lastSyncAt
      })
    }
  )
);
```

## Backend Architecture

### Service Architecture

#### Serverless Architecture (T3 Stack + Vercel)

##### Function Organization
```
packages/api/src/
├── router/
│   ├── auth.ts                    # Authentication and user management
│   ├── habit.ts                   # Habit CRUD and priority logic
│   ├── foundation.ts              # Foundation health check-ins and trends
│   ├── user.ts                    # User profile and onboarding
│   └── analytics.ts               # Internal analytics tracking
├── middleware/
│   ├── auth.ts                    # JWT validation and user context
│   ├── rateLimit.ts               # API rate limiting
│   ├── cors.ts                    # Cross-origin request handling
│   └── analytics.ts               # Request tracking middleware
├── services/
│   ├── habitPriority.ts           # Maintenance-first algorithms
│   ├── foundationHealth.ts        # Foundation scoring calculations
│   ├── streakCalculation.ts       # Habit streak logic
│   └── notifications.ts           # Push notification scheduling
├── utils/
│   ├── dateHelpers.ts             # Week calculations and date utilities
│   ├── validationSchemas.ts       # Zod input validation
│   └── errorHandling.ts           # Standardized error responses
└── root.ts                        # Main tRPC app router
```

### Authentication and Authorization

#### Auth Flow
```mermaid
sequenceDiagram
    participant C as Client App
    participant V as Vercel Function
    participant S as Supabase Auth
    participant DB as PostgreSQL
    participant N as NextAuth

    C->>V: POST /api/auth/signup
    V->>S: createUser(email, password)
    S->>DB: INSERT INTO auth.users
    S-->>V: User + JWT token
    V->>DB: INSERT INTO public.users (onboarding_completed: false)
    V->>N: createSession(user)
    N-->>V: Session cookie
    V-->>C: User profile + session

    Note over C,V: Subsequent API calls
    C->>V: POST /api/trpc/habit.create (with JWT)
    V->>N: validateSession(cookie)
    N->>S: verifyJWT(token)
    S-->>N: Valid user
    N-->>V: Authenticated context
    V->>DB: INSERT INTO habits (user_id = authenticated_user.id)
    V-->>C: Success response
```

## Unified Project Structure

T3 Turbo monorepo structure optimized for FoundationFirst's maintenance-first philosophy and cross-platform development:

```plaintext
foundationfirst/
├── .github/                           # CI/CD workflows and issue templates
│   └── workflows/
│       ├── ci.yml                     # Lint, test, type check
│       ├── deploy-mobile.yml          # EAS build and deployment
│       ├── deploy-web.yml             # Vercel deployment for future web dashboard
│       └── release.yml                # Automated versioning and releases
├── apps/                              # Application packages
│   ├── expo/                          # React Native mobile app
│   │   ├── src/
│   │   │   ├── components/            # UI components organized by domain
│   │   │   │   ├── habits/            # Habit-related components
│   │   │   │   ├── foundation/        # Foundation health components
│   │   │   │   ├── dashboard/         # Priority dashboard components
│   │   │   │   ├── onboarding/        # Guided onboarding flow
│   │   │   │   └── ui/                # Shared design system components
│   │   │   ├── screens/               # Screen components
│   │   │   ├── navigation/            # Navigation configuration
│   │   │   ├── hooks/                 # Custom React hooks
│   │   │   ├── stores/                # Zustand state management
│   │   │   ├── services/              # Business logic services
│   │   │   ├── utils/                 # Frontend utilities
│   │   │   └── types/                 # Frontend-specific types
│   │   ├── assets/                    # Images, fonts, and static assets
│   │   ├── app.config.ts              # Expo configuration
│   │   ├── package.json               # Mobile app dependencies
│   │   └── eas.json                   # Expo Application Services config
│   └── nextjs/                        # Future web dashboard (Phase 2)
│       ├── src/
│       │   ├── pages/
│       │   │   ├── api/               # tRPC API endpoints
│       │   │   └── _app.tsx
│       │   └── components/            # Web-specific components
│       └── package.json               # Web app dependencies
├── packages/                          # Shared packages across applications
│   ├── api/                          # tRPC API layer (backend logic)
│   │   ├── src/
│   │   │   ├── router/               # tRPC routers by domain
│   │   │   ├── services/             # Business logic services
│   │   │   ├── middleware/           # API middleware
│   │   │   ├── utils/                # Backend utilities
│   │   │   ├── trpc.ts               # tRPC setup and base procedures
│   │   │   └── root.ts               # Main app router
│   │   └── package.json              # API package dependencies
│   ├── auth/                         # Authentication configuration
│   │   ├── src/
│   │   │   ├── config.ts             # NextAuth configuration
│   │   │   └── providers/            # Auth provider configurations
│   │   └── package.json
│   ├── db/                           # Database schema and client
│   │   ├── prisma/
│   │   │   ├── schema.prisma         # Database schema definition
│   │   │   └── migrations/           # Database migrations
│   │   ├── src/
│   │   │   ├── client.ts             # Prisma client configuration
│   │   │   └── index.ts              # Database exports
│   │   └── package.json              # Database package dependencies
│   ├── shared/                       # Shared types and utilities
│   │   ├── src/
│   │   │   ├── types/                # Shared TypeScript interfaces
│   │   │   ├── constants/            # Shared constants
│   │   │   ├── utils/                # Cross-platform utilities
│   │   │   └── index.ts              # Shared package exports
│   │   └── package.json              # Shared package dependencies
│   ├── ui/                           # Shared UI component library
│   │   ├── src/
│   │   │   ├── components/           # Cross-platform components
│   │   │   ├── styles/               # Shared design system
│   │   │   └── index.ts              # UI package exports
│   │   └── package.json              # UI package dependencies
│   └── config/                       # Shared configuration packages
│       ├── eslint/
│       ├── typescript/
│       └── tailwind/
├── scripts/                          # Build and development scripts
├── docs/                             # Project documentation
│   ├── prd.md                        # Product Requirements Document
│   ├── front-end-spec.md             # UI/UX Specification
│   ├── architecture.md               # This architecture document
│   └── README.md                     # Project overview and setup
├── .env.example                      # Environment variables template
├── package.json                      # Root monorepo configuration
├── turbo.json                        # Turborepo pipeline configuration
└── README.md                         # Project overview and setup
```

## Development Workflow

### Local Development Setup

#### Prerequisites
```bash
# Required software and versions
node --version    # v18.17.0 or higher
npm --version     # v9.0.0 or higher
git --version     # v2.34.0 or higher

# Install global dependencies
npm install -g @expo/cli@latest
npm install -g eas-cli@latest
npm install -g turbo@latest
```

#### Initial Setup
```bash
# Clone and setup FoundationFirst monorepo
git clone https://github.com/your-org/foundationfirst.git
cd foundationfirst

# Install all dependencies across monorepo
npm install

# Copy environment template and configure
cp .env.example .env.local
# Edit .env.local with your Supabase credentials, Mixpanel token, etc.

# Setup database
npm run db:push          # Push Prisma schema to Supabase
npm run db:seed          # Seed with sample data for development

# Build shared packages
npm run build:packages   # Build shared packages first

# Start development servers
npm run dev              # Starts all development servers
```

#### Development Commands
```bash
# Start all services (mobile, web, API)
npm run dev

# Start mobile app only
npm run dev:mobile

# Start web dashboard only (Phase 2)
npm run dev:web

# Start API server only
npm run dev:api

# Database operations
npm run db:push          # Push schema changes
npm run db:migrate       # Create and run migrations  
npm run db:seed          # Seed database with test data
npm run db:studio        # Open Prisma Studio GUI

# Testing commands
npm run test             # Run all tests across packages
npm run test:mobile      # Test mobile app only
npm run test:api         # Test API endpoints only
npm run test:e2e         # Run end-to-end tests

# Code quality
npm run lint             # Lint all packages
npm run lint:fix         # Fix auto-fixable linting issues
npm run type-check       # TypeScript type checking
npm run format           # Format code with Prettier

# Build commands
npm run build            # Build all packages for production
npm run build:mobile     # Build mobile app for EAS
npm run build:web        # Build web dashboard for Vercel
```

### Environment Configuration

#### Required Environment Variables

```bash
# Frontend (.env.local - for Expo app)
EXPO_PUBLIC_API_URL=http://localhost:3000
EXPO_PUBLIC_SUPABASE_URL=https://your-project.supabase.co  
EXPO_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
EXPO_PUBLIC_MIXPANEL_TOKEN=your-mixpanel-token

# Backend (.env - for API and database)
DATABASE_URL=postgresql://postgres:[password]@db.your-project.supabase.co:5432/postgres
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
SUPABASE_JWT_SECRET=your-jwt-secret

# Authentication
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your-nextauth-secret
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

# External services
MIXPANEL_PROJECT_TOKEN=your-mixpanel-project-token
MIXPANEL_API_SECRET=your-mixpanel-api-secret

# Push notifications
EXPO_ACCESS_TOKEN=your-expo-access-token

# Development
NODE_ENV=development
LOG_LEVEL=debug
```

## Deployment Architecture

### Deployment Strategy

**Frontend Deployment:**
- **Platform:** Expo Application Services (EAS) for mobile apps
- **Build Command:** `eas build --platform all`
- **Output Directory:** Built .ipa and .apk files for app stores
- **CDN/Edge:** Expo CDN for over-the-air updates and assets

**Backend Deployment:**
- **Platform:** Vercel for tRPC API and Next.js web dashboard
- **Build Command:** `turbo build --filter=nextjs`
- **Deployment Method:** Git-based automatic deployments from main branch

### CI/CD Pipeline

```yaml
# .github/workflows/ci.yml - Continuous Integration
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  quality-checks:
    name: Quality Checks
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'
      
      - run: npm ci
      - run: npm run build:packages
      - run: npm run lint
      - run: npm run type-check
      - run: npm run test
```

### Environments

| Environment | Frontend URL | Backend URL | Purpose |
|-------------|--------------|-------------|---------|
| Development | expo://localhost:8081 | http://localhost:3000/api/trpc | Local development and testing |
| Staging | https://staging.foundationfirst.app | https://staging-api.foundationfirst.app/api/trpc | Pre-production testing and QA |
| Production | iOS/Android App Stores | https://api.foundationfirst.app/api/trpc | Live environment for users |

## Security and Performance

### Security Requirements

**Frontend Security:**
- CSP Headers: `default-src 'self'; script-src 'self' 'unsafe-inline' https://api.mixpanel.com; connect-src 'self' https://*.supabase.co https://api.mixpanel.com`
- XSS Prevention: React Native's built-in XSS protection + input sanitization on all user-generated content (habit names, notes, foundation reflections)
- Secure Storage: Expo SecureStore for authentication tokens, AsyncStorage for non-sensitive cache data with encryption for offline habit logs

**Backend Security:**
- Input Validation: Zod schema validation on all tRPC endpoints with habit name length limits, foundation score ranges (1-10), and SQL injection prevention
- Rate Limiting: 100 requests per minute per user via Upstash Redis, with elevated limits for habit logging endpoints during peak usage
- CORS Policy: Restricted to production domains (`foundationfirst.app`, `*.foundationfirst.app`) with credentials allowed for authenticated requests

**Authentication Security:**
- Token Storage: JWT tokens in Expo SecureStore with automatic rotation, session cookies with httpOnly and secure flags for web dashboard
- Session Management: NextAuth.js with 7-day session expiration and automatic refresh, Supabase RLS policies for user data isolation
- Password Policy: Minimum 8 characters with complexity requirements, optional social login via Google OAuth for improved security

### Performance Optimization

**Frontend Performance:**
- Bundle Size Target: <5MB mobile app bundle, <500KB web dashboard initial load with code splitting
- Loading Strategy: Priority dashboard loads from cache first (instant), then fetches updates in background with optimistic UI updates
- Caching Strategy: React Query with 5-minute stale time for habit data, indefinite cache for user profile, aggressive caching for foundation trends

**Backend Performance:**
- Response Time Target: <200ms for priority dashboard queries, <500ms for foundation health calculations, <100ms for habit logging
- Database Optimization: Composite indexes on (user_id, classification, is_active) for dashboard queries, denormalized user_id in habit_logs for efficient filtering
- Caching Strategy: Redis caching for frequently accessed user priority data (5-minute TTL), Prisma query result caching, CDN caching for static assets

## Testing Strategy

### Testing Pyramid

```
                  E2E Tests
                 /        \
            Integration Tests
               /            \
          Frontend Unit  Backend Unit
```

### Test Organization

#### Frontend Tests
```
apps/expo/src/__tests__/
├── components/
│   ├── habits/
│   │   ├── HabitCard.test.tsx
│   │   ├── TrafficLightIndicator.test.tsx
│   │   └── HabitClassificationSelector.test.tsx
│   ├── foundation/
│   │   ├── FoundationHealthScale.test.tsx
│   │   └── WeeklyCheckinModal.test.tsx
│   └── dashboard/
│       ├── PriorityDashboard.test.tsx
│       └── MaintenanceSection.test.tsx
├── hooks/
│   ├── useHabits.test.ts
│   ├── useFoundationHealth.test.ts
│   └── useOfflineSync.test.ts
└── services/
    ├── habitPriorityService.test.ts
    ├── offlineStorageService.test.ts
    └── analyticsService.test.ts
```

#### Backend Tests
```
packages/api/src/__tests__/
├── router/
│   ├── habit.test.ts
│   ├── foundation.test.ts
│   ├── auth.test.ts
│   └── user.test.ts
├── services/
│   ├── habitPriority.test.ts
│   ├── foundationHealth.test.ts
│   └── streakCalculation.test.ts
└── integration/
    ├── habitWorkflow.test.ts
    ├── foundationCheckinFlow.test.ts
    └── onboardingFlow.test.ts
```

#### E2E Tests
```
e2e/
├── tests/
│   ├── onboarding.e2e.ts
│   ├── dailyHabitLogging.e2e.ts
│   ├── foundationCheckin.e2e.ts
│   ├── habitCreation.e2e.ts
│   └── offlineSync.e2e.ts
├── fixtures/
│   ├── users.ts
│   └── habits.ts
└── support/
    ├── commands.ts
    └── helpers.ts
```

## Coding Standards

### Critical Fullstack Rules

- **Type Sharing:** Always define shared types in packages/shared and import from there. Never duplicate type definitions between mobile and API packages.
- **API Calls:** Never make direct HTTP calls from frontend - always use tRPC procedures through the api client to maintain type safety.
- **Environment Variables:** Access only through typed config objects in packages/shared/src/config, never process.env directly in components.
- **Error Handling:** All tRPC procedures must use standardized error responses with proper TRPC error codes (UNAUTHORIZED, BAD_REQUEST, etc.).
- **State Updates:** Never mutate Zustand state directly - always use provided setter methods to maintain proper reactivity and persistence.
- **Habit Classification Logic:** All maintenance-first priority calculations must use shared utilities from packages/shared/src/utils/habitCalculations.ts.
- **Database Queries:** Always use Prisma client through repository pattern services, never raw SQL except for complex analytics queries.
- **Offline Storage:** Sensitive data must use Expo SecureStore, cached data uses AsyncStorage with proper encryption for habit logs.
- **Component Props:** All React components must have explicit TypeScript interfaces, no implicit any types allowed.
- **Authentication Context:** Never access user session data directly - always use tRPC context or React context providers.

### Naming Conventions

| Element | Frontend | Backend | Example |
|---------|----------|---------|---------|
| Components | PascalCase | - | `HabitCard.tsx`, `FoundationHealthScale.tsx` |
| Hooks | camelCase with 'use' | - | `useHabits.ts`, `useFoundationHealth.ts` |
| tRPC Procedures | - | camelCase | `getPriorityOrder`, `logCompletion` |
| Database Tables | - | snake_case | `habit_logs`, `foundation_checkins` |
| Stores | camelCase with 'Store' | - | `habitStore.ts`, `foundationStore.ts` |
| Services | camelCase with 'Service' | camelCase with 'Service' | `habitPriorityService.ts` |
| API Routes | - | kebab-case | `/api/trpc/habit.getPriorityOrder` |
| Constants | SCREAMING_SNAKE_CASE | SCREAMING_SNAKE_CASE | `MAINTENANCE_PRIORITY_WEIGHT` |

## Error Handling Strategy

### Error Flow

```mermaid
sequenceDiagram
    participant Client as Mobile App
    participant API as tRPC API
    participant DB as Database
    participant EXT as External APIs
    participant LOG as Error Tracking

    Client->>API: habit.logCompletion()
    
    alt Validation Error
        API->>API: Zod validation fails
        API->>LOG: Track validation error
        API-->>Client: BAD_REQUEST with field details
        Client->>Client: Show field-specific error
    else Database Error
        API->>DB: INSERT habit_log
        DB-->>API: Connection timeout
        API->>LOG: Track database error
        API-->>Client: INTERNAL_SERVER_ERROR
        Client->>Client: Store for offline sync
    else External API Error
        API->>EXT: Track to Mixpanel
        EXT-->>API: Rate limit exceeded
        API->>LOG: Track external API error
        API-->>Client: Success (tracking failure not critical)
    else Success
        API->>DB: INSERT success
        DB-->>API: Log created
        API->>EXT: Track completion event
        API-->>Client: Success with log data
        Client->>Client: Update UI optimistically
    end
```

### Error Response Format

```typescript
interface ApiError {
  error: {
    code: string;
    message: string;
    details?: Record<string, any>;
    timestamp: string;
    requestId: string;
    path?: string;
  };
}

// Standardized error codes for FoundationFirst
enum ErrorCode {
  // Authentication errors
  UNAUTHORIZED = 'UNAUTHORIZED',
  TOKEN_EXPIRED = 'TOKEN_EXPIRED',
  
  // Validation errors
  INVALID_HABIT_NAME = 'INVALID_HABIT_NAME',
  INVALID_FOUNDATION_SCORE = 'INVALID_FOUNDATION_SCORE',
  INVALID_CLASSIFICATION = 'INVALID_CLASSIFICATION',
  
  // Business logic errors
  HABIT_NOT_FOUND = 'HABIT_NOT_FOUND',
  DUPLICATE_DAILY_LOG = 'DUPLICATE_DAILY_LOG',
  ONBOARDING_INCOMPLETE = 'ONBOARDING_INCOMPLETE',
  
  // System errors
  DATABASE_ERROR = 'DATABASE_ERROR',
  RATE_LIMIT_EXCEEDED = 'RATE_LIMIT_EXCEEDED',
  EXTERNAL_API_ERROR = 'EXTERNAL_API_ERROR'
}
```

## Monitoring and Observability

### Monitoring Stack

- **Frontend Monitoring:** Sentry React Native SDK for crash reporting and performance monitoring
- **Backend Monitoring:** Vercel Analytics + Sentry Node.js for API performance and error tracking
- **Error Tracking:** Sentry with custom error boundaries and tRPC error integration
- **Performance Monitoring:** React Native Performance Monitor + Web Vitals tracking for future web dashboard

### Key Metrics

**Frontend Metrics:**
- Core Web Vitals (for future web dashboard)
- JavaScript errors and crash rates
- API response times from mobile app perspective
- User interactions (habit completions, foundation check-ins)
- Offline sync success/failure rates
- App launch time and screen transition performance

**Backend Metrics:**
- Request rate and response time per tRPC procedure
- Error rate by endpoint with habit classification breakdowns
- Database query performance for priority dashboard
- External API response times (Mixpanel, push notifications)
- Authentication success/failure rates
- Offline sync batch processing times

**Business Metrics (via Mixpanel):**
- Daily Active Users (DAU) and Weekly Active Users (WAU)
- Habit completion rates by classification (maintenance vs goal)
- Foundation check-in completion rates (target: 65%+)
- Onboarding completion rates and drop-off points
- User retention at 1, 7, and 30 days
- Traffic light urgency distribution (red/yellow/green habits)

## Artificial Intelligence Integration

### AI Strategy and Implementation

Based on FoundationFirst's maintenance-first philosophy and user-centric design, AI will enhance the user experience through intelligent insights, personalization, and automated guidance while maintaining the human-centered approach to habit formation.

### Phase 1 (MVP): Foundation AI Features

#### 1. **Intelligent Habit Classification Assistance**
```typescript
// AI-powered habit classification suggestions during creation
interface HabitClassificationAI {
  suggestedClassification: 'MAINTENANCE' | 'GOAL';
  confidence: number; // 0-1 confidence score
  reasoning: string;
  alternativeClassification?: {
    type: 'MAINTENANCE' | 'GOAL';
    reasoning: string;
  };
}

// Example implementation
const classifyHabitWithAI = async (habitName: string, description?: string): Promise<HabitClassificationAI> => {
  const prompt = `
    Analyze this habit and determine if it's a MAINTENANCE habit (foundational wellness that supports other goals) 
    or a GOAL habit (something you're building toward).
    
    Habit: "${habitName}"
    Description: "${description || 'None provided'}"
    
    MAINTENANCE examples: Sleep 7+ hours, Daily meditation, Take vitamins, Call family weekly
    GOAL examples: Learn Spanish, Run a marathon, Save $10k, Write a book
    
    Consider: Does this habit support your foundation and other goals, or is it an achievement you're working toward?
  `;
  
  // Integration with OpenAI API or similar
  return await aiClassificationService.analyze(prompt);
};
```

#### 2. **Smart Weekly Target Recommendations**
```typescript
// AI suggests appropriate weekly targets based on habit type and user history
interface WeeklyTargetRecommendation {
  suggestedTarget: number; // 1-7 days per week
  reasoning: string;
  adjustmentTips: string[];
  difficultyLevel: 'easy' | 'moderate' | 'challenging';
}

const recommendWeeklyTarget = async (
  habit: Partial<Habit>, 
  userHistory: UserHabitHistory
): Promise<WeeklyTargetRecommendation> => {
  // AI considers user's past success rates and habit type
  if (habit.classification === 'MAINTENANCE') {
    // Maintenance habits typically need higher frequency
    return {
      suggestedTarget: 5, // Start with 5 days for sustainability
      reasoning: "Maintenance habits build your foundation - consistency matters more than perfection",
      adjustmentTips: ["Start with 5 days and increase to 7 once established", "Focus on same time each day"],
      difficultyLevel: 'moderate'
    };
  } else {
    // Goal habits can be less frequent but more intensive
    return {
      suggestedTarget: 3,
      reasoning: "Goal habits benefit from focused, consistent effort rather than daily pressure",
      adjustmentTips: ["Choose specific days (e.g., Mon/Wed/Fri)", "Allow rest days for skill integration"],
      difficultyLevel: 'moderate'
    };
  }
};
```

### Phase 2 (Post-MVP): Advanced AI Features

#### 1. **Personalized Maintenance-First Coach**
```typescript
// AI coach that understands user's unique maintenance-first patterns
interface PersonalizedCoach {
  dailyGuidance: string;
  weeklyReflections: string;
  habitAdjustmentSuggestions: HabitAdjustment[];
  motivationalMessages: string[];
}

// AI learns from user behavior to provide personalized guidance
const generatePersonalizedCoaching = async (
  user: User,
  recentActivity: UserActivity
): Promise<PersonalizedCoach> => {
  // AI considers user's unique patterns, preferences, and success factors
  return {
    dailyGuidance: "Your energy tends to be highest when you complete your meditation habit first thing in the morning. Consider starting there today.",
    weeklyReflections: "This week you maintained 85% completion on your foundation habits and saw a 40% increase in goal habit success. This validates the maintenance-first approach for your lifestyle.",
    habitAdjustmentSuggestions: [
      {
        habitId: "sleep-habit-id",
        suggestion: "Consider moving your 'wind down routine' 30 minutes earlier - your best sleep weeks correlate with this timing.",
        reasoning: "Data shows 7.5+ hour sleep nights lead to 60% higher maintenance habit completion the following day."
      }
    ],
    motivationalMessages: ["Your consistent foundation work is paying off - your goal completion rate has improved 40% since focusing on maintenance habits first."]
  };
};
```

### AI Technology Stack

| AI Component | Technology | Purpose | Implementation Phase |
|--------------|------------|---------|---------------------|
| Habit Classification | OpenAI GPT-4 API | Classify habits as maintenance vs goal | Phase 1 (MVP) |
| Insight Generation | OpenAI GPT-4 API | Generate foundation health insights | Phase 1 (MVP) |
| Personalized Coaching | Custom fine-tuned model | Provide maintenance-first guidance | Phase 2 |
| Predictive Analytics | TensorFlow.js (client-side) | Predict foundation health trends | Phase 2 |
| Habit Recommendations | Hybrid AI + rule-based | Suggest complementary habits | Phase 2 |

### AI Ethics and User Control

#### 1. **Transparent AI Decision Making**
```typescript
// All AI recommendations include reasoning and confidence levels
interface AIRecommendation {
  recommendation: string;
  reasoning: string;
  confidence: number;
  dataUsed: string[];
  userCanOverride: boolean;
  alternativeOptions?: string[];
}
```

#### 2. **User Control and Opt-Out**
```typescript
// Users maintain full control over AI features
interface AISettings {
  enableHabitClassificationAI: boolean;
  enableFoundationInsights: boolean;
  enablePersonalizedCoaching: boolean;
  enablePredictiveAlerts: boolean;
  dataProcessingLevel: 'minimal' | 'standard' | 'comprehensive';
}
```

#### 3. **Maintenance-First AI Philosophy**
- **AI as Support, Not Replacement:** AI enhances human intuition about habits rather than dictating behavior
- **Foundation-Focused:** AI prioritizes recommendations that strengthen foundational wellness over achievement-oriented suggestions
- **Gentle Guidance:** AI recommendations use supportive, non-judgmental language aligned with the app's wellness-first philosophy
- **User Agency:** All AI suggestions are presented as options with clear rationale, never as mandates

---

## Architecture Validation Summary

This FoundationFirst fullstack architecture document has been comprehensively validated against industry standards and best practices. The architecture demonstrates:

✅ **Complete Requirements Alignment** - All PRD functional and non-functional requirements addressed with specific technical solutions

✅ **Production-Ready Design** - Comprehensive security, monitoring, deployment, and error handling strategies 

✅ **Maintenance-First Integration** - Core business philosophy embedded throughout all architectural layers

✅ **Type-Safe Implementation** - End-to-end TypeScript with tRPC + Prisma ensuring runtime error elimination

✅ **AI-Agent Optimized** - Clear patterns, interfaces, and comprehensive examples enabling effective AI development

✅ **Scalable Foundation** - T3 stack architecture supports growth from MVP to enterprise scale

The architecture is ready for immediate implementation with a **HIGH confidence rating** for successful delivery within the 6-8 week MVP timeline.