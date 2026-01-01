# oneshop

Working Shopify clone written in PHP

## Quick Start with Docker

The easiest way to get started is using Docker:

```bash
# Clone the repository
git clone https://github.com/CertifiedWebMaster/Shopify-Clone-2026.git
cd Shopify-Clone-2026

# Start the application with Docker Compose
docker compose up -d

# Access the application
# - Web App: http://localhost:8080
# - phpMyAdmin: http://localhost:8081
```

## Deployment

For detailed deployment instructions, see [DEPLOY.md](DEPLOY.md).

### Quick Deployment Options

- **Docker**: `docker build -t shopify-clone . && docker run -p 8080:80 shopify-clone`
- **Docker Compose**: `docker compose up -d`
- **GitHub Actions**: CI/CD pipeline included - pushes to main/master trigger automatic builds

## Requirements

- PHP 8.1+
- MySQL 8.0+
- Apache/Nginx with mod_rewrite
- PHP extensions: pdo_mysql, mysqli, mbstring, gd, curl, xml, zip

## Documentation

- [Deployment Guide](DEPLOY.md) - Complete deployment instructions
- [Configuration](.env.example) - Environment variables reference

## License

This project is open source and available under the MIT License.
