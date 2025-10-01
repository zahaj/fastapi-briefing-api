[![Run Python Tests](https://github.com/zahaj/fastapi-briefing-api/actions/workflows/ci.yml/badge.svg)](https://github.com/zahaj/fastapi-briefing-api/actions/workflows/ci.yml)

# Daily Briefing API

A containerized, tested, and authenticated backend service built with Python and FastAPI. This project serves as a comprehensive demonstration of professional backend development practices, from initial design to automated CI/CD deployment.

## Features

- **RESTful API**: A clean, documented API built with FastAPI for generating user briefings.
- **Authentication**: Secure endpoints using JWT token-based authentication (OAuth2 Password Flow).
- **Database Integration**: Persistent data storage using PostgreSQL, managed with SQLAlchemy ORM.
- **Containerization**: Fully containerized with Docker and Docker Compose for consistent development and deployment environments.
- **Automated Testing**: A robust test suite using `pytest` as a runner for tests written with both unittest and pytest conventions, including isolated unit tests and end-to-end integration tests.
- **CI/CD Pipeline**: A complete CI pipeline with GitHub Actions that automatically builds, tests, and validates the application on every push.
- **Command-Line Interface**: A user-friendly CLI built with Typer for interacting with the application directly from the terminal.

## Tech Stack

- **Backend**: Python, FastAPI
- **Database**: PostgreSQL, SQLAlchemy
- **Containerization**: Docker, Docker Compose
- **Testing**: Pytest, unittest, HTTPX
- **CI/CD**: GitHub Actions

## Project Structure

```
.
├── .github/
│   └── workflows/
│       └── ci.yml              # GitHub Actions workflow for Continuous Integration
├── src/
│   └── daily_briefing/         # The main installable Python package
│       ├── __init__.py
│       ├── api_interactions.py # Client for the JSONPlaceholder external API
│       ├── auth.py             # Authentication (JWT, password hashing) logic
│       ├── config_reader.py    # Utility for reading configuration
│       ├── daily_briefing_app.py # Core application business logic
│       ├── database.py         # SQLAlchemy models and database connection
│       ├── main.py             # Typer CLI application entry point
│       ├── models.py           # Pydantic models for data validation and serialization
│       └── web_api.py          # FastAPI application entry point
├── tests/                      # Unit and integration tests
│   ├── __init__.py
│   ├── conftest.py             # Pytest fixtures and configuration
│   ├── test_api_interactions.py
│   ├── test_daily_briefing_app.py
│   ├── test_main_cli.py
│   ├── test_web_api_integration.py
│   ├── test_web_api_unit.py
│   └── test_weather_client.py
├── .dockerignore               # Specifies files to ignore in the Docker build context
├── .gitignore                  # Specifies files for Git to ignore
├── config.ini.example          # Example configuration file
├── docker-compose.yml          # Docker Compose configuration for multi-container setup
├── Dockerfile                  # Docker configuration for building the API image
├── pyproject.toml              # Project definition, dependencies, and packaging
└── README.md                   # This file
```

## Getting Started

These instructions will get you a copy of the project up and running on your local machine.

### Prerequisites

You must have [Docker](https://www.docker.com/products/docker-desktop/) and Docker Compose installed.

### Installation and Setup

1.  **Clone the Repository**
    ```
    git clone https://github.com/zahaj/fastapi-briefing-api.git
    cd fastapi-briefing-api
    ```

2.  **Configuration**
    The application requires an API key for the weather service. Create a `config.ini` file in the project root from the provided example.

    ```
    cp config.ini.example config.ini
    ```

    Now, open the newly created config.ini file and add your real API key from OpenWeatherMap.
    
### Running the Application (Docker)

The primary way to run this application is with Docker Compose. This single command will build the image, start the API and database containers, and connect them.
```
docker compose up --build
```

The API will be available at http://127.0.0.1:8000, and the interactive documentation can be accessed at http://127.0.0.1:8000/docs.

### Running the Test Suite

The integration tests require the Docker containers to be running. In a **separate terminal**, run the following commands:

1. Create a virtual environment:

    ```
    python -m venv .venv
    ```

    and activate it:

    On macOS/Linux:
    ```
    source .venv/bin/activate
    ```

    On Windows (Command Prompt):
    ```
    .venv\Scripts\activate.bat
    ```

    On Windows (PowerShell):
    ```
    .\.venv\Scripts\Activate.ps1
    ```

    Note for PowerShell users: If you encounter an error about scripts being disabled, you may need to adjust your execution policy by running PowerShell as an administrator and executing the command: Set-ExecutionPolicy RemoteSigned.

2. Install dependencies (including test libraries):

    ```
    pip install -e ".[test]"
    ```

3. Run pytest:

    ```
    pytest
    ```

## Usage

### API Usage Example

1. Get an Authentication Token:

    - Navigate to the interactive docs at `http://127.0.0.1:8000/docs`.

    - Click "Authorize" and enter the test credentials (`testuser` / `testpassword`).

2. Call a Protected Endpoint:

    - Go to the `GET /logs` endpoint.

    - Click "Try it out" and "Execute". You will get a `200 OK` response.

### CLI Usage Example

First, install the project in your virtual environment (`pip install -e .`).

1. Get a Briefing:
- you can call the module directly:

    ```
    python -m daily_briefing.main get-briefing 1 --city "Wrocław"
    ```
- or use the installed script entry point:

    ```
    briefing get-briefing 1 --city "London"
    ```

2. Check Configuration:
    ```
    briefing check-config
    ```