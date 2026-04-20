# Requirements Document: Kairos Firebase Rebuild

## Introduction

The Kairos Program Tracker is a web application designed to manage entrepreneurship accelerator programs. This rebuild transforms the application from a single-user, local-storage system into a multi-user, real-time collaborative platform using Firebase/Firestore. The system supports two distinct user roles: Teams (participants in the program) and Program Managers (administrators who oversee multiple teams and cohorts).

The application manages a 7-month program structure (M0-M6) spanning five phases: Pre-Program, Problem Validation, Solution Design, Pilot/Traction, and Demo Day. Teams progress through monthly gates with checklists and assignments, while Program Managers review submissions, manage schedules, and configure program settings.

## Glossary

- **Team_Portal**: The user interface accessible to program participant teams without authentication
- **PM_Portal**: The Program Manager Portal, a PIN-protected administrative interface
- **Team**: A group of participants progressing through the accelerator program
- **Cohort**: A collection of teams that move through the program together on the same schedule
- **Gate**: A milestone checkpoint at the end of each program month requiring specific deliverables
- **Assignment**: A deliverable that teams must submit for Program Manager review
- **Gate_Decision**: A Program Manager's determination of whether a team has met gate requirements (Approved, Conditional, or Not Approved)
- **Meeting**: A scheduled program event (M0-M6) with date and Zoom link
- **Firestore**: The Firebase real-time database used for data persistence
- **Call_Target**: The cumulative number of customer discovery calls expected by specific program milestones
- **Assignment_Status**: The state of an assignment (Not Started, In Progress, Submitted, Approved, Changes Requested)
- **Program_Phase**: One of five stages in the program lifecycle (Pre-Program, Problem Validation, Solution Design, Pilot/Traction, Demo Day)
- **Review_Queue**: A centralized view of all submitted assignments awaiting Program Manager review
- **Settings**: Configurable program parameters managed by the Program Manager

## Requirements

### Requirement 1: Portal Selection Interface

**User Story:** As a user, I want to select between Team Portal and Program Manager Portal on the home page, so that I can access the appropriate interface for my role.

#### Acceptance Criteria

1. THE Application SHALL display two portal selection options on the home page
2. WHEN a user selects "Team Portal", THE Application SHALL navigate to the Team Portal authentication screen
3. WHEN a user selects "Program Manager Portal", THE Application SHALL navigate to the PM Portal PIN entry screen
4. THE Application SHALL display clear visual distinction between the two portal options
5. FOR ALL portal selections, THE Application SHALL preserve the selection in the browser session

### Requirement 2: Team Portal Authentication

**User Story:** As a team member, I want to select my team name from a dropdown without entering a password, so that I can quickly access my team's workspace.

#### Acceptance Criteria

1. THE Team_Portal SHALL display a dropdown containing all team names from Firestore
2. WHEN a team name is selected, THE Team_Portal SHALL authenticate the user without requiring a password
3. WHEN authentication succeeds, THE Team_Portal SHALL load the team's data from Firestore
4. THE Team_Portal SHALL display the selected team name in the interface header
5. FOR ALL team selections, THE Team_Portal SHALL maintain the authentication state during the browser session

### Requirement 3: Program Manager Portal Authentication

**User Story:** As a Program Manager, I want to enter a PIN to access the PM Portal, so that only authorized users can manage program data.

#### Acceptance Criteria

1. THE PM_Portal SHALL display a PIN entry field on the authentication screen
2. WHEN the entered PIN matches the stored PIN in Firestore Settings, THE PM_Portal SHALL grant access
3. WHEN the entered PIN does not match, THE PM_Portal SHALL display an error message and remain on the authentication screen
4. THE PM_Portal SHALL use a default PIN of "1234" when no PIN is configured in Settings
5. FOR ALL successful PIN entries, THE PM_Portal SHALL maintain the authentication state during the browser session

### Requirement 4: Team Portal Home Tab

**User Story:** As a team member, I want to view my team's progress overview on the Home tab, so that I can quickly understand our current status and next steps.

#### Acceptance Criteria

1. THE Team_Portal SHALL display the current gate status on the Home tab
2. THE Team_Portal SHALL display overall progress percentage on the Home tab
3. THE Team_Portal SHALL display the next scheduled meeting date and Zoom link on the Home tab
4. THE Team_Portal SHALL display the current program phase on the Home tab
5. WHEN team data changes in Firestore, THE Team_Portal SHALL update the Home tab display in real-time

### Requirement 5: Team Portal Work Tab

**User Story:** As a team member, I want to view gate checklists and submit assignments on the Work tab, so that I can complete program requirements and receive feedback.

#### Acceptance Criteria

1. THE Team_Portal SHALL display all gate checklists for the current month on the Work tab
2. THE Team_Portal SHALL display all assignments with their current Assignment_Status on the Work tab
3. WHEN a team member submits an assignment, THE Team_Portal SHALL update the Assignment_Status to "Submitted" in Firestore
4. WHEN a Program Manager provides feedback, THE Team_Portal SHALL display the feedback on the Work tab
5. THE Team_Portal SHALL allow teams to mark checklist items as complete and persist the state to Firestore
6. FOR ALL assignment submissions, THE Team_Portal SHALL record the submission timestamp in Firestore

### Requirement 6: Team Portal Calls Tab

**User Story:** As a team member, I want to view an embedded Airtable form on the Calls tab, so that I can log customer discovery calls.

#### Acceptance Criteria

1. THE Team_Portal SHALL display an Airtable iframe on the Calls tab
2. THE Team_Portal SHALL load the Airtable URL configured in Firestore Settings
3. WHEN no Airtable URL is configured, THE Team_Portal SHALL display a message indicating the feature is not yet configured
4. THE Team_Portal SHALL display the current call count and Call_Target on the Calls tab
5. FOR ALL teams, THE Team_Portal SHALL use the team-specific Airtable configuration if available

### Requirement 7: Team Portal Schedule Tab

**User Story:** As a team member, I want to view my assigned meetings on the Schedule tab, so that I know when program events are scheduled.

#### Acceptance Criteria

1. THE Team_Portal SHALL display all meetings assigned to the team's Cohort on the Schedule tab
2. THE Team_Portal SHALL display meeting dates, times, and Zoom links in read-only format
3. THE Team_Portal SHALL display meetings in chronological order (M0 through M6)
4. WHEN meeting data changes in Firestore, THE Team_Portal SHALL update the Schedule tab in real-time
5. THE Team_Portal SHALL indicate which meetings have passed and which are upcoming

### Requirement 8: Team Portal Resources Tab

**User Story:** As a team member, I want to access program resources on the Resources tab, so that I can find important links and program information.

#### Acceptance Criteria

1. THE Team_Portal SHALL display the Canvas URL configured in Firestore Settings on the Resources tab
2. THE Team_Portal SHALL display the CRM URL configured in Firestore Settings on the Resources tab
3. THE Team_Portal SHALL display monthly objectives for all program months on the Resources tab
4. THE Team_Portal SHALL display the full program roadmap showing all phases and gates on the Resources tab
5. WHEN Settings change in Firestore, THE Team_Portal SHALL update the Resources tab in real-time

### Requirement 9: Program Manager Portal Teams Tab

**User Story:** As a Program Manager, I want to create and manage cohorts and teams on the Teams tab, so that I can organize program participants.

#### Acceptance Criteria

1. THE PM_Portal SHALL display all existing Cohorts on the Teams tab
2. THE PM_Portal SHALL allow the Program Manager to create new Cohorts and persist them to Firestore
3. THE PM_Portal SHALL allow the Program Manager to add new Teams to a Cohort and persist them to Firestore
4. THE PM_Portal SHALL allow the Program Manager to delete Teams and remove their data from Firestore
5. THE PM_Portal SHALL allow the Program Manager to reassign Teams to different Cohorts and update Firestore
6. WHEN a Program Manager selects a Team, THE PM_Portal SHALL open a full team detail view
7. THE PM_Portal SHALL display gate decisions and assignment reviews in the team detail view
8. FOR ALL team management operations, THE PM_Portal SHALL update Firestore and reflect changes in real-time

### Requirement 10: Program Manager Portal Review Queue Tab

**User Story:** As a Program Manager, I want to review submitted assignments in a centralized queue, so that I can efficiently provide feedback to all teams.

#### Acceptance Criteria

1. THE PM_Portal SHALL display all assignments with Assignment_Status "Submitted" in the Review_Queue
2. THE PM_Portal SHALL allow the Program Manager to approve assignments and update Assignment_Status to "Approved" in Firestore
3. THE PM_Portal SHALL allow the Program Manager to request changes and update Assignment_Status to "Changes Requested" in Firestore
4. THE PM_Portal SHALL allow the Program Manager to add feedback text that persists to Firestore
5. THE PM_Portal SHALL display team name and assignment name for each item in the Review_Queue
6. WHEN assignments are submitted or reviewed, THE PM_Portal SHALL update the Review_Queue in real-time
7. FOR ALL review actions, THE PM_Portal SHALL record the review timestamp in Firestore

### Requirement 11: Program Manager Portal Schedule Tab

**User Story:** As a Program Manager, I want to manage program meetings on the Schedule tab, so that I can coordinate the program calendar.

#### Acceptance Criteria

1. THE PM_Portal SHALL display all seven program meetings (M0-M6) on the Schedule tab
2. THE PM_Portal SHALL allow the Program Manager to add new meetings and persist them to Firestore
3. THE PM_Portal SHALL allow the Program Manager to edit meeting dates and Zoom links and update Firestore
4. THE PM_Portal SHALL allow the Program Manager to delete meetings and remove them from Firestore
5. THE PM_Portal SHALL allow the Program Manager to assign meetings to specific Cohorts and update Firestore
6. THE PM_Portal SHALL pre-populate the seven standard program meetings (M0-M6) when a new program is initialized
7. FOR ALL schedule changes, THE PM_Portal SHALL update Firestore and reflect changes in real-time to all users

### Requirement 12: Program Manager Portal Settings Tab

**User Story:** As a Program Manager, I want to configure program settings, so that I can customize the application for my program's needs.

#### Acceptance Criteria

1. THE PM_Portal SHALL allow the Program Manager to set the program name and persist it to Firestore Settings
2. THE PM_Portal SHALL allow the Program Manager to set the Canvas URL and persist it to Firestore Settings
3. THE PM_Portal SHALL allow the Program Manager to set the CRM URL and persist it to Firestore Settings
4. THE PM_Portal SHALL allow the Program Manager to configure Airtable settings and persist them to Firestore Settings
5. THE PM_Portal SHALL allow the Program Manager to change the PM PIN and persist it to Firestore Settings
6. WHEN Settings are updated, THE PM_Portal SHALL update Firestore and reflect changes in real-time to all users
7. FOR ALL Settings changes, THE PM_Portal SHALL validate input format before persisting to Firestore

### Requirement 13: Seven-Month Program Structure

**User Story:** As a Program Manager, I want the application to support a seven-month program structure (M0-M6), so that teams can progress through the standard accelerator timeline.

#### Acceptance Criteria

1. THE Application SHALL define seven program months labeled M0 through M6
2. THE Application SHALL associate each month with one of five Program_Phases (Pre-Program, Problem Validation, Solution Design, Pilot/Traction, Demo Day)
3. THE Application SHALL define gate requirements for each month and store them in the application logic
4. THE Application SHALL associate checklists with each gate and store them in the application logic
5. FOR ALL teams, THE Application SHALL track progress through all seven months in Firestore

### Requirement 14: Gate Requirements and Checklists

**User Story:** As a team member, I want to see gate requirements and checklists for each month, so that I know what deliverables are expected.

#### Acceptance Criteria

1. THE Application SHALL define gate requirements for each program month in the application logic
2. THE Application SHALL define checklist items for each gate in the application logic
3. THE Team_Portal SHALL display gate requirements and checklists on the Work tab
4. THE Team_Portal SHALL allow teams to mark checklist items as complete and persist the state to Firestore
5. THE Team_Portal SHALL calculate gate completion percentage based on completed checklist items

### Requirement 15: Assignment Tracking with Status Management

**User Story:** As a team member, I want to track assignment status from creation through approval, so that I understand where each deliverable stands in the review process.

#### Acceptance Criteria

1. THE Application SHALL support five Assignment_Status values (Not Started, In Progress, Submitted, Approved, Changes Requested)
2. THE Team_Portal SHALL allow teams to update Assignment_Status to "In Progress" or "Submitted" and persist to Firestore
3. THE PM_Portal SHALL allow Program Managers to update Assignment_Status to "Approved" or "Changes Requested" and persist to Firestore
4. THE Application SHALL display Assignment_Status with visual indicators (colors or icons) in both portals
5. FOR ALL assignment status changes, THE Application SHALL record the timestamp in Firestore

### Requirement 16: Gate Decisions at Key Milestones

**User Story:** As a Program Manager, I want to make gate decisions at key milestones (M2, M4, M6), so that I can formally evaluate team progress.

#### Acceptance Criteria

1. THE PM_Portal SHALL allow Program Managers to record Gate_Decision values (Approved, Conditional, Not Approved) at M2, M4, and M6
2. THE PM_Portal SHALL persist Gate_Decision values to Firestore
3. THE Team_Portal SHALL display Gate_Decision values on the Home tab and Work tab
4. THE PM_Portal SHALL allow Program Managers to add notes to Gate_Decision records and persist them to Firestore
5. FOR ALL Gate_Decision entries, THE PM_Portal SHALL record the decision timestamp in Firestore

### Requirement 17: Progress Tracking and Visualization

**User Story:** As a team member, I want to see visual progress indicators, so that I can understand how far along we are in the program.

#### Acceptance Criteria

1. THE Team_Portal SHALL calculate overall progress percentage based on completed gates and assignments
2. THE Team_Portal SHALL display progress percentage on the Home tab
3. THE Team_Portal SHALL display a visual progress indicator (progress bar or similar) on the Home tab
4. THE Team_Portal SHALL display gate-specific progress on the Work tab
5. WHEN progress data changes in Firestore, THE Team_Portal SHALL update progress visualizations in real-time

### Requirement 18: Call Target Tracking

**User Story:** As a team member, I want to track customer discovery calls against cumulative targets (15, 30, 50, 70), so that I can monitor our customer validation progress.

#### Acceptance Criteria

1. THE Application SHALL define Call_Target milestones at 15, 30, 50, and 70 cumulative calls
2. THE Team_Portal SHALL display current call count and next Call_Target on the Calls tab
3. THE Team_Portal SHALL display progress toward the next Call_Target with a visual indicator
4. THE Application SHALL calculate call count based on data from the configured Airtable integration
5. FOR ALL teams, THE Application SHALL track call count in Firestore

### Requirement 19: Firebase/Firestore Data Persistence

**User Story:** As a user, I want all application data stored in Firestore, so that data persists across sessions and is accessible to all users.

#### Acceptance Criteria

1. THE Application SHALL store all team data in a Firestore "teams" collection
2. THE Application SHALL store all settings in a Firestore "settings" collection
3. THE Application SHALL store all meetings in a Firestore "schedule" collection
4. THE Application SHALL store all cohorts in a Firestore "cohorts" collection
5. THE Application SHALL initialize Firestore connection on application load
6. WHEN Firestore connection fails, THE Application SHALL display an error message to the user
7. FOR ALL data operations, THE Application SHALL use Firestore transactions to ensure data consistency

### Requirement 20: Real-Time Data Synchronization

**User Story:** As a user, I want to see data updates in real-time, so that I always have the most current information without refreshing the page.

#### Acceptance Criteria

1. THE Application SHALL use Firestore real-time listeners for all data collections
2. WHEN data changes in Firestore, THE Application SHALL update the user interface within 2 seconds
3. THE Application SHALL maintain real-time listeners throughout the user session
4. WHEN a user loses network connectivity, THE Application SHALL display a connection status indicator
5. WHEN network connectivity is restored, THE Application SHALL automatically resume real-time synchronization

### Requirement 21: Single-Page React Application Architecture

**User Story:** As a developer, I want the application built as a single-page React application, so that it provides a smooth user experience without page reloads.

#### Acceptance Criteria

1. THE Application SHALL be implemented as a single-page React application
2. THE Application SHALL use React component architecture for all UI elements
3. THE Application SHALL use React hooks for state management
4. THE Application SHALL implement client-side routing for navigation between tabs and portals
5. THE Application SHALL load all dependencies from CDN links without requiring a build process

### Requirement 22: Tailwind CSS Styling

**User Story:** As a developer, I want to use Tailwind CSS for styling, so that the application has a consistent, modern design system.

#### Acceptance Criteria

1. THE Application SHALL use Tailwind CSS for all component styling
2. THE Application SHALL load Tailwind CSS from a CDN link
3. THE Application SHALL use Tailwind utility classes for responsive design
4. THE Application SHALL maintain consistent color scheme and spacing throughout the interface
5. THE Application SHALL use Tailwind components for common UI patterns (buttons, forms, cards)

### Requirement 23: GitHub Pages Deployment

**User Story:** As a developer, I want to deploy the application to GitHub Pages, so that it is publicly accessible without server infrastructure.

#### Acceptance Criteria

1. THE Application SHALL be deployable to GitHub Pages as a static site
2. THE Application SHALL load all dependencies from CDN links to avoid build requirements
3. THE Application SHALL include a deployment script for GitHub Pages
4. THE Application SHALL function correctly when served from a GitHub Pages URL
5. THE Application SHALL include configuration for custom domain support if needed

### Requirement 24: No Build Process Requirement

**User Story:** As a developer, I want to run the application without a build process, so that development and deployment are simplified.

#### Acceptance Criteria

1. THE Application SHALL load React from a CDN link
2. THE Application SHALL load Firebase SDK from a CDN link
3. THE Application SHALL load Tailwind CSS from a CDN link
4. THE Application SHALL use JSX transformation in the browser or plain JavaScript
5. THE Application SHALL be runnable by opening the HTML file directly in a browser

### Requirement 25: Data Model for Teams Collection

**User Story:** As a developer, I want a well-defined Teams collection structure in Firestore, so that team data is organized and queryable.

#### Acceptance Criteria

1. THE Application SHALL store each team as a document in the "teams" collection
2. THE Teams document SHALL include fields for team name, cohort ID, gates, assignments, decisions, and notes
3. THE Teams document SHALL include nested structures for gate progress and assignment status
4. THE Teams document SHALL include timestamps for creation and last update
5. FOR ALL teams, THE Application SHALL enforce consistent document structure when writing to Firestore

### Requirement 26: Data Model for Settings Collection

**User Story:** As a developer, I want a well-defined Settings collection structure in Firestore, so that program configuration is centralized and accessible.

#### Acceptance Criteria

1. THE Application SHALL store program settings as a document in the "settings" collection
2. THE Settings document SHALL include fields for program name, Canvas URL, CRM URL, Airtable settings, and PM PIN
3. THE Settings document SHALL use a fixed document ID for easy retrieval
4. THE Settings document SHALL include a timestamp for last update
5. THE Application SHALL initialize default settings when the Settings document does not exist

### Requirement 27: Data Model for Schedule Collection

**User Story:** As a developer, I want a well-defined Schedule collection structure in Firestore, so that meeting data is organized and queryable.

#### Acceptance Criteria

1. THE Application SHALL store each meeting as a document in the "schedule" collection
2. THE Schedule document SHALL include fields for meeting name (M0-M6), date, Zoom link, and assigned cohort IDs
3. THE Schedule document SHALL include a timestamp for creation and last update
4. THE Application SHALL support querying meetings by cohort ID
5. FOR ALL meetings, THE Application SHALL enforce consistent document structure when writing to Firestore

### Requirement 28: Data Model for Cohorts Collection

**User Story:** As a developer, I want a well-defined Cohorts collection structure in Firestore, so that cohort data is organized and teams can be grouped effectively.

#### Acceptance Criteria

1. THE Application SHALL store each cohort as a document in the "cohorts" collection
2. THE Cohorts document SHALL include fields for cohort name, start date, and team IDs
3. THE Cohorts document SHALL include a timestamp for creation and last update
4. THE Application SHALL support querying teams by cohort ID
5. FOR ALL cohorts, THE Application SHALL enforce consistent document structure when writing to Firestore

### Requirement 29: Parser and Pretty Printer for Firestore Data

**User Story:** As a developer, I want to parse and serialize Firestore data correctly, so that data integrity is maintained throughout the application lifecycle.

#### Acceptance Criteria

1. WHEN Firestore data is retrieved, THE Application SHALL parse it into JavaScript objects
2. WHEN JavaScript objects are written to Firestore, THE Application SHALL serialize them correctly
3. THE Application SHALL include a Pretty_Printer that formats Firestore data for debugging and logging
4. FOR ALL valid Firestore data objects, parsing then printing then parsing SHALL produce an equivalent object (round-trip property)
5. WHEN invalid data is encountered, THE Application SHALL log a descriptive error and handle gracefully

### Requirement 30: Error Handling for Firestore Operations

**User Story:** As a user, I want clear error messages when data operations fail, so that I understand what went wrong and can take corrective action.

#### Acceptance Criteria

1. WHEN a Firestore read operation fails, THE Application SHALL display an error message to the user
2. WHEN a Firestore write operation fails, THE Application SHALL display an error message and preserve user input
3. WHEN authentication fails, THE Application SHALL display a specific authentication error message
4. THE Application SHALL log all Firestore errors to the browser console for debugging
5. FOR ALL error conditions, THE Application SHALL allow the user to retry the failed operation

