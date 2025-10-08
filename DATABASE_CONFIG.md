# Database Configuration

## PostgreSQL Database Details

**Database Cluster ID**: `f602cb3b-dd2e-46de-bccd-26f5ef770b35`
**Database Name**: `n8n-postgres`
**Engine**: PostgreSQL 16
**Region**: sfo3
**Status**: online

## Database Connection Information

### Public Connection
- **Host**: `n8n-postgres-do-user-24961063-0.h.db.ondigitalocean.com`
- **Port**: `25060`
- **Database**: `defaultdb`
- **SSL**: Required

### Private Connection (VPC)
- **Host**: `private-n8n-postgres-do-user-24961063-0.h.db.ondigitalocean.com`
- **Port**: `25060`
- **Database**: `defaultdb`
- **SSL**: Required

## Database Users

### 1. Admin User (doadmin)
- **Username**: `doadmin`
- **Password**: `AVNS_5DYPiP3ElomXBEq4SrJ`
- **Role**: Primary (full administrative access)
- **Usage**: Database administration and initial setup

### 2. Application User (n8n_user) ✅ NEWLY CREATED
- **Username**: `n8n_user`
- **Password**: `AVNS_MANEYcFG71Md4XYzRLF`
- **Role**: Normal (application-level access)
- **Usage**: n8n application database operations

## Security Configuration

- SSL/TLS encryption is **required** for all connections
- Database is configured with proper firewall rules
- Separate application user created for n8n with limited privileges
- Admin credentials kept separate from application credentials

## Next Steps

1. ✅ Database cluster created and online
2. ✅ Application user `n8n_user` created with appropriate permissions
3. ⏳ Update n8n application configuration to use `n8n_user` credentials
4. ⏳ Deploy n8n application with database connection
5. ⏳ Test database connectivity and n8n functionality

## Environment Variables for n8n

Use these values in your DigitalOcean App Platform environment variables:

```
DB_POSTGRESDB_HOST=n8n-postgres-do-user-24961063-0.h.db.ondigitalocean.com
DB_POSTGRESDB_PORT=25060
DB_POSTGRESDB_DATABASE=defaultdb
DB_POSTGRESDB_USER=n8n_user
DB_POSTGRESDB_PASSWORD=AVNS_MANEYcFG71Md4XYzRLF
DB_POSTGRESDB_SSL=true
```

**Important**: The `DB_POSTGRESDB_PASSWORD` should be added as an **encrypted environment variable** in the DigitalOcean App Platform dashboard.