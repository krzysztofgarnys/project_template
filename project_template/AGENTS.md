# Project Instructions

## System

This repository contains a .NET backend application.

Before making changes, inspect the existing solution and follow its architecture.

Do not introduce a new architecture style unless explicitly justified.

## Architecture

Expected dependency direction:

Api
  ↓
Application
  ↓
Domain

Infrastructure implements interfaces required by Application or Domain.

Domain must not depend on Infrastructure.

Business logic must not live in controllers.

## Technology

Backend:
- C#
- .NET
- ASP.NET Core
- Entity Framework Core

Database:
- SQL Server / PostgreSQL

Infrastructure:
- Azure
- Azure Service Bus where applicable

## Development workflow

Before implementation:

1. Find all relevant code.
2. Explain current behaviour.
3. Identify affected components.
4. Propose implementation plan.
5. Identify risks.

Only then implement.

After implementation run:

dotnet build
dotnet test

Do not claim the task is complete when tests fail.

## Database

Be careful with:

- schema migrations
- production migrations
- indexes
- transaction boundaries
- concurrency
- query performance

Never modify an already deployed migration without explicit approval.

## Messaging

Message consumers should be idempotent.

When database changes and message publishing belong to the same logical operation,
consider Transactional Outbox.

Assume messages may:

- arrive more than once
- arrive late
- arrive out of order

## Security

Never expose:

- secrets
- passwords
- connection strings
- access tokens

Never log sensitive authentication data.

## Scope control

Do not refactor unrelated code.

Do not add dependencies without explaining why they are needed.

Prefer using existing project conventions.