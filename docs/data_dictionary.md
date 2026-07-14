# Data Dictionary

This document defines a manageable schema for the Fintech Customer Intelligence & Churn Prediction project. The tables are designed for a graduate-level data science portfolio and focus on fields that are useful, realistic, and not overly personal.

## Table relationships

`customer_id` is the shared key across the four tables:

- `customers.customer_id` identifies each customer.
- `transactions.customer_id` links financial activity to that customer.
- `app_activity.customer_id` links app engagement to that customer.
- `subscriptions.customer_id` links plan information to that customer.

Together, the tables support a customer-level view of profile context, transaction behaviour, app usage, and subscription status.

## Churn definition

A customer is treated as **churned** after **60 consecutive days without a successful transaction**.

Only transactions where `transaction_status` is `successful` count as activity for this rule. Failed, pending, cancelled, or reversed transactions do not reset the 60-day churn window.

## Data leakage note

Cancellation-related fields, such as `cancelled` and `cancellation_date`, must only be used if their values are known on or before the model's prediction date. If cancellation happens after the prediction date, using it as an input feature would leak future information into the model.

---

## Table 1: `customers`

One row per customer, covering basic account and acquisition details.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `customer_id` | String | Unique customer identifier and primary join key. | `CUST_000123` | Connects customer information to transactions, app activity, and subscriptions. |
| `signup_date` | Date | Date the customer created the account. | `2024-03-18` | Measures customer tenure, which can affect churn risk. |
| `age_group` | String | Customer age range rather than exact age. | `30-39` | Supports demographic comparison without storing exact birth dates. |
| `country` | String | Customer's country of residence. | `United States` | Helps compare churn across markets with different product adoption patterns. |
| `acquisition_channel` | String | How the customer first joined the product. | `Referral` | Customers from different channels may have different expectations and retention rates. |
| `verification_completed` | Boolean | Whether the customer completed identity or account verification. | `true` | Incomplete verification can block usage and increase churn risk. |

---

## Table 2: `transactions`

One row per transaction, covering transaction timing, value, type, and outcome.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `transaction_id` | String | Unique transaction identifier. | `TXN_987654` | Prevents double-counting when measuring transaction frequency. |
| `customer_id` | String | Customer associated with the transaction. | `CUST_000123` | Links transaction behaviour to the customer record. |
| `transaction_timestamp` | DateTime | Exact date and time of the transaction. | `2025-01-22 14:35:08` | Main field for calculating recency and the 60-day churn rule. |
| `transaction_amount` | Decimal | Value of the transaction. | `49.99` | Lower or declining transaction value may indicate reduced engagement. |
| `transaction_type` | String | Category of money movement. | `card_payment` | Some transaction types may reflect stronger product dependence than others. |
| `merchant_category` | String | Spending or merchant category. | `Groceries` | Recurring everyday categories can signal regular product use. |
| `transaction_status` | String | Final transaction outcome. | `successful` | Determines whether the transaction counts toward active usage. |
| `currency` | String | Currency used for the transaction. | `USD` | Keeps transaction values interpretable across countries. |
| `fee_amount` | Decimal | Fee charged on the transaction. | `1.50` | High or frequent fees may contribute to dissatisfaction. |

---

## Table 3: `app_activity`

One row per customer activity date, summarising app engagement for that day.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `activity_id` | String | Unique app activity record identifier. | `ACT_456789` | Keeps daily activity records distinct. |
| `customer_id` | String | Customer associated with the activity record. | `CUST_000123` | Links app usage to transaction and subscription behaviour. |
| `activity_date` | Date | Date of the app activity summary. | `2025-01-23` | Shows whether app engagement drops before transaction inactivity. |
| `app_sessions` | Integer | Number of app sessions on that date. | `3` | Falling session counts can be an early sign of disengagement. |
| `feature_used` | String | Main app feature used on that date. | `budget_tracker` | Helps identify features associated with retention or churn. |
| `support_contacted` | Boolean | Whether the customer contacted support on that date. | `true` | Support contact may indicate friction, confusion, or unresolved issues. |
| `push_notification_opened` | Boolean | Whether the customer opened a push notification. | `false` | Measures response to retention or re-engagement messaging. |

---

## Table 4: `subscriptions`

One row per customer subscription record, covering plan type, price, and cancellation status.

| Column name | Data type | Meaning | Example value | Use for churn analysis |
| --- | --- | --- | --- | --- |
| `subscription_id` | String | Unique subscription record identifier. | `SUB_246810` | Tracks each subscription record separately. |
| `customer_id` | String | Customer associated with the subscription. | `CUST_000123` | Links plan information to customer, transaction, and app data. |
| `plan_type` | String | Customer's subscription level. | `Premium` | Churn may vary between free, basic, and premium plans. |
| `monthly_price` | Decimal | Monthly cost of the subscription. | `9.99` | Supports analysis of price sensitivity and plan value. |
| `subscription_start_date` | Date | Date the subscription began. | `2024-06-01` | Measures subscription tenure before inactivity or cancellation. |
| `cancelled` | Boolean | Whether the customer cancelled the subscription. | `false` | Can help explain churn if known before the prediction date. |
| `cancellation_date` | Date | Date the subscription was cancelled, if applicable. | `2025-02-14` | Useful for analysing churn timing, but must not be used as a future-looking feature. |

---

## Notes

- Use `customer_id` to join the four tables.
- Build the churn label from successful transactions in the `transactions` table.
- App activity and subscription information are supporting signals, not the churn definition.
- This document does not create data, train models, or add prediction code.
