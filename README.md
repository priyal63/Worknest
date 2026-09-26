# Worknest

> Find a trusted worker for any job at home.

Worknest is a robust platform designed to connect users with verified local specialists such as plumbers, electricians, house maids, home cooks, and carpenters. It streamlines the process of discovering nearby professionals, booking services, and managing jobs, while offering specialized dashboards for customers, workers, and administrators.

## Features

* **Role-Based Access Control:** Distinct interfaces and permissions for Users, Workers, and Administrators.
* **Worker Discovery & Geolocation:** Browse workers by service category and discover nearby specialists using Haversine-based distance calculations.
* **Booking Management System:** Send job requests, track job statuses (pending, accepted, in progress, completed), and record job duration.
* **Trust & Verification:** Workers can upload ID proofs which are verified by administrators before they appear in public listings.
* **Rating & Feedback:** Users can rate workers (1-5 stars) and leave reviews upon job completion, updating the worker's overall rating.
* **Admin Dashboard:** Comprehensive tools to monitor platform statistics, verify worker identities, flag fraudulent accounts, and manage bookings.
* **Notification System:** Users and workers receive status alerts and administrative messages.

## Tech Stack

### Frontend
* **UI Structure:** HTML5
* **Styling:** Tailwind CSS (via CDN)
* **Logic:** Vanilla JavaScript
* **Typography & Icons:** Manrope Font, Material Symbols

### Backend
* **Core:** Python, Django
* **API:** Django REST Framework, django-cors-headers

### Database
* **Primary Database:** PostgreSQL

### Architecture
The project follows a monolithic Django architecture. The Django backend handles both rendering the frontend HTML templates and exposing a RESTful JSON API (via the `/api/` routing). The frontend interfaces asynchronously communicate with these API endpoints using JavaScript.

## Project Structure

```text
Worknest/
├── frontend/             # Main landing page assets
├── frontend_admin/       # Admin dashboard HTML/CSS
├── frontend_user/        # Consumer-facing HTML/CSS
├── frontend_worker/      # Worker-facing HTML/CSS
├── index.html            # Primary entry point template
├── logo.png              # Application logo
└── worknest_backend/     # Django Backend Core
    └── backend/
        ├── api/          # Main application (Models, Views, URLs, Serializers)
        ├── backend/      # Django Project settings and core routing
        ├── media/        # Uploaded files (Profile pics, ID proofs)
        └── manage.py     # Django execution script
```

## Installation

### 1. Prerequisites
* Python 3.8+
* PostgreSQL
* Git

### 2. Clone the Repository
```bash
git clone <your-repository-url>
cd Worknest
```

### 3. Database Setup
Create a PostgreSQL database for the project. By default, the project expects:
* **Database Name:** `worknest_db`
* **User:** `postgres`
* **Password:** `2007` (This must be updated in `settings.py` based on your local pgAdmin setup)
* **Port:** `5432`

### 4. Install Dependencies
Navigate to the backend directory and install the required Python packages:
```bash
cd worknest_backend/backend
pip install django djangorestframework django-cors-headers psycopg2-binary Pillow
```
*(Note: Use `psycopg2-binary` if you encounter build errors on Windows/macOS).*

### 5. Run Database Migrations
```bash
python manage.py makemigrations
python manage.py migrate
```

### 6. Create a Superuser (Admin)
```bash
python manage.py createsuperuser
```
Follow the prompts to set up your administrator account.

### 7. Start the Server
```bash
python manage.py runserver
```
The application will now be running at `http://localhost:8000/`.

## Environment Variables

Currently, the project directly manages its configuration within `worknest_backend/backend/backend/settings.py`. However, if you plan to deploy or secure the app, you should extract the following variables into an environment configuration:

```env
# Database Credentials
DB_NAME=worknest_db
DB_USER=postgres
DB_PASSWORD=your_database_password
DB_HOST=localhost
DB_PORT=5432

# Django Security
SECRET_KEY=django-insecure-your-secret-key-here
DEBUG=True
```

## Usage

1. **Access the App:** Open a browser and navigate to `http://localhost:8000/`.
2. **User Workflow:** Click "Get Started" to register as a regular user. You can browse categories, view workers on the map, and send booking requests.
3. **Worker Workflow:** Navigate to the "Join as Worker" section to create a specialist profile. Upload an ID proof and await admin verification. Once verified, you will appear in the public listings.
4. **Admin Workflow:** Log in with your superuser credentials and navigate to the admin dashboard (`/admin-dashboard/`) to verify pending workers, view platform statistics, and manage bookings.

## API Documentation

The backend exposes several REST endpoints under the `/api/` base URL:

### Authentication
* `POST /api/signup/` - Register a new user or worker (handles ID proof uploads).
* `POST /api/login/` - Authenticate a user and create a session.
* `POST /api/logout/` - Terminate the active session.

### Workers & Categories
* `GET /api/categories/` - Fetch all unique service categories currently offered by verified workers.
* `GET /api/workers/` - Retrieve a list of verified workers.
* `GET /api/workers/<worker_id>/` - Retrieve detailed information for a specific worker.

### Bookings
* `GET /api/bookings/` - Retrieve all bookings for the authenticated user/worker.
* `POST /api/bookings/rate/` - Submit a star rating and review for a completed job.

### Administration
* `GET /api/admin/unverified/` - Retrieve a list of workers awaiting ID verification.
* `POST /api/admin/verify/<worker_id>/` - Approve a worker's identity proof.
* `POST /api/admin/fraud/<worker_id>/` - Flag a worker's account for fraudulent activity.
* `GET /api/admin/stats/` - Retrieve overarching platform statistics (total users, active jobs, etc.).

## Security

The project incorporates the following security mechanisms natively:
* **CSRF Protection:** Django's built-in Cross-Site Request Forgery middleware protects template-rendered forms.
* **CORS:** Configured via `django-cors-headers` to restrict cross-origin requests.
* **Authentication:** Password hashing and session-based validation using Django's authentication backends.
* **Role Verification:** Endpoints enforce logical checks (e.g., verifying if the requesting user has Admin privileges before executing verification endpoints).

## Troubleshooting

* **Database Connection Failed:** Ensure your PostgreSQL service is running and that the credentials (especially the password) in `backend/settings.py` match your local database exactly.
* **Images/ID Proofs not loading:** The project saves uploads to the `media/` directory. Ensure `MEDIA_URL` and `MEDIA_ROOT` are correctly configured in `settings.py` and that `DEBUG = True` is set for local development, as Django statically serves media files only in debug mode.
* **CORS Errors:** If you test the frontend independently from a different port (e.g., via Live Server on port 5500), ensure that your origin is included in the `CORS_ALLOWED_ORIGINS` list in `settings.py`.

## Future Improvements

* **Environment Variable Integration:** Move hardcoded database credentials and the Django `SECRET_KEY` into a `.env` file using `python-dotenv`.
* **JWT Authentication:** Transition from session-based auth to JSON Web Tokens (JWT) for a more decoupled API architecture.
* **Automated Testing:** Introduce unit and integration testing via `pytest` or Django's `TestCase`.
* **Containerization:** Add a `Dockerfile` and `docker-compose.yml` to simplify database and application deployment.

## Contributing

1. Fork the repository
2. Create a new branch (`git checkout -b feature/your-feature-name`)
3. Make your changes and commit them (`git commit -m 'Add new feature'`)
4. Test your changes locally to ensure no existing functionality breaks
5. Push to the branch (`git push origin feature/your-feature-name`)
6. Create a Pull Request

## License

License: Not specified
