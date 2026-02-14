# Car Dealership Platform

A full-stack web application for managing car dealerships and reviews. This project includes a Django backend, a React frontend, a Node.js/Express service for dealership data, and a Python/Flask microservice for sentiment analysis.

## Architecture

The application is composed of four main services:

1.  **Django App** (`server/djangoapp`): The main web application that serves the frontend and handles user authentication and business logic.
2.  **React Frontend** (`server/frontend`): A Single Page Application (SPA) built with React, which is built into static files and served by the Django backend.
3.  **Dealerships & Reviews API** (`server/database`): A Node.js and Express service that manages dealership and review data, backed by a MongoDB database.
4.  **Sentiment Analyzer** (`server/djangoapp/microservices`): A Python and Flask microservice that performs sentiment analysis on reviews.

## Prerequisites

Before running the application, ensure you have the following installed:

-   **Python 3.8+**
-   **Node.js 18+** and **npm**
-   **Docker** (Recommended for running the database and Node.js service)

## Setup & Running

It is recommended to run the services in the following order:

### 1. Database & Dealerships API (Node.js/Mongo)

The easiest way to run the MongoDB database and the Dealerships API is using Docker Compose.

1.  Open a terminal and navigate to the database directory:
    ```bash
    cd server/database
    ```

2.  Build the Docker image for the Node.js application:
    ```bash
    docker build . -t nodeapp
    ```

3.  Start the services using Docker Compose:
    ```bash
    docker-compose up
    ```
    *This will start MongoDB on port `27017` and the Node.js API on port `3030`.*

### 2. Sentiment Analyzer (Flask)

1.  Open a new terminal window and navigate to the microservices directory:
    ```bash
    cd server/djangoapp/microservices
    ```

2.  (Optional) Create and activate a Python virtual environment:
    ```bash
    python3 -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```

3.  Install the required dependencies:
    ```bash
    pip install -r requirements.txt
    ```

4.  Run the Flask application on port `5050`:
    ```bash
    export FLASK_APP=app.py
    flask run --port=5050
    ```
    *Note: If you encounter an error regarding NLTK data, you may need to run the following python command:*
    ```bash
    python -m nltk.downloader vader_lexicon
    ```

### 3. Frontend (React)

The React frontend needs to be built so that the Django backend can serve the static files.

1.  Open a new terminal window and navigate to the frontend directory:
    ```bash
    cd server/frontend
    ```

2.  Install the Node dependencies:
    ```bash
    npm install
    ```

3.  Build the project:
    ```bash
    npm run build
    ```
    *This will generate the production build of the React app in the `build` directory.*

### 4. Main Application (Django)

1.  Open a new terminal window and navigate to the root server directory:
    ```bash
    cd server
    ```

2.  (Optional) Create and activate a Python virtual environment (if you haven't already for the Flask app, or use a separate one):
    ```bash
    python3 -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```

3.  Install the required dependencies:
    ```bash
    pip install -r requirements.txt
    ```

4.  **Configuration**:
    The Django application uses environment variables for configuration.

    -   Locate the `.env` file in `djangoapp/.env`.
    -   Open it and ensure `backend_url` points to `http://localhost:3030` and `sentiment_analyzer_url` points to `http://localhost:5050/`.
    -   Alternatively, you can rename or delete this file, and the application will default to the localhost URLs defined in the code.

    Example `.env` for local development:
    ```
    backend_url=http://localhost:3030
    sentiment_analyzer_url=http://localhost:5050/
    ```

5.  Run the database migrations:
    ```bash
    python manage.py migrate
    ```

6.  Start the Django development server:
    ```bash
    python manage.py runserver
    ```

## Accessing the Application

Once all services are running, you can access the full application by opening your web browser and navigating to:

**http://localhost:8000**

-   **Admin Panel**: http://localhost:8000/admin
-   **Dealerships API**: http://localhost:3030
-   **Sentiment Analyzer**: http://localhost:5050
