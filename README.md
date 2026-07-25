# Password Manager (Flask API)

## Project Title and Description

This is a small Flask web application that acts as a basic in-memory password
manager. You can save a username and password through one endpoint, retrieve
the stored password by username through another, and delete a stored user
through a third. There is no database — everything is kept in a Python
dictionary in memory, so data resets every time the server restarts. This
project was built to practice REST API design in Flask and a proper Git
branching workflow (dev → main).

## Installation and Setup Steps

1. Clone the repository:
   ```
   git clone https://github.com/<your-username>/<your-repo>.git
   ```
2. Move into the project folder:
   ```
   cd <your-repo>
   ```
3. (Optional but recommended) Create and activate a virtual environment:
   ```
   python3 -m venv venv
   source venv/bin/activate      # on Windows: venv\Scripts\activate
   ```
4. Install the dependencies:
   ```
   pip install -r requirements.txt
   ```
5. Run the app:
   ```
   python app.py
   ```
6. The server will start at `http://localhost:5000/`.

## API Endpoint Reference

| Endpoint          | Method | Description                                    | Example Response |
|-------------------|--------|-------------------------------------------------|-------------------|
| `/`               | GET    | Welcome message                                 | `Welcome to the App` |
| `/health`         | GET    | Health check                                    | `App is running` |
| `/add`            | POST   | Adds a new username/password pair. Body: `{"username": "alice", "password": "secret123"}` | `{"message": "User 'alice' added successfully"}` |
| `/get/<username>` | GET    | Retrieves the stored password for a username    | `{"username": "alice", "password": "secret123"}` (or `{"error": "User 'bob' not found"}` with a 404 if the user doesn't exist) |
| `/delete/<username>` | GET or DELETE | Removes a stored user (added in Version 2) | `{"message": "User 'alice' deleted successfully"}` (or `{"error": "User 'alice' not found"}` with a 404 if the user doesn't exist) |

### Testing with curl

```bash
# Add a user
curl -X POST http://localhost:5000/add -H "Content-Type: application/json" -d "{\"username\": \"alice\", \"password\": \"secret123\"}"

# Get a user's password
curl http://localhost:5000/get/alice

# Delete a user
curl -X DELETE http://localhost:5000/delete/alice
```

`/add` must be a POST request (not GET) because it creates/changes data on
the server. GET requests are meant to only retrieve data and shouldn't have
side effects, and GET request parameters/URLs can also end up logged or
cached — which would be unsafe for something like a password.

## Git Workflow

All development work happened on a `dev` branch, never directly on `main`.
The flow for each version was:

1. Work on the feature inside the `dev` branch.
2. Commit the changes with a descriptive message.
3. Push `dev` to GitHub.
4. Once the feature was tested and working, merge `dev` into `main`.
5. Push `main` to GitHub.

This kept `main` always in a stable, working state, while `dev` was where
active changes and testing happened.

```
main:  ---o------------o----------->  (stable releases only)
            \          /            \
dev:         o---o----o  (v1)         o----o  (v2: adds /delete)
```

## Version History

| Version | What's Included |
|---------|-----------------|
| Version 1 | `/` and `/health` endpoints, plus `/add` (POST) and `/get/<username>` (GET) for the password manager |
| Version 2 | Added `/delete/<username>` endpoint to remove a stored user, built on top of Version 1 |

## Screenshots

> Add the following screenshots here before submitting:
> 1. The app running in a browser showing a working endpoint (e.g. `/health`)
> 2. The GitHub repo page showing both the `dev` and `main` branches
> 3. The commit/merge history showing the Version 1 and Version 2 merges
