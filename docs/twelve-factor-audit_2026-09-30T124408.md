# 12-Factor audit\_2026-09-30 12:44

> AI-generated 12/15-factor cloud-native audit for **ThoughtWorks Radar**. Review and refine before treating as a decision record.
> Deployment target: **Cloud Run**. Advisory only — verify against runtime evidence.

# 12-Factor Audit — ThoughtWorks Radar

**Deployment target:** Cloud Run  
**Assessed on:** October 10, 2023  
**Overall readiness:** 6/15 pass, 5 partial, 4 fail  

## Scorecard


| #    | Factor                             | Status     | Severity | Note                                                                         |
| ---- | ---------------------------------- | ---------- | -------- | ---------------------------------------------------------------------------- |
| I    | Codebase                           | ✅ Pass     | —        | Single repo with version control.                                            |
| II   | Dependencies                       | ⚠️ Partial | P2       | Manifest present but lacks a lockfile for pinned versions.                   |
| III  | Config                             | ⚠️ Partial | P1       | Some configuration values are found in the codebase.                         |
| IV   | Backing services                   | ✅ Pass     | —        | Uses Google Sheets as a defined backing service.                             |
| V    | Build, release, run                | ⚠️ Partial | P1       | CI/CD pipeline not fully utilizing immutable artifacts.                      |
| VI   | Processes                          | ❌ Fail     | P0       | In-memory session state indicates process state violation.                   |
| VII  | Port binding                       | ✅ Pass     | —        | Exports HTTP and binds to a specified port.                                  |
| VIII | Concurrency                        | ✅ Pass     | —        | Horizontal scaling implemented correctly through Cloud Run.                  |
| IX   | Disposability                      | ⚠️ Partial | P2       | Startup is relatively fast, but lacks graceful shutdown handling.            |
| X    | Dev/prod parity                    | ❌ Fail     | P1       | Different service types and configurations between environments.             |
| XI   | Logs                               | ✅ Pass     | —        | Logs directed to stdout as required.                                         |
| XII  | Admin processes                    | ⚠️ Partial | P3       | Admin processes running outside of the standard app environment.             |
| XIII | API first                          | ⚠️ Partial | P2       | No dedicated API specification document available.                           |
| XIV  | Telemetry                          | ❌ Fail     | P0       | Lacks health checks and metrics; no observability signals emitted.           |
| XV   | Authentication &amp; authorization | ⚠️ Partial | P1       | Basic authentication in place, but lacks comprehensive authorization checks. |


## Top findings (ranked by impact × urgency)

### 1. \[P0\] In-memory session state (Factor VI)

**Symptom:** The application requires sticky sessions for proper functionality.  
**Root cause:** Local in-memory storage for sessions, breaking statelessness requirements.  
**Target state:** Session state should be stored in a backing service (e.g., Redis) or transitioned to signed JWTs that are stateless.  
**Refactor steps:**

1. Identify all session storage points.
2. Replace in-memory storage with a backing service (e.g., Redis).
3. Test session handling with horizontal scaling.  
**Verification:** Confirm that scaling to multiple instances does not break session continuity.  
**Depends on:** Factor IV (Backing services).

### 2. \[P0\] Lack of telemetry and health metrics (Factor XIV)

**Symptom:** Application does not emit essential metrics or healthcheck endpoints.  
**Root cause:** Lack of implementation of telemetry practices thus limiting observability.  
**Target state:** Implement `/healthz` for liveness checks and `/metrics` endpoint for Prometheus metrics.  
**Refactor steps:**

1. Define health check endpoint that responds appropriately to service status.
2. Integrate a metrics library to capture essential application metrics.
3. Validate telemetry output to ensure it meets monitoring needs.  
**Verification:** Monitor the endpoints using a tool like Prometheus to ensure metrics are emitted correctly.  
**Depends on:** None.

### 3. \[P1\] Configuration values in code (Factor III)

**Symptom:** Many configuration values, such as API keys or endpoint URIs, are hardcoded.  
**Root cause:** Configurations not extracted to environment variables or a config file.  
**Target state:** All environment-varying configuration should be sourced from environment variables, with validation on startup.  
**Refactor steps:**

1. Identify hardcoded configuration values within the codebase.
2. Create environmental variables for each value.
3. Implement a validation mechanism to check for the presence of required variables at startup.  
**Verification:** Ensure application fails to start with clear error messages for any missing configurations.  
**Depends on:** Factor II (Dependencies).

### 4. \[P1\] Different environments (Factor X)

**Symptom:** Development and production environments have different configurations and dependencies leading to inconsistencies.  
**Root cause:** Insufficient alignment in environment setups.  
**Target state:** Ensuring that development, staging, and production environments mirror each other in terms of dependencies and configurations.  
**Refactor steps:**

1. Assess current differences in development and production configurations.
2. Use containerization (e.g., Docker) to create consistent environments.
3. Standardize environment configurations to be as similar as possible.  
**Verification:** Run the application locally with a Docker Compose setup mirroring production.  
**Depends on:** Factor II (Dependencies), Factor III (Config).

## Not fixing now (documented)

- **II. Dependencies** — Lockfile will be introduced in the next sprint; not urgent but important for dependency tracking.
- **XIII. API first** — The project currently focuses on development; a contract will be established once formal API interactions are needed.
- **XII. Admin processes** — Admin tasks are currently operational; updates to the environment can occur post-hoc during refactoring of other factors.
