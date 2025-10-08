# GitHub Setup Guide for n8n DigitalOcean Deployment

## 🔐 Step 1: Configure GitHub Secrets

You need to add the following secrets to your GitHub repository for automated deployment:

### Required Secrets:

1. **DIGITALOCEAN_ACCESS_TOKEN**
   - Your DigitalOcean API token (the same one you used for MCP)
   - Used for: Authenticating with DigitalOcean API and deploying to App Platform

2. **DB_POSTGRESDB_PASSWORD**
   - Value: `AVNS_MANEYcFG71Md4XYzRLF` (n8n_user password)
   - Used for: Database connection authentication

3. **N8N_ENCRYPTION_KEY**
   - Value: `[Use the generated encryption key from previous steps]`
   - Used for: Encrypting sensitive data in n8n database

4. **N8N_USER_MANAGEMENT_JWT_SECRET**
   - Value: `[Use the generated JWT secret from previous steps]`
   - Used for: JWT token signing for user authentication

## 📝 How to Add GitHub Secrets:

1. Go to your forked repository: `https://github.com/wlau0721/n8nFork`
2. Click on **Settings** tab
3. In the left sidebar, click **Secrets and variables** → **Actions**
4. Click **New repository secret**
5. Add each secret one by one:
   - **Name**: Enter the secret name (e.g., `DIGITALOCEAN_ACCESS_TOKEN`)
   - **Secret**: Enter the secret value
   - Click **Add secret**

## 🚀 Step 2: GitHub Actions Workflow

The workflow file `.github/workflows/deploy.yml` has been created with the following features:

### Workflow Triggers:
- **Push to main/master branch**: Automatic deployment
- **Pull Request**: Testing deployment (can be configured)

### Deployment Process:
1. **Checkout code** from your repository
2. **Install doctl** (DigitalOcean CLI)
3. **Deploy to App Platform** using your `app.yaml` configuration
4. **Handle both new and existing apps** automatically
5. **Wait for deployment** and show status

### Security Features:
- All sensitive data is stored as encrypted GitHub secrets
- Secrets are injected during deployment, never stored in code
- App.yaml is dynamically updated with secrets during CI/CD

## 🔄 Deployment Flow:

```
GitHub Push → GitHub Actions → DigitalOcean App Platform → n8n Running
```

## ✅ Next Steps:

1. **Add all 4 GitHub secrets** to your repository
2. **Push your code** to the main/master branch
3. **Monitor the Actions tab** for deployment progress
4. **Access your n8n instance** at `https://lexagora.ondigitalocean.app`

## 🛠️ Manual Deployment Alternative:

If you prefer manual deployment:
1. Use the `app.yaml` file directly in DigitalOcean App Platform
2. Manually add the 3 encrypted environment variables in the dashboard
3. Deploy and restart the application

## 📋 Verification Checklist:

- [ ] GitHub repository forked: `https://github.com/wlau0721/n8nFork.git`
- [ ] All 4 GitHub secrets added
- [ ] GitHub Actions workflow file created
- [ ] App.yaml configured with domain and database
- [ ] Ready for deployment

## 🔍 Troubleshooting:

- **Deployment fails**: Check GitHub Actions logs for detailed error messages
- **Database connection issues**: Verify PostgreSQL credentials and network access
- **Domain not accessible**: Check DigitalOcean DNS settings and App Platform status