# DSCC Django Blog

A blog application built as an exercise project for the **DSCC (Distributed Systems and Cloud Computing)** university module. This project explores building, containerising, and deploying a Django application to the cloud.

## Tech Stack

- **Backend:** Python 3.11, Django 4.2–5.0
- **Database:** PostgreSQL 15
- **Cache:** Redis 7
- **Web Server:** Gunicorn + Nginx
- **Infrastructure:** Docker, Docker Compose
- **CI/CD:** GitHub Actions → Google Cloud Run

## Features

- Blog Post model with title, content, author, timestamps, and publish toggle
- Django admin interface for content management
- Health-check endpoint (`GET /health/`)
- Production-ready security settings (HTTPS, HSTS, secure cookies)
- Static/media file serving via WhiteNoise and Nginx

## Quick Start

### Local Development

```bash
python -m venv venv
venv\Scripts\activate    # Windows
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

### With Docker

```bash
docker-compose up --build
```

The app will be available at `http://localhost:8000`.

### Environment Variables

Copy `.env.save` to `.env` and adjust as needed:

```
SECRET_KEY=your-secret-key
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1
DB_ENGINE=django.db.backends.postgresql
DB_NAME=blog_db
DB_USER=bloguser
DB_PASSWORD=blogpass123
DB_HOST=db
DB_PORT=5432
```

## Project Structure

```
dscc-django-project/
├── blog/                    # Main Django app
│   ├── models.py            # Post model
│   ├── views.py             # Health-check endpoint
│   ├── admin.py             # Admin configuration
│   └── migrations/
├── blog_project/            # Django project config
│   ├── settings.py          # Base settings
│   ├── urls.py              # URL routing
│   └── wsgi.py / asgi.py
├── settings/                # Production settings
│   └── production.py
├── nginx/                   # Nginx Docker config
├── docker-compose.yml       # Local dev services
├── docker-compose.prod.yml  # Production services
├── Dockerfile               # Dev Dockerfile
├── Dockerfile.multi         # Multi-stage prod build
├── gunicorn.conf.py         # WSGI server config
└── .github/workflows/       # CI/CD pipeline
```

## CI/CD Pipeline

On push to `main`, GitHub Actions:

1. **Tests** — runs flake8 linting and Django tests against a PostgreSQL service container
2. **Build** — builds and pushes the Docker image to Docker Hub
3. **Deploy** — deploys to Google Cloud Run via Workload Identity Federation

## Deployment

The production stack uses:
- Gunicorn behind an Nginx reverse proxy
- PostgreSQL as the database
- Docker multi-stage builds for minimal image size
- GCP Cloud Run for serverless deployment

Production settings enforce HTTPS, HSTS, and other security best practices.

## License

MIT
