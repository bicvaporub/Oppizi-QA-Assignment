# Manual Test Suite: Route Reassignment

## Test Design
### Functional Test Cases
| **Test Case ID** | **Description** | **Steps** | **Expected Result** |
|-------------------|-----------------|-----------|---------------------|
| F1 | Reassign route to an eligible agent | 1. Log in as Admin.<br>2. Navigate to Campaign A dashboard.<br>3. Select Route 1 (outdoor, start date in future).<br>4. Reassign from Agent X to Agent Y (no overlapping routes, outdoor-compatible).<br>5. Confirm reassignment. | Route reassigned to Agent Y; audit log created; emails sent to X and Y; route locked for 24 hours. |
| F2 | Verify audit log entry after reassignment | 1. Perform F1.<br>2. Access audit system (via UI or API: `GET /audit/logs`).<br>3. Filter logs for Route 1 reassignment. | Log entry exists with timestamp, admin ID, Route 1 ID, Agent X → Agent Y details. |
| F3 | Verify email notifications | 1. Perform F1.<br>2. Check test environment inboxes for Agents X and Y (or mock email service). | Both agents receive emails with campaign name, route ID, and reassignment details. |

### Negative Test Cases
| **Test Case ID** | **Description** | **Steps** | **Expected Result** |
|-------------------|-----------------|-----------|---------------------|
| N1 | Attempt reassignment after campaign start | 1. Log in as Admin.<br>2. Select Campaign B (already started).<br>3. Try to reassign Route 2 to Agent Z.<br>4. Confirm. | Error message: “Cannot reassign after campaign start.” No audit log or emails created. |
| N2 | Attempt reassignment to agent with overlapping route | 1. Log in as Admin.<br>2. Select Campaign A, Route 3.<br>3. Try to reassign to Agent W (has overlapping route in Campaign C).<br>4. Confirm. | Error message: “Agent has conflicting route schedule.” No reassignment, no log, no emails. |
| N3 | Attempt reassignment with mismatched location type | 1. Log in as Admin.<br>2. Select Campaign A, Route 4 (indoor).<br>3. Try to reassign to Agent V (only outdoor routes).<br>4. Confirm. | Error message: “Location type mismatch.” No reassignment, no log, no emails. |

### Edge Test Cases
| **Test Case ID** | **Description** | **Steps** | **Expected Result** |
|-------------------|-----------------|-----------|---------------------|
| E1 | Reassign route just before campaign start | 1. Log in as Admin.<br>2. Select Campaign D (starts in 5 minutes).<br>3. Reassign Route 5 to Agent U.<br>4. Confirm. | Reassignment succeeds; audit log created; emails sent; route locked. |
| E2 | Attempt reassignment exactly 24 hours after prior reassignment | 1. Perform F1.<br>2. Wait exactly 24 hours (mock system time if needed).<br>3. Try reassigning Route 1 to Agent Z.<br>4. Confirm. | Reassignment succeeds; new audit log created; emails sent. |
| E3 | Reassign route with maximum allowed routes per agent | 1. Log in as Admin.<br>2. Assign maximum routes (assume 10) to Agent T.<br>3. Try reassigning Route 6 to Agent T.<br>4. Confirm. | Error message: “Agent has reached maximum route limit.” No reassignment, no log, no emails. |

### Permission-Related Test Cases
| **Test Case ID** | **Description** | **Steps** | **Expected Result** |
|-------------------|-----------------|-----------|---------------------|
| P1 | Non-admin attempts reassignment | 1. Log in as Agent X (non-admin).<br>2. Try to access reassignment feature for Route 1.<br>3. Attempt reassignment. | Error message: “Insufficient permissions.” Feature inaccessible; no changes. |
| P2 | Admin reassigns to unassigned agent | 1. Log in as Admin.<br>2. Select Campaign A, Route 7.<br>3. Reassign to unassigned Agent Q.<br>4. Confirm. | Reassignment succeeds; audit log created; email sent to Agent Q (none to original, as unassigned). |

## Test Data Table
| **Test Case ID** | **Campaign Name** | **Agent(s)** | **Route ID** | **Schedule** | **Location Type** | **Notes** |
|-------------------|-------------------|--------------|--------------|--------------|-------------------|-----------|
| F1, F2, F3 | Campaign A | X (original), Y (new) | Route 1 | 2025-09-01, 10:00-12:00 | Outdoor | Future start date |
| N1 | Campaign B | Z | Route 2 | 2025-08-20, 09:00-11:00 | Outdoor | Already started |
| N2 | Campaign A, Campaign C | W | Route 3 | 2025-09-01, 10:00-12:00 | Outdoor | W has overlapping route in Campaign C (10:00-11:00) |
| N3 | Campaign A | V | Route 4 | 2025-09-01, 13:00-15:00 | Indoor | V only handles outdoor routes |
| E1 | Campaign D | U | Route 5 | 2025-08-21, 16:00-18:00 | Outdoor | Starts in 5 minutes |
| E2 | Campaign A | Z | Route 1 | 2025-09-01, 10:00-12:00 | Outdoor | Test after 24-hour lock expires |
| E3 | Campaign A | T | Route 6 | 2025-09-01, 14:00-16:00 | Outdoor | T has 10 routes already |
| P1 | Campaign A | X | Route 1 | 2025-09-01, 10:00-12:00 | Outdoor | X is non-admin |
| P2 | Campaign A | Q | Route 7 | 2025-09-01, 15:00-17:00 | Outdoor | Route unassigned initially |

## Audit and Email Verification
- **Audit Log Verification**:
  - **Steps**: 1. After a successful reassignment (e.g., F1), log in as Admin.<br>2. Navigate to audit log UI or use API (`GET /audit/logs?campaign=CampaignA&route=Route1`).<br>3. Filter logs by timestamp and Route ID.<br>4. Verify log contains: Admin ID, Route ID, original/new agent names, timestamp, action (“Reassigned”).
  - **Expected**: Log entry matches reassignment details; no logs for failed attempts (e.g., N1-N3).
- **Email Verification**:
  - **Steps**: 1. After a successful reassignment (e.g., F1), access test environment email service (e.g., mock service like Mailtrap or real test inbox).<br>2. Check inboxes for Agents X and Y.<br>3. Verify email content: Subject (“Route Reassignment Notification”), body includes Campaign A, Route 1, schedule, and new assignment details.<br>4. For P2 (unassigned route), verify only Agent Q receives an email.
  - **Expected**: Emails sent only on successful reassignment; content accurate; no emails for failed attempts.
