# n8n DigitalOcean Deployment - Configuration Guide

This document explains how to configure and use the environment variables and configuration files for deploying n8n to DigitalOcean App Platform.

## 📁 Configuration Files Overview

The following configuration files have been created for your n8n deployment:

### 1. `.env.template` - Environment Variables Template
**Purpose**: Template file showing all required environment variables  
**Usage**: Reference for setting up production and development environments  
**Version Control**: ✅ Commit this file (contains no sensitive data)

### 2. `.env.local` - Local Development Environment
**Purpose**: Actual environment variables for local development  
**Usage**: Use when running n8n locally for development/testing  
**Version Control**: ❌ NEVER commit this file (contains sensitive data)

### 3. `app.yaml` - DigitalOcean App Platform Specification
**Purpose**: Infrastructure-as-Code definition for App Platform deployment  
**Usage**: Defines your app's infrastructure, build process, and runtime configuration  
**Version Control**: ✅ Commit this file (customize before deployment)

### 4. `.gitignore` - Version Control Exclusions
**Purpose**: Prevents sensitive files from being committed to Git  
**Usage**: Automatically excludes environment files and other sensitive data  
**Version Control**: ✅ Commit this file

## 🚀 Quick Start Guide

### Step 1: Set Up Local Development

1. **Copy the local environment file** (already created with secure keys):
   ```bash
   # The .env.local file is ready to use with generated secure keys
   # Update database credentials for your local PostgreSQL instance
   ```

2. **Update database credentials in `.env.local`**:
   ```bash
   DB_POSTGRESDB_HOST=localhost
   DB_POSTGRESDB_USER=your-local-db-user
   DB_POSTGRESDB_PASSWORD=your-local-db-password
   DB_POSTGRESDB_DATABASE=n8n_dev
   ```

3. **Install and run n8n locally**:
   ```bash
   npm install
   npm run dev
   ```

### Step 2: Prepare for Production Deployment

1. **Customize `app.yaml`**:
   - Update the GitHub repository reference:
     ```yaml
     github:
       repo: your-github-username/n8n  # Replace with your forked repo
       branch: main
     ```
   - Choose your preferred region:
     ```yaml
     region: nyc1  # Options: nyc1, nyc3, ams3, sgp1, lon1, fra1, tor1, sfo3, blr1, syd1
     ```
   - Adjust instance size if needed:
     ```yaml
     instance_size_slug: basic-xxs  # Start small, scale up as needed
     ```

2. **Set up DigitalOcean Managed PostgreSQL**:
   - Create a PostgreSQL database in DigitalOcean
   - Note the connection details for environment variables

3. **Configure App Platform Environment Variables**:
   In the DigitalOcean App Platform dashboard, add these **encrypted** environment variables:
   ```
   DB_POSTGRESDB_HOST=your-postgres-host
   DB_POSTGRESDB_USER=your-db-username
   DB_POSTGRESDB_PASSWORD=your-db-password
   N8N_ENCRYPTION_KEY=your-32-character-encryption-key
   N8N_USER_MANAGEMENT_JWT_SECRET=your-jwt-secret
   ```

## 🔐 Security Best Practices

### Environment Variable Security

1. **Never commit sensitive data**:
   - ❌ Don't put real passwords/keys in `app.yaml`
   - ✅ Use App Platform's encrypted environment variables
   - ✅ Use the `.env.template` as a reference only

2. **Use strong encryption keys**:
   - The `.env.local` file contains pre-generated secure keys
   - Generate new keys for production using:
     ```bash
     node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
     ```

3. **Database security**:
   - Use DigitalOcean Managed PostgreSQL for production
   - Enable SSL connections
   - Use strong database passwords
   - Restrict database access to your App Platform app only

### Access Control

1. **Basic Authentication** (optional but recommended):
   ```bash
   N8N_BASIC_AUTH_ACTIVE=true
   N8N_BASIC_AUTH_USER=admin
   N8N_BASIC_AUTH_PASSWORD=strong-password-here
   ```

2. **Custom Domain with SSL**:
   - Configure a custom domain in App Platform
   - SSL certificates are automatically managed

## 🗄️ Database Configuration

### PostgreSQL Setup

1. **Create DigitalOcean Managed Database**:
   - Go to DigitalOcean → Databases
   - Create PostgreSQL cluster
   - Choose appropriate size (start with `db-s-1vcpu-1gb`)

2. **Database Connection**:
   ```bash
   # Individual settings (recommended)
   DB_POSTGRESDB_HOST=your-cluster-host
   DB_POSTGRESDB_PORT=25060
   DB_POSTGRESDB_DATABASE=n8n
   DB_POSTGRESDB_USER=doadmin
   DB_POSTGRESDB_PASSWORD=your-password
   
   # Or single connection URL
   DB_POSTGRESDB_URL=postgresql://user:pass@host:port/database?sslmode=require
   ```

3. **Database Initialization**:
   - n8n will automatically create required tables on first run
   - Ensure the database user has CREATE privileges

## 📦 Binary Data Storage (Enterprise Feature)

For storing workflow execution data and files:

### Option 1: Default (In-Memory)
- Suitable for basic workflows
- Data lost on app restart
- No additional configuration needed

### Option 2: DigitalOcean Spaces (Recommended for Production)
1. **Create Spaces bucket**:
   - Go to DigitalOcean → Spaces
   - Create new bucket
   - Note the endpoint and region

2. **Configure environment variables**:
   ```bash
   N8N_DEFAULT_BINARY_DATA_MODE=s3
   N8N_BINARY_DATA_S3_ENDPOINT=https://nyc3.digitaloceanspaces.com
   N8N_BINARY_DATA_S3_REGION=nyc3
   N8N_BINARY_DATA_S3_BUCKET=your-bucket-name
   N8N_BINARY_DATA_S3_ACCESS_KEY_ID=your-spaces-key
   N8N_BINARY_DATA_S3_SECRET_ACCESS_KEY=your-spaces-secret
   ```

## 🔧 Customization Options

### App Platform Configuration

Edit `app.yaml` to customize:

1. **Instance Size**:
   ```yaml
   instance_size_slug: basic-s  # Upgrade for better performance
   ```

2. **Auto-scaling**:
   ```yaml
   instance_count: 2  # Run multiple instances
   ```

3. **Health Checks**:
   ```yaml
   health_check:
     http_path: /healthz
     initial_delay_seconds: 60
   ```

4. **Custom Domain**:
   ```yaml
   domains:
     - domain: n8n.yourdomain.com
       type: PRIMARY
   ```

### n8n Configuration

Common customizations in environment variables:

1. **Timezone**:
   ```bash
   GENERIC_TIMEZONE=America/New_York
   ```

2. **Logging**:
   ```bash
   N8N_LOG_LEVEL=info  # Options: error, warn, info, verbose, debug
   ```

3. **Payload Limits**:
   ```bash
   N8N_PAYLOAD_SIZE_MAX=32  # MB
   ```

## 🚨 Troubleshooting

### Common Issues

1. **App won't start**:
   - Check environment variables are set correctly
   - Verify database connection
   - Check App Platform logs

2. **Database connection failed**:
   - Verify PostgreSQL credentials
   - Check SSL requirements
   - Ensure database allows connections from App Platform

3. **Webhooks not working**:
   - Verify `WEBHOOK_URL` matches your app domain
   - Check `N8N_HOST` and `N8N_PROTOCOL` settings
   - Ensure App Platform routes are configured correctly

### Debugging Steps

1. **Check App Platform logs**:
   ```bash
   doctl apps logs <app-id>
   ```

2. **Verify environment variables**:
   - Go to App Platform dashboard
   - Check all required variables are set
   - Ensure sensitive variables are encrypted

3. **Test database connection**:
   ```bash
   # From your local machine
   psql "postgresql://user:pass@host:port/database?sslmode=require"
   ```

## 📚 Next Steps

After completing the configuration:

1. **Test locally** with `.env.local`
2. **Customize `app.yaml`** for your needs
3. **Set up DigitalOcean services** (Database, Spaces if needed)
4. **Deploy to App Platform**
5. **Configure domain and SSL**
6. **Set up monitoring and backups**

## 🔗 Useful Links

- [n8n Documentation](https://docs.n8n.io/)
- [DigitalOcean App Platform Docs](https://docs.digitalocean.com/products/app-platform/)
- [DigitalOcean Managed Databases](https://docs.digitalocean.com/products/databases/)
- [DigitalOcean Spaces](https://docs.digitalocean.com/products/spaces/)

---

**⚠️ Important**: Always keep your `.env.local` file secure and never commit it to version control. The `.gitignore` file is configured to prevent this automatically.