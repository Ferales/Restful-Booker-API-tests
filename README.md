# Restful Booker API Test Automation

API test automation project for the **Restful Booker** public API built with **Postman**, **Postman CLI**, and **GitHub Actions**.

The project demonstrates a complete API testing workflow including functional testing, negative scenarios, data-driven testing, end-to-end validation, and Continuous Integration.

---

## Features

- Functional API testing
- Positive and negative test scenarios
- Data-driven authentication tests
- End-to-end Happy Path workflow
- Collection and environment variables
- Dynamic token handling
- Validation of multiple request body formats (JSON, XML and `application/x-www-form-urlencoded`)
- GitHub Actions CI pipeline
- HTML test report generation

---

## Tech Stack

- Postman
- Postman CLI
- JavaScript (Postman Scripts)
- GitHub Actions
- Bash (CI automation)

---

## Test Coverage

### Health Check

- Verify API availability (`GET /ping`)

### Authentication

- Valid login
- Invalid credentials (data-driven)
- Empty credentials (data-driven)
- Missing request fields (data-driven)

### Booking

The project covers both public and authenticated Booking endpoints.

Tests validate:

- both supported authentication methods (Basic Authentication and Token Cookie Authentication)
- all supported request body formats (JSON, XML and `application/x-www-form-urlencoded`)
- positive and negative scenarios

#### Public endpoints

- Get Booking
- Get Booking IDs
- Create Booking

#### Authenticated endpoints

- Update Booking (PUT)
- Partial Update Booking (PATCH)
- Delete Booking

### End-to-End

The project contains an end-to-end Happy Path scenario covering the complete booking lifecycle:

1. Login
2. Create Booking
3. Retrieve created booking
4. Update booking
5. Verify updated booking
6. Delete booking
7. Verify booking has been removed

---

## Project Structure

- **postman/collections/** – API test collections organized by feature
  - Authentication
  - Booking
  - Health Check
  - E2E
- **postman/environments/** – environment configuration
- **postman/globals/** – global variables
- **data/** – JSON datasets used for data-driven authentication tests
- **.github/workflows/** – GitHub Actions CI pipeline

---

# Running Tests Locally

## Prerequisites

Install the Postman CLI.

Official installation guide:

https://learning.postman.com/docs/postman-cli/postman-cli-installation/

---

## Configure Credentials

The repository does **not** contain authentication credentials.

Before running authenticated tests, provide valid Restful Booker credentials using **one** of the following methods.

### Option 1 – Configure the Postman Environment (recommended for local development)

Edit the environment file:

```text
postman/environments/Test.environment.yaml
```

and provide values for:

```yaml
username: <your_username>
password: <your_password>
```

### Option 2 – Pass Credentials via Postman CLI (recommended for CI/CD)

Override the environment variables directly from the command line:

```bash
--env-var "username=<your_username>"
--env-var "password=<your_password>"
```

For example:

```bash
postman collection run "postman/collections/Restful Booker API Tests" \
-e "postman/environments/Test.environment.yaml" \
--env-var "username=<your_username>" \
--env-var "password=<your_password>"
```

Command-line variables override values defined in the environment file. This approach is used by the GitHub Actions workflow to inject credentials securely from GitHub Secrets.

---

## Run Complete Test Suite

Using credentials stored in the environment file:

```bash
postman collection run "postman/collections/Restful Booker API Tests" \
-e "postman/environments/Test.environment.yaml"
```

Or override them from the command line:

```bash
postman collection run "postman/collections/Restful Booker API Tests" \
-e "postman/environments/Test.environment.yaml" \
--env-var "username=<your_username>" \
--env-var "password=<your_password>"
```

---

## Run Data-Driven Authentication Tests

### Missing Fields

```bash
postman collection run "postman/collections/Restful Booker API Tests" \
-e "postman/environments/Test.environment.yaml" \
-i "Authentication/POST Login - Missing Fields" \
-d "data/auth/missing-fields.json"
```

### Invalid Credentials

```bash
postman collection run "postman/collections/Restful Booker API Tests" \
-e "postman/environments/Test.environment.yaml" \
-i "Authentication/POST Login - Invalid Credentials" \
-d "data/auth/invalid-credentials.json"
```

### Empty Credentials

```bash
postman collection run "postman/collections/Restful Booker API Tests" \
-e "postman/environments/Test.environment.yaml" \
-i "Authentication/POST Login - Empty Credentials" \
-d "data/auth/empty-credentials.json"
```

> If your environment file does not contain credentials, append the `--env-var` parameters shown above.

---

## Continuous Integration

The project includes a GitHub Actions workflow that automatically:

- executes the complete test suite on every push
- validates pull requests
- supports manual execution (`workflow_dispatch`)
- executes scheduled runs
- generates an HTML execution report
- uploads the report as a workflow artifact

Authentication credentials used in CI are stored securely as **GitHub Secrets**.