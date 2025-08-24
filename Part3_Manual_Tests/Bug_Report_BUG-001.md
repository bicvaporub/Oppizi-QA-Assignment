# Bug Report: Route Lock Persists Beyond 24 Hours

**Bug Title**: Route Lock Persists Beyond 24 Hours  
**Bug ID**: BUG-001  
**Module**: Route Reassignment Feature  
**Environment**: Test Environment (Oppizi Dashboard v2.1)  
**Severity**: High  
**Priority**: Medium  
**Preconditions**: Admin logged in, Campaign A with Route 1 reassigned to Agent Y.  
**Steps to Reproduce**:  
1. Perform reassignment of Route 1 (Campaign A) from Agent X to Agent Y.  
2. Confirm reassignment (audit log and emails verified).  
3. Wait 24 hours and 5 minutes (mock system time if needed).  
4. Attempt to reassign Route 1 to Agent Z.  
**Actual Result**: Error message: “Route is locked from reassignment.”  
**Expected Result**: Reassignment should succeed after 24-hour lock expires; new audit log and emails created.  
**Attachments**: Screenshot of error, audit log export.  
**Additional Notes**: Issue may affect campaign scheduling; verify lock duration logic in backend.
