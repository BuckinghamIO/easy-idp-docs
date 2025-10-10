# Installation

Easy IDP can be installed in multiple ways depending on your environment and preferences.

## Prerequisites

- Docker and Docker Compose (for containerized deployment)
- OR Python 3.12+ and pip (for local development)
- Git (for GitOps functionality)

## Docker Compose (Recommended)

The easiest way to get started with Easy IDP is using Docker Compose.

### 1. Clone the Repository

```bash
git clone https://github.com/BuckinghamIO/easy-idp.git
cd easy-idp
```

### 2. Configure Environment Variables

Create a `.env` file from the example:

```bash
cp .env.example .env
```

Edit `.env` and set your credentials:

```bash
SECRET_KEY=your-secret-key-here
ADMIN_USERNAME=admin
ADMIN_PASSWORD=your-secure-password
```

!!! warning "Security"
    Make sure to change the default admin password before deploying to production!

### 3. Start Easy IDP

```bash
docker-compose up -d
```

### 4. Access the Application

Open your browser and navigate to:

```
http://localhost:8000
```

Login with your admin credentials.

!!! success "Installation Complete"
    You're now ready to create your first workflow! Continue to the [Quickstart Guide](quickstart.md).

---

## Local Development

For local development or testing, you can run Easy IDP directly with Python.

### 1. Clone and Install Dependencies

```bash
git clone https://github.com/BuckinghamIO/easy-idp.git
cd easy-idp
pip install -r requirements.txt
```

### 2. Set Environment Variables

```bash
export SECRET_KEY="dev-secret-key"
export ADMIN_USERNAME="admin"
export ADMIN_PASSWORD="admin"
```

### 3. Run the Application

```bash
cd src
python app.py
```

The application will start on `http://localhost:8000`.

---

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `SECRET_KEY` | `dev-secret-key-change-in-production` | Flask secret key for session management |
| `ADMIN_USERNAME` | `admin` | Admin username for initial login |
| `ADMIN_PASSWORD` | `admin` | Admin password for initial login |
| `DB_PATH` | `idp.db` | SQLite database file path |

---

## Verification

After installation, verify that Easy IDP is running correctly:

### 1. Check the Health Endpoint

```bash
curl http://localhost:8000/
```

You should see the login page HTML.

### 2. Login to the Admin Panel

1. Navigate to `http://localhost:8000`
2. Login with your admin credentials
3. You should see the dashboard

### 3. Check the Database

The SQLite database should be created:

```bash
ls -lh database/idp.db
```

---

## Troubleshooting

### Port Already in Use

If port 8000 is already in use, modify `docker-compose.yml`:

```yaml
ports:
  - "8080:8000"  # Change host port to 8080
```

### Permission Issues

If you encounter permission issues with the database:

```bash
# Create database directory
mkdir -p database
chmod 755 database
```

### Docker Issues

If Docker containers won't start:

```bash
# Check Docker logs
docker-compose logs

# Rebuild containers
docker-compose down
docker-compose up --build
```

---

## Next Steps

- [Quickstart Guide →](quickstart.md) - Create your first workflow
- [GitOps Setup →](../gitops/setup.md) - Configure GitOps sync
- [First Workflow →](first-workflow.md) - Build a deployment workflow

---

## Upgrading

To upgrade Easy IDP to a new version:

```bash
# Pull latest changes
cd easy-idp
git pull

# Rebuild and restart
docker-compose down
docker-compose up --build -d
```

!!! tip "Database Backups"
    Always backup your database before upgrading:
    ```bash
    cp database/idp.db database/idp.db.backup
    ```
