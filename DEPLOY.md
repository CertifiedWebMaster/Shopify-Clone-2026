# Deployment Guide

This guide provides instructions for deploying the Shopify Clone application using various methods.

## Table of Contents
- [Docker Deployment](#docker-deployment)
- [Docker Compose (Recommended for Development)](#docker-compose-recommended-for-development)
- [Manual PHP Deployment](#manual-php-deployment)
- [CI/CD with GitHub Actions](#cicd-with-github-actions)
- [Production Considerations](#production-considerations)

## Prerequisites

- Docker (version 20.10 or higher)
- Docker Compose (version 2.0 or higher)
- PHP 8.1+ (for manual deployment)
- MySQL 8.0+ (for manual deployment)
- Apache or Nginx web server (for manual deployment)

## Docker Deployment

### Quick Start

Build and run the Docker container:

```bash
# Build the Docker image
docker build -t shopify-clone .

# Run the container
docker run -d -p 8080:80 --name shopify-clone shopify-clone

# Access the application
open http://localhost:8080
```

### Stop and Remove

```bash
docker stop shopify-clone
docker rm shopify-clone
```

## Docker Compose (Recommended for Development)

Docker Compose provides a complete development environment with the application, database, and phpMyAdmin.

### Start Services

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Access services:
# - Application: http://localhost:8080
# - phpMyAdmin: http://localhost:8081
```

### Stop Services

```bash
# Stop all services
docker-compose down

# Stop and remove volumes (WARNING: This deletes database data)
docker-compose down -v
```

### Database Configuration

The Docker Compose setup includes a MySQL database with the following credentials:

- **Host**: db
- **Port**: 3306
- **Database**: shopify_clone
- **User**: shopify_user
- **Password**: shopify_pass
- **Root Password**: root_password

Update `config.php` to use these credentials when running with Docker Compose.

## Manual PHP Deployment

### Requirements

- PHP 8.1 or higher
- Apache with mod_rewrite enabled
- MySQL 8.0 or higher
- Required PHP extensions:
  - pdo_mysql
  - mysqli
  - mbstring
  - gd
  - curl
  - xml
  - zip

### Installation Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/CertifiedWebMaster/Shopify-Clone-2026.git
   cd Shopify-Clone-2026
   ```

2. **Configure web server**
   - Point document root to the repository root
   - Ensure `.htaccess` files are enabled
   - Enable mod_rewrite for Apache

3. **Set permissions**
   ```bash
   chmod -R 755 .
   chmod -R 777 apps/shop/cache
   chmod -R 777 apps/www/cache
   chmod -R 777 assets
   ```

4. **Configure database**
   - Create a MySQL database
   - Import database schema (if available)
   - Update `config.php` with database credentials

5. **Update configuration**
   - Edit `config.php` to match your environment
   - Update domain settings (OS_DOMAIN, OS_BASE_DOMAIN)
   - Set CI_ENV to 'production' for production deployments

## CI/CD with GitHub Actions

The project includes a GitHub Actions workflow that automatically:

1. Builds the Docker image on every push
2. Runs syntax checks on PHP code
3. Tests the built image
4. Publishes the image to GitHub Container Registry (on main/master branch)

### Workflow Triggers

- Push to `main` or `master` branch
- Pull requests to `main` or `master` branch
- Manual trigger via GitHub Actions UI

### Using Published Images

Pull and run the latest image:

```bash
docker pull ghcr.io/certifiedwebmaster/shopify-clone:latest
docker run -d -p 8080:80 ghcr.io/certifiedwebmaster/shopify-clone:latest
```

## Production Considerations

### Security

1. **Change default credentials**
   - Update all database passwords
   - Use strong, unique passwords
   - Never commit sensitive credentials to version control

2. **Environment variables**
   - Use environment variables for sensitive configuration
   - Consider using a secrets management solution

3. **HTTPS**
   - Always use HTTPS in production
   - Configure SSL certificates
   - Update config.php to use HTTPS URLs

4. **File permissions**
   - Restrict write permissions to only necessary directories
   - Run web server with minimal privileges

### Performance

1. **PHP optimization**
   - Enable OPcache
   - Tune PHP-FPM settings if using PHP-FPM
   - Set appropriate memory limits

2. **Database optimization**
   - Configure MySQL for production workload
   - Set up regular backups
   - Monitor query performance

3. **Caching**
   - Enable browser caching
   - Consider using Redis for session storage
   - Implement application-level caching

### Monitoring

1. **Logs**
   - Configure proper log rotation
   - Monitor application and web server logs
   - Set up error tracking (e.g., Sentry)

2. **Health checks**
   - The Docker image includes a health check endpoint
   - Set up uptime monitoring
   - Configure alerts for downtime

### Backup

1. **Database backups**
   - Set up automated database backups
   - Test restore procedures regularly
   - Store backups in secure, off-site location

2. **Application files**
   - Back up uploaded files and assets
   - Version control for code changes

## Deployment Platforms

### DigitalOcean App Platform

1. Fork or clone the repository
2. Create a new app in DigitalOcean App Platform
3. Connect your repository
4. Select "Dockerfile" as the deployment method
5. Configure environment variables
6. Deploy

### Heroku

1. Install Heroku CLI
2. Create a new Heroku app
   ```bash
   heroku create your-app-name
   ```
3. Add MySQL addon
   ```bash
   heroku addons:create cleardb:ignite
   ```
4. Deploy
   ```bash
   git push heroku main
   ```

### AWS ECS / Azure Container Instances / GCP Cloud Run

1. Build and push Docker image to container registry
2. Create container service
3. Configure environment variables
4. Set up load balancer (if needed)
5. Deploy container

## Troubleshooting

### Common Issues

1. **Permission denied errors**
   ```bash
   chmod -R 777 apps/shop/cache
   chmod -R 777 apps/www/cache
   chmod -R 777 assets
   ```

2. **Database connection errors**
   - Verify database credentials in config.php
   - Ensure database server is running
   - Check network connectivity

3. **404 errors**
   - Enable mod_rewrite in Apache
   - Verify .htaccess files are present
   - Check AllowOverride settings

4. **Docker build fails**
   - Ensure Docker is running
   - Check available disk space
   - Try building without cache: `docker build --no-cache -t shopify-clone .`

## Support

For issues and questions:
- Open an issue on GitHub
- Check existing issues for solutions
- Review the CodeIgniter documentation

## License

This project is licensed under the MIT License - see the LICENSE file for details.
