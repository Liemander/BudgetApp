# Budget App Development Instructions

This repository contains a self-hosted household budgeting application written in C# and .NET.

Before making architectural or domain changes, read:

- docs/PRODUCT.md
- docs/ARCHITECTURE.md
- docs/DATA_MODEL.md
- docs/BUSINESS_RULES.md
- docs/MIGRATION.md

## Development principles

- Preserve financial history.
- Prefer explicit, understandable code over clever abstractions.
- Business and scheduling logic belongs on the server.
- Web and mobile clients must not independently implement financial rules.
- Use `decimal` for money. Do not use `float` or `double` for currency.
- Prefer `DateOnly` for date-only financial events where appropriate.
- All database schema changes must use migrations.
- Add automated tests for budgeting and scheduling rules.
- Do not silently change established business rules.
- Keep changes small and reviewable.
- Before implementing a substantial feature, propose a plan.
- Do not introduce a dependency without explaining why it is needed.
- Do not delete historical data during migrations or refactors.

## Technology direction

- .NET 10
- ASP.NET Core backend
- Blazor web UI
- SQLite as the default self-contained database
- MariaDB as an optional advanced database provider
- Future Android client using .NET MAUI
- Git for source control

## Architecture constraints

- Core budgeting logic must not depend directly on SQLite or MariaDB.
- The backend/API is authoritative.
- Clients consume server-calculated results.
- Database providers must be swappable without changing core business logic.
- Prefer simple architecture over unnecessary layers.

## Current priorities

Build the web/server product first.

Do not start the Android application until:
- the domain model is stable,
- the API is stable,
- core budgeting workflows work in the web application.

Initial implementation order:

1. Accounts
2. Income sources and paychecks
3. Recurring bills
4. Dashboard / Overview
5. Internal transfers
6. Revolving debt
7. Major loans
8. Spreadsheet import
9. Backups and packaging
10. Android application