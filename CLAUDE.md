# CLAUDE.md — GymApp Project Context

## Project Overview

Minimalistic gym/workout tracker for iOS + Apple Watch, inspired by Strong and Hevy. No social features. Focus on clean UX and fast in-gym usage.

## Tech Stack

- **Language:** Swift
- **UI Framework:** SwiftUI
- **Storage:** SwiftData (local only, on-device)
- **Platforms:** iOS (primary), watchOS (companion)
- **Min Targets:** iOS 17+, watchOS 10+
- **IDE:** Xcode

## Core Features

- Workout templates (create, edit, reorder)
- Pre-built exercise library with muscle group tags
- Custom exercise creation (name, muscle group, equipment type)
- Workout logging: Sets × Reps × Weight per exercise
- Rest timer between sets
- Workout history / log
- Apple Watch companion for active workout tracking

## Architecture Decisions

- SwiftData for persistence — no Core Data, no SQLite
- Shared Swift package for models and business logic (reused by iOS and watchOS targets)
- No backend, no auth, no cloud sync (for now)
- Soft-delete pattern for exercises (`isHidden` flag, not hard delete)
- Duplicate exercise name validation before insert

## Data Models (established)

- `Exercise` — name, muscleGroup, equipment, isCustom, isHidden
- `WorkoutTemplate` — name, ordered list of exercises with default sets/reps
- `WorkoutSession` — date, duration, exercises performed
- `ExerciseSet` — reps, weight, completed flag (belongs to a session entry)

## Project Phases

### Phase 1 — iPhone App Foundation
- [ ] Xcode project setup with iOS + shared Swift package
- [ ] SwiftData model definitions (Exercise, WorkoutTemplate, WorkoutSession, ExerciseSet)
- [ ] Pre-seeded exercise library (JSON bundled in app, imported on first launch)
- [ ] Exercise list screen with search and muscle group filtering
- [ ] Custom exercise form (add/edit) with duplicate name validation
- [ ] Soft-delete for custom exercises

### Phase 2 — Workout Templates
- [ ] Create/edit workout templates
- [ ] Add exercises to template with default sets/reps/weight
- [ ] Reorder exercises within a template (drag & drop)
- [ ] Template list screen (home screen)
- [ ] Duplicate/clone a template

### Phase 3 — Active Workout
- [ ] Start workout from template (creates a WorkoutSession)
- [ ] Log sets: reps, weight, mark complete
- [ ] Add/remove sets dynamically during workout
- [ ] Rest timer (configurable duration, auto-start on set completion)
- [ ] Add exercises mid-workout (not in original template)
- [ ] Finish/cancel workout with confirmation
- [ ] Workout duration tracking

### Phase 4 — Workout History
- [ ] Workout history list (date, name, duration, volume)
- [ ] Workout detail view (exercises, sets, weights)
- [ ] Per-exercise history (progress over time)
- [ ] Basic stats (total workouts, total volume, streaks)

### Phase 5 — Apple Watch App
- [ ] Add watchOS target to Xcode project
- [ ] Share Swift package (models + business logic) with watchOS
- [ ] Watch app: start workout from template
- [ ] Watch app: log sets (reps, weight) with Digital Crown input
- [ ] Watch app: rest timer with haptic feedback
- [ ] Watch app: finish workout
- [ ] Data sync between iPhone and Watch via SwiftData + App Group

### Phase 6 — Polish & QA
- [ ] Onboarding / empty states
- [ ] Haptics and animations
- [ ] Accessibility (VoiceOver, Dynamic Type)
- [ ] App icon and launch screen
- [ ] Unit tests for business logic
- [ ] UI tests for critical flows
- [ ] TestFlight beta

## Code Style Preferences

- Use SwiftUI views — no UIKit unless absolutely necessary
- Prefer `@Query` and `@Environment(\.modelContext)` for data access in views
- Keep views small: extract subviews early
- Business logic stays out of views — use service/manager types in the shared package
- Name files after the primary type they contain (e.g., `Exercise.swift`, `WorkoutSession.swift`)
- Group files by feature, not by type (e.g., `Exercises/`, `Templates/`, `Workout/`, `History/`)

## What NOT to Build (for now)

- No social features (sharing, leaderboards, friends)
- No cloud sync or backend
- No in-app purchases or subscriptions
- No Android / Wear OS
- No HealthKit integration (consider for future)
- No analytics or tracking SDKs
