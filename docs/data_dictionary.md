# Data Dictionary

This document defines the planned tables for the Fintech Customer Intelligence & Churn Prediction project. It is a schema guide only; no data has been generated.

## Table relationships

`customer_id` is the shared key across the four tables:

- `customers.customer_id` identifies each customer.
- `transactions.customer_id` links payments and transfers to that customer.
- `app_activity.customer_id` links app usage records to that customer.
- `subscriptions.customer_id` links subscription records to that customer.

Together, these tables support a customer-level view of profile details, financial activity, app engagement, and subscription history.

## Churn definition

A customer is treated as **churned** after **60 consecutive days without a successful transaction**.

Only transactions where `transaction_status` is `successful` count as activity for this rule. Failed, pending, cancelled, or reversed transactions do not reset the 60-day churn window.

## Data-leakage note

Cancellation fields must not be used as model features when the cancellation occurs after the model's prediction date.

---

## Table 1: `customers`

One row per customer, covering account setup and profile attributes.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `customer_id` | String | Unique customer identifier and primary join key. | `CUST_000123` | Combines profile data with transactions, app activity, and subscriptions. |
| `signup_date` | Date | Date the account was created. | `2024-03-18` | Supports tenure analysis. |
| `age_group` | String | Customer age range. | `30-39` | Enables age-based churn comparisons without storing exact birth dates. |
| `country` | String | Customer's country of residence. | `United States` | Helps compare churn across markets. |
| `acquisition_channel` | String | Channel that brought the customer to the product. | `paid_search` | Shows whether retention differs by acquisition source. |
| `verification_completed` | Boolean | Whether account verification was completed. | `true` | Helps distinguish fully onboarded customers from incomplete signups. |

---

## Table 2: `transactions`

One row per transaction, covering money movement, status, amount, and timing.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `transaction_id` | String | Unique transaction identifier. | `TXN_987654` | Prevents double-counting when calculating transaction frequency. |
| `customer_id` | String | Customer associated with the transaction. | `CUST_000123` | Links transaction behaviour back to the customer record. |
| `transaction_timestamp` | DateTime | Exact date and time of the transaction. | `2025-01-22 14:35:08` | Core field for measuring the 60-day period with no successful transactions. |
| `transaction_amount` | Decimal | Transaction value. | `49.99` | Supports transaction value and usage trend analysis. |
| `transaction_type` | String | Category of money movement. | `card_payment` | Different transaction types may show different levels of product dependence. |
| `merchant_category` | String | Spending or merchant category. | `Groceries` | Helps identify usage patterns linked with retention or disengagement. |
| `transaction_status` | String | Final transaction outcome. | `successful` | Determines whether a transaction counts toward the churn rule. |
| `currency` | String | Currency used for the transaction. | `USD` | Needed for consistent value comparisons across markets. |
| `fee_amount` | Decimal | Fee charged on the transaction. | `1.50` | Helps evaluate whether fees are associated with churn risk. |

---

## Table 3: `app_activity`

One row per app activity event or daily activity summary, depending on the final dataset design.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `activity_id` | String | Unique app activity record identifier. | `ACT_456789` | Keeps activity records distinct when aggregating engagement. |
| `customer_id` | String | Customer associated with the activity. | `CUST_000123` | Links app behaviour with transaction and subscription patterns. |
| `activity_date` | Date | Date of the activity event or summary. | `2025-01-23` | Tracks engagement changes before transaction inactivity reaches 60 days. |
| `app_sessions` | Integer | Number of app sessions during the recorded period. | `3` | Measures app engagement frequency. |
| `feature_used` | String | Main feature used in the activity record. | `budget_tracker` | Shows which features are associated with retention or disengagement. |
| `support_contacted` | Boolean | Whether the customer contacted support. | `false` | Indicates potential friction or service needs. |
| `push_notification_opened` | Boolean | Whether the customer opened a push notification. | `true` | Measures response to reactivation or retention messaging. |

---

## Table 4: `subscriptions`

One row per subscription period, covering plan details, billing, and cancellation information.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `subscription_id` | String | Unique subscription record identifier. | `SUB_246810` | Tracks subscription periods separately. |
| `customer_id` | String | Customer associated with the subscription. | `CUST_000123` | Connects plan history with profile, transaction, and app usage data. |
| `plan_type` | String | Subscription plan type. | `Premium` | Enables churn comparisons across plans. |
| `monthly_price` | Decimal | Monthly subscription cost. | `9.99` | Helps evaluate price sensitivity. |
| `subscription_start_date` | Date | Date the subscription period began. | `2024-06-01` | Measures plan tenure before inactivity or cancellation. |
| `cancelled` | Boolean | Whether the subscription was cancelled. | `false` | Supports post-period analysis when used without leakage. |
| `cancellation_date` | Date | Date the customer cancelled, if applicable. | `2025-02-14` | Can be compared with the 60-day transaction-based churn date when available before prediction. |

---

## Notes

- Use `customer_id` to join the four tables.
- Build the churn label from successful transactions in the `transactions` table.
- App inactivity and subscription cancellation are supporting signals, not the churn definition.
- This document does not create data, train models, or add prediction code.
