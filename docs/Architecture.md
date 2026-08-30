# Architecture

## Overview

The application will use a server-authoritative architecture.

Clients do not connect directly to the database.

```text
Blazor Web
     |
     | HTTPS / application services
     v
ASP.NET Core Backend
     |
     +-- Business logic
     +-- Scheduling
     +-- Validation
     +-- Authentication
     +-- Audit history
     |
     v
Database

Default: SQLite
Optional: MariaDB