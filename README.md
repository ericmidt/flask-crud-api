# Flask CRUD API + Docker

The "flask_crud_api" folder contains the code for a CRUD API using the Flask framework that saves and manipulates data in a local PostgreSQL database. The "flask_app.py" program implements create, read, update, and delete operations for patients with flu symptoms.

The code is available for analysis, but to run the container, I recommend pulling the image from the [Docker Hub repository](https://hub.docker.com/r/ericmidt/flask_crud_api).

To run the container, follow these steps:
1. Make sure you have Docker installed.

2. Run the Docker Desktop application.

3. In the directory of your choice, pull the container image:
    ```bash
    docker pull ericmidt/flask_crud_api
    ```
    If there is any issue executing the command, log in to your Docker Hub account in the terminal and try again.

4. Execute the following command (don't forget to replace the credentials
   with your own to access your local PostgreSQL database):
    ```bash
    docker run -e DB_NAME="mydb" -e DB_USER="myuser" -e DB_PASSWORD="mypassword" -e DB_HOST="host.docker.internal" -e DB_PORT="your_db_port" -p 5000:5000 ericmidt/flask_crud_api:latest
    ```
    Example:
    ```bash
    docker run -e DB_NAME="postgres" -e DB_USER="postgres" -e DB_PASSWORD="12345" -e DB_HOST="host.docker.internal" -e DB_PORT="5432" -p 5000:5000 ericmidt/flask_crud_api:latest
    ```
    Usually, the default port for PostgreSQL is "5432".

    You should see the following message in your terminal:
    ```bash
    * Serving Flask app 'flask_app'
    * Debug mode: off
    WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
    * Running on all addresses (0.0.0.0)
    * Running on http://127.0.0.1:5000
    * Running on http://172.17.0.2:5000
    Press CTRL+C to quit
    ```
5. The API will be accessible at http://localhost:5000.

## Endpoints
### List Patients
Returns a list of all patients.

- URL: /patients
- Method: GET
- Response:
    - 200 OK - List of patients.

### Create Patient
Creates a new patient entry.

- URL: /patient
- Method: POST
- Request Body:
    ```json
    {
    "timestamp": "YYYY-MM-DD HH:MM:SS",
    "sex": "Gender",
    "age": 99,
    "symptoms": "Symptoms",
    "symptomsStartDate": "YYYY-MM-DD HH:MM:SS",
    "city": "City",
    "state": "State",
    "tookCovidVaccine": true
    }
    ```
- Response:
    - 200 OK - Patient created successfully.

### Update Patient
Updates an existing patient entry.

- URL: /patient/{id}
- Method: PUT
- URL Parameters:
    - id - Patient ID
- Request Body:
    ```json
    {
    "timestamp": "YYYY-MM-DD HH:MM:SS",
    "sex": "Gender",
    "age": 0,
    "symptoms": "Symptoms",
    "symptomsStartDate": "YYYY-MM-DD HH:MM:SS",
    "city": "City",
    "state": "State",
    "tookCovidVaccine": true
    }
    ```
- Response:
    - 200 OK - Patient updated successfully.
    - 404 Not Found - Patient with the specified ID not found.

### Remove Patient
Removes an existing patient entry.

- URL: /patient/{id}
- Method: DELETE
- URL Parameters:
    - id - Patient ID
- Response:
    - 200 OK - Patient removed successfully.
    - 404 Not Found - Patient with the specified ID not found.

## Tests (example requests)
### Create Patient
To create three patients, execute the following commands in your terminal:

```bash
curl -X POST -H "Content-Type: application/json" -d "{\"timestamp\": \"2023-08-18T12:00:00Z\", \"sex\": \"Female\", \"age\": 25, \"symptoms\": \"Headache, Fatigue\", \"symptomsStartDate\": \"2023-08-18T00:00:00Z\", \"city\": \"Curitiba\", \"state\": \"PR\", \"tookCovidVaccine\": true}" http://localhost:5000/patient

curl -X POST -H "Content-Type: application/json" -d "{\"timestamp\": \"2023-08-19T12:00:00Z\", \"sex\": \"Male\", \"age\": 74, \"symptoms\": \"Cough, Runny nose\", \"symptomsStartDate\": \"2023-08-18T00:00:00Z\", \"city\": \"Curitiba\", \"state\": \"PR\", \"tookCovidVaccine\": false}" http://localhost:5000/patient

curl -X POST -H "Content-Type: application/json" -d "{\"timestamp\": \"2023-08-20T12:00:00Z\", \"sex\": \"Female\", \"age\": 36
```

## Get in touch
[LinkedIn](https://www.linkedin.com/in/ericmidt/)
[GitHub](https://github.com/ericmidt)