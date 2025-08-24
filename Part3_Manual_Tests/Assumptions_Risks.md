# Assumptions & Risks

## Assumptions
- Test environment provides mock email service or real inboxes for verification.
- Audit logs are accessible via UI or API; format includes required fields (Admin ID, Route ID, etc.).
- Maximum route limit per agent is 10 (assumed for E3).
- System time can be mocked for edge cases (e.g., E1, E2).
- “Indoor vs. outdoor” location types are enforced in agent profiles.

## Risks
- **Unclear Behaviors**: Lack of clarity on maximum routes per agent or exact lock duration could lead to missed edge cases.
- **Missed Edge Cases**: Not testing reassignment near system boundaries (e.g., network failures, partial audit logging) may hide bugs.
- **Scalability**: High-volume reassignments might overload audit/email systems, untested here.
- **Permission Gaps**: Incomplete role-based access control testing could allow unauthorized actions.
