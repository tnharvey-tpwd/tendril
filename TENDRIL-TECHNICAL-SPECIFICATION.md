# Tendril Technical Specification and Product Roadmap

**Document status:** Living specification  
**Current implementation baseline:** Tendril Alpha v0.3  
**Purpose:** Preserve the product model, current technical design, implementation decisions, known limitations, and future direction so development can resume with minimal reorientation.

---

## 1. Product summary

Tendril is a browser-based personal stewardship system. It is designed to help a user capture incoming concerns, connect them to meaningful contexts, identify the next useful action, and regularly review what deserves attention.

Tendril differs from a conventional task manager in one central respect:

> Tasks are not the center of the system. Threads, commitments, relationships, and stewardship are the center. Actions are the concrete expressions of that stewardship.

The product should answer two recurring questions:

1. **What deserves my attention today?**
2. **What may need renewed attention this week?**

The initial deployment is a static Progressive Web App hosted on GitHub Pages. Application data remains in the browser using IndexedDB. The design intentionally avoids requiring a local development runtime on a restricted work computer.

---

## 2. Product principles

### 2.1 Stewardship before productivity

Tendril should help users tend projects, relationships, responsibilities, ideas, and areas of life. Completion counts may be useful, but they are not the primary measure of value.

### 2.2 Review-driven operation

Capture and execution matter, but Reviews are the heartbeat of the system. Daily and Weekly Reviews bring neglected, aging, blocked, or meaningful concerns back into view.

### 2.3 Gentle but operational language

Relationship and stewardship prompts should avoid unnecessary guilt.

Preferred:

- Consider reconnecting
- Approaching rhythm
- Needs attention
- Waiting item ready for follow-up

Avoid where relationally inappropriate:

- Contact overdue
- Relationship failed
- Missed relationship task

Operational views may still use precise language such as overdue, blocked, or waiting when describing Actions.

### 2.4 Frictionless Capture

A user should be able to preserve an incoming concern without classifying it immediately. Capture, clarification, and execution are separate modes.

### 2.5 Context without mandatory bureaucracy

The hierarchy should provide meaning without forcing every record through every layer. An Action may connect directly to a Thread or Relationship without requiring an Area, Role, and Commitment.

### 2.6 Transparent recommendations

When Tendril surfaces an item, the system should eventually explain why it appeared. Rule-based recommendations should be understandable and dismissible.

### 2.7 Local-first and portable

The application should remain useful without a server connection. Users must be able to export and restore their data. Future synchronization should be added through an abstraction rather than replacing the local-first model.

---

## 3. Conceptual model

### 3.1 Areas

Areas are broad parts of life that require ongoing attention.

Examples:

- Career
- Relationships
- Home
- Health
- Learning
- Community

Areas are not normally completed. They provide a top-level portfolio view.

**Implementation status:** Planned.

### 3.2 Roles

Roles describe who the user is within a context.

Examples:

- Training and Development Specialist
- Team member
- Friend
- Mentor
- Learner
- Homeowner

A Role may relate to more than one Area.

**Implementation status:** Planned.

### 3.3 Commitments

Commitments are ongoing responsibilities arising from Roles, Areas, values, mission, or priorities.

Examples:

- Maintain effective professional relationships
- Support stakeholder success
- Continue professional development
- Encourage and support others

Commitments are not time-bound in the same way as Actions or Projects. They are usually paused or archived rather than completed.

The term may require onboarding because users may otherwise interpret Commitment inconsistently. A future alignment layer may connect values, mission, and vision to Roles and Commitments.

**Implementation status:** Planned.

### 3.4 Threads

A Thread is anything requiring stewardship or engagement over time.

Current Thread types:

- Project
- Relationship
- Responsibility
- Idea
- Interest
- General

A Thread may include:

- Purpose
- Desired outcome
- Status
- Parent Thread
- Related person
- Next review date
- Resume note
- Updates
- Related Actions

Thread nesting should remain shallow. The current target is two Thread levels:

```text
Thread
  └── Subthread
```

Areas, Roles, and Commitments supply higher-level context, so deeper Thread nesting should not be necessary.

**Implementation status:** Implemented in Alpha, with a parent Thread field and related Action counts.

### 3.5 Relationships

A Person is a reusable entity. A Relationship is the user’s stewardship context with that Person.

Relationship data may include:

- Display name
- Organization
- Relationship type
- Season
- Rhythm
- Notes
- Meaningful interactions
- Encouragement entries
- Follow-up context
- Linked Threads
- Linked Actions

Current relationship seasons:

- Active
- Close
- Collaborative
- Occasional
- Dormant
- Renewing

Current rhythms:

- Weekly
- Biweekly
- Monthly
- Quarterly
- Event-driven

Rhythms are guides rather than obligations. The current rhythm calculation uses the most recent Interaction or Encouragement entry. Ordinary notes do not reset a relationship cadence.

**Implementation status:** Implemented in Alpha, including rhythm-based sorting and basic cadence indicators.

### 3.6 Actions

Actions are concrete work that the user can perform and complete.

Current Action statuses:

- Planned
- Active
- Waiting
- Blocked
- Completed
- Cancelled

Current Action properties include:

- Title
- Notes
- Status
- Linked Thread
- Linked Person
- Scheduled date
- Due date
- Effort
- Energy
- Focus state
- Waiting-on description
- Poke date
- Completion date
- Source Capture

If work must be performed and later marked complete, it should be represented as an Action.

**Implementation status:** Implemented in Alpha.

### 3.7 Captures

A Capture is unprocessed input. It may originate from an email, meeting, conversation, webpage, idea, or other source.

A Capture can produce one to three Processing Results in the current design:

- Action
- Waiting item
- New Thread
- Thread update
- Relationship entry
- Reference
- Incubated item

The immediate system behavior is record creation or modification. A Processing Result is not the same as an external automation.

**Implementation status:** Implemented in Alpha.

### 3.8 Waiting and poke behavior

Waiting is an Action status, not a separate permanent object type.

A Waiting Action may record:

- Waiting on whom or what
- Waiting since
- Expected response date
- Poke date

When the poke date arrives, Tendril should surface the Waiting Action and offer to create a follow-up Action. The original Waiting Action remains open until the dependency is resolved or no longer needed.

**Implementation status:** Basic version implemented.

### 3.9 Relationship entries and encouragement

Relationship Entry subtypes include:

- Interaction
- Note
- Encouragement
- Follow-up context
- Encouragement opportunity

Encouragement is intentionally broader than recognition. It may include:

- Recognition
- Celebration
- Support
- Mentoring
- Advocacy
- Thoughtfulness
- Connection

Encouragement should be intentionally surfaced without turning relationships into compliance tasks.

**Implementation status:** Basic logging and Review-based encouragement opportunities implemented.

### 3.10 Reviews

Reviews are guided sessions that surface relevant records and save user decisions.

Current Review types:

- Daily Review
- Weekly Stewardship Review

Future Review types may include:

- Monthly Alignment Review
- Quarterly Roles and Direction Review
- Custom Reviews

**Implementation status:** Daily and Weekly Reviews implemented in Alpha.

---

## 4. Intended hierarchy

The conceptual hierarchy is:

```text
Values / Mission / Vision        Future alignment layer
  └── Area
      └── Role
          └── Commitment
              └── Thread
                  └── Subthread
                      └── Action
```

This is not a mandatory database path. Records may connect across levels where useful.

Example:

```text
Area: Career
  Role: Training and Development Specialist
    Commitment: Support stakeholder success
      Thread: Training intake redesign
        Action: Review stakeholder feedback
```

Relationships may participate in multiple Threads and Commitments. Therefore, Person should remain a separate entity rather than being stored only as a Thread label.

---

## 5. Current user interface

### 5.1 Primary navigation

Current views:

- Today
- Capture
- Threads
- Actions
- Relationships
- Reviews
- Operations
- Backup and data

A persistent **+ Capture** control is available in the application header.

### 5.2 Today

Purpose: Answer what deserves attention today.

Current content:

- Focus Action count
- Unprocessed Capture count
- Waiting item count
- Focus Actions
- Active Threads

Planned enhancements:

- Calendar context
- Notification summary
- Quick wins
- Recent Review snapshot
- Suggested next Action

### 5.3 Capture

Purpose: Display unprocessed Captures and guide clarification.

Current behavior:

- Quick Capture requires only text
- Optional source metadata can be recorded
- Clarification uses an adaptive form
- One primary Processing Result is required
- Up to two additional Processing Results may be added
- Original source provenance is preserved

Planned enhancements:

- Templates
- Suggested classifications
- Duplicate detection
- Better email source handling
- Processing shortcuts
- Persistent Review context when opening Capture from a Review

### 5.4 Threads

Purpose: Browse and resume projects, responsibilities, ideas, interests, and relationship-related work.

Current behavior:

- Create and edit Threads
- Parent Thread linkage
- Purpose and desired outcome
- Resume note
- Related Person
- Next review date
- Open Action count
- Link to filtered Actions

Planned enhancements:

- Area, Role, and Commitment linkage
- Thread status and review filters
- Milestones, risks, stakeholders, and project metadata
- Thread timeline
- Network visualization
- Gantt or dependency view where appropriate

### 5.5 Actions

Purpose: Manage completable work.

Current behavior:

- Create, edit, complete, and delete Actions
- Link to Thread and Person
- Schedule and due dates
- Waiting and poke data
- Focus selection
- Filter by state
- Sort by due date, scheduled date, Thread, Relationship, status, or creation date
- URL parameters for selected filters

Current default behavior hides completed and cancelled Actions.

Planned enhancements:

- Indexed database queries instead of full-store filtering
- Pagination or incremental rendering
- Bulk operations
- Recurrence
- Saved filters
- Tags or contexts
- Calendar placement
- Attachments and document links

### 5.6 Relationships

Purpose: Support intentional stewardship of relationships.

Current behavior:

- Create, edit, and delete People/Relationships
- Assign type, season, and rhythm
- Display recent entries
- Record encouragement
- Sort by cadence urgency, recent contact, or name
- Link to filtered open Actions

Planned enhancements:

- Configurable rhythm tolerance
- Last meaningful interaction editor
- Organization and group relationships
- Relationship-focused Reviews
- Encouragement categories
- Contact methods and preferences
- Calendar and message integrations

### 5.7 Operations

Purpose: Provide a task-oriented safety net and surface what may be slipping.

Current behavior:

- Displays Waiting and Blocked Actions
- Displays overdue and poke-ready Actions
- Displays Threads without an open Action
- Creates follow-up Actions

Planned enhancements:

- Explain why each record is surfaced
- Aging thresholds
- Dismiss, defer, or tune a rule
- Missing-next-Action detection
- Repeated deferral detection
- Stale Capture detection

### 5.8 Reviews

#### Daily Review

Current stages:

1. Capture check
2. Operational risks
3. Select Focus Actions
4. Ready State snapshot

Current capabilities:

- Shows Capture count without forcing Capture processing
- Displays operational risks
- Edits and creates Actions during Review
- Selects Focus Actions
- Saves a Review record
- Displays a final snapshot

#### Weekly Stewardship Review

Current stages:

1. Clear the ground
2. Review Threads
3. Review Relationships
4. Encouragement reflection
5. Prepare the week

Current capabilities:

- Displays outstanding Actions
- Links to Capture
- Edits and creates Actions
- Selects Focus Threads
- Selects Focus Relationships
- Shows relationship cadence status
- Records an encouragement opportunity
- Optionally creates an encouragement Action
- Saves a Review record
- Displays a final weekly snapshot

Planned enhancements:

- Restore a Review after navigating to another view or reloading
- Configurable Review templates and stages
- Monthly and Quarterly Reviews
- Linked Review decisions and outcomes
- Better history, comparisons, and reflection prompts

### 5.9 Backup and data

Current behavior:

- Export all stores to JSON
- Replace current data from a JSON backup
- Merge a JSON backup by record ID
- Clear all data with confirmation

Replace import clears all stores before loading the file. A valid empty backup therefore clears the database.

Planned enhancements:

- Automatic pre-import backup
- Schema validation and migration preview
- Conflict resolution during merge
- Storage usage display
- Persistent-storage status
- Scheduled backup reminders
- Optional encrypted backup

---

## 6. Current technical architecture

### 6.1 Deployment model

Current deployment:

```text
GitHub Pages
  └── Static HTML, CSS, JavaScript, manifest, icons, service worker
        └── IndexedDB in the local browser profile
```

No server, build process, or runtime is required on the end-user computer.

### 6.2 Current implementation stack

- Static HTML
- CSS
- Browser-native JavaScript modules
- IndexedDB
- Service worker
- Web App Manifest
- GitHub Pages

Earlier architectural planning proposed React, TypeScript, Vite, and Dexie. Those remain sensible options for a production rewrite, but the Alpha intentionally uses browser-native code to simplify deployment and validate workflows.

### 6.3 Origin and storage boundaries

IndexedDB data is scoped to the site origin. For GitHub Pages, applications under the same scheme and hostname may share an origin-level storage quota even when hosted in different repository paths.

Data is also specific to:

- Browser
- Browser profile
- Device
- Site origin

Work and home installations do not currently synchronize.

### 6.4 Current object stores

The Alpha database currently uses these stores:

```text
captures
threads
actions
people
relationshipEntries
threadUpdates
references
reviews
```

The current database uses a stable database name so Alpha upgrades can retain prior browser data.

### 6.5 Current storage-access pattern

The Alpha commonly uses full-store retrieval:

```javascript
objectStore.getAll()
```

Filtering and sorting then occur in JavaScript. This is reasonable for an Alpha and modest data volume but should not remain the primary access pattern at scale.

---

## 7. Target data model

The production-oriented schema should preserve current entities while adding explicit relationships, lifecycle data, indexes, and schema versioning.

### 7.1 Common record fields

Most records should include:

```typescript
interface BaseRecord {
  id: string;
  createdAt: string;
  updatedAt: string;
  archivedAt?: string;
  deletedAt?: string;
  schemaVersion: number;
}
```

Soft deletion may be preferable for mature versions, especially when records are linked from Reviews, Captures, or historical entries.

### 7.2 Area

```typescript
interface Area extends BaseRecord {
  name: string;
  description?: string;
  color?: string;
  icon?: string;
  status: "active" | "inactive" | "archived";
}
```

### 7.3 Role

```typescript
interface Role extends BaseRecord {
  name: string;
  description?: string;
  areaIds: string[];
  status: "active" | "inactive" | "archived";
}
```

### 7.4 Commitment

```typescript
interface Commitment extends BaseRecord {
  title: string;
  description?: string;
  areaIds: string[];
  roleIds: string[];
  status: "active" | "paused" | "archived";
  reviewRhythmId?: string;
  nextReviewAt?: string;
}
```

### 7.5 Thread

```typescript
interface Thread extends BaseRecord {
  title: string;
  type:
    | "project"
    | "relationship"
    | "responsibility"
    | "idea"
    | "interest"
    | "general";
  status:
    | "active"
    | "waiting"
    | "paused"
    | "incubating"
    | "completed"
    | "archived";
  parentThreadId?: string;
  areaIds: string[];
  roleIds: string[];
  commitmentIds: string[];
  personIds: string[];
  purpose?: string;
  desiredOutcome?: string;
  nextReviewAt?: string;
  lastMeaningfulActivityAt?: string;
  resumeNote?: {
    whereILeftOff?: string;
    nextStep?: string;
    openQuestions?: string;
    blockers?: string;
  };
}
```

### 7.6 Action

```typescript
interface Action extends BaseRecord {
  title: string;
  description?: string;
  status:
    | "inbox"
    | "planned"
    | "active"
    | "waiting"
    | "blocked"
    | "completed"
    | "cancelled";
  actionType?:
    | "general"
    | "email"
    | "call"
    | "review"
    | "research"
    | "meeting"
    | "encouragement"
    | "administrative";
  threadId?: string;
  personIds: string[];
  areaIds: string[];
  roleIds: string[];
  commitmentIds: string[];
  scheduledFor?: string;
  dueAt?: string;
  reviewAt?: string;
  completedAt?: string;
  effort?: "xs" | "s" | "m" | "l" | "xl";
  energy?: "low" | "medium" | "high";
  priority?: "low" | "normal" | "high" | "critical";
  focus?: boolean;
  waitingDetails?: {
    waitingOn?: string;
    waitingSince?: string;
    expectedBy?: string;
    pokeAt?: string;
  };
  recurrenceRuleId?: string;
  sourceCaptureId?: string;
}
```

Current Alpha Actions use a single Person ID. The target schema should use `personIds` when an Action may involve multiple people. A compatibility migration will be required.

### 7.7 Person and Relationship Profile

```typescript
interface Person extends BaseRecord {
  displayName: string;
  organization?: string;
  title?: string;
  emailAddresses?: string[];
  phoneNumbers?: string[];
  notes?: string;
}

interface RelationshipProfile extends BaseRecord {
  personId: string;
  threadId?: string;
  relationshipTypes: string[];
  season:
    | "active"
    | "close"
    | "collaborative"
    | "occasional"
    | "dormant"
    | "renewing";
  rhythmId?: string;
  rhythmEnabled: boolean;
  interactionPreference?:
    | "frequent"
    | "periodic"
    | "occasional"
    | "event-driven";
  pausedUntil?: string;
  notes?: string;
}
```

The Alpha stores Relationship fields directly on Person. A production migration should separate identity/contact information from the user-specific Relationship Profile.

### 7.8 Relationship Entry

```typescript
interface RelationshipEntry extends BaseRecord {
  personId: string;
  threadId?: string;
  subtype:
    | "interaction"
    | "note"
    | "encouragement"
    | "encouragementOpportunity"
    | "followUpContext";
  interactionType?:
    | "email"
    | "meeting"
    | "call"
    | "message"
    | "inPerson"
    | "other";
  meaningful?: boolean;
  encouragementType?:
    | "recognition"
    | "celebration"
    | "support"
    | "mentoring"
    | "advocacy"
    | "thoughtfulness"
    | "connection";
  occurredAt: string;
  notes?: string;
  sourceCaptureId?: string;
}
```

### 7.9 Rhythm Definition

```typescript
interface RhythmDefinition extends BaseRecord {
  ownerType: "relationship" | "commitment" | "thread" | "review";
  ownerId: string;
  type:
    | "days"
    | "weekly"
    | "biweekly"
    | "monthly"
    | "quarterly"
    | "custom"
    | "event-driven";
  interval?: number;
  toleranceDays?: number;
  pausedUntil?: string;
}
```

Calculated relationship states may include:

- Recently tended
- Within rhythm
- Approaching rhythm
- Consider reconnecting
- Intentionally paused
- Event-driven

### 7.10 Capture

```typescript
interface Capture extends BaseRecord {
  text: string;
  notes?: string;
  status:
    | "unprocessed"
    | "processing"
    | "processed"
    | "incubating"
    | "discarded";
  source?: {
    type:
      | "email"
      | "meeting"
      | "conversation"
      | "web"
      | "idea"
      | "other";
    title?: string;
    person?: string;
    receivedAt?: string;
    url?: string;
    reference?: string;
  };
  reviewAt?: string;
  processedAt?: string;
}
```

### 7.11 Capture Processing Result

```typescript
interface CaptureProcessingResult extends BaseRecord {
  captureId: string;
  resultType:
    | "action"
    | "thread"
    | "threadUpdate"
    | "relationshipEntry"
    | "reference";
  createdRecordId: string;
}
```

The current Alpha preserves `sourceCaptureId` on created records but does not fully normalize Processing Results into their own visible store. A future version should add this store for provenance and reliable review of Capture history.

### 7.12 Review Template and Session

```typescript
interface ReviewTemplate extends BaseRecord {
  name: string;
  type: "daily" | "weekly" | "monthly" | "quarterly" | "custom";
  stages: ReviewStageDefinition[];
  active: boolean;
}

interface ReviewSession extends BaseRecord {
  templateId?: string;
  type: "daily" | "weekly" | "monthly" | "quarterly" | "custom";
  startedAt: string;
  completedAt?: string;
  currentStageId?: string;
  status: "inProgress" | "completed" | "abandoned";
  focusActionIds: string[];
  focusThreadIds: string[];
  focusPersonIds: string[];
  createdActionIds: string[];
  encouragementNote?: string;
  notes?: ReviewStageResult[];
}
```

The current Alpha stores completed Review records. The target schema should save in-progress Review state in IndexedDB so navigation or reload does not lose progress.

### 7.13 Recurrence Rule

```typescript
interface RecurrenceRule extends BaseRecord {
  ownerActionId: string;
  frequency: "daily" | "weekly" | "monthly" | "yearly" | "custom";
  interval: number;
  daysOfWeek?: number[];
  dayOfMonth?: number;
  startAt?: string;
  endAt?: string;
  nextOccurrenceAt?: string;
  exceptionDates?: string[];
  generationMode: "onCompletion" | "aheadOfTime";
}
```

Recurring templates should generate Action instances. Editing an occurrence should not silently mutate the recurrence template.

### 7.14 External Resource Link

```typescript
interface ExternalResourceLink extends BaseRecord {
  ownerType: "action" | "thread" | "person" | "relationshipEntry";
  ownerId: string;
  provider:
    | "web"
    | "outlook"
    | "onedrive"
    | "sharepoint"
    | "googleDrive"
    | "teams"
    | "localReference"
    | "other";
  label: string;
  url?: string;
  searchableReference?: string;
  externalId?: string;
}
```

This permits document and message associations without requiring Tendril to store the files themselves.

---

## 8. Database and query changes required for scale

### 8.1 Problem with current calls

The current Alpha often retrieves all records from a store and performs filtering, sorting, and aggregation in memory. This is acceptable for early use but creates three scaling problems:

1. Unnecessary records are transferred from IndexedDB into memory.
2. JavaScript repeatedly sorts full datasets.
3. The interface may render thousands of DOM nodes.

Interface performance is likely to degrade before IndexedDB reaches its storage quota.

### 8.2 Required indexes

A schema upgrade should add indexes such as:

#### Actions

```text
status
focus
scheduledFor
dueAt
threadId
personId
completedAt
createdAt
pokeAt
[status, dueAt]
[status, scheduledFor]
[threadId, status]
[personId, status]
```

If Actions support multiple People, a multi-entry `personIds` index should be used where browser support and the abstraction permit.

#### Captures

```text
status
createdAt
reviewAt
[status, createdAt]
```

#### Threads

```text
status
type
parentThreadId
nextReviewAt
lastMeaningfulActivityAt
[status, nextReviewAt]
```

Multi-entry indexes may be appropriate for:

```text
areaIds
roleIds
commitmentIds
personIds
```

#### People and Relationships

```text
displayName
organization
season
rhythmId
```

#### Relationship Entries

```text
personId
subtype
occurredAt
[personId, occurredAt]
[personId, subtype, occurredAt]
```

#### Reviews

```text
type
status
startedAt
completedAt
[type, completedAt]
```

#### External links

```text
ownerType
ownerId
provider
[ownerType, ownerId]
```

### 8.3 Query-service abstraction

UI components should not call IndexedDB directly. Introduce repositories or query services:

```typescript
interface ActionRepository {
  getById(id: string): Promise<Action | undefined>;
  list(query: ActionQuery): Promise<PagedResult<Action>>;
  count(query: ActionQuery): Promise<number>;
  save(action: Action): Promise<void>;
  archive(id: string): Promise<void>;
}
```

Example query:

```typescript
interface ActionQuery {
  statuses?: Action["status"][];
  personId?: string;
  threadId?: string;
  focus?: boolean;
  dueBefore?: string;
  scheduledOn?: string;
  sort?: "due" | "scheduled" | "thread" | "relationship" | "status" | "created";
  direction?: "asc" | "desc";
  cursor?: string;
  limit?: number;
}
```

This allows IndexedDB, a future cloud backend, or an encrypted file provider to satisfy the same application-level contract.

### 8.4 Pagination and progressive rendering

List screens should not render every historical record.

Recommended initial limits:

- Actions: 50 per page or fetch
- Relationship Entries: 20 per Relationship, with Load More
- Recent Reviews: 10, with history pagination
- Captures: 50 unprocessed at a time
- Thread updates: 20 per Thread

Infinite scrolling is possible, but explicit **Load more** controls are easier to understand and debug.

### 8.5 Archiving

Archiving should reduce default query volume without destroying history.

Suggested behavior:

- Open Actions remain in active queries.
- Recently completed Actions remain visible for a configurable interval.
- Older completed Actions move to an archived state or archive view.
- Archived records remain searchable and exportable.

A separate physical archive store is optional. A status/index-based archive may be sufficient until data volume becomes large.

### 8.6 Aggregates and derived data

Counts displayed on Today, Reviews, and Relationship cards should eventually use targeted count queries rather than loading entire stores.

Examples:

```text
Count unprocessed Captures
Count open Actions by Person
Count open Actions by Thread
Count Waiting Actions whose poke date has arrived
Get latest meaningful Relationship Entry
```

Frequently used derived values may be cached, but the source records should remain authoritative.

### 8.7 Schema migration

Every database change should increment the IndexedDB version and include deterministic migration logic.

Expected migrations include:

- Move Relationship-specific fields from Person into RelationshipProfile
- Convert `action.personId` into `action.personIds`
- Convert flat `resumeNote` text into a structured resume-note object
- Add schema versions to records
- Create CaptureProcessingResult records from existing `sourceCaptureId` links where possible
- Normalize Waiting fields into `waitingDetails`

Before a destructive migration:

1. Validate existing stores.
2. Create or prompt for a backup.
3. Run the transaction.
4. Report migration success or failure.
5. Never partially advance the schema version.

### 8.8 Storage monitoring

Add browser storage estimates to Backup and data:

```javascript
const estimate = await navigator.storage.estimate();
```

Display:

- Approximate origin usage
- Approximate origin quota
- Percentage used
- Date of last backup

The estimate applies to origin-level browser storage and may include service-worker caches and other applications on the same origin.

### 8.9 Persistent storage

Offer an informational control using:

```javascript
await navigator.storage.persist();
```

Persistent storage reduces the risk of automatic eviction under storage pressure. It does not replace backups and does not prevent manual or administrative deletion.

### 8.10 Performance planning thresholds

Planning thresholds for the current text-heavy model:

- Under 1,000 records per frequently loaded store: expected to feel immediate
- 1,000 to 5,000: current approach likely remains acceptable
- 5,000 to 15,000: begin expecting visible hesitation in some views
- 15,000 to 50,000: indexes and pagination become important
- Above 50,000 in a frequently rendered store: full-store/full-list rendering should be considered unsupported

These are engineering planning ranges, not hard browser limits.

---

## 9. Attention and recommendation engine

The Attention Engine decides what to surface without changing source records unnecessarily.

Initial transparent rules:

```text
IF Action due date is before today
AND status is not Completed or Cancelled
THEN surface as overdue
```

```text
IF Action status is Waiting
AND poke date is today or earlier
THEN offer follow-up options
```

```text
IF active Thread has no open Action
THEN surface as No next Action
```

```text
IF Thread next-review date has arrived
THEN include in Weekly Review
```

```text
IF Relationship rhythm is approaching or surpassed
THEN include as a gentle Relationship suggestion
```

```text
IF Capture remains unprocessed beyond a threshold
THEN surface its count in Daily Review and Operations
```

Future rules may recognize:

- Repeatedly deferred Actions
- Actions that remain Active without progress
- Threads repeatedly selected but not advanced
- Overloaded daily Focus lists
- Excessive notifications
- Imbalanced attention across Areas or Commitments

Every recommendation should eventually support:

- Why am I seeing this?
- Act now
- Defer
- Dismiss once
- Adjust rule
- Pause this rhythm

---

## 10. Notifications roadmap

Notifications should reduce forgotten commitments without becoming a new source of cognitive load.

### 10.1 Notification types

Potential notification categories:

- Scheduled Action reminder
- Due-soon reminder
- Waiting-item poke
- Review reminder
- Relationship rhythm suggestion
- Recurring Action availability
- Backup reminder
- Calendar conflict or preparation reminder

### 10.2 Cognitive-load controls

Users should be able to configure:

- Allowed notification categories
- Quiet hours
- Work and personal notification windows
- Maximum notifications per day
- Digest versus individual notifications
- Minimum priority or urgency
- Snooze duration
- Whether relationship prompts may notify or only appear in Reviews
- Whether a dismissed suggestion returns

Default behavior should be conservative. Most stewardship prompts should appear in Reviews or a digest rather than as immediate push notifications.

### 10.3 Technical constraints

Browser notifications require permission. Background behavior varies by browser, installed-PWA status, operating system, and enterprise policy. Reliable notifications when the application is closed may eventually require a server-side push service or platform-specific scheduling.

The first notification implementation should therefore be:

1. In-app reminders
2. Review and Today surfacing
3. Optional browser notification while Tendril is open
4. Installed-PWA or push-service investigation later

### 10.4 Notification model

```typescript
interface NotificationPreference extends BaseRecord {
  category: string;
  enabled: boolean;
  delivery: "inApp" | "browser" | "digest";
  quietHours?: { start: string; end: string };
  maxPerDay?: number;
}

interface NotificationEvent extends BaseRecord {
  category: string;
  ownerType: string;
  ownerId: string;
  scheduledAt: string;
  deliveredAt?: string;
  dismissedAt?: string;
  snoozedUntil?: string;
}
```

---

## 11. Calendar roadmap

### 11.1 Calendar view

A future Calendar should combine:

- Scheduled Actions
- Due dates
- Poke dates
- Review dates
- Recurring occurrences
- Imported calendar events

Potential views:

- Day
- Week
- Month
- Agenda

Calendar placement should not convert every due date into an appointment. Scheduled work and hard deadlines must remain distinct.

### 11.2 ICS support

A low-complexity first integration can export selected Actions or Reviews as `.ics` events.

Capabilities:

- Export one Action to ICS
- Export a selected set of scheduled Actions
- Export a subscription-style calendar later
- Include a Tendril URL or record reference in the event description

### 11.3 Outlook and Google integration

Potential phases:

1. Create downloadable ICS files
2. Open a pre-addressed calendar or email action where supported
3. Read-only calendar import
4. Two-way synchronization

Two-way synchronization introduces identity, permissions, conflict resolution, recurring-event complexity, and tenant restrictions. It should not be an early dependency.

---

## 12. Visualization roadmap

### 12.1 Network graph

A network graph can visualize relationships among:

- Areas
- Roles
- Commitments
- Threads
- People
- Actions
- References

Potential interactions:

- Filter by entity type
- Expand a selected node
- Highlight orphaned Actions
- Show relationship density around a Thread
- Navigate from graph node to record

The graph should be an optional lens, not the primary editing interface.

### 12.2 Gantt and timeline

A Gantt-style view may be useful for project Threads that include:

- Milestones
- Start and target dates
- Dependencies
- Planned duration

Not every Thread should require Gantt metadata. Enable the view only for project-oriented Threads with sufficient scheduling information.

### 12.3 Other visualizations

Potential future views:

- Portfolio by Thread status
- Attention by Area
- Commitment coverage
- Relationship cadence heatmap
- Waiting-item aging
- Completed Actions over time
- Encouragement activity
- Review consistency

Visualizations should support reflection rather than incentivize meaningless activity counts.

---

## 13. Behavioral support and adaptive tools

### 13.1 Behavioral-assistance concept

A future Pattern Engine may recognize usage patterns and offer optional productivity tools.

Examples:

- Repeatedly deferred Actions may benefit from decomposition.
- High-effort Actions may benefit from a Pomodoro session.
- Actions frequently completed after rescheduling may need more realistic scheduling.
- Threads repeatedly selected for focus but never advanced may lack a clear next Action.
- Overly large Focus lists may indicate planning overload.

The system should offer assistance, not diagnose the user or make hidden judgments.

### 13.2 Guardrails

Behavioral recommendations should be:

- Transparent
- Explainable
- Optional
- Easy to dismiss
- Locally processed where practical
- Based on sufficient data
- Adjustable by the user
- Free from manipulative streaks or shame-based language

### 13.3 Pattern model

```typescript
interface ObservedPattern extends BaseRecord {
  patternType: string;
  recordIds: string[];
  evidence: Record<string, unknown>;
  confidence?: number;
  firstObservedAt: string;
  lastObservedAt: string;
  dismissedAt?: string;
}

interface ToolSuggestion extends BaseRecord {
  patternId?: string;
  toolType: "pomodoro" | "decompose" | "reschedule" | "review" | "reduceFocus";
  explanation: string;
  acceptedAt?: string;
  dismissedAt?: string;
}
```

---

## 14. Pomodoro tool

### 14.1 Purpose

The Pomodoro tool should convert selected Actions into a short, bounded work plan rather than operate as an isolated timer.

### 14.2 Proposed flow

1. Select one or more Actions.
2. Choose session length and break pattern.
3. Arrange the selected Actions into Pomodoro goals.
4. Start a focused timer.
5. Record progress or completion after each interval.
6. Return unfinished work to an Action with an updated resume note.

### 14.3 Proposed data model

```typescript
interface FocusSession extends BaseRecord {
  startedAt: string;
  completedAt?: string;
  workMinutes: number;
  breakMinutes: number;
  actionIds: string[];
  goals: FocusGoal[];
  notes?: string;
}

interface FocusGoal {
  id: string;
  actionId?: string;
  description: string;
  position: number;
  status: "planned" | "completed" | "carriedForward" | "cancelled";
}
```

### 14.4 Integration with Reviews

Daily Review may offer:

```text
Three Focus Actions selected.
Would you like to arrange them into a focus session?
```

The Pattern Engine may suggest Pomodoro only when evidence supports the suggestion. The user should also be able to launch it manually at any time.

---

## 15. Productivity-tool extensions

Tendril can use Action data to power focused tools without replacing the central data model.

Potential extensions:

- Eisenhower or urgency/importance lens
- Energy-based Action picker
- Time-box planner
- Meeting-preparation view
- End-of-day shutdown routine
- Waiting-item follow-up batch
- Email-response batch
- Encouragement session
- Weekly time-entry checklist
- Project re-entry assistant
- Action decomposition tool

Each tool should read and update the same Actions, Threads, and Relationships rather than create a disconnected subsystem.

---

## 16. Integrations roadmap

### 16.1 Integration principles

- Keep Tendril usable without integrations.
- Store external IDs and links rather than duplicating documents where possible.
- Separate read, create, update, and delete permissions.
- Make synchronization status visible.
- Avoid sending work information to unapproved services.
- Expect work-tenant restrictions.

### 16.2 Microsoft integrations

Potential Microsoft integrations:

- OneDrive and SharePoint document links
- Outlook calendar import/export
- Outlook email source links
- Microsoft Graph calendar and mail APIs
- Teams deep links or message composition

Possible low-complexity features:

- Store a pasted Outlook or OneDrive link
- Open a linked document or email
- Create an email using a `mailto:` link
- Create an ICS event for Outlook
- Generate a formatted Teams message for copying
- Investigate supported Teams deep links for message composition

A default-email-client action can use a link such as:

```text
mailto:person@example.com?subject=...&body=...
```

This opens the configured mail client but does not send automatically.

### 16.3 Google integrations

Potential Google integrations:

- Google Calendar import/export
- Google Drive links
- Calendar event creation
- Optional OAuth-based synchronization

### 16.4 Synchronization provider abstraction

```typescript
interface SyncProvider {
  connect(): Promise<void>;
  disconnect(): Promise<void>;
  pull(since?: string): Promise<SyncBatch>;
  push(batch: SyncBatch): Promise<SyncResult>;
  getStatus(): Promise<SyncStatus>;
}
```

Possible providers:

- Local IndexedDB only
- JSON file
- OneDrive file
- Google Drive file
- Azure or other hosted backend

### 16.5 Conflict handling

Future synchronization requires:

- Record version or revision
- Last-modified timestamp
- Device/client identifier
- Tombstones for deleted records
- Conflict policy
- Sync log

A simple last-write-wins model may lose meaningful updates. User-visible conflict resolution should be considered for notes, Actions, and Reviews.

---

## 17. Notifications, automations, and operations

### 17.1 Distinctions

- **Processing Result:** A record Tendril creates or updates while clarifying a Capture.
- **Action:** Work the user must perform.
- **Tendril Operation:** Internal system behavior, such as surfacing a poke-ready item.
- **Automation:** A configurable trigger and operation.
- **External Automation:** An operation involving another system.

### 17.2 Future automation model

```typescript
interface AutomationRule extends BaseRecord {
  name: string;
  enabled: boolean;
  trigger: AutomationTrigger;
  conditions: AutomationCondition[];
  operations: AutomationOperation[];
}
```

Potential examples:

```text
When a Waiting Action reaches its poke date,
create a suggested follow-up Action.
```

```text
When a recurring administrative Action is completed,
generate its next occurrence.
```

```text
When a Relationship approaches its rhythm,
include it in the next Weekly Review.
```

General-purpose automations require duplicate prevention, execution history, error handling, and permission boundaries. They should follow, not precede, stable core workflows.

---

## 18. Data safety, privacy, and resilience

### 18.1 Current risks

Browser-local storage may be lost through:

- Browser data clearing
- Profile reset
- Device replacement
- Enterprise cleanup policy
- Origin change
- Accidental Clear Data
- Failed migration

### 18.2 Required protections

Near-term:

- Export full JSON backup
- Replace or Merge import
- Clear-data confirmation
- Last-backup reminder
- Storage usage display
- Import validation
- Automatic pre-import backup

Longer-term:

- Optional encrypted backups
- Revision history
- Cloud synchronization
- Export of selected entity types
- CSV export for interoperability
- Migration rollback where practical

### 18.3 Attachments

Large attachments would materially change storage planning. Prefer links to external documents rather than storing files in IndexedDB. If attachments are added:

- Track total attachment size
- Set per-file and total limits
- Separate metadata from binary blobs
- Allow attachment cleanup
- Exclude or include attachments explicitly in backup options

---

## 19. PWA and application shell

Current PWA assets:

- `index.html`
- `app.js`
- `styles.css`
- `manifest.webmanifest`
- `sw.js`
- Favicon and application icons

Current goals:

- Installable desktop-style experience where browser policy allows
- Offline application shell
- Relative paths compatible with GitHub Pages repository subpaths
- Cache version changes on release

Future service-worker improvements:

- Explicit update notification
- Safer cache-first versus network-first strategy by resource type
- Offline fallback page
- Background synchronization where supported
- Version display and migration status

---

## 20. Recommended production architecture

When the Alpha workflow stabilizes, migrate toward:

```text
Tendril PWA
│
├── Presentation
│   ├── React
│   ├── TypeScript
│   ├── Router
│   └── Accessible component system
│
├── Domain
│   ├── Models
│   ├── Validation
│   ├── Attention rules
│   ├── Review engine
│   ├── Rhythm evaluator
│   ├── Recurrence engine
│   └── Behavioral suggestion engine
│
├── Application services
│   ├── Capture processor
│   ├── Review coordinator
│   ├── Notification coordinator
│   ├── Calendar service
│   ├── Import/export
│   └── Sync coordinator
│
├── Data access
│   ├── Repository interfaces
│   ├── IndexedDB provider
│   ├── Migration service
│   └── Future cloud providers
│
└── Platform services
    ├── PWA installation
    ├── Service worker
    ├── Browser notifications
    ├── Storage estimates
    └── File import/export
```

Suggested tools:

- React
- TypeScript
- Vite
- Dexie or another IndexedDB abstraction
- React Router
- A test runner such as Vitest
- Component and end-to-end testing where practical

The migration should preserve the storage-provider abstraction so Tendril can remain local-first.

---

## 21. Accessibility and interaction requirements

Future implementation should include:

- Keyboard-accessible navigation and dialogs
- Visible focus indicators
- Proper labels and landmarks
- Escape-to-close where safe
- Validation that does not trap Cancel actions
- Color-independent status indicators
- Sufficient contrast
- Screen-reader announcements for saves and errors
- Reduced-motion support
- Mobile and desktop responsiveness

Destructive operations must be clearly differentiated and confirmed.

---

## 22. Testing strategy

### 22.1 Unit tests

Priorities:

- Rhythm calculations
- Attention rules
- Recurrence generation
- Import validation
- Schema migrations
- Action filtering and sorting
- Review summary generation
- Notification throttling

### 22.2 Integration tests

Priorities:

- Capture producing multiple connected results
- Waiting Action producing a follow-up Action
- Relationship interaction updating cadence
- Thread Action count
- Review selections persisting
- Replace versus Merge import
- Delete behavior with linked records

### 22.3 End-to-end tests

Critical user journeys:

1. Quick Capture
2. Clarify into multiple Processing Results
3. Create and link Thread, Relationship, and Action
4. Run Daily Review
5. Run Weekly Review
6. Export and restore backup
7. Upgrade database schema without data loss

### 22.4 Performance tests

Seed test databases with:

- 1,000 Actions
- 10,000 Actions
- 50,000 Actions
- 10,000 Relationship Entries
- 5,000 Captures

Measure:

- Initial view load
- Filter and sort duration
- Review calculation duration
- DOM node count
- Memory use
- Export/import time

---

## 23. Roadmap

### Phase A: Trial readiness and Alpha stabilization

- Continue one-to-two-week usage trial
- Resolve workflow defects
- Improve filtering and sorting
- Improve delete behavior and linked-record warnings
- Preserve Review state across internal navigation
- Improve Review summaries and history
- Add Help link to onboarding documentation
- Add storage usage and backup reminder

### Phase B: Structural model

- Add Areas
- Add Roles
- Add Commitments
- Add explicit links among hierarchy levels
- Add alignment onboarding
- Improve Thread hierarchy controls
- Add indexed queries and pagination
- Add schema migrations

### Phase C: Time and recurrence

- Recurring Action templates and generated instances
- Calendar view
- ICS export
- Improved schedule versus due-date behavior
- In-app reminders
- Notification preferences

### Phase D: Relationship stewardship

- Configurable rhythm tolerance
- Improved meaningful-interaction model
- Relationship-focused Reviews
- Encouragement categories and opportunities
- Organization and group relationships
- Improved action/message creation

### Phase E: Visual and focused tools

- Network graph
- Gantt/timeline for qualified project Threads
- Pomodoro and focus sessions
- Energy and time-based Action picker
- Project re-entry assistant

### Phase F: Integration and synchronization

- External resource links
- Email composition links
- Teams message support investigation
- OneDrive and SharePoint links
- Calendar import/export
- Optional OneDrive or Google Drive data provider
- Microsoft Graph or Google API integration if justified

### Phase G: Adaptive assistance

- Usage-pattern analysis
- Explainable productivity suggestions
- Notification refinement based on user response
- Optional behavioral tools
- User-controlled automation rules

---

## 24. Open design questions

1. Should Commitment be renamed or explained through mandatory onboarding?
2. How should values, mission, and vision connect to Roles and Commitments?
3. Should Relationship remain a specialized Thread in the interface while using separate Person and RelationshipProfile records underneath?
4. How should deletion behave when historical Reviews or Captures refer to a record?
5. When should completed Actions archive automatically?
6. What rhythm tolerance defaults are appropriate for different Relationship types?
7. Should Focus Relationships be temporary Review selections or a durable state?
8. Which notification categories should be enabled by default?
9. Should calendar events be stored locally, referenced externally, or both?
10. Which visualizations genuinely improve decisions rather than adding novelty?
11. When is a production rewrite justified versus continued Alpha iteration?
12. What synchronization provider is acceptable under work-system restrictions?

---

## 25. Definition of success

Tendril succeeds when the user can:

1. Capture an incoming concern without interrupting current work.
2. Clarify the Capture into trusted, connected records.
3. See concrete work without losing the relationships and commitments that give it meaning.
4. Resume an interrupted Thread quickly.
5. Notice Waiting, Blocked, overdue, or disconnected work.
6. Tend Relationships according to appropriate rhythms.
7. Identify encouragement opportunities.
8. Leave Daily and Weekly Reviews with greater confidence and lower cognitive load.
9. Retain control over notifications, recommendations, integrations, and data.
10. Continue using the system as it grows without losing performance or portability.

---

## 26. Concise orientation for future development

```text
Tendril is a local-first, review-driven personal stewardship system.

Core model:
Areas → Roles → Commitments → Threads → Actions
People and Relationships connect across that hierarchy.
Captures are clarified into one or more connected records.
Waiting is an Action state with poke behavior.
Daily Review selects immediate Focus.
Weekly Review renews awareness of Threads, Relationships,
encouragement, and operational risk.

Current Alpha:
Static GitHub Pages PWA + IndexedDB + JSON backup.

Near-term technical priority:
Move from getAll() plus in-memory filtering to indexed repository
queries, pagination, schema migrations, persistent Review state,
and stronger backup/storage controls.

Long-term direction:
Calendar, notifications, recurrence, graphs, project timelines,
Pomodoro and behavioral tools, document links, optional integrations,
and carefully controlled synchronization.
```
