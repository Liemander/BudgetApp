# Business Rules

This document captures established budgeting behavior from the existing system.

Do not silently change these rules during implementation.

## General principles

### Financial history

Historical actual data must be preserved.

Do not regenerate historical records in a way that overwrites:

- actual paycheck amounts,
- actual bill amounts,
- payment status,
- payment dates,
- notes,
- debt history.

### Deterministic schedules

Schedules must be generated from explicit anchors and configuration.

Do not use the current date to create or shift schedule dates.

The current date may be used to determine:

- overdue,
- upcoming,
- next paycheck,
- dashboard grouping.

---

# Accounts

Accounts have:

- Name
- Type
- Active
- Notes

Only active accounts should normally appear in selection lists.

---

# Income

Income sources have:

- Name
- Active
- Income Source / Employer
- Recipient
- Schedule
- Deposit Account
- First Pay Date

Supported schedules:

- Weekly
- Biweekly
- Semimonthly
- Monthly

## Weekly

Each paycheck is 7 days after the previous one.

## Biweekly

Each paycheck is 14 days after the previous one.

## Monthly

Each paycheck is one calendar month after the previous paycheck using calendar-month behavior.

## Semimonthly

Two configured days of the month are used.

Example:

- Pay Day 1 = 1
- Pay Day 2 = 15

If a configured day does not exist in a month, use the last day of that month.

Pay dates must remain deterministic from the configured schedule.

## Projected versus actual pay

Each paycheck can contain:

- projected amount,
- actual amount.

Historical actual pay overrides projections for historical reporting.

The system may support fixed or adaptive projections later, but projection calculations must be testable and deterministic.

---

# Dashboard paycheck cutoffs

The dashboard identifies the next two unpaid paycheck dates across all active income sources.

Duplicate household paycheck dates should count as one calendar cutoff date.

The dashboard uses:

1. Next paycheck date
2. Paycheck after that

These dates define upcoming bill buckets.

---

# Recurring Bills

A recurring bill definition includes:

- Name
- Active
- Provider
- Frequency
- First Due Date
- Budgeted Amount

Supported frequencies:

- Monthly
- Quarterly
- Semiannual
- Annual

Schedules are generated from First Due Date.

Examples:

Monthly:
- anchor
- +1 month
- +2 months

Quarterly:
- anchor
- +3 months
- +6 months

Semiannual:
- anchor
- +6 months

Annual:
- anchor
- +12 months

## Recurring bill occurrence

Each occurrence has:

- Due Date
- Budgeted Amount
- Actual Amount
- Paid?
- Paid Date
- Notes

If Actual Amount is blank, Budgeted Amount is the expected amount.

Paid occurrences should no longer appear as unpaid dashboard obligations.

---

# Dashboard bill grouping

The dashboard should show at minimum:

## Past Due / Missing Payments

Any active unpaid obligation with:

DueDate < today

## Due Before Next Paycheck

Any active unpaid obligation where:

today <= DueDate <= NextPaycheckDate

## Due Before Paycheck After That

Any active unpaid obligation where:

NextPaycheckDate < DueDate <= FollowingPaycheckDate

Dashboard rows should allow updating the underlying payment record.

The dashboard is a view over source records, not a duplicate data store.

---

# Internal Transfers

An internal transfer has:

- Name
- Active
- From Account
- To Account
- Amount Per Paycheck
- Start Date

A transfer can be tied to one or more selected active income sources.

For each selected income source:

- use that source's generated paycheck dates,
- include dates on or after Start Date.

Combined transfer occurrences are sorted by:

1. Pay Date
2. Income Source

Each occurrence contains:

- Pay Date
- Income Source
- Planned Amount
- Actual Amount
- Transferred?
- Transfer Date
- Notes

Historical transfer data must remain intact even if source selection changes.

---

# Revolving Debt

Revolving debt represents balances that may increase or decrease.

Examples:

- Credit Card
- Store Card
- Line of Credit
- HELOC

Important values:

- Statement Balance
- Expected Balance
- Balance Used
- Minimum Payment
- Planned Payment
- Actual Payment
- New Charges / Credits

## Balance selection

For a cycle:

If Statement Balance is available:

Balance Used = Statement Balance

Otherwise:

Balance Used = Expected Balance

## Expected balance

A future expected balance is derived from the previous cycle.

Conceptually:

Previous Balance Used
- actual payment if known
- otherwise planned payment
+ new charges / credits

Result must not fall below zero.

Positive New Charges / Credits increase debt.

Negative values reduce debt.

Do not treat revolving debt like a fixed amortizing loan.

---

# Major Loans

Major loans are installment-style debt.

Examples:

- Mortgage
- Student Loan
- Auto Loan
- Personal Loan

A loan may include:

- scheduled principal and interest,
- escrow,
- extra principal,
- actual payment,
- balance history.

Payments should be scheduled deterministically from First Due Date and schedule configuration.

Historical principal, interest, escrow, and extra-principal values must be preserved.

---

# Status rules

Status is derived, not authoritative history.

Examples include:

- Paid
- Missing
- Overdue
- Upcoming
- Paid Off

The authoritative facts are the underlying dates, amounts, and completion flags.

---

# Editing from Dashboard

When a payment is edited on the Dashboard:

- update the underlying occurrence/payment record,
- do not create a second independent payment record,
- refresh the dashboard from source data.

For recurring bills, dashboard editing should support:

- Actual Amount
- Paid?
- Paid Date
- Notes

For loans and revolving debt, dashboard editing should at least support:

- Actual Payment
- Notes
- completion/payment status as appropriate to the final model.