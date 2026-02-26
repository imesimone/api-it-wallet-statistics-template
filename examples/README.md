# Entra con IT-Wallet Statistics Examples

This directory contains JSON examples of requests and responses for the "Entra con IT-Wallet" Statistics API.

## Scenarios

### 1. Global Summary
Retrieve total authentication counts for a given period across all managed entities.
- **Request**: [summary_request.json](summary_request.json)
- **Response**: [summary_response.json](summary_response.json)

### 2. Daily Breakdown for a Specific Entity
Monitor a specific administration (by IPA code) with daily granularity.
- **Request**: [daily_ipa_request.json](daily_ipa_request.json)
- **Response**: [daily_ipa_response.json](daily_ipa_response.json)

### 3. Monthly Regional Breakdown
Retrieve monthly trends aggregated across all managed entities for a Regional Hub.
- **Request**: [monthly_hub_request.json](monthly_hub_request.json)
- **Response**: [monthly_hub_response.json](monthly_hub_response.json)

### 4. Yearly Breakdown (Custom Period)
Example of a yearly breakdown requesting a period that crosses years (June 2023 to September 2024).
- **Request**: [yearly_custom_request.json](yearly_custom_request.json)
- **Response**: [yearly_custom_response.json](yearly_custom_response.json)

### 5. Daily Pagination
Example of paginating daily results (retrieving the 3rd week of March).
- **Request**: [daily_pagination_request.json](daily_pagination_request.json)
- **Response**: [daily_pagination_response.json](daily_pagination_response.json)

## Note on Statistics
Technical identifiers (paths, schema names, and JSON keys) use generic names (`total`, `success`, `failure`, etc.) for simplicity and consistency with standard monitoring patterns.

Following the schema, only the `total` field is strictly required in the statistics objects. Other fields are optional and can be omitted if data is unavailable.
