# Event Booking App - Setup Guide

Complete step-by-step guide to set up the Event Booking App locally.

## 🔧 Prerequisites

### Required Software
- **Java 21** or higher ([Download](https://adoptium.net/))
- **PostgreSQL 17.6** ([Download](https://www.postgresql.org/download/))
- **Maven 3.6+** ([Download](https://maven.apache.org/download.cgi))
- **Keycloak** ([Download](https://www.keycloak.org/downloads))
- **Git** ([Download](https://git-scm.com/downloads))

### Hardware Requirements
- **RAM**: 4GB minimum, 8GB recommended
- **Storage**: 2GB free space
- **CPU**: Any modern processor

## 🗄️ Database Setup

### 1. Install PostgreSQL
1. Download and install PostgreSQL 17.6
2. Set password for postgres user during installation
3. Ensure PostgreSQL service is running

### 2. Create Database
```sql
-- Connect to PostgreSQL as postgres user
psql -U postgres

-- Create the database
CREATE DATABASE Event_Booking_App_db;

-- Grant permissions (if using different user)
GRANT ALL PRIVILEGES ON DATABASE Event_Booking_App_db TO your_username;

-- Exit psql
\q
```

### 3. Verify Database Connection
```bash
psql -h localhost -U postgres -d Event_Booking_App_db -c "SELECT version();"
```

## 🔐 Keycloak Setup

### 1. Download and Start Keycloak
```bash
# Download Keycloak
wget https://github.com/keycloak/keycloak/releases/download/22.0.4/keycloak-22.0.4.zip
unzip keycloak-22.0.4.zip
cd keycloak-22.0.4

# Start Keycloak
bin/kc.sh start-dev
```

### 2. Access Keycloak Admin Console
- URL: http://localhost:9090
- Create admin user when prompted

### 3. Create Realm
1. Click "Create Realm"
2. Realm name: `event-ticket-platform`
3. Click "Create"

### 4. Create Client
1. Go to Clients → Create Client
2. Client ID: `event-booking-app`
3. Client Type: `OpenID Connect`
4. Click "Next"
5. Enable "Direct access grants"
6. Click "Save"

### 5. Configure Client Settings
1. Go to client settings
2. Access Type: `public`
3. Valid Redirect URIs: `http://localhost:8081/*`
4. Web Origins: `http://localhost:8081`
5. Save changes

### 6. Create Roles
1. Go to Realm Roles → Create Role
2. Create these roles:
   - `ORGANIZER` - Can manage events and ticket types
   - `ATTENDEE` - Can browse events and purchase tickets
   - `STAFF` - Can validate tickets

### 7. Create Test Users
1. Go to Users → Add User
2. Create users for testing:
   - Username: `organizer1`, Email: `organizer@test.com`
   - Username: `attendee1`, Email: `attendee@test.com`
   - Username: `staff1`, Email: `staff@test.com`
3. Set passwords in Credentials tab
4. Assign roles in Role Mappings tab

## ⚙️ Application Configuration

### 1. Update application.properties
```properties
# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/Event_Booking_App_db
spring.datasource.username=postgres
spring.datasource.password=your_postgres_password

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# Server Configuration
server.port=8081

# Keycloak Configuration
spring.security.oauth2.resourceserver.jwt.issuer-uri=http://localhost:9090/realms/event-ticket-platform
```

### 2. Create application-local.properties (for local development)
```properties
# Local development overrides
spring.jpa.show-sql=false
logging.level.com.event.tickets=DEBUG
```

## 🚀 Running the Application

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/event-booking-app.git
cd event-booking-app/Backend/tickets
```

### 2. Install Dependencies
```bash
mvn clean install
```

### 3. Run the Application
```bash
mvn spring-boot:run
```

### 4. Verify Application is Running
- Application URL: http://localhost:8081
- Health Check: http://localhost:8081/actuator/health (if actuator enabled)

## 🧪 Testing the Setup

### 1. Get Authentication Token
```bash
curl -X POST http://localhost:9090/realms/event-ticket-platform/protocol/openid-connect/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=password" \
  -d "client_id=event-booking-app" \
  -d "username=organizer1" \
  -d "password=your_password" \
  -d "scope=openid profile email"
```

### 2. Test API Endpoints
Use the provided Postman collection in `postman/` directory for comprehensive testing.

### 3. Import Postman Collection
1. Open Postman
2. Click Import
3. Select `postman/EventBookingApp.postman_collection.json`
4. Update variables with your token

## 🐳 Docker Setup (Optional)

### Using Docker Compose
```bash
# Start PostgreSQL and Keycloak with Docker
docker-compose up -d

# Run the application
mvn spring-boot:run
```

## 🔧 Troubleshooting

### Common Issues

#### Database Connection Failed
- Verify PostgreSQL is running: `pg_ctl status`
- Check connection string in application.properties
- Ensure database exists and user has permissions

#### Keycloak Authentication Issues
- Verify Keycloak is running on port 9090
- Check realm name is correct: `event-ticket-platform`
- Ensure client configuration is correct
- Verify user roles are assigned properly

#### Application Won't Start
- Check Java version: `java --version`
- Verify Maven is working: `mvn --version`
- Check for port conflicts on 8081
- Review application logs for specific errors

#### 401/403 Errors in API
- Get fresh token from Keycloak
- Verify user has correct role for endpoint
- Check token format in Authorization header

### Getting Help
- Check the [API Documentation](postman/API_DOCUMENTATION.md)
- Review error messages for detailed diagnostics
- Check application logs for stack traces
- Create GitHub issue with detailed description

## 🏗️ Development Workflow

### Feature Development
1. Create feature branch: `git checkout -b feature/new-feature`
2. Make changes and test thoroughly
3. Update documentation if needed
4. Commit with conventional commit format
5. Push and create Pull Request

### Database Changes
- Use `spring.jpa.hibernate.ddl-auto=update` for development
- Document any manual schema changes needed
- Test database migrations thoroughly

### API Changes
- Update API documentation
- Ensure backward compatibility or document breaking changes
- Test with Postman collection
- Update error handling if needed

## 📊 Performance Considerations

### Database Optimization
- Use appropriate indexes for frequent queries
- Monitor query performance with `spring.jpa.show-sql=true`
- Consider connection pooling for production

### Security Best Practices
- Never commit sensitive configuration
- Use environment variables for production secrets
- Regularly update dependencies
- Follow OWASP security guidelines

## 🎯 Next Steps

After successful setup:
1. Explore the API using Postman collection
2. Create test events and ticket types
3. Test ticket purchasing workflow
4. Try ticket validation features
5. Review analytics and reporting features

## 📞 Support

For setup assistance:
- Check this guide thoroughly
- Review troubleshooting section
- Create GitHub issue with setup details
- Include error logs and configuration details
