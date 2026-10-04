# TaskFlow

TaskFlow is a modern task-management mobile application built with **React Native, Expo, and TypeScript**.

The application provides task creation and management, local SQLite persistence, CSV bulk import/export, calendar-based task browsing, search/filter/sort functionality, theme switching, validation, and a reusable component-based UI.

## Features

### Dashboard
- Total task count
- Completed and pending task statistics
- Tasks due today
- Quick task creation
- CSV import and export
- Calendar / Today navigation
- Light and dark themes

### Task Management
- Create tasks
- Edit tasks
- View task details
- Delete tasks
- Mark tasks as completed
- Search tasks
- Filter tasks
- Sort tasks
- Task categories
- Low / Medium / High priorities
- Start and due dates
- Pending / Completed status
- Overdue task handling

### Calendar
- Browse tasks by date
- Select a date to view associated tasks
- Quick navigation to today

### Bulk CSV Import
TaskFlow supports importing task data from CSV files.

Expected columns:

```csv
id,title,description,category,priority,start_date,due_date,status
```

The importer validates:
- Required fields
- Required CSV columns
- Date format
- Start date / due date relationship
- Priority values
- Status values
- Duplicate IDs within the CSV
- IDs already present in SQLite
- Empty or malformed CSV files
- Mixed valid and invalid rows

Valid records can be imported while invalid rows are reported with validation errors.

### Settings
- Light / dark theme
- Clear locally stored tasks
- Application information

## Tech Stack

| Technology | Purpose |
|---|---|
| React Native | Mobile application |
| Expo SDK 57 | Development and runtime platform |
| TypeScript | Static type safety |
| Expo Router | File-based navigation |
| expo-sqlite | Local persistent storage |
| Papa Parse | CSV parsing |
| Expo Document Picker | File selection |
| Jest / jest-expo | Automated testing |
| EAS Build | Android APK builds |
| pnpm | Package management |

## Architecture

TaskFlow follows a layered architecture that keeps UI, application logic, and persistence separated:

```text
Screens
   ↓
Reusable UI Components
   ↓
Hooks / Application Logic
   ↓
Repository Layer
   ↓
SQLite
```

### Data Flow

- **Parent → child:** props
- **Child → parent:** callback props
- **Sibling components:** shared state is lifted to their common parent
- **Application-wide theme:** React Context
- **Persistent task data:** SQLite through `useTasks()` and the repository layer
- **Unfinished task form:** `TaskFormDraftContext`
- **Screen navigation:** Expo Router route parameters

SQLite is the source of truth for persistent task data. UI state is synchronized after successful database operations.

## Reusable UI

The application uses shared UI primitives to maintain consistent behavior and visual design across screens.

Examples include:

- `Button`
- `IconButton`
- `Card`
- `Input`
- `DateField`
- `ScreenHeader`
- `Dialog`
- `BottomNav`
- Task-specific reusable components

Common controls and screen headers are not independently reimplemented on each screen.

## Project Structure

```text
TaskFlow/
├── src/
│   ├── app/              # Expo Router screens
│   ├── components/
│   │   ├── task/         # Task-specific components
│   │   └── ui/           # Reusable UI components
│   ├── context/          # Application contexts
│   ├── database/         # SQLite initialization and repository
│   ├── hooks/            # Application hooks
│   ├── services/         # Import/export services
│   ├── theme/            # Theme context and design tokens
│   ├── types/            # Shared TypeScript types
│   └── utils/            # Validation and utility functions
├── __tests__/             # Automated tests
├── assets/                # Application assets
├── app.json               # Expo configuration
├── eas.json               # EAS Build configuration
├── package.json
├── pnpm-lock.yaml
└── tsconfig.json
```

## Validation

Task validation is centralized in the utility layer.

Required task fields include:

- Title
- Category
- Priority
- Start date
- Due date

Business validation ensures:

```text
Due Date >= Start Date
```

The same validation principles are applied to imported task records before they are persisted.

## Local Persistence

TaskFlow does not require a backend server.

Task data is stored locally in SQLite, allowing the application to:

- Create tasks offline
- Edit tasks offline
- Delete tasks offline
- Search and filter locally
- Import CSV data
- Persist tasks between application launches

SQLite was selected because task data requires structured querying, filtering, updating, deleting, duplicate detection, and bulk operations.

## Testing

The project includes automated Jest tests covering core validation and utility logic.

Current test result:

```text
Test Suites: 3 passed, 3 total
Tests:       11 passed, 11 total
Snapshots:   0 total
```

Run the test suite:

```bash
pnpm test
```

Run TypeScript validation:

```bash
pnpm exec tsc --noEmit
```

## QA

The application has been manually tested for:

- Task CRUD flows
- Navigation and back behavior
- Form validation
- Draft persistence
- Form reset after successful creation
- SQLite persistence
- Light / dark theme behavior
- Search, filtering, and sorting
- Calendar task browsing
- CSV validation and error handling
- Duplicate CSV IDs
- Existing database duplicates
- Mixed valid / invalid CSV rows
- Large CSV imports, including a 500-row test file

## Getting Started

### Requirements

- Node.js
- pnpm
- Android emulator/device or iOS simulator/device

### Install dependencies

```bash
pnpm install
```

### Start the development server

```bash
pnpm expo start
```

Useful commands:

```bash
pnpm expo start -c
pnpm expo start --android
pnpm expo start --ios
pnpm expo start --web
pnpm lint
pnpm test
```

## Android Build

Android preview builds are configured through EAS.

Build an APK with:

```bash
pnpm dlx eas-cli@latest build --platform android --profile preview
```

The generated APK can be installed directly on an Android device.

## Design

TaskFlow uses a clean, modern productivity-app design language:

- Consistent reusable components
- Light and dark themes
- Blue primary accent
- Rounded cards
- Subtle borders
- Consistent spacing and typography
- Clear loading, empty, and error states
- Accessible interactive controls
- Consistent screen headers
- Mobile-friendly layouts

## Assessment Coverage

| Requirement | Status |
|---|---|
| Dashboard | ✅ |
| Task List | ✅ |
| Add / Edit Task | ✅ |
| Task Details | ✅ |
| Bulk CSV Import | ✅ |
| SQLite persistence | ✅ |
| Navigation | ✅ |
| Reusable components | ✅ |
| Validation | ✅ |
| Loading states | ✅ |
| Empty states | ✅ |
| Error handling | ✅ |
| Light / Dark theme | ✅ |
| Search | ✅ |
| Filtering | ✅ |
| Sorting | ✅ |
| Calendar | ✅ |
| CSV Export | ✅ |
| Android APK | ✅ |
| Automated tests | ✅ |

## Repository

GitHub: https://github.com/ajay-biswal/Task-Management-App

## License

This project is licensed under the MIT License.
