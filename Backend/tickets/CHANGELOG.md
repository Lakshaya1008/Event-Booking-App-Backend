# Changelog

All notable changes to the Event Booking App will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-09-21

### Added
- Complete Event Booking App with Spring Boot 3.5.5
- OAuth2 JWT authentication with Keycloak integration
- Role-based access control (ORGANIZER, ATTENDEE, STAFF)
- Event management with flexible ticket types
- Quantity-based ticket purchasing (1-10 tickets per transaction)
- Automatic QR code generation for each ticket
- Ticket validation system with manual and QR scan methods
- Sales analytics and attendee reporting
- Comprehensive error handling with detailed diagnostics
- Complete REST API with OpenAPI documentation
- PostgreSQL database integration with JPA/Hibernate
- User provisioning filter for automatic user creation
- MapStruct-based entity-DTO mapping

### API Endpoints
- Event Management (ORGANIZER role)
- Ticket Type Management (ORGANIZER role)
- Published Events Browsing (ATTENDEE role)
- Ticket Purchase with Quantity Support (ATTENDEE role)
- User Ticket Management (ATTENDEE role)
- Ticket Validation (STAFF role)

### Security Features
- JWT-based authentication
- Role-based authorization
- Resource ownership validation
- Custom security error handlers with detailed diagnostics

### Database Schema
- Events with organizer relationships
- Flexible ticket types with individual availability
- Individual tickets with QR codes
- Ticket validation history tracking
- User management with role support

### Breaking Changes
- Ticket purchase API now requires JSON body with quantity
- Response format changed from 204 No Content to 201 Created with ticket array
- Enhanced error responses with comprehensive diagnostic information

### Technical Improvements
- Fixed QR code database constraint violations
- Enhanced error handling with client/server issue classification
- Atomic transaction handling for multiple ticket purchases
- Database schema auto-fix capabilities
- Comprehensive API documentation

## [Unreleased]

### Planned Features
- Email notifications for ticket purchases
- Event capacity management at venue level
- Seat selection for reserved seating events
- Payment integration
- Mobile app support
- Advanced analytics dashboard
