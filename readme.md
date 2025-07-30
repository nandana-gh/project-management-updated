# Quality Assurance Project Management System

[![FastAPI](https://img.shields.io/badge/FastAPI-0.104.1-009688.svg?style=flat&logo=FastAPI)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-18.3.1-61DAFB.svg?style=flat&logo=React)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.5.3-3178C6.svg?style=flat&logo=TypeScript)](https://www.typescriptlang.org/)
[![Python](https://img.shields.io/badge/Python-3.11+-3776AB.svg?style=flat&logo=Python)](https://www.python.org/)

A comprehensive web-based project management system designed for aerospace and satellite development projects with role-based access control, activity tracking, and interactive progress monitoring.

## 🎯 Overview

The Quality Assurance Project Management System is a full-stack application that enables organizations to manage complex aerospace projects through structured workflows, user role management, and real-time progress tracking. The system supports both FPGA and Processor-based software development activities with dedicated subsystem management.

### Key Features

- **🔐 Role-Based Access Control (RBAC)** - Four distinct user roles with granular permissions
- **📊 Interactive Dashboards** - Real-time project progress visualization with Gantt charts
- **🛠️ Activity Management** - Track FPGA and Processor-based development activities
- **📈 Progress Monitoring** - Date-aware progress tracking with automatic status updates
- **🏗️ Project-Subsystem Mapping** - Structured project organization with subsystem assignments
- **📱 Responsive Design** - Modern UI with TailwindCSS and mobile-friendly interface
- **🔄 Real-time Updates** - Live progress updates with persistent state management

## 🏗️ System Architecture

### Architecture Pattern
**Model-View-Controller (MVC) with Context API**

```
┌─────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                       │
├─────────────────────────────────────────────────────────────┤
│  React Components (TSX)                                     │
│  ├── Header, Navigation, Login                              │
│  ├── ProjectManagement, ActivityManagement                  │
│  ├── UserManagement, Reports                                │
│  └── Context Providers & Hooks                              │
├─────────────────────────────────────────────────────────────┤
│                    BUSINESS LOGIC LAYER                     │
├─────────────────────────────────────────────────────────────┤
│  TypeScript Interfaces & Types                             │
│  ├── User, Project, Subsystem Types                        │
│  ├── Activity, Progress Types                              │
│  └── Business Rule Validation                              │
├─────────────────────────────────────────────────────────────┤
│                    API LAYER                                │
├─────────────────────────────────────────────────────────────┤
│  FastAPI Backend                                           │
│  ├── RESTful Endpoints                                     │
│  ├── JWT Authentication                                    │
│  ├── Pydantic Validation                                  │
│  └── SQLAlchemy ORM                                       │
├─────────────────────────────────────────────────────────────┤
│                    DATA ACCESS LAYER                        │
├─────────────────────────────────────────────────────────────┤
│  SQLite Database                                           │
│  ├── 6 Core Tables with Relationships                     │
│  ├── Referential Integrity                                │
│  └── Business Rule Enforcement                            │
└─────────────────────────────────────────────────────────────┘
```

## 🗄️ Database Schema

### Core Tables

1. **users** - User accounts and role-based access control
2. **projects** - Project information and metadata  
3. **subsystems** - Available subsystems (POWER, TM, TC, AOCE, OBC)
4. **activities** - FPGA and Processor activities with associations
5. **project_subsystem_mappings** - One-to-one project-subsystem relationships
6. **project_progress** - User progress tracking on activities

### Entity Relationships

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│     USERS       │    │    PROJECTS     │    │   SUBSYSTEMS    │
├─────────────────┤    ├─────────────────┤    ├─────────────────┤
│ user_id (PK)    │◄──┐│ project_id (PK) │    │ subsystem_id(PK)│
│ username        │   ││ project_name    │    │ subsystem_name  │
│ password        │   ││ program_type    │    │ description     │
│ role            │   ││ description     │    │ created_at      │
│ created_at      │   ││ created_by (FK) │    │ updated_at      │
│ updated_at      │   │└─────────────────┘    └─────────────────┘
└─────────────────┘   │         │                       │
         │             │         │                       │
         │             │         ▼                       │
         │             │┌─────────────────────────────────┴──┐
         │             ││  PROJECT_SUBSYSTEM_MAPPINGS       │
         │             │├───────────────────────────────────┤
         │             ││ mapping_id (PK)                   │
         │             ││ project_id (FK)                   │
         │             ││ subsystem_id (FK)                 │
         │             ││ assigned_by (FK)                  │
         │             │└───────────────────────────────────┘
         │             │
         ▼             ▼
┌─────────────────┐    ┌─────────────────┐
│   ACTIVITIES    │    │ PROJECT_PROGRESS│
├─────────────────┤    ├─────────────────┤
│ activity_id(PK) │◄───┤ progress_id(PK) │
│ activity_name   │    │ project_id (FK) │
│ activity_type   │    │ subsystem_id(FK)│
│ associated_with │    │ activity_id(FK) │
│ description     │    │ user_id (FK)    │
│ created_at      │    │ status          │
│ updated_at      │    │ start_date      │
└─────────────────┘    │ completion_date │
                       │ notes           │
                       │ created_at      │
                       │ updated_at      │
                       └─────────────────┘
```

## 🚀 Getting Started

### Prerequisites

- **Python 3.11+** with pip
- **Node.js 18+** with npm
- **Git** for version control

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd project-management-updated
   ```

2. **Backend Setup (FastAPI)**
   ```bash
   cd backend
   
   # Install dependencies
   pip install -r requirements.txt
   
   # Start the development server
   uvicorn main:app --reload --host 0.0.0.0 --port 8000
   
   # Alternative using run.py
   python run.py
   ```

3. **Frontend Setup (React + Vite)**
   ```bash
   cd project
   
   # Install dependencies
   npm install
   
   # Start the development server
   npm run dev
   ```

4. **Access the Application**
   - **Frontend**: http://localhost:5173
   - **Backend API**: http://localhost:8000
   - **API Documentation**: http://localhost:8000/docs

### Default Login Credentials

| Role | Username | Password |
|------|----------|----------|
| Admin | admin | admin123 |
| Project Manager | pm1 | pm123 |
| Deputy Project Director | dpd1 | dpd123 |
| Engineer | eng1 | eng123 |

## 💻 Tech Stack

### Backend

- **Framework**: FastAPI 0.104.1
- **Database**: SQLite (with SQLAlchemy ORM)
- **Authentication**: JWT (JSON Web Tokens)
- **Validation**: Pydantic
- **Server**: Uvicorn ASGI server

### Frontend

- **Framework**: React 18.3.1 with TypeScript
- **Build Tool**: Vite 7.0.6
- **Styling**: TailwindCSS 3.4.1
- **Charts**: Chart.js 4.5.0 with react-chartjs-2
- **Icons**: Lucide React 0.344.0
- **HTTP Client**: Axios 1.11.0
- **Date Handling**: date-fns 4.1.0

### Development Tools

- **TypeScript**: 5.5.3 for type safety
- **ESLint**: Code linting and formatting
- **PostCSS**: CSS processing
- **Autoprefixer**: CSS vendor prefixing

## 👥 User Roles & Permissions

### 1. **ADMIN** (System Administrator)
- ✅ **User Management**: Create, edit, delete users
- ✅ **System Access**: Full system access
- ❌ **Project Creation**: Cannot create projects
- ❌ **Subsystem Assignment**: Cannot assign subsystems

### 2. **PM** (Project Manager)
- ✅ **Project Management**: Create, edit projects
- ✅ **Subsystem Assignment**: Assign projects to subsystems
- ✅ **Activity Management**: Add, edit activities
- ✅ **Team Oversight**: View all team progress
- ❌ **User Management**: Cannot manage users

### 3. **DPD** (Deputy Project Director)
- ✅ **Project Creation**: Create new projects
- ✅ **Subsystem Assignment**: Assign projects to subsystems
- ✅ **Progress Monitoring**: View-only access to progress
- ❌ **Activity Management**: Cannot modify activities
- ❌ **User Management**: Cannot manage users

### 4. **ENGINEER**
- ✅ **Progress Updates**: Update own activity progress
- ✅ **Date Management**: Set start and completion dates
- ✅ **Activity Tracking**: Mark activities as started/completed
- ❌ **Project Management**: Cannot create/edit projects
- ❌ **User Management**: Cannot manage users

## 📋 Features Overview

### 1. **Dashboard & Navigation**
- Role-based navigation menu
- User profile display with role indicators
- Real-time system status

### 2. **Project Management**
- Create projects with metadata (name, type, description)
- Mandatory subsystem assignment (1:1 mapping)
- Project overview with progress indicators

### 3. **Activity Management**
- **Activity Types**: FPGA-based and Processor-based software
- **Association Levels**: Project-level and Subsystem-level activities
- **Progress Tracking**: Three states (Not Started, In Progress, Completed)
- **Date Management**: Manual and automatic date setting

### 4. **User Management** (Admin Only)
- Create, edit, delete user accounts
- Role assignment and management
- User activity monitoring

### 5. **Reporting & Analytics**
- **Interactive Gantt Charts**: Timeline visualization with Chart.js
- **Progress Reports**: Activity completion status
- **Date-based Analytics**: Start dates, completion dates, duration tracking
- **Visual Differentiation**: Actual vs. estimated progress indicators

### 6. **Project Selection & Context**
- Project and subsystem selection interface
- Context-aware activity display
- Filtered views based on selections

## 🔧 Development

### Project Structure

```
project-management-updated/
├── backend/                     # FastAPI Backend
│   ├── __init__.py
│   ├── main.py                 # FastAPI application entry point
│   ├── models.py               # SQLAlchemy database models
│   ├── schemas.py              # Pydantic data validation schemas
│   ├── crud.py                 # Database CRUD operations
│   ├── database.py             # Database connection and session management
│   ├── run.py                  # Development server runner
│   ├── requirements.txt        # Python dependencies
│   └── project_management.db   # SQLite database file
│
├── project/                    # React Frontend
│   ├── src/
│   │   ├── components/         # React components
│   │   │   ├── Header.tsx
│   │   │   ├── Navigation.tsx
│   │   │   ├── Login.tsx
│   │   │   ├── ProjectManagement.tsx
│   │   │   ├── ActivityManagement.tsx
│   │   │   ├── UserManagement.tsx
│   │   │   ├── Reports.tsx
│   │   │   └── ProjectSelection.tsx
│   │   ├── context/
│   │   │   └── AppContext.tsx  # Global state management
│   │   ├── types/
│   │   │   └── index.ts        # TypeScript type definitions
│   │   ├── data/
│   │   │   └── initialData.ts  # Default data and activities
│   │   ├── App.tsx             # Main application component
│   │   ├── main.tsx            # React entry point
│   │   └── index.css           # Global styles
│   ├── package.json            # Node.js dependencies
│   ├── vite.config.ts          # Vite configuration
│   ├── tailwind.config.js      # TailwindCSS configuration
│   └── tsconfig.json           # TypeScript configuration
│
├── database/                   # Database documentation
│   ├── README.md
│   ├── table-relationships.md
│   └── docs/
│       ├── DATABASE_OPERATIONS.md
│       └── TECHNICAL_REPORT.md
│
├── setup_instructions.md       # Quick setup guide
└── README.md                   # This file
```

### API Endpoints

#### Authentication
- `POST /auth/login` - User authentication
- `POST /auth/logout` - User logout
- `GET /auth/me` - Get current user info

#### Users
- `GET /users` - List all users (Admin only)
- `POST /users` - Create new user (Admin only)
- `PUT /users/{user_id}` - Update user (Admin only)
- `DELETE /users/{user_id}` - Delete user (Admin only)

#### Projects
- `GET /projects` - List all projects
- `POST /projects` - Create new project (PM/DPD only)
- `PUT /projects/{project_id}` - Update project
- `DELETE /projects/{project_id}` - Delete project (Admin only)

#### Activities
- `GET /activities` - List all activities
- `POST /activities` - Create new activity (PM/Engineer)
- `PUT /activities/{activity_id}` - Update activity
- `DELETE /activities/{activity_id}` - Delete activity

#### Progress
- `GET /progress` - Get user progress
- `POST /progress` - Update progress (Engineers only)
- `PUT /progress/{progress_id}` - Modify progress entry

### State Management

The application uses React Context API for global state management:

```typescript
interface AppState {
  auth: {
    isAuthenticated: boolean;
    user: User | null;
  };
  users: User[];
  projects: Project[];
  subsystems: Subsystem[];
  activities: Activity[];
  progress: ProjectProgress[];
  selectedProject: string | null;
  selectedSubsystem: string | null;
}
```

### Activity Categories

#### FPGA Project Activities
- Preliminary Design Review (PDR)
- Critical Design Review (CDR)
- Test Readiness Review (TRR)
- Flight Readiness Review (FRR)

#### FPGA Subsystem Activities
- Requirements Analysis
- Design Implementation
- Unit Testing
- Integration Testing

#### Processor Project Activities
- Architecture Design
- Code Development
- System Integration
- Performance Testing

#### Processor Subsystem Activities
- Module Development
- Code Review
- Debugging
- Documentation

## 🔒 Security Features

### Authentication
- JWT-based stateless authentication
- Secure password handling (ready for bcrypt hashing)
- Session management with automatic logout

### Authorization
- Role-based access control (RBAC)
- Component-level permission checks
- API endpoint protection

### Data Security
- Input validation with Pydantic schemas
- SQL injection prevention with SQLAlchemy ORM
- XSS protection with React's built-in sanitization

## 📊 Gantt Chart Features

### Visual Elements
- **Green Bars**: Activities with actual start dates
- **Yellow Bars**: Activities with estimated dates (shown with "~" prefix)
- **Duration Display**: Shows actual days for completed activities
- **Timeline Calculation**: Smart positioning based on project timeline

### Functionality
- Interactive timeline with date ranges
- Proportional bar sizing based on actual duration
- Visual distinction between planned vs. actual progress
- Automatic timeline bounds calculation
- Responsive design for different screen sizes

## 🚀 Deployment

### Production Deployment

1. **Backend Deployment**
   ```bash
   # Install production dependencies
   pip install -r requirements.txt
   
   # Set environment variables
   export SECRET_KEY="your-production-secret-key"
   export DATABASE_URL="your-production-database-url"
   
   # Run with Gunicorn
   gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app
   ```

2. **Frontend Deployment**
   ```bash
   # Build for production
   npm run build
   
   # Serve with nginx or any static file server
   # Built files will be in the 'dist' directory
   ```

### Environment Variables

Create a `.env` file in the backend directory:

```env
SECRET_KEY=your-super-secret-jwt-key
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
DATABASE_URL=sqlite:///./project_management.db
CORS_ORIGINS=http://localhost:5173,http://localhost:3000
```

## 🛠️ Development Commands

### Backend
```bash
# Start development server
uvicorn main:app --reload

# Run with custom port
uvicorn main:app --reload --port 8080

# Generate requirements.txt
pip freeze > requirements.txt

# Database operations (if using migrations)
alembic init alembic
alembic revision --autogenerate -m "Initial migration"
alembic upgrade head
```

### Frontend
```bash
# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Run linting
npm run lint

# Type checking
npx tsc --noEmit
```

## 🔄 Future Enhancements

### Phase 1: Backend Integration
- [ ] MySQL/PostgreSQL database migration
- [ ] Advanced authentication (OAuth, SAML)
- [ ] Real-time notifications with WebSockets
- [ ] File upload and document management

### Phase 2: Advanced Features
- [ ] Advanced reporting with PDF/Excel export
- [ ] Audit trail and activity logging
- [ ] Email notifications for project updates
- [ ] Advanced user permissions and groups

### Phase 3: Enterprise Features
- [ ] Multi-tenancy support
- [ ] Mobile application (React Native)
- [ ] Advanced analytics and machine learning insights
- [ ] Integration with external project management tools

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow TypeScript best practices
- Use proper component structure and hooks
- Implement proper error handling
- Add comprehensive comments for complex logic
- Follow the existing code style and formatting
- Test your changes thoroughly
