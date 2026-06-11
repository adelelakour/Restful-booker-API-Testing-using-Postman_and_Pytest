# Restful Booker API Testing

This repository contains API tests for the public [Restful Booker](https://restful-booker.herokuapp.com) service using two complementary approaches:

- `Postman` for exploratory, manual, negative, edge-case, and collection-based API validation
- `Pytest` for automated regression checks and schema validation in Python

The project focuses on core booking workflows, authentication, health checks, and response contract validation against the live API.

## Project Overview

The test assets in this repository validate the main behavior of the Restful Booker API, including:

- service availability
- authentication token generation
- booking creation, retrieval, update, partial update, and deletion
- JSON schema validation for selected responses
- negative and boundary scenarios in Postman

Because the target system is a live public API, test results depend on the current availability and behavior of `https://restful-booker.herokuapp.com`.

## Tech Stack

- `Postman` for API collections and manual/collection runner execution
- `Python 3` for automated API testing
- `pytest` for test discovery and execution
- `requests` for HTTP calls
- `jsonschema` for schema validation

## Folder Structure

```text
.
├── documents/
│   └── api-observations.md
├── postman/
│   └── collections/
│       └── Restful-booker-API-testing.postman_collection.json
├── pytest/
│   ├── conftest.py
│   ├── pytest.ini
│   ├── README.md
│   ├── requirements.txt
│   └── tests/
│       ├── Booking_CRUD_Flow/
│       │   └── test_CRUD.py
│       ├── Schema_Validation/
│       │   └── test_schema_validation.py
│       └── Smoke_Tests/
│           └── test_if_system_alive.py
├── reports/
│   └── report.html
└── README.md
```

## Test Coverage

### Postman Collection Coverage

The Postman collection is organized into the following folders:

- `Smoke Tests`
  - `GET /ping`
  - `GET /booking`
- `Booking CRUD Flow`
  - `POST /auth`
  - `GET /booking`
  - `GET /booking/{id}`
  - `POST /booking`
  - `PUT /booking/{id}`
  - `PATCH /booking/{id}`
  - `DELETE /booking/{id}`
- `Auth`
  - `POST /auth` with valid and invalid credential scenarios
- `Negative Tests`
  - invalid booking lookup
  - update without authentication
  - delete without authentication
  - invalid booking payload
  - invalid login credentials
- `Schema Validation`
  - token response validation
  - booking creation response validation
  - booking retrieval response validation
  - booking update response validation
- `Edge & Boundary Case Tests`
  - negative price
  - zero price
  - hyphenated names
  - blank and whitespace-only names
  - very long names
  - old check-in dates
  - check-in after checkout
  - invalid date formats

### Pytest Coverage

The automated Pytest suite currently covers:

- `Smoke_Tests/test_if_system_alive.py`
  - `GET /ping`
  - `GET /booking`
- `Booking_CRUD_Flow/test_CRUD.py`
  - `GET /booking`
  - `GET /booking/{id}`
  - `PUT /booking/{id}`
  - `PATCH /booking/{id}`
  - `DELETE /booking/{id}`
  - booking creation via shared fixture in `conftest.py`
- `Schema_Validation/test_schema_validation.py`
  - `POST /auth`
  - `POST /booking`
  - `GET /booking/{id}`

## API Endpoints Covered

Across Postman and Pytest, the project exercises these API endpoints:

- `POST /auth`
- `GET /ping`
- `GET /booking`
- `GET /booking/{id}`
- `POST /booking`
- `PUT /booking/{id}`
- `PATCH /booking/{id}`
- `DELETE /booking/{id}`

## Installation

### Python Dependencies

Create a virtual environment and install the Pytest dependencies from the `pytest` folder:

```bash
cd pytest
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

On Windows PowerShell:

```powershell
cd pytest
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### Postman Setup

No code installation is required for the Postman assets. Import the collection below into Postman:

- `postman/collections/Restful-booker-API-testing.postman_collection.json`

## How to Run Tests

### Run Pytest Suite

From the `pytest` directory:

```bash
pytest
```

The suite uses `pytest.ini` with verbose output enabled by default.

You can also run specific test groups:

```bash
pytest tests/Smoke_Tests/
pytest tests/Booking_CRUD_Flow/
pytest tests/Schema_Validation/
```

### Run Postman Collection

1. Open Postman.
2. Import `postman/collections/Restful-booker-API-testing.postman_collection.json`.
3. Run the full collection or selected folders from the Collection Runner.

## Notes

- The tests run against the live public Restful Booker environment.
- Authentication in the Pytest suite uses the sample credentials defined in `pytest/conftest.py`.
- Some tests create, update, and delete real booking records during execution.
- `documents/api-observations.md` records observed API validation issues found during testing.
