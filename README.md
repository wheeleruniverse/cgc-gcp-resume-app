# GCP Resume App - Backend API

This repository contains the backend API for the CGC GCP Resume App, originally created for the [A Cloud Guru Community Challenge](https://www.pluralsight.com/resources/blog/cloud/cloudguruchallenge-your-resume-on-gcp).

The application is a containerized Flask API deployed on Google Cloud Run. Its primary purpose is to provide a simple visitor counter service that interacts with a Google Firestore database.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [API Endpoints](#api-endpoints)
  - [GET /api/visitors](#get-apivisitors)
  - [POST /api/visitors](#post-apivisitors)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Deployment](#deployment)
- [Local Development & Testing](#local-development--testing)
- [Project Structure](#project-structure)
- [License](#license)

## Overview

The backend is a standalone REST API built with Python and the Flask framework. It is designed to be built into a Docker container and deployed as a serverless service on Google Cloud Run. The API exposes endpoints to get and increment a visitor count, which is persisted in a Google Firestore database.

The project also includes a Swagger UI interface for easy interaction and testing of the API endpoints, defined by an OpenAPI specification.

## Architecture

The backend infrastructure is composed of the following services:

* **Google Cloud Run:** Hosts the containerized Flask application, providing a fully managed, serverless environment.
* **Google Cloud Build:** A CI/CD pipeline, defined in `cloudbuild.yml`, automatically builds the Docker image and deploys it to Cloud Run.
* **Google Container Registry (or Artifact Registry):** Stores the Docker images built by Cloud Build.
* **Google Cloud Firestore:** A NoSQL database used to persist the visitor count.
* **Docker:** Used to containerize the Flask application, ensuring a consistent runtime environment.
* **Flask & Gunicorn:** The web framework used to build the API and the WSGI server to run it in production.
* **Swagger/OpenAPI:** Provides API documentation and an interactive UI.

## API Endpoints

The core of the backend is a single resource with two methods, defined in `static/api.json`.

### GET /api/visitors

* **Description:** Retrieves the current visitor count.
* **Method:** `GET`
* **Response:**
    * `200 OK`
    * **Body:** `{"visitor_count": <integer>}`
* **Example:**
    ```json
    {
      "visitor_count": 1234
    }
    ```

### POST /api/visitors

* **Description:** Increments the visitor count by one and returns the new count.
* **Method:** `POST`
* **Response:**
    * `200 OK`
    * **Body:** `{"visitor_count": <integer>}`
* **Example:**
    ```json
    {
      "visitor_count": 1235
    }
    ```

## Getting Started

### Prerequisites

* [Google Cloud SDK](https://cloud.google.com/sdk/docs/install) installed and configured.
* A Google Cloud Project with billing enabled.
* [Docker](https://docs.docker.com/get-docker/) installed locally.

### Deployment

The application is designed to be deployed via Google Cloud Build.

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/wheeleruniverse/cgc-gcp-resume-app.git](https://github.com/wheeleruniverse/cgc-gcp-resume-app.git)
    cd cgc-gcp-resume-app
    ```

2.  **Configure Cloud Build:** The `cloudbuild.yml` file is configured to build the Docker image and deploy it to Cloud Run. You may need to adjust project-specific variables within the file.

3.  **Submit the build:**
    ```bash
    gcloud builds submit --config cloudbuild.yml .
    ```
    This command will trigger the build and deployment process defined in the configuration file.

## Local Development & Testing

You can run the Flask application locally for development and testing.

1.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

2.  **Set Environment Variables:** The application requires credentials to connect to Firestore. Set the `GCP_CREDENTIALS` environment variable with your service account key.
    ```bash
    # Example for Linux/macOS
    export FLASK_APP=index.py
    export GCP_CREDENTIALS='{"type": "service_account", ...}'
    ```

3.  **Run the Flask development server:**
    ```bash
    flask run
    ```
    This will start a local server, typically on `http://127.0.0.1:5000`.

## Project Structure

```
.
├── Dockerfile              \# Defines the container image for the Flask app
├── README.md               \# This file
├── cloudbuild.yml          \# Configuration for Google Cloud Build
├── index.py                \# Main Flask application file with API logic
├── requirements.txt        \# Python package dependencies
├── static                  \# Static assets for Swagger UI
│   ├── api.json            \# The OpenAPI specification file
│   ├── css/                \# CSS for Swagger UI
│   ├── img/                \# Favicon images
│   └── js/                 \# JavaScript for Swagger UI
└── templates
└── swagger.html        \# HTML template that renders the Swagger UI
```

## License

This project is licensed under the MIT License.
