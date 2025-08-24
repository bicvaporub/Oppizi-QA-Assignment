# Open Charge Map API Tests

This repository contains API tests for the Open Charge Map API, as specified in the QA Engineer Assignment (Part 2). Tests are built using Postman to validate the `/poi/` and `/referencedata/` endpoints.

## Prerequisites
- **Postman**: Install Postman (https://www.postman.com/downloads/).
- **API Key**: Obtain a valid API key by inspecting network calls on https://map.openchargemap.io/#/search.
- **Internet**: Stable connection to access the API.

## Setup Instructions
1. **Import the Collection**:
   - Open Postman.
   - Click **Import** > **Choose Files**.
   - Select `OpenChargeMap_Tests.json` from this repository.
   - The collection will appear in Postman under "Collections".

2. **Configure Environment**:
   - In Postman, go to **Environments** > **Create New Environment**.
   - Add two variables:
     - `baseUrl`: Set to `https://api.openchargemap.io/v3`.
     - `apiKey`: Set to your valid API key (replace `YOUR_API_KEY`).
   - Save and select this environment in Postman.

3. **Run the Tests**:
   - Open the "Open Charge Map API Tests" collection.
   - Run each request (`GET /poi/` and `GET /referencedata/`) individually by clicking **Send**.
   - Alternatively, use the **Collection Runner** to execute all tests:
     - Click **Runner** in Postman.
     - Select the collection, choose your environment, and click **Run**.
   - View test results in the "Test Results" tab for pass/fail status.

## Test Details
- **GET /poi/**:
  - Validates status code (200), response time (<1000ms), JSON schema (array of POIs with ID and AddressInfo), and business logic (POIs near latitude 51.5074, longitude -0.1278; max 10 results).
  - Parameters: `latitude=51.5074`, `longitude=-0.1278`, `distance=10`, `countrycode=GB`, `maxresults=10`.
- **GET /referencedata/**:
  - Validates status code (200), response time (<1000ms), JSON schema (object with ConnectionTypes and Countries arrays), and business logic (non-empty arrays for ConnectionTypes and Countries).
- Tests use Postman’s `pm.test` and `jsonSchema` assertions.

## Notes
- Replace `YOUR_API_KEY` in the environment with a valid key before running.
- If the API is down or the key is invalid, tests will fail with 401 or timeout errors.
- Schema validations are based on inferred structures; adjust if the actual API response differs.
