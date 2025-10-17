# GitHub Secrets Setup Guide

This guide will help you set up the required GitHub secrets for successful deployment to DigitalOcean App Platform.

## Required GitHub Secrets

You need to add the following secrets to your GitHub repository:

### 1. DIGITALOCEAN_ACCESS_TOKEN
- **Purpose**: Authenticates with DigitalOcean API for deployment
- **How to get it**:
  1. Go to [DigitalOcean Control Panel](https://cloud.digitalocean.com/)
  2. Click "API" in the left sidebar
  3. Click "Generate New Token"
  4. Name it "GitHub Actions Deploy" and select "Write" scope
  5. Copy the generated token

### 2. DB_POSTGRESDB_PASSWORD
- **Purpose**: Password for your PostgreSQL database
- **Value**: Use the password from your DigitalOcean Managed PostgreSQL database
- **Location**: Found in your DigitalOcean database dashboard

### 3. N8N_ENCRYPTION_KEY
- **Purpose**: Encrypts sensitive data in n8n
- **Value**: Generate a 32-character random string
- **Example**: `abcd1234efgh5678ijkl9012mnop3456`
- **Generate with**: `openssl rand -hex 16` or use an online generator

### 4. N8N_USER_MANAGEMENT_JWT_SECRET
- **Purpose**: JWT token signing for n8n user management
- **Value**: Generate a secure random string (32+ characters)
- **Example**: `super-secret-jwt-key-for-n8n-user-management-2024`
- **Generate with**: `openssl rand -base64 32` or use an online generator

## How to Add GitHub Secrets

1. Go to your GitHub repository: `https://github.com/wlau0721/n8nFork`
2. Click on "Settings" tab
3. In the left sidebar, click "Secrets and variables" → "Actions"
4. Click "New repository secret"
5. Add each secret with the exact name and value

## Verification Steps

### Check DigitalOcean App Platform GitHub Integration
1. Go to [DigitalOcean App Platform](https://cloud.digitalocean.com/apps)
2. Verify your GitHub account is connected
3. If not connected, click "Manage" → "Source Control" → "Connect GitHub"

### Test Deployment
After adding all secrets:
1. Go to your repository's "Actions" tab
2. Click "Run workflow" on the "Deploy to DigitalOcean App Platform" workflow
3. Monitor the deployment logs for any errors

## Common Issues and Solutions

### Issue: "doctl auth init failed"
- **Solution**: Check that `DIGITALOCEAN_ACCESS_TOKEN` is correctly set and has write permissions

### Issue: "Database connection failed"
- **Solution**: Verify `DB_POSTGRESDB_PASSWORD` matches your database password

### Issue: "n8n startup failed"
- **Solution**: Ensure `N8N_ENCRYPTION_KEY` and `N8N_USER_MANAGEMENT_JWT_SECRET` are properly set

## Security Notes

- Never commit secrets to your repository
- Use GitHub's encrypted secrets feature
- Rotate tokens periodically for security
- The deployment workflow replaces placeholders in `app.yaml` with actual secret values during deployment

## Next Steps

1. Add all four GitHub secrets
2. Verify DigitalOcean GitHub integration
3. Trigger a new deployment
4. Monitor deployment logs
5. Test the deployed n8n application