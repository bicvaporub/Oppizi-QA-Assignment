# Oppizi QA Assignment

This repository contains the deliverables for the QA assignment for Oppizi, as specified in the document: QA_Assignment.pdf.

## Structure
- **Part1_Diagrams/**: Contains PNG images of sequence diagrams for the Campaign Creation and Agent Flyer Scan user journeys, illustrating integration points for system integration testing.
- **Part2_API_Tests/**: Postman collection for testing the Open Charge Map API (`/poi/` and `/referencedata/`), with setup instructions and a test report.
- **Part3_Manual_Tests/**: Manual test suite for the Route Reassignment feature, including test cases, test data, a sample bug report, and assumptions/risks.

## Part 1: System Journeys and Architecture
- **Files**: 
  - `Campaign_Creation.png`: Sequence diagram for campaign creation, showing API validations and database storage.
  - `Agent_Flyer_Scan.png`: Sequence diagram for flyer scanning, highlighting geofencing and logging integrations.
- **Details**: Diagrams focus on integration points for testing purposes.

## Part 2: API Testing
- **Files**:
  - `OpenChargeMap_Tests.json`: Postman collection with tests for `/poi/` and `/referencedata/`.
  - `README.md`: Instructions for importing and running tests in Postman.
  - `Test_Report.md`: Summary of test results and observations.
- **Details**: Tests validate status code (200), response time (<1000ms), schema, and business logic.

## Part 3: Manual Testing
- **Files**:
  - `Manual_Test_Suite_Route_Reassignment.md`: Test cases, data table, and verification steps for route reassignment.
  - `Bug_Report_BUG-001.md`: Sample bug report for a route lock issue.
  - `Assumptions_Risks.md`: Assumptions and risks for the test scope.
- **Details**: Covers functional, negative, edge, and permission-related scenarios, with audit and email checks.

## Submission Notes
- All deliverables meet the requirements of the assignment PDF file.
- Diagrams are provided as PNGs.
- Contact: kevin_marbox@hotmail.com 
