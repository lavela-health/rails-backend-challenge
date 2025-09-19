# Lavela Health Backend Take-Home

Welcome! This repository is the starting point for a short backend exercise. The goal is to ingest third-party availability data and expose appointment booking endpoints on top of a small scheduling domain.

## Prerequisites

- Ruby 3.4.3 (see `.ruby-version`)
- Bundler (`gem install bundler` if it is missing)
- SQLite (bundled with macOS, no extra setup required)

## Getting Started

1. Fork this repo. 
1. Install dependencies: `bundle install`
1. Create your database schema: add a migration that models the scheduling domain described below, then run `bin/rails db:setup`
1. Run the test suite: `bin/rails test`

Feel free to add seed data (`db/seeds.rb`) once your models are ready—it is currently empty on purpose.

## What We Already Provide

- REST routes and empty controllers for:
  - `GET /providers/:provider_id/availabilities`
  - `POST /appointments`
  - `DELETE /appointments/:id` (bonus)
- A Calendly-like client that reads from a local fixture (`app/services/calendly_client.rb`, fixture at `spec/fixtures/calendly_slots.json`)
- A service object stub where you can implement ingest logic (`app/services/availability_sync.rb`)
- Skeleton integration tests with `skip` markers so you can focus on behaviour (`test/controllers/*`)

## What You Need to Build

1. Design the data model for four core entities: clients (people booking time), providers (people offering care), availability windows for each provider, and appointments connecting a client to a provider at a specific time. Capture enough attributes to uniquely identify each record, store contact or descriptive information as needed, track whether providers accept new clients, keep start/end timestamps, and represent appointment status (scheduled vs. canceled). Add indexes that make querying by provider and time efficient.
2. Implement the corresponding ActiveRecord models with associations, validations, and any enums or scopes you find helpful.
3. Implement `AvailabilitySync#call` to fetch slots from `CalendlyClient` and upsert `Availability` records. No external HTTP requests are needed.
4. Expose `GET /providers/:provider_id/availabilities?from=...&to=...` to return free slot windows that fall within the requested time range.
5. Build `POST /appointments` that validates the requested slot fits inside an existing availability window and does not conflict with other appointments. Persist the appointment in a scheduled state.
6. (Bonus) Implement `DELETE /appointments/:id` to soft-cancel an appointment by marking it as canceled rather than deleting it outright.

### Things to Keep in Mind

- Enforce data integrity with validations and appropriate error responses.
- Use the provided indexes guidance to keep queries fast as data grows.
- Feel free to add more tests or helper classes if they help you demonstrate your approach.
- Document any assumptions or trade-offs directly in code comments or a short write-up.

Good luck, and thanks for taking the time to work on this exercise!
