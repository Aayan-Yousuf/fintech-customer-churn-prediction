# Data Dictionary

This document defines the planned tables for the Fintech Customer Intelligence & Churn Prediction project. It is a schema guide only; no data has been generated.

## Table relationships

`customer_id` is the shared key across the four tables:

- `customers.customer_id` identifies each customer.
- `transactions.customer_id` links payments and transfers to that customer.
- `app_activity.customer_id` links app usage events to that customer.
- `subscriptions.customer_id` links subscription records to that customer.

Together, these tables support a customer-level view of profile details, financial activity, app engagement, and subscription history.

## Churn definition

A customer is treated as **churned** after **60 consecutive days without a successful transaction**.

Only transactions where `transaction_status` is `successful` count as activity for this rule. Failed, pending, cancelled, or reversed transactions do not reset the 60-day churn window.

---

## Table 1: `customers`

One row per customer, covering account setup and profile attributes.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `customer_id` | String | Unique customer identifier and primary join key. | `CUST_000123` | Combines profile data with transactions, app activity, and subscriptions. |
| `signup_date` | Date | Date the account was created. | `2024-03-18` | Supports tenure analysis, since early-life churn can differ from long-term inactivity. |
| `date_of_birth` | Date | Customer birth date, if collected. | `1992-08-11` | Can be converted into age for demographic segmentation. |
| `age_group` | String | Age range derived from date of birth. | `30-39` | Makes age-based churn comparisons easier and less sensitive than exact birth dates. |
| `gender` | String | Self-reported gender, if provided. | `Female` | Useful for segment checks, provided it is handled carefully and not used for unfair targeting. |
| `country` | String | Customer's country of residence. | `United States` | Helps compare churn across markets with different payment habits or product fit. |
| `state_or_region` | String | State, province, or region. | `California` | Adds regional detail when country-level patterns are too broad. |
| `city` | String | Customer's city. | `San Francisco` | Can reveal local adoption or engagement patterns when sample sizes are large enough. |
| `customer_segment` | String | Business-defined customer group. | `Mass Market` | Helps compare churn across strategic segments such as standard, premium, or business users. |
| `employment_status` | String | Work status, if collected. | `Employed` | May relate to income stability, spending frequency, and product usage. |
| `income_band` | String | Approximate income range rather than exact income. | `$50k-$75k` | Allows financial segmentation without storing unnecessary precision. |
| `credit_score_band` | String | Grouped credit score category, if available. | `Good` | May explain differences in product eligibility, transaction behaviour, or account health. |
| `account_status` | String | Current account state, such as active, closed, suspended, or under review. | `Active` | Separates voluntary inactivity from operational account restrictions. |
| `marketing_opt_in` | Boolean | Whether the customer agreed to marketing communications. | `true` | Indicates whether retention emails, offers, or reminders can be sent. |

---

## Table 2: `transactions`

One row per transaction, covering money movement, status, amount, and timing.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `transaction_id` | String | Unique transaction identifier. | `TXN_987654` | Prevents double-counting when calculating transaction frequency. |
| `customer_id` | String | Customer associated with the transaction. | `CUST_000123` | Links transaction behaviour back to the customer record. |
| `transaction_date` | Date | Calendar date of the transaction. | `2025-01-22` | Core field for measuring the 60-day period with no successful transactions. |
| `transaction_timestamp` | DateTime | Exact date and time of the transaction. | `2025-01-22 14:35:08` | Supports more precise recency, frequency, and usage-timing features. |
| `transaction_type` | String | Category of money movement. | `card_payment` | Different transaction types may show different levels of product dependence. |
| `transaction_amount` | Decimal | Transaction value. | `49.99` | Falling amounts can signal lower engagement before full inactivity. |
| `currency` | String | Currency used for the transaction. | `USD` | Needed for consistent value comparisons across markets. |
| `merchant_category` | String | Spending or merchant category. | `Groceries` | Helps identify lifestyle patterns linked with regular usage or disengagement. |
| `transaction_channel` | String | Channel used to complete the transaction. | `mobile_app` | Channel shifts or drop-offs can highlight changes in customer behaviour. |
| `transaction_status` | String | Final transaction outcome. | `successful` | Determines whether a transaction counts toward the churn rule. |
| `failure_reason` | String | Reason for failure when the transaction was not successful. | `insufficient_funds` | Repeated failures may indicate frustration, financial stress, or payment friction. |
| `is_successful` | Boolean | Flag showing whether the transaction succeeded. | `true` | Simplifies churn-label creation and successful-activity counts. |
| `fee_amount` | Decimal | Fee charged on the transaction. | `1.50` | High or repeated fees may contribute to dissatisfaction. |
| `balance_after_transaction` | Decimal | Account balance after the transaction. | `1240.75` | Declining balances may show customers moving funds away before churning. |

---

## Table 3: `app_activity`

One row per app activity event or daily activity summary, depending on the final dataset design.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `activity_id` | String | Unique app activity record identifier. | `ACT_456789` | Keeps activity events distinct when aggregating engagement. |
| `customer_id` | String | Customer associated with the activity. | `CUST_000123` | Links app behaviour with transaction and subscription patterns. |
| `activity_date` | Date | Date of the activity event or summary. | `2025-01-23` | Tracks engagement changes before transaction inactivity reaches 60 days. |
| `session_start_timestamp` | DateTime | Exact start time of an app session. | `2025-01-23 09:12:44` | Supports recency and usage-pattern features. |
| `session_duration_seconds` | Integer | Length of the app session in seconds. | `315` | Shorter sessions can suggest weaker engagement. |
| `login_count` | Integer | Number of logins during the recorded period. | `3` | Falling login counts may be an early churn warning. |
| `screens_viewed_count` | Integer | Number of app screens viewed. | `12` | Measures depth of use beyond simply opening the app. |
| `feature_used` | String | Main feature used in the activity record. | `budget_tracker` | Shows which features are associated with retention or disengagement. |
| `device_type` | String | Device or operating system used. | `iOS` | Helps spot churn differences that may reflect app experience issues. |
| `app_version` | String | App version used by the customer. | `2.4.1` | Older versions may be linked with bugs, missing features, or lower engagement. |
| `push_notification_opened` | Boolean | Whether the customer opened a push notification. | `false` | Measures response to reactivation or retention messaging. |
| `support_page_viewed` | Boolean | Whether the customer viewed help or support content. | `true` | May indicate confusion, product friction, or unresolved issues. |
| `last_app_open_date` | Date | Most recent date the customer opened the app. | `2025-01-23` | Allows comparison between app inactivity and transaction inactivity. |

---

## Table 4: `subscriptions`

One row per subscription period, covering plan details, billing, and cancellation information.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `subscription_id` | String | Unique subscription record identifier. | `SUB_246810` | Tracks subscription periods and plan changes separately. |
| `customer_id` | String | Customer associated with the subscription. | `CUST_000123` | Connects plan history with profile, transaction, and app usage data. |
| `plan_name` | String | Name of the subscription plan. | `Premium` | Plan value and benefits may affect retention. |
| `plan_tier` | String | Broad plan level. | `Paid` | Enables churn comparisons between free, basic, and premium customers. |
| `subscription_start_date` | Date | Date the subscription period began. | `2024-06-01` | Measures plan tenure before inactivity or cancellation. |
| `subscription_end_date` | Date | Date the subscription period ended, if applicable. | `2025-06-01` | Helps compare transaction churn with formal plan endings. |
| `subscription_status` | String | Current state of the subscription. | `Active` | Distinguishes active, cancelled, paused, expired, or past-due customers. |
| `billing_cycle` | String | Billing frequency. | `Monthly` | Monthly and annual plans often have different churn timing. |
| `monthly_price` | Decimal | Monthly subscription cost. | `9.99` | Price sensitivity can be tested against churn behaviour. |
| `payment_method` | String | Method used for subscription billing. | `debit_card` | Expired or unreliable payment methods can create involuntary churn. |
| `auto_renew_enabled` | Boolean | Whether the plan renews automatically. | `true` | Non-renewing customers may require earlier retention outreach. |
| `trial_start_date` | Date | Start date of a free trial, if any. | `2024-05-18` | Helps analyse conversion and churn around trial periods. |
| `trial_end_date` | Date | End date of a free trial, if any. | `2024-06-01` | Trial-end timing can explain sudden drops in activity. |
| `cancellation_date` | Date | Date the customer cancelled, if applicable. | `2025-02-14` | Can be compared with the 60-day transaction-based churn date. |
| `cancellation_reason` | String | Customer-provided cancellation reason, if available. | `Too expensive` | Provides direct context for churn drivers such as price, missing features, or poor experience. |

---

## Notes

- Use `customer_id` to join the four tables.
- Build the churn label from successful transactions in the `transactions` table.
- App inactivity and subscription cancellation are supporting signals, not the churn definition.
- This document does not create data, train models, or add prediction code.
