# LifeOS - Personal AI Operating System

## Overview

LifeOS is a secure, privacy-first personal AI assistant that learns a user's complete life profile across multiple domains (health, career, finance, relationships, learning, goals) and provides personalized guidance and actionable insights. The application is built as a full-stack web application with a focus on progressive profiling, end-to-end encryption, and user-controlled data management.

The system combines a React-based frontend with shadcn/ui components, an Express backend, PostgreSQL database via Drizzle ORM, and OpenAI GPT-5 integration for AI-powered assistance. Authentication is handled through Replit's OIDC provider with session management.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework**: React 18 with TypeScript, built using Vite for development and production bundling.

**UI Component System**: shadcn/ui (New York variant) with Radix UI primitives provides accessible, composable components. The design system follows a hybrid approach drawing from Linear (clean productivity), Notion (flexible data views), and Material Design (structured information architecture).

**Styling**: Tailwind CSS with custom design tokens for colors, typography, and spacing. The system supports light/dark themes with HSL-based color variables. Typography uses Inter for body text and DM Sans for headings/accents.

**State Management**: TanStack Query (React Query) handles server state, caching, and data synchronization. Custom hooks (useAuth, useIsMobile) encapsulate reusable logic.

**Routing**: Wouter provides lightweight client-side routing with protected routes based on authentication status.

**Layout Structure**: 
- Fixed sidebar navigation (280px) with domain-specific sections
- Main content area with responsive grid layouts
- Progressive onboarding wizard with step indicators
- Tab-based profile sections for different life domains

**Rationale**: This architecture prioritizes developer experience, accessibility, and performance. shadcn/ui provides full component ownership while maintaining consistency. TanStack Query eliminates boilerplate for API interactions and provides automatic caching/refetching.

### Backend Architecture

**Framework**: Express.js with TypeScript running on Node.js.

**API Design**: RESTful endpoints organized by domain:
- `/api/auth/*` - Authentication and user management
- `/api/profile` - User profile CRUD operations
- `/api/goals` - Goal tracking and management
- `/api/habits` - Habit tracking and management
- `/api/chat/*` - AI chat message handling
- `/api/generate/*` - AI-generated content (resumes, budgets, meal plans, study schedules)

**Authentication**: OpenID Connect (OIDC) via Replit Auth with Passport.js strategy. Sessions stored in PostgreSQL using connect-pg-simple with 7-day TTL.

**Error Handling**: Centralized error handling with proper HTTP status codes. 401 errors trigger automatic redirect to login on the frontend.

**Logging**: Request/response logging middleware tracks API calls, duration, and response data (truncated for readability).

**Rationale**: Express provides flexibility while remaining lightweight. RESTful design ensures predictable API patterns. OIDC authentication leverages Replit's secure identity provider rather than implementing custom auth.

### Data Architecture

**Database**: PostgreSQL hosted on Neon (serverless PostgreSQL).

**ORM**: Drizzle ORM provides type-safe database queries and migrations. Schema defined in TypeScript with automatic type inference.

**Schema Design**:

**Core Tables**:
- `users` - User identity (id, email, names, profile image, onboarding status)
- `profiles` - Comprehensive life data across domains (health, career, finance, relationships, learning)
- `goals` - User goals with status tracking, deadlines, and progress
- `habits` - Habit tracking with frequency, streaks, and completion records
- `chat_messages` - AI conversation history
- `generated_outputs` - AI-generated content (resumes, budgets, etc.)
- `sessions` - Session storage for authentication

**Data Relationships**:
- One-to-one: user → profile
- One-to-many: user → goals, habits, chat messages, generated outputs

**Data Storage Strategy**: All sensitive user data stored in PostgreSQL with foreign key constraints ensuring referential integrity. Sessions table supports automatic expiration cleanup.

**Rationale**: PostgreSQL provides ACID compliance and robust data types (JSONB for flexible metadata storage). Drizzle ORM offers better TypeScript integration than alternatives like Prisma while maintaining SQL visibility. Neon's serverless architecture eliminates infrastructure management.

### AI Integration

**Provider**: OpenAI API with GPT-5 model (latest as of August 2025).

**AI Capabilities**:
- Personal assistant chat with context awareness
- Resume generation from profile data
- Budget planning based on financial information
- Meal plan creation using health/dietary data
- Study schedule generation for learning goals

**Context Management**: User profile data serialized and included in system prompts to provide personalized responses. Chat history stored for conversation continuity.

**Rationale**: GPT-5 provides state-of-the-art natural language understanding and generation. Structured prompts ensure AI responses align with LifeOS's actionable, privacy-focused philosophy.

### Security & Privacy Design

**Authentication Security**:
- OIDC-based authentication with Replit as identity provider
- Secure session cookies (httpOnly, secure flags, 7-day max age)
- Session data encrypted in PostgreSQL

**Data Protection**:
- All user data associated with authenticated user ID
- Row-level security via foreign key constraints
- Sensitive operations require authentication middleware

**Privacy Features** (designed but implementation varies):
- Local-first data storage capability (referenced in design docs)
- End-to-end encryption for cloud sync (referenced, not yet implemented)
- User-controlled data export and deletion
- Zero-knowledge architecture principles (aspirational)

**Rationale**: OIDC delegating to Replit Auth reduces security surface area. Session-based auth simpler than JWT for server-rendered pages. Privacy-by-design principles guide feature development even if full encryption not yet implemented.

### Progressive Profiling System

**Onboarding Flow**: 5-step wizard collects minimal initial data:
1. Identity basics (DOB, gender, location)
2. Health information
3. Career details
4. Financial overview
5. Learning preferences

**Profile Expansion**: Tab-based interface allows users to add detailed information across domains on-demand rather than requiring everything upfront.

**Guided Data Collection**: AI assistant can request specific information when needed for personalized recommendations.

**Rationale**: Progressive profiling reduces initial friction while enabling deep personalization over time. Users provide data as they see value, improving retention.

## External Dependencies

### Authentication & Identity
- **Replit OIDC** - Identity provider for user authentication
- **Passport.js with openid-client** - OIDC strategy implementation

### Database & Storage
- **Neon Database** - Serverless PostgreSQL hosting
- **@neondatabase/serverless** - PostgreSQL driver optimized for serverless environments
- **Drizzle ORM** - TypeScript-first ORM for type-safe queries
- **connect-pg-simple** - PostgreSQL session store for Express

### AI Services
- **OpenAI API** - GPT-5 model for AI assistant, content generation, and personalized recommendations

### Frontend Libraries
- **React 18** - UI framework
- **TanStack Query** - Server state management and caching
- **Wouter** - Lightweight routing
- **shadcn/ui** - Component library built on Radix UI primitives
- **Radix UI** - Headless UI primitives (30+ component packages)
- **Tailwind CSS** - Utility-first CSS framework
- **class-variance-authority** - Component variant management
- **React Hook Form** - Form state management with validation
- **Zod** - Schema validation
- **date-fns** - Date manipulation utilities
- **Lucide React** - Icon library

### Backend Libraries
- **Express.js** - Web application framework
- **ws** - WebSocket library for Neon database connections
- **memoizee** - Function memoization for OIDC config caching

### Build Tools
- **Vite** - Frontend build tool and dev server
- **TypeScript** - Type safety across stack
- **esbuild** - Fast JavaScript/TypeScript bundler for backend
- **tsx** - TypeScript execution for development
- **PostCSS** - CSS processing with Tailwind

### Development Tools (Replit-specific)
- **@replit/vite-plugin-runtime-error-modal** - Development error overlay
- **@replit/vite-plugin-cartographer** - Code navigation
- **@replit/vite-plugin-dev-banner** - Development environment indicator