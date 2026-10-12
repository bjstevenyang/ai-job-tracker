# AI Job Application Tracker

Version: 0.1
Status: Approved MVP Baseline

## 1. Product Overview

A personal web application for managing
job applications, interviews, and follow-ups.

## 2. Target Users

- Initially a single user.
- Multi-user support may be considered later.
- Multi-user functionality is out of MVP scope.

## 3. MVP Goals

- Track job applications.
- Manage interview schedules.
- Manage follow-up tasks.
- Support manual job entry.
- Support best-effort job URL extraction.

## 4. Core Entities

### Company
Stores company information.
A nonblank company name is required.

### Job Posting
Stores job descriptions and job details.
A job posting can exist without an application.
Each job posting belongs to exactly one company.
A nonblank job title and company name are required when saving.
Other job details are optional.

### Application
Represents a recruitment process associated with exactly one job posting,
not necessarily a formal job submission. It can be created when a recruiter
contacts the user without a formal submission.
Tracks the current status and append-only status history.
A valid initial status is required when creating an application.

### Interview
Stores interview type, date, time,
timezone, and notes.
Each interview belongs to exactly one application.
Scheduled date, time, and timezone are required; type and notes are optional.

### Follow-up
Tracks tasks, due dates, and completion status.
Each follow-up belongs to exactly one application.
A nonblank task description is required; the due date is optional.

## 5. MVP Features

- Manual job entry
- Job URL extraction
- Application tracking
- Interview scheduling
- Follow-up management
- Dashboard
- Search and filtering
- JSON export and restore

## 6. Out of Scope

- AI resume matching
- AI interview preparation
- Multi-user authentication
- Email synchronization
- Calendar synchronization
- Automatic job discovery
- Multiple genuine application attempts against the same job posting
- Editing or deleting application status history entries
- Backdating application status history
- Automatic duplicate matching or merging of companies and job postings

## 7. Business Rules

- A company can have multiple job postings.
- A job posting may exist without an application.
- One job posting has at most one application in MVP.
- An application can have multiple interviews.
- An application can have multiple follow-ups.
- Interviews and follow-ups cannot exist independently of an application.
- A job posting without an application represents a saved opportunity.
- Partial extraction results must be completed with the required company name
  and job title before saving a job posting.
- Interview times must preserve timezone information.
- URL extraction must not overwrite confirmed user data.

### Application Status and History

- The allowed statuses are Contacted, Applied, Interviewing, Offer, Accepted,
  Rejected, and Withdrawn.
- Contacted represents recruiter outreach; Applied represents a submitted
  application; Interviewing represents the interview stage; Offer represents
  a received offer; Accepted represents an accepted offer, not employment start.
- Rejected means the employer ended the process unsuccessfully. Withdrawn
  means the user ended their pursuit, including declining an offer.
- The user selects the initial status. Existing recruitment processes may
  be entered at any allowed status without inventing earlier history.
- Status changes are manual, and all transitions are allowed.
- Scheduling an interview does not automatically change application status.
- Creation records an initial history entry containing the initial status
  and timestamp, with no previous status.
- Every subsequent status change records the previous status, new status,
  and timestamp. Saving an unchanged status creates no history entry.
- Timestamps record when the tracker was updated, not necessarily when the
  real-world event occurred.
- History is append-only. Correct an incorrect status by making another
  status change; existing history entries remain unchanged.
- The current status and its corresponding history entry must update together:
  both succeed or neither does.
- Reopening a rejected or withdrawn process continues the same recruitment
  process; it does not represent a new application attempt.

### Duplicate and Repeat-Application Behavior

- If a posting already has an application, an attempt to create another
  directs the user to the existing application without changing it.
- Repeating an application creation action must not create a second
  application or additional initial history entries.
- Duplicate company names and job posting records are allowed. Names, titles,
  and URLs alone do not establish record identity; records are not automatically
  matched or merged.
- A reposted vacancy may be entered as a separate posting when the user
  considers it a separate opportunity.
- An existing application must not be reset or its history erased to represent
  a new application attempt against the same posting.

## 8. Security Requirements

- URL extraction must protect against SSRF.
- Internal network access must be restricted.
- Redirects must be validated.
- Response sizes and timeouts must be limited.

## 9. Acceptance Criteria

The user must be able to:

1. Create and edit a job posting.
2. Import job details from a URL when extraction succeeds.
3. Enter details manually when extraction fails.
4. Create and update an application.
5. View application status history.
6. Schedule interviews with timezone information.
7. Create and complete follow-up tasks.
8. View upcoming interviews and overdue tasks.
9. Search and filter applications.
10. Export and restore application data.

For the approved entity, status, and duplicate rules:

- Saving a company without a nonblank name, or a job posting without a
  nonblank title and an associated company with a nonblank name, is rejected.
- An application requires an existing job posting and a valid selected initial
  status. A Contacted application can be created without formal submission.
- An interview requires an existing application, scheduled date, time, and
  timezone. A follow-up requires an existing application and a nonblank task
  description; a follow-up can be saved without a due date.
- Creating an application at any allowed status records exactly one initial
  history entry and does not create assumed earlier history.
- Changing between any two distinct allowed statuses updates the current
  status and appends exactly one timestamped history entry. If the operation
  fails, neither the current status nor history changes.
- Saving without a status change leaves history unchanged. Correcting a
  status appends a new entry and preserves existing entries.
- Scheduling an interview leaves application status unchanged.
- Attempting to create an application for a posting that already has one,
  including repeating a creation action, preserves the existing application
  and history and directs the user to that application.
- Separate postings with identical company names, titles, or URLs can be
  saved without automatic merging.
