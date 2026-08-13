# Student Data ML Prediction Pipeline

A machine learning pipeline that monitors a student database for newly inserted records and automatically generates predictions using a trained **LightGBM** model.

The system continuously checks the database for new student records, processes the new data, runs inference with the trained model, and records the prediction results. Application logging is also used to monitor the pipeline and make it easier to track its execution and troubleshoot issues.

## Overview

The system is designed around a simple continuous prediction workflow:

```text
                ┌─────────────────┐
                │  Student DB     │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │ Detect New Rows  │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │ Data Processing │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │ LightGBM Model  │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │   Prediction    │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │ Logging/Monitor │
                └─────────────────┘
```

## How It Works

1. The application connects to the student database.
2. It checks for records that have not yet been processed.
3. Newly detected records are prepared for inference.
4. The trained LightGBM model generates predictions.
5. The prediction results are handled by the application.
6. The system records relevant events and errors using application logging.
7. The process continues monitoring the database for new records.

This allows predictions to be generated automatically whenever new student data becomes available without requiring the entire dataset to be processed again.

## Tech Stack

* **Python**
* **Flask** — application/API framework
* **LightGBM** — machine learning model
* **Database** — student data storage
* **Logging** — application monitoring and debugging
* **GitHub Actions** — CI/CD workflow automation

## Machine Learning

The prediction model was trained using **LightGBM**, a gradient boosting framework designed for efficient training and inference on tabular datasets.

The trained model is used for inference rather than being retrained every time a new student record is detected.

The inference workflow is:

```text
New Student Record
        │
        ▼
Feature Preparation
        │
        ▼
Trained LightGBM Model
        │
        ▼
Prediction
```

## Continuous Data Processing

Rather than loading the entire database and predicting on every record, the application keeps track of newly available data.

This makes the system suitable for scenarios where student records are continuously added to the database.

Conceptually:

```text
while application_is_running:

    check_database_for_new_rows()

    if new_rows_exist:
        prepare_data()
        generate_predictions()
        record_results()

    wait_for_next_check()
```

## Logging and Monitoring

The application includes logging to provide visibility into the prediction pipeline.

Logs can be used to track events such as:

* Application startup
* Database connection issues
* Detection of new records
* Prediction execution
* Number of records processed
* Prediction errors
* Unexpected application failures

This makes it easier to understand what the system is doing while it is running and to diagnose problems when something goes wrong.





Create a virtual environment:

```bash
python -m venv venv
```

Activate it:

### Linux/macOS

```bash
source venv/bin/activate
```

### Windows

```bash
venv\Scripts\activate
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

## Environment Variables

Create a `.env` file containing the configuration required by the application.

For example:

```env
DATABASE_URL=your_database_connection_string
```

Add any other environment variables required by the application.

Do not commit secrets or credentials to the repository.

## Running the Application

Start the Flask application using the project's configured entry point.

For example:

```bash
flask run
```

Once running, the application can connect to the database and begin processing newly available student records.

## GitHub Actions

The repository includes a **GitHub Actions workflow** for automating project tasks.

The workflow can be used to automatically run checks whenever changes are pushed to the repository or when a pull request is created.

Workflow files are located in:

```text
.github/workflows/
```

## Docker

Docker is **not currently used** in this project.

The application is intended to run directly in a Python environment with its required dependencies installed.

## Future Improvements

Potential improvements include:

* Containerizing the application with Docker
* Replacing polling with an event-driven mechanism
* Adding a dedicated task queue for predictions
* Adding a prediction-results database table
* Adding model version tracking
* Adding automated model retraining
* Adding more comprehensive monitoring and metrics
* Deploying the application to a cloud environment


