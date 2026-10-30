# Electronic-Medical-Record-App-Dockerfile
The EMRdjangoreact-dockerfile is a Dockerized deployment configuration for eClinIC an Electronic Medical Records (EMR) web application built with a Django backend and a React frontend. The setup is orchestrated via Docker Compose and comprises six services working together: the Django web server (web), an Nginx reverse proxy, a PostgreSQL 16 database (db), a PgBouncer connection pooler, a pgAdmin database management UI, and a pre-built React frontend served through Nginx as static files on port 8081. All services communicate over a dedicated internal Docker bridge network called eclicnic-network, ensuring clean isolation and service discovery between containers.

The backend is built from 

Dockerfile.backend
, based on python:3.12-slim, and installs a rich set of system libraries including libcairo2, libpango, and libgdk-pixbuf — indicating support for PDF/document generation (likely via WeasyPrint). It runs Gunicorn with 3 workers and 4 threads using the gthread worker class, and boots through a custom entrypoint.sh script that likely handles database migrations and static file collection before starting the server. The frontend, built from 

Dockerfile.frontend
, uses a multi-stage build compiling the React app with node:20-alpine and then serving the resulting dist/ output via nginx:alpine. A notable production-grade feature is the inclusion of PgBouncer in transaction pool mode, which manages up to 200 client connections with a pool size of 20 — making the stack well-optimized for handling concurrent EMR workloads efficiently.
