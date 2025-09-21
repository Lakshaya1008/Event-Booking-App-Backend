# Event Booking App

A comprehensive Spring Boot application for managing events, ticket sales, and attendee registration with QR code generation and validation capabilities.

## 🌟 Features

- **Event Management**: Create, update, and manage events with flexible ticket types
- **Ticket Sales**: Purchase tickets with quantity support (1-10 tickets per transaction)
- **QR Code Generation**: Automatic QR code generation for each ticket
- **Role-Based Security**: ORGANIZER, ATTENDEE, and STAFF roles with specific permissions
- **Ticket Validation**: Manual and QR scan validation for event entry
- **Real-time Analytics**: Sales dashboard and attendee reports
- **RESTful API**: Complete REST API with comprehensive error handling

## 🏗️ Architecture

- **Backend**: Spring Boot 3.5.5 with Spring Security
- **Database**: PostgreSQL with JPA/Hibernate
- **Authentication**: OAuth2 JWT with Keycloak
- **QR Generation**: ZXing library for QR code creation
- **API Documentation**: Comprehensive Postman collection included

## 🔧 Tech Stack

- **Java 21**
- **Spring Boot 3.5.5**
- **Spring Security 6.5.3**
- **Spring Data JPA**
- **PostgreSQL 17.6**
- **Keycloak (OAuth2 JWT)**
- **ZXing (QR Code Generation)**
- **MapStruct (Entity-DTO Mapping)**
- **Lombok**
- **Maven**

## 🚀 Quick Start

### Prerequisites

- Java 21 or higher
- PostgreSQL 17.6
- Keycloak Server
- Maven 3.6+

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/event-booking-app.git
cd event-booking-app/Backend/tickets
```

### 2. Database Setup

```sql
-- Create database
CREATE DATABASE Event_Booking_App_db;

-- Update connection details in application.properties if needed
```

### 3. Keycloak Setup

1. Start Keycloak server on `http://localhost:9090`
2. Create realm: `event-ticket-platform`
3. Configure client for Resource Owner Password Grant
4. Set up user roles: `ORGANIZER`, `ATTENDEE`, `STAFF`

### 4. Configuration

Update `src/main/resources/application.properties`:

```properties
# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/Event_Booking_App_db
spring.datasource.username=your_username
spring.datasource.password=your_password

# Keycloak Configuration
spring.security.oauth2.resourceserver.jwt.issuer-uri=http://localhost:9090/realms/event-ticket-platform
```

### 5. Run the Application

```bash
mvn spring-boot:run
```

The application will start on `http://localhost:8081`

## 📚 API Documentation

### Base URL
```
http://localhost:8081/api/v1
```

### Authentication
All endpoints require Bearer JWT token in Authorization header:
```
Authorization: Bearer <your-jwt-token>
```

### Key Endpoints

#### Event Management (ORGANIZER)
- `POST /events` - Create event
- `GET /events` - List organizer's events
- `PUT /events/{id}` - Update event
- `DELETE /events/{id}` - Delete event

#### Ticket Types (ORGANIZER)
- `POST /events/{eventId}/ticket-types` - Create ticket type
- `GET /events/{eventId}/ticket-types` - List ticket types
- `PUT /events/{eventId}/ticket-types/{id}` - Update ticket type

#### Ticket Purchase (ATTENDEE)
- `POST /events/{eventId}/ticket-types/{id}/tickets` - Purchase tickets
  ```json
  {
    "quantity": 3
  }
  ```

#### Browse Events (ATTENDEE)
- `GET /published-events` - List published events
- `GET /published-events/{id}` - Get event details

#### My Tickets (ATTENDEE)
- `GET /tickets` - List my tickets
- `GET /tickets/{id}` - Get ticket details
- `GET /tickets/{id}/qr-codes` - Get QR code image

#### Ticket Validation (STAFF)
- `POST /ticket-validations` - Validate ticket
- `GET /ticket-validations/events/{eventId}` - List validations

### Detailed API Documentation
See [API_DOCUMENTATION.md](postman/API_DOCUMENTATION.md) for complete endpoint reference with examples.

## 🗃️ Database Schema

### Key Entities

- **Events**: Event details with organizer information
- **TicketTypes**: Different ticket categories with pricing and availability
- **Tickets**: Individual purchased tickets
- **QrCodes**: Generated QR codes for ticket validation
- **Users**: System users with roles
- **TicketValidations**: Validation history for entry tracking

### Relationships

- One Event has many TicketTypes
- One TicketType has many Tickets
- One Ticket has one QrCode
- One User can purchase many Tickets
- Tickets can have multiple Validations

## 🔐 Security & Roles

### User Roles

- **ORGANIZER**: Can create and manage events, view analytics
- **ATTENDEE**: Can browse events and purchase tickets
- **STAFF**: Can validate tickets and view validation reports

### Security Features

- JWT-based authentication with Keycloak
- Role-based access control
- Resource ownership validation
- Comprehensive error handling with detailed diagnostics

## 🛠️ Development

### Project Structure

```
src/main/java/com/event/tickets/
├── config/           # Security and application configuration
├── controllers/      # REST API controllers
├── domain/           # Entities and DTOs
├── exceptions/       # Custom exception classes
├── filters/          # Custom filters
├── mappers/          # MapStruct entity-DTO mappers
├── repositories/     # JPA repositories
├── services/         # Business logic services
└── util/            # Utility classes
```

### Key Features Implemented

- **Quantity-based ticket purchasing**: Buy 1-10 tickets per transaction
- **Individual QR codes**: Each ticket gets unique QR code
- **Atomic transactions**: Multiple tickets created together or fail together
- **Availability checking**: Prevents overselling
- **Comprehensive error handling**: Detailed error diagnostics
- **Sales analytics**: Revenue and attendee reporting

## 📊 API Error Handling

The application provides comprehensive error diagnostics with:

- **Error classification**: CLIENT ISSUE, SERVER ISSUE, CODE ISSUE, DATA ISSUE
- **Detailed causes**: Specific reasons for each error type
- **Step-by-step solutions**: How to fix each error
- **Endpoint-specific analysis**: Tailored advice per API endpoint

### Example Error Response
```json
{
  "error": "Authentication Failed",
  "message": "Invalid or missing authentication credentials",
  "statusCode": 401,
  "statusDescription": "UNAUTHORIZED - Authentication required",
  "timestamp": "2025-09-21T15:25:30.123",
  "path": "/api/v1/events/123",
  "possibleCauses": [
    "CLIENT ISSUE: Missing Authorization header in request",
    "CLIENT ISSUE: Invalid or expired JWT token"
  ],
  "solutions": [
    "Add 'Authorization: Bearer <your-token>' header",
    "Get fresh token from Keycloak"
  ]
}
```

## 🧪 Testing

### Postman Collection

Import the provided Postman collection:
```
postman/EventBookingApp.postman_collection.json
```

### PowerShell Testing

Complete PowerShell testing guide available in [API_DOCUMENTATION.md](postman/API_DOCUMENTATION.md)

### Test Data

Use the API to create test events, ticket types, and users for comprehensive testing.

## 🚀 Deployment

### Local Development
```bash
mvn spring-boot:run
```

### Production Build
```bash
mvn clean package
java -jar target/tickets-0.0.1-SNAPSHOT.jar
```

### Docker Deployment
See `docker-compose.yml` for containerized deployment setup.

## 📋 Recent Updates

### v1.0.0 (Current)
- ✅ **Fixed database schema issues**: Resolved QR code column mapping conflicts
- ✅ **Enhanced ticket purchasing**: Added quantity support (1-10 tickets per transaction)
- ✅ **Improved error handling**: Comprehensive error diagnostics with cause classification
- ✅ **Individual QR codes**: Each ticket gets unique QR code for validation
- ✅ **Breaking API changes**: Updated ticket purchase to return array of tickets

### Breaking Changes from Previous Versions
- **Ticket Purchase API**: Now requires JSON body with quantity, returns array instead of single ticket
- **Response Format**: Changed from 204 No Content to 201 Created with ticket data

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**Lakshaya**
- GitHub: [@your-github-username](https://github.com/your-github-username)

## 🙏 Acknowledgments

- Spring Boot Team for the excellent framework
- Keycloak Team for OAuth2/JWT implementation
- ZXing library for QR code generation
- MapStruct for entity mapping

## 📞 Support

For support and questions:
- Create an issue in the GitHub repository
- Check the comprehensive API documentation
- Review the error handling guide for troubleshooting

---

**Status**: ✅ Production Ready | 🔄 Actively Maintained | 📈 Feature Complete
