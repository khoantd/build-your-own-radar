## ADR-0001: Use Google Sheets as Primary Data Source

## Context

The ThoughtWorks Radar project is designed to generate an interactive technology radar using various data formats. The README specifies that the easiest out-of-the-box method for users to input data into the radar application is through a public Google Sheet. Other options include CSV and JSON formats, but these require additional setup or hosting, potentially complicating the user experience.

## Decision

Adopt the use of public Google Sheets as the primary data source for the radar generation process due to its accessibility and ease of use for end users.

## Consequences

### Positive

- Simplifies the onboarding process for new users, making it more likely they can successfully utilize the radar application.
- Reduces the need for users to have technical knowledge necessary to configure CSV or JSON inputs.

### Negative

- Users must make their Google Sheets public, which could be a concern for sensitive or proprietary data.
- Reliance on Google's platform may present issues if there are outages or changes to their API.

### Follow-ups

- Monitor the usage feedback to assess if users encounter issues with data privacy.
- Investigate alternative methods for providing data that may offer more control over data privacy while still being user-friendly.
