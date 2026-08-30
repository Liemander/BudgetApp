# Spreadsheet Migration

## Purpose

The existing Google Sheets workbook is the source of historical budgeting data and also serves as a functional specification for the first version of the application.

Migration must preserve historical actual values.

## Existing workbook concepts

Current major concepts include:

- Overview
- Admin / Accounts
- Income
- Recurring Bills
- Internal Transfers
- Revolving Debt
- Major Loans

## Current account data

The current workbook contains accounts such as:

- Bills
- Kelley's Personal
- Justin's personal
- Mortgage
- Savings

These are examples from the existing dataset and should not be hard-coded into the product.

## Income history

Existing income sheets contain:

- projected pay dates,
- projected pay,
- actual pay,
- notes,
- schedule configuration.

Historical actual paycheck values must be imported as historical Paycheck records.

## Recurring bill history

Recurring bill sheets contain:

- due dates,
- budgeted amounts,
- actual bill amounts,
- paid flags,
- paid dates,
- notes.

Import these into RecurringBill and BillOccurrence records.

## Internal transfer history

Internal transfer sheets contain:

- income source,
- pay date,
- planned transfer,
- actual transfer,
- transferred flag,
- transfer date,
- notes.

Historical rows with user-entered data must be preserved even if they no longer match current generation rules.

## Revolving debt history

Revolving debt sheets contain values such as:

- statement date,
- due date,
- statement balance,
- expected balance,
- minimum payment,
- planned payment,
- actual payment,
- new charges / credits,
- notes.

Preserve entered statement and payment history.

## Major loan history

Major loan sheets may contain:

- due date,
- required payment,
- actual payment,
- principal paid,
- interest paid,
- escrow,
- extra principal,
- resulting balance,
- notes.

Preserve these as historical loan payment records.

## Migration strategy

Migration should be incremental and verifiable.

Suggested process:

1. Export or read workbook data.
2. Parse records without modifying the workbook.
3. Validate counts and totals.
4. Import into a temporary/new application database.
5. Compare source and destination.
6. Produce a migration report.
7. Only treat the application as authoritative after validation.

## Requirements

- Do not overwrite the source workbook.
- Do not invent missing historical values.
- Preserve original dates.
- Preserve exact actual monetary values.
- Preserve notes.
- Log rows that cannot be imported cleanly.
- Support re-running migration into a clean database during development.

## Initial migration tool

The first migration tool can be a developer utility rather than an end-user feature.

Eventually the product may include:

- Google Sheets import,
- CSV import,
- JSON import/export.