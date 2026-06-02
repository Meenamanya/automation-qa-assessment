# automation-qa-assessment
Automation &amp; QA Developer Assessment Submission
# Task 1 – Web App QA & Debug Report
## Overview
This task involved testing the Conduit RealWorld demo application and identifying functional, usability, and error-handling issues.
## Objective
The objective was to evaluate the application's primary user flows, identify defects, assess their impact, and provide root cause analysis for a critical issue.
## Testing Scope
The following user flows were tested:
- Homepage Loading
- Article Feed Loading
- Popular Tags Navigation
- User Registration (Sign Up)
- User Authentication (Sign In)
- API Error Handling
## Summary of Findings
A total of **5 issues** were identified:
| Severity | Count |
|-----------|---------|
| Critical | 4 |
| High | 1 |

### Critical Issues
- Homepage articles never load
- Popular tags never load
- Sign Up functionality fails completely
- Sign In functionality fails completely

### High Severity Issue
- No user-facing error messages during API failures
## Root Cause
The investigation revealed that the backend API service (`api.realworld.io`) was not returning the required CORS headers.
Because of this:
- Browser requests were blocked
- API calls failed before reaching the backend
- Users could not register or log in
- Application data never loaded
- No meaningful error feedback was displayed
## Root Cause Analysis Performed
Detailed root cause analysis was completed for:
**Bug #3 – Sign Up Button Completely Non-Functional**
The issue was traced to a failed CORS preflight request caused by missing `Access-Control-Allow-Origin` headers on the API server.
## Recommendations
### Backend Improvements
- Configure proper CORS headers
- Validate OPTIONS preflight requests
- Improve API availability monitoring
### Frontend Improvements
- Add global HTTP error interceptor
- Display user-friendly error messages
- Add loading and failure states
- Improve error logging
## Deliverables

This folder contains:

- QA Report Document
- Testing Evidence Screenshots
- Root Cause Analysis

---

## Conclusion

The application is currently not suitable for production use because critical backend communication failures prevent all major user workflows from functioning. Fixing the CORS configuration and implementing proper frontend error handling would restore usability and significantly improve the user experience.


# Task 2 – n8n API Integration Workflow

## Overview

This task demonstrates an n8n workflow that collects data from Hacker News, processes the results, enriches the data, applies conditional logic, and sends a formatted digest to Discord.

---

## Objective

Build an automated workflow that:

- Retrieves data from a public API
- Processes and filters the data
- Enriches the data using a second API request
- Applies conditional branching
- Sends notifications to Discord
- Handles errors gracefully

---

## APIs Used

### 1. Hacker News API
Used to retrieve the latest top stories.

Endpoint:
https://hacker-news.firebaseio.com/v0/topstories.json

### 2. Hacker News Item API
Used to fetch detailed information about each story.

Endpoint:
https://hacker-news.firebaseio.com/v0/item/{id}.json

### 3. Discord Webhook API
Used to send the final digest and error notifications to a Discord channel.

---

## Workflow Architecture

Webhook Trigger
↓
Fetch Top Story IDs
↓
Filter Top 5 IDs
↓
Fetch Story Details
↓
IF Score > 100
├── True → Build Discord Embed → Send to Discord
└── False → Log Low Score Stories

Error Handling:
Error Trigger
↓
Format Error Alert
↓
Send Error to Discord

---

## Data Transformation

The workflow:

1. Retrieves the list of top Hacker News story IDs.
2. Selects only the first 5 stories.
3. Fetches detailed information for each story.
4. Extracts:
   - Title
   - Score
   - URL
   - Number of Comments
5. Formats the data into a Discord Embed message.

---

## Conditional Logic

An IF node checks:

Score > 100

If TRUE:
- Story is included in the digest.

If FALSE:
- Story is logged separately as a low-score story.

---

## Error Handling

The workflow includes:

### Continue On Fail
Used on API request nodes to prevent workflow crashes.

### Error Trigger
Captures workflow failures.

### Error Formatting Node
Creates a readable error message.

### Discord Alert
Automatically sends failure notifications to Discord.

This ensures the workflow never fails silently.

---

## Output

The final output is a Discord message titled:

"HackerNews Morning Digest"

Each digest contains:

- Story Title
- Story Score
- Comment Count
- Story Link
- Generation Timestamp

---

## Files Included

- Task2_Workflow_manyameena.json
- Workflow Canvas Screenshot
- Successful Execution Screenshot
- Discord Output Screenshot
- README.md

---

## Result

The workflow successfully:

✅ Retrieves data from Hacker News

✅ Processes and filters records

✅ Enriches story information

✅ Applies conditional branching

✅ Sends formatted Discord notifications

✅ Handles failures through automated error alerts

This solution satisfies all Task 2 requirements specified in the assessment.




# Bonus Task – Uptime Monitor Workflow

## Overview

This workflow monitors the availability of the Conduit RealWorld application and automatically sends alerts to Discord when the application becomes unavailable.

---

## Objective

Create an automated uptime monitoring workflow that:

- Checks a web application at regular intervals
- Verifies the HTTP response status
- Detects downtime or failures
- Sends alerts to a notification channel
- Helps identify service outages quickly

---

## Monitored Application

Application:
https://angular.realworld.io

Testing Endpoint:
https://angular.realworld.io/bad

The invalid endpoint was intentionally used to simulate application failures and verify alert functionality.

---

## Workflow Architecture

Schedule Trigger
↓
HTTP Request
↓
Check HTTP Status Code
↓
IF Status Code ≠ 200
↓
Send Alert to Discord

---

## Workflow Components

### Schedule Trigger

- Runs automatically at regular intervals
- Continuously monitors application health

### HTTP Request

- Sends a request to the monitored application
- Retrieves the HTTP response status

### IF Condition

Checks:

Status Code ≠ 200

If the condition is true:

- Application is considered unavailable
- Alert is triggered

### Discord Alert

Posts an alert message to Discord:

"this server is down"

This provides immediate visibility when the application is unavailable.

---

## Error Detection Logic

| Status Code | Result |
|------------|---------|
| 200 | Application Healthy |
| Any Other Status | Alert Generated |

---

## Output

When downtime is detected:

- Discord notification is sent
- Team members are alerted immediately
- Application availability issues can be investigated quickly

---

## Files Included

- uptime_monitor_manyameena.json
- Uptime Workflow Screenshot
- Discord Alert Screenshot
- README.md

---

## Result

Successfully implemented:

✅ Automated uptime monitoring

✅ HTTP status verification

✅ Conditional alerting

✅ Discord notification integration

✅ Scheduled execution

The workflow successfully detects application failures and sends automated alerts to Discord, satisfying the Bonus Task requirements.
