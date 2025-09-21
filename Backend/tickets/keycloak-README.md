# Keycloak Production Deployment

This repository contains the Keycloak configuration for the Event Booking App.

## Environment Variables Required

- `KEYCLOAK_ADMIN`: Admin username (default: admin)
- `KEYCLOAK_ADMIN_PASSWORD`: Admin password (set a strong password)
- `PORT`: Port for the service (Render sets this automatically)

## Local Testing

```bash
docker build -f Dockerfile.keycloak -t keycloak-prod .
docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin keycloak-prod
```

## Render Deployment

1. Connect this repository to Render
2. Choose Docker environment
3. Set environment variables
4. Deploy
