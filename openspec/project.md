# Project Context

## Purpose
LTI (Talent Tracking System) is a full-stack recruitment and talent management application designed to help recruiters track candidates through the hiring process. The system manages candidate profiles, positions, applications, interviews, and the recruitment workflow.

**Key Goals:**
- Manage candidate information including personal details, education, work experience, and resumes
- Track positions and job openings
- Manage applications and interview processes
- Provide a recruiter dashboard for visualizing candidates in different stages of the hiring pipeline

## Tech Stack

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Language**: TypeScript (strict mode enabled)
- **ORM**: Prisma
- **Database**: PostgreSQL
- **Testing**: Jest with ts-jest
- **API Documentation**: Swagger/OpenAPI (swagger-jsdoc, swagger-ui-express)
- **File Upload**: Multer

### Frontend
- **Framework**: React 18
- **Language**: TypeScript
- **Build Tool**: Create React App
- **Routing**: React Router DOM
- **UI Library**: React Bootstrap, Bootstrap 5
- **Drag & Drop**: react-beautiful-dnd, react-dnd
- **Date Handling**: react-datepicker
- **Testing**: Jest, React Testing Library

### Infrastructure & DevOps
- **Containerization**: Docker, Docker Compose
- **Cloud**: AWS EC2
- **CI/CD**: GitHub Actions
- **Process Manager**: PM2 (production)

## Project Conventions

### Code Style
- **Language**: All code, comments, documentation, and commit messages MUST be in English
- **TypeScript**: Strict mode enabled (`strict: true` in tsconfig.json)
- **Naming Conventions**:
  - **Files**: camelCase for utilities/services, PascalCase for components/classes
  - **Variables/Functions**: camelCase
  - **Classes/Interfaces**: PascalCase
  - **Constants**: UPPER_SNAKE_CASE
  - **Database**: camelCase for fields, PascalCase for models (Prisma convention)
- **Formatting**: Prettier and ESLint configured
- **Type Safety**: All code must be fully typed; avoid `any` types where possible
- **Clear Naming**: Use descriptive, self-documenting names for all variables and functions

### Architecture Patterns

#### Domain-Driven Design (DDD)
The project follows Domain-Driven Design principles:

- **Entities**: Objects with unique identity (e.g., `Candidate`, `Position`)
- **Value Objects**: Objects describing domain aspects without identity (e.g., `Education`, `WorkExperience` within Candidate context)
- **Aggregates**: `Candidate` acts as aggregate root containing `Education`, `WorkExperience`, `Resume`, and `Application`
- **Repositories**: Interfaces for data access (partially implemented, should be extended)
- **Domain Services**: Business logic that doesn't belong to entities (e.g., `CandidateService`)

#### Layered Architecture
Backend follows a layered architecture:

```
backend/src/
├── domain/          # Domain models and business logic
├── application/     # Application services and use cases
├── presentation/   # Controllers and HTTP handling
├── routes/         # Route definitions
└── infrastructure/ # Database access (Prisma)
```

#### SOLID Principles
- **Single Responsibility**: Each class should have one reason to change
- **Open/Closed**: Open for extension, closed for modification
- **Liskov Substitution**: Derived classes must be substitutable
- **Interface Segregation**: Prefer specific interfaces over general ones
- **Dependency Inversion**: Depend on abstractions, not concretions

#### DRY (Don't Repeat Yourself)
- Centralize common logic (e.g., validation, database operations)
- Use factory functions for test data
- Abstract repetitive patterns into reusable functions/classes

### Testing Strategy

#### Framework & Configuration
- **Framework**: Jest with ts-jest preset
- **Coverage Threshold**: 90% for branches, functions, lines, and statements
- **Test Location**: Test files alongside source code (`*.test.ts`)

#### Test Organization
- **Structure**: Use AAA pattern (Arrange-Act-Assert)
- **Naming**: `should_[expected_behavior]_when_[condition]`
- **Grouping**: Use descriptive `describe` blocks
- **Test Data**: Use factory functions for creating test data

#### Test Patterns
```typescript
describe('[ComponentName] - [methodName]', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  describe('should_[expected_behavior]_when_[condition]', () => {
    it('should [specific test case]', async () => {
      // Arrange
      // Act  
      // Assert
    });
  });
});
```

#### Testing Approach
- **TDD**: Start with failing tests for new functionality
- **Unit Tests**: Test individual functions/methods in isolation
- **Integration Tests**: Test controller/database interactions
- **Mocking**: Mock external dependencies (database, APIs)
- **Async Testing**: Always use `async/await` for asynchronous operations

### Git Workflow

#### Branching Strategy
- **Base Branch**: `main` or `develop` (check project default)
- **Feature Branches**: `feature/[ticket-id]-backend` or `feature/[ticket-id]-frontend`
  - Separate branches for frontend and backend work
  - Use ticket ID (e.g., `SCRUM-10`) in branch name
- **Workflow**: Fork-based workflow for students/contributors

#### Commit Conventions
- **Language**: Commit messages in English
- **Format**: Descriptive commit messages that explain what and why
- **Scope**: Stage only files affected by the ticket/change
- **PR Linking**: Include ticket ID in PR title/description for Jira linking

#### Development Process
1. Create feature branch from base branch
2. Implement changes following TDD approach
3. Ensure all tests pass
4. Run linting and type checking
5. Stage only affected files
6. Create descriptive commit
7. Push and create PR with ticket ID

#### Pre-PR Checklist
- [ ] Application builds without errors
- [ ] All tests pass successfully
- [ ] CI/CD pipeline runs without errors
- [ ] Code passes linting and type checking
- [ ] Changes documented
- [ ] Evidence of working functionality included

## Domain Context

### Core Entities
- **Candidate**: Person applying for positions (aggregate root)
  - Personal information (name, email, phone, address)
  - Education history
  - Work experience
  - Resume/CV files
  - Applications to positions

- **Position**: Job opening being recruited for
  - Position details and requirements
  - Associated applications

- **Application**: Relationship between Candidate and Position
  - Tracks candidate's progress through interview stages

- **Interview**: Interview sessions for applications
  - Interview types and flows
  - Interview steps and stages

### Business Rules
- Candidates can apply to multiple positions
- Each application has an interview flow with multiple steps
- Candidates move through stages (e.g., initial screening, technical interview, final interview)
- File uploads supported for resumes/CVs

### Data Model
- See `backend/ModeloDatos.md` for detailed data model documentation
- Prisma schema: `backend/prisma/schema.prisma` (single source of truth for database structure)

## Important Constraints

### Technical Constraints
- **TypeScript Strict Mode**: Must maintain strict typing throughout
- **Test Coverage**: 90% threshold must be maintained
- **English Only**: All technical artifacts must be in English
- **Database Migrations**: All database changes must be version-controlled through Prisma migrations

### Development Constraints
- **Small Incremental Changes**: Work in baby steps, one at a time
- **TDD Required**: Start with failing tests for new functionality
- **No Credentials in Code**: Never commit secrets or credentials
- **Fork-Based Workflow**: Students must work on personal forks before creating PRs

### Business Constraints
- **Fork Validation**: All functionality must be validated on personal fork before PR
- **CI/CD Evidence**: PRs must include evidence of successful CI/CD pipeline
- **AWS Resources**: Each student must use their own AWS resources for testing

## External Dependencies

### Services & APIs
- **PostgreSQL Database**: Managed via Docker Compose locally, PostgreSQL on EC2 for production
- **AWS EC2**: Cloud hosting for production deployment
- **GitHub Actions**: CI/CD pipeline automation
- **GitHub**: Version control and PR workflow

### Key Libraries
- **Prisma Client**: Database ORM and query builder
- **Express**: Web framework for API
- **React**: UI framework
- **React Router**: Client-side routing
- **Multer**: File upload handling
- **Swagger**: API documentation

### Environment Variables
Required environment variables (configured via `.env` files):
- `DATABASE_URL`: PostgreSQL connection string
- `DB_PASSWORD`, `DB_USER`, `DB_NAME`, `DB_PORT`: Database configuration (Docker Compose)
- AWS credentials (via GitHub Secrets for CI/CD)

### API Documentation
- API specifications: `backend/api-spec.yaml` (OpenAPI 3.0)
- Swagger UI available at runtime for API exploration
