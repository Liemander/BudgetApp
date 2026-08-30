# Product

## Working name

Budget App

## Purpose

A self-hosted household budgeting application focused on paychecks, upcoming obligations, recurring bills, transfers, and debt.

The application is intended to replace an existing Google Sheets budgeting workbook that has evolved into a small application with:

- data entry,
- schedule generation,
- projections,
- dashboards,
- payment tracking,
- debt tracking,
- account management,
- and workflow automation.

## Primary goals

- Make household budgeting easier to understand than a spreadsheet.
- Organize expenses around upcoming paychecks.
- Make day-to-day updates fast.
- Preserve complete financial history.
- Support a self-contained installation for non-technical users.
- Support advanced self-hosted users with MariaDB.
- Provide a responsive web application.
- Add an Android application later.

## Target users

### Standard user

A friend or family member who wants to:

- install one application,
- avoid managing a database server,
- access the application from a browser,
- track household finances,
- back up and restore their data easily.

The standard edition should use SQLite internally.

### Advanced user

A technically comfortable user who may:

- self-host the web application,
- use an existing MariaDB server,
- use reverse proxies,
- use Docker or Linux,
- configure remote access.

## Product philosophy

The application is not intended to be a general accounting system.

It is primarily a household cash-flow and budgeting tool centered around:

- when income arrives,
- what is due before each paycheck,
- what has not been paid,
- how much money must be reserved,
- and how debt balances change over time.

## Main workflows

### Dashboard

The user should be able to see:

- next paycheck,
- paycheck after that,
- projected income,
- past-due or missing payments,
- bills due before the next paycheck,
- bills due before the following paycheck,
- upcoming transfers,
- debt obligations.

The user should be able to update payments directly from the dashboard.

### Account management

Maintain household accounts such as:

- checking,
- savings,
- credit cards,
- cash,
- loans,
- investments.

### Income

Track:

- income source,
- recipient,
- deposit account,
- schedule,
- projected pay,
- actual pay,
- notes,
- historical paychecks.

### Recurring bills

Track recurring obligations with deterministic schedules.

### Internal transfers

Track planned transfers that may be tied to selected income sources.

### Revolving debt

Track debt where balances may rise or fall over time.

### Major loans

Track scheduled installment debt such as mortgages, auto loans, and student loans.

## Non-goals for the first release

Do not prioritize:

- bank API integrations,
- automatic transaction import,
- investment portfolio tracking,
- tax accounting,
- double-entry accounting,
- AI financial advice,
- complex multi-tenant hosting.

These may be considered later.