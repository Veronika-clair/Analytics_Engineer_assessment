# Engineering Analyst Assignment

## Background

At Clair, we're on a mission to create financial freedom for America's workers by giving them access to free wage advances so they can be paid as soon as they clock out of work. To understand user activation and readiness for lending, we need clean, analytics-ready data that tracks onboarding completion and first-time loan usage.

You’ve been provided with sample data from our raw and semi-modeled tables. Your job is to derive key lifecycle metrics and outline how we can transform noisy operational data into useful analytics models.

## Part I – Lifecycle Summary Table

Create a table called `user_lending_readiness` where each row represents a unique user and includes the following metrics:

### Source Tables

1. **`stg_prod__users`**
   - `user_id`
   - `employee_id`
   - `started_onboarding_at`
   - `kyc_status`
   - `date_of_birth`

2. **`stg_gusto_prod__webhook_logs`**
   - `user_id`
   - `subject_type`
   - `payload` (extract `enrollment_completed_at` from `enrollment.pass` event)
   - `response_status`
   
3. **`gusto_loans`**
   - `loan_uuid`
   - `user_id`
   - `amount`
   - `applied_at`
   - `settled_at`
   - `fully_repaid_at`
   - `is_confirmed`
   - `is_instant`

### Expected Output: `user_lending_readiness`

| Column                          | Description                                                   |
|---------------------------------|---------------------------------------------------------------|
| `user_id`                       | Unique user identifier                                        |
| `started_onboarding_at`        | Timestamp from when the user began onboarding                |
| `enrollment_completed_at`      | Extracted timestamp from webhook payload                     |
| `has_completed_enrollment`     | TRUE if `kyc_status = 'Pass'` and enrollment timestamp exists |
| `first_loan_confirmed_at`      | First confirmed loan timestamp (`is_confirmed = TRUE`)       |
| `first_loan_fully_repaid_at`   | Timestamp when first loan was fully repaid                   |
| `lifetime_loans_confirmed_count` | Count of all confirmed loans                                |
| `lifetime_full_repaid_loan_amount` | Total of fully repaid loan amounts                      |
| `days_to_create_account`       | Days between onboarding and enrollment completion             |
| `days_to_first_loan`           | Days between enrollment completion and first loan             |
| `is_returning`                 | TRUE if the user has at least one fully repaid loan           |

Your output should be derived from the source data. Do not assume it is clean or ready for use.

## Part II – Documentation

Please include a `README.md` alongside your code or notebook that addresses:

- **Field Derivations**: How each column in `user_lending_readiness` was calculated
- **Assumptions & Filters**: Any logic or assumption rules you applied
- **Data Quality Observations**: Issues you noticed and suggested improvements
- **Testing Ideas**: What tests could be implemented to validate the data integrity

## Part III – ETL Design for Webhook Logs

The `webhook_logs` table is an operational system table that captures many webhook events. It is large and not suitable for direct use in reporting. Outline how you would transform this data into an analytics-ready model (just a document, no actual work needs to be done). 

### Please include:

#### 1. ETL Flow Design
- What layers would you include (e.g., raw → staging → intermediate → reporting)?
- Would you isolate enrollment events from other types? If yes, how?
- What intermediate models would you build?

#### 2. Performance & Cost Optimization
- How would you make downstream queries efficient?
- What materialization strategies would you use (e.g., table, incremental, view)?
- Would you filter, partition, or cluster data? Where and why?

#### 3. Data Quality & Monitoring
- How would you handle retries or duplicate webhook entries?
- How would you validate the structure of JSON payloads?
- Would you implement tests, alerts, or logging mechanisms? What would they check for?
