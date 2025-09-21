# Contributing to Event Booking App

Thank you for your interest in contributing to the Event Booking App! This document provides guidelines for contributing to this project.

## 🚀 Getting Started

### Prerequisites
- Java 21 or higher
- PostgreSQL 17.6
- Keycloak Server
- Maven 3.6+
- Git

### Development Setup

1. **Fork the repository**
2. **Clone your fork**
   ```bash
   git clone https://github.com/your-username/event-booking-app.git
   cd event-booking-app/Backend/tickets
   ```

3. **Set up the database**
   ```sql
   CREATE DATABASE Event_Booking_App_db;
   ```

4. **Configure application properties**
   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/Event_Booking_App_db
   spring.datasource.username=your_username
   spring.datasource.password=your_password
   ```

5. **Start Keycloak**
   - Run Keycloak on `http://localhost:9090`
   - Create realm: `event-ticket-platform`
   - Configure roles: ORGANIZER, ATTENDEE, STAFF

6. **Run the application**
   ```bash
   mvn spring-boot:run
   ```

## 📋 Development Guidelines

### Code Style
- Follow Java naming conventions
- Use Lombok annotations for boilerplate code
- Write meaningful commit messages
- Add JavaDoc for public methods
- Keep methods small and focused

### Testing
- Write unit tests for new features
- Test API endpoints using Postman collection
- Ensure all tests pass before submitting PR

### Database Changes
- Use Hibernate DDL auto-update for development
- Document any schema changes in PR description
- Test migrations thoroughly

## 🐛 Bug Reports

When reporting bugs, please include:
- Steps to reproduce
- Expected behavior
- Actual behavior
- Error messages or logs
- Environment details (OS, Java version, etc.)

## ✨ Feature Requests

For new features:
- Describe the feature and its benefits
- Provide use cases
- Consider backwards compatibility
- Discuss implementation approach

## 📝 Pull Request Process

1. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**
   - Write clean, well-documented code
   - Add tests for new functionality
   - Update documentation if needed

3. **Test thoroughly**
   - Run all existing tests
   - Test new functionality manually
   - Check API endpoints work correctly

4. **Commit your changes**
   ```bash
   git add .
   git commit -m "feat: add new feature description"
   ```

5. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

6. **Create Pull Request**
   - Provide clear description of changes
   - Reference any related issues
   - Include testing notes

## 🏷️ Commit Message Convention

Use conventional commits format:
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `style:` - Code style changes
- `refactor:` - Code refactoring
- `test:` - Adding tests
- `chore:` - Maintenance tasks

Examples:
```
feat: add quantity support for ticket purchases
fix: resolve QR code database constraint violation
docs: update API documentation for new endpoints
```

## 🔍 Code Review Checklist

Before submitting PR, ensure:
- [ ] Code follows project conventions
- [ ] All tests pass
- [ ] Documentation is updated
- [ ] No sensitive data in commits
- [ ] Error handling is comprehensive
- [ ] Security considerations addressed

## 🚫 What Not to Include

- Sensitive configuration data
- Database passwords
- JWT tokens or API keys
- IDE-specific files
- Compiled artifacts
- Personal configuration files

## 📞 Getting Help

- Check existing issues and documentation
- Ask questions in issue discussions
- Review API documentation and examples
- Test with provided Postman collection

## 🙏 Recognition

Contributors will be acknowledged in:
- README.md file
- Release notes for significant contributions
- GitHub contributor graph

Thank you for helping make Event Booking App better! 🎉
