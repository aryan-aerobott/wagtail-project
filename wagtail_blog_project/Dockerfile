# Use Python 3.12 slim image
FROM python:3.12-slim-bookworm

# Create user
RUN useradd wagtail

# Environment variables
ENV PYTHONUNBUFFERED=1 \
    PORT=8080

# Set working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update --yes --quiet && apt-get install --yes --quiet --no-install-recommends \
    build-essential \
    libpq-dev \
    libmariadb-dev \
    libjpeg62-turbo-dev \
    zlib1g-dev \
    libwebp-dev \
 && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt /app/
RUN pip install --upgrade pip && pip install -r requirements.txt gunicorn

# Copy project
COPY . /app/

# Set ownership
RUN chown -R wagtail:wagtail /app

# Switch user
USER wagtail

# Collect static
RUN python manage.py collectstatic --noinput --clear

# Start app (Cloud Run FIXED)
CMD ["gunicorn", "config.wsgi:application", "--bind", "0.0.0.0:8080", "--workers", "2", "--threads", "8", "--timeout", "0"]
