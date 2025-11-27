# LifeOS Design Guidelines

## Design Approach
**System:** Hybrid approach drawing from Linear (clean productivity), Notion (flexible data views), and Material Design (structured information architecture)

**Rationale:** LifeOS handles sensitive, multi-domain personal data requiring trust, clarity, and efficiency over visual flair. Users need confidence in security and quick access to life-critical information.

**Core Principles:**
- Trust through clarity: Clean, professional interface signals security
- Information density without overwhelm: Smart progressive disclosure
- Domain separation: Visual distinction between life areas (Health, Career, Finance, etc.)
- Guided intelligence: AI feels helpful, not intrusive

## Typography System

**Font Families:**
- Primary: Inter (clean, readable for data and forms)
- Accent: DM Sans (headings, emphasis areas)

**Hierarchy:**
- Page titles: text-3xl font-semibold (32px)
- Section headers: text-xl font-medium (20px)
- Card titles: text-lg font-medium (18px)
- Body text: text-base (16px)
- Labels/metadata: text-sm (14px)
- Micro-copy: text-xs (12px)

## Layout System

**Spacing Primitives:** Use tailwind units of 2, 4, 6, 8, 12, 16, 24
- Tight spacing: p-2, gap-2 (within components)
- Standard spacing: p-4, gap-4, m-6 (between elements)
- Section spacing: p-8, py-12 (major sections)
- Generous spacing: p-16, py-24 (dashboard panels)

**Grid Structure:**
- Dashboard: Sidebar (280px fixed) + Main content (fluid)
- Profile tabs: Full-width with max-w-5xl container
- Cards: grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6
- Forms: max-w-2xl single column for readability

## Component Library

### Navigation
- **Sidebar:** Fixed left, 280px width with domain icons (Health, Career, Finance, Goals, etc.) - each domain gets unique icon, collapsible on mobile
- **Top bar:** User profile dropdown (right), notification bell, privacy status indicator (lock icon with "Encrypted" badge)
- **Tab navigation:** Horizontal tabs for profile sections with clear active state (underline + font-semibold)

### Profile & Onboarding
- **Progress wizard:** Step indicator (1/5) with connecting lines, current step highlighted, completed steps with checkmark
- **Profile cards:** Each domain (Identity, Health, Career) as distinct card with icon, completion percentage, "Edit" button
- **Smart questions:** Single question per screen, large input area, "Skip for now" option always visible, subtle "Why we ask" expandable tooltip

### AI Chat Interface
- **Layout:** Fixed bottom-right chat bubble (collapsed), expands to 400px width panel overlay
- **Messages:** User messages right-aligned, AI responses left with avatar icon, timestamp text-xs
- **Input:** Floating textarea with "Ask anything..." placeholder, send button, voice input icon
- **Context indicators:** Small pills showing which profile data AI referenced ("Using: Career + Finance data")

### Dashboard Components
- **Stat cards:** Icon + number (text-3xl font-bold) + label + trend indicator
- **Goal tracker:** Progress bars with percentage, target date, "Update progress" quick action
- **Quick actions grid:** 2x2 or 3x3 button grid ("Generate Resume", "Create Budget", "Plan Workout") with icons
- **Recent activity feed:** Timeline-style list with timestamps, icons, and action descriptions

### Forms & Data Entry
- **Input fields:** Full-width with clear labels above, helper text below, validation states
- **Section grouping:** Fieldset with subtle border, legend as section title
- **File uploads:** Drag-and-drop zone (dashed border) with upload icon and file list below
- **Multi-select:** Pill-style tags with remove button, searchable dropdown

### Privacy & Security Elements
- **Privacy center:** Grid of permission cards showing what data is collected, toggle switches, "Last accessed" timestamps
- **Encryption badge:** Persistent indicator in footer "End-to-end encrypted" with lock icon
- **Data export:** Clear "Download all data" button with file format options

### Modals & Overlays
- **Modal:** Centered, max-w-2xl, backdrop blur, close button top-right
- **Action confirmations:** Smaller modal (max-w-md) with clear primary/secondary buttons
- **Toast notifications:** Top-right, slide-in animation, auto-dismiss after 4s

## Accessibility
- All interactive elements min-height: h-11 (44px touch target)
- Form inputs with explicit labels, proper ARIA attributes
- Focus rings visible on all interactive elements (ring-2 offset-2)
- Keyboard navigation for dashboard, tabs, and modals
- Screen reader announcements for AI responses and form validations

## Images
**Hero Section:** No traditional hero - dashboard launches immediately after login to prioritize utility
**Onboarding splash:** Single welcome screen with illustration of person + AI assistant (abstract, friendly style) centered, max-w-md
**Empty states:** Illustrations for each domain when no data exists (e.g., health icon with "Add your first health record")
**Profile placeholders:** Avatar placeholder with upload icon until user adds photo

## Animations
**Minimal and purposeful only:**
- Page transitions: 200ms fade
- Card hover: Subtle lift (translate-y-1) with shadow increase
- Modal entry: 300ms scale + fade
- Progress indicators: Smooth width transitions
- NO scroll animations, parallax, or decorative motion