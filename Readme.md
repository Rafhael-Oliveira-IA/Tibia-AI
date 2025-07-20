# Tibia IA Helper - Developer Documentation Small Version

## System Overview

Tibia IA Helper is a web application that provides AI-powered tools for The Forgotten Server (TFS) developers. It features script generation, code analysis, forum interaction, and documentation for both modern and legacy TFS versions.

## Core System Architecture

```
Tibia IA Helper/
├── assets/             # Static assets (CSS, JS, images)
├── config/             # Configuration files
├── controllers/        # Controller classes
├── core/               # Core framework components
├── lang/               # Language files
├── models/             # Data models
├── scripts/            # Utility scripts
├── storage/            # App storage (cache, logs)
├── views/              # View templates
│   └── components/     # Reusable view components 
└── index.php           # Application entry point
```

## Key Components

### 1. MVC Framework

The system follows a custom MVC (Model-View-Controller) architecture:

- **Models**: Data layer & business logic (e.g., Forum.php, User.php)
- **Views**: Template files for rendering HTML
- **Controllers**: Process requests and coordinate between models and views

### 2. Core Classes

| Class | Description |
|-------|-------------|
| `Router` | Handles URL routing and HTTP requests |
| `Database` | Manages PDO database connections |
| `Auth` | Authentication and user management |
| `Language` | Internationalization (i18n) support |
| `View` | Template rendering engine |
| `GptClient` | OpenAI API integration |

### 3. Script Generation System

The script generation feature creates TFS scripts via a multi-step process:

1. User inputs script parameters and description
2. ScriptController processes the request
3. GptClient sends a context-aware prompt to the OpenAI API
4. Response is parsed, formatted, and displayed to the user

**Supported Script Types:**
- Actions
- Movements
- Creature Events
- Global Events
- Talk Actions
- Spells
- Weapons

**Supported Formats:**
- RevScriptSys (TFS 1.x)
- XML-based (TFS 1.x and 0.x)

### 4. Database Structure

Main database tables:

- `users` - User accounts
- `conversations` - Chat history
- `forum_categories` - Forum categories
- `forum_topics` - Forum threads
- `forum_posts` - Forum replies
- `script_analysis` - Script analysis results

### 5. Localization System

- Supports multiple languages via JSON files
- Currently implemented: English (en), Portuguese (pt)
- Uses `lang/en.json` and `lang/pt.json`
- URL parameter-based language switching

## Key Files Reference

### Configuration Files

- `config/config.php` - Main application settings
- `config/database.php` - Database connection parameters
- `config/routes.php` - URL routing definitions

### Core System Files

- `core/Router.php` - Routes HTTP requests
- `core/Database.php` - PDO database wrapper
- `core/Language.php` - i18n functionality
- `core/Auth.php` - Authentication & sessions
- `core/View.php` - Template rendering
- `core/GptClient.php` - OpenAI API client
- `core/helpers.php` - Global helper functions

### Controllers

- `controllers/HomeController.php` - Main page 
- `controllers/AuthController.php` - Login/registration
- `controllers/ScriptController.php` - Script generation
- `controllers/ForumController.php` - Forum functionality
- `controllers/TfsController.php` - TFS tools

### Assets

- `assets/css/main.css` - Core styles
- `assets/css/components/` - Component styles
- `assets/css/pages/` - Page-specific styles
- `assets/js/main.js` - Main JavaScript

## API Reference

### Authentication API

```php
// Login user
POST /login
Parameters: email, password

// Register user
POST /register
Parameters: username, email, password, password_confirm

// Logout
GET /logout
```

### Script Generator API

```php
// Generate script
POST /script/generate
Parameters: tfs_version, module_type, script_type, description, database

// Analyze script
POST /script/analyze
Parameters: script_content, tfs_version

// Review script
POST /script/review
Parameters: script_content, tfs_version
```

### Forum API

```php
// Create topic
POST /forum/topic/create
Parameters: category_id, title, content

// Create post
POST /forum/post/create
Parameters: topic_id, content

// Vote on post
POST /forum/post/{id}/vote
Parameters: vote_type
```

## Development Workflow

### Setup

1. Clone the repository
2. Import the database schema from `database_setup.sql`
3. Configure `config/database.php` and `config/config.php`
4. Ensure proper permissions for `storage/` directory
5. Start your PHP server pointing to the project directory

### Adding New Features

1. Create appropriate models in `models/`
2. Implement controller logic in `controllers/`
3. Create view templates in `views/`
4. Add CSS in `assets/css/pages/` or `assets/css/components/`
5. Update routes in `config/routes.php`

### Language Considerations

When adding new strings:
1. Add the string key to `lang/en.json`
2. Add the corresponding translation to other language files
3. Use `__('key')` in templates to display translated text

## Troubleshooting

### Common Issues

- **404 Errors**: Check `routes.php` and ensure the route is defined
- **Database Errors**: Verify credentials in `database.php`
- **White Screen**: Check error logs in `storage/logs/`
- **API Issues**: Confirm OpenAI API key is valid in `config.php`

### Diagnostic Tools

- `/test.php` - System test page
- `/config_check.php` - Configuration validation
- `/create_flags.php` - Recreate language flag images

## TFS Documentation

For reference, the system includes comprehensive documentation for both TFS versions:

- `Tibia-IA-TFS-1x.txt` - Documentation for TFS 1.x
- `Tibia-IA-TFS-0x-Legacy.txt` - Documentation for TFS 0.x

These files are referenced by the GPT client when generating scripts.

## Style Guide

The application uses a modern dark theme with these key style components:

- **Color Palette**:
  - Dark background (`--dark-bg: #0a0a0c`)
  - Surface elements (`--dark-surface: #121216`)
  - Primary accent (`--primary: #6366f1`)
  - Secondary accent (`--secondary: #22d3ee`)

- **Typography**:
  - Primary font: 'Inter'
  - Code font: 'Fira Code'
  - Base size: 16px

- **Components**:
  - Cards: Rounded corners with subtle border
  - Buttons: Primary, secondary variants
  - Forms: Clean inputs with validation states
  - Code blocks: Syntax highlighting

## Security Considerations

- CSRF protection via tokens for all forms
- PDO prepared statements for database queries
- Input validation and sanitization 
- Password hashing with bcrypt
- Session security with httponly cookies
- API rate limiting

## Future Development

Planned features:
- Script optimization recommendations
- Performance impact analysis
- Custom script templates
- Expanded forum functionality
- TFS 0.x full support for all script types

## License

This project is proprietary software. All rights reserved.
