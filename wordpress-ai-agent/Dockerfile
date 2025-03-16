FROM python:3.10-slim

# Set working directory
WORKDIR /app

# Install system dependencies including WP-CLI
RUN apt-get update && apt-get install -y \
    curl \
    less \
    sudo \
    mariadb-client \
    && rm -rf /var/lib/apt/lists/* \
    && curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp

# Create a non-root user to run the application
RUN useradd -m -s /bin/bash appuser \
    && mkdir -p /home/appuser/backups \
    && chown -R appuser:appuser /home/appuser \
    && chown -R appuser:appuser /app

# Copy requirements file
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy source code
COPY wordpress_ai_agent/ /app/wordpress_ai_agent/
COPY config/ /app/config/
COPY main.py setup.py ./

# Create necessary directories
RUN mkdir -p data logs \
    && chown -R appuser:appuser /app

# Switch to non-root user
USER appuser

# Expose the web API port
EXPOSE 8080

# Command to run the application
CMD ["python", "main.py"]
