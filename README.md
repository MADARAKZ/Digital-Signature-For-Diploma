# DevLake Automation & Monitoring

Automated DevLake project creation and blueprint monitoring with intelligent failure detection.

## Purpose

Automates the complete DevLake workflow:

- **Plugin & Connection Discovery**: Automatically scans all configured DevLake plugins and connections
- **Repository Synchronization**: Syncs repositories from remote sources (GitHub, GitLab, Bitbucket, Jira, Azure)
- **Project & Blueprint Creation**: Creates DevLake projects and scope configs for each repository  
- **Data Collection Orchestration**: Runs data collection for each project with intelligent retry logic
- **Real-time Monitoring**: Monitors blueprint status and sends failure alerts via Telegram

## Workflow Overview

### 1. Automation Process (`python manage_projects.py`)
1. **Discovery**: Scans all configured plugins and connections
2. **Repository Sync**: Automatically syncs new repositories from remote sources (GitHub, GitLab, etc.)  
3. **Configuration**: Creates scope configs for repository connections
4. **Project Creation**: Creates DevLake projects and blueprints for each repository
5. **Time Update**: Updates blueprint timeAfter values to previous Saturday

### 2. Monitoring Process (`python blueprint_monitor.py`)
1. **Status Check**: Monitors blueprint pipeline status at configurable intervals
2. **Failure Detection**: Identifies failed data collection jobs
3. **Alerting**: Sends Telegram notifications for failures only
4. **Auto-Completion**: Automatically exits when all blueprints are finished (no more running/pending)

## Key Features

- **Automatic Discovery**: No manual configuration - scans all plugins and connections automatically
- **Remote Repository Sync**: Auto-adds new repos from GitHub/GitLab/Bitbucket/Azure/Jira
- **Smart Project Creation**: Each repository gets its own project and blueprint
- **Production Ready**: Robust error handling, retry mechanisms, comprehensive logging  
- **Smart Monitoring**: Automatically exits when all blueprints complete, only alerts on failures
- **Pagination Support**: Handles large repository sets efficiently
- **Flexible Deployment**: Run standalone, Docker, or Kubernetes Job
- **Modular Architecture**: Clean separation of concerns with organized code structure

## Production Features

- **Centralized Configuration**: All settings managed via environment variables
- **Robust Logging**: Structured logging with configurable levels and timezone support
- **Comprehensive Error Handling**: Graceful failure handling with retry mechanisms
- **API Rate Limiting**: Smart handling of API limits and timeouts
- **Telegram Integration**: Alerts only for failures and critical errors, no success notifications
- **Modular Architecture**: Clean separation of concerns across modules

## Usage

### 1. Automation (Run once to setup projects)

```bash
pip install -r requirements.txt
export API_BASE_URL=http://localhost:8080  
export TELEGRAM_BOT_TOKEN=your_bot_token
export TELEGRAM_CHAT_ID=your_chat_id
python manage_projects.py
```

### 2. Monitoring (Run until all blueprints complete)

```bash
python blueprint_monitor.py
```

*Note: Monitoring will automatically exit when all blueprints are finished*



## 3. Code Structure

### Entry Points
- **manage_projects.py**: Main entry point for project management workflow. Orchestrates project/blueprint creation and updates blueprint time windows.
- **blueprint_monitor.py**: Entry point for monitoring blueprint status. Automatically exits when all blueprints are finished, only sends failure alerts.

### Core Modules (`modules/`)
- **orchestrator.py**: High-level orchestration logic. Coordinates the entire workflow of syncing repos, creating configs, and projects across all plugins/connections.
- **api.py**: DevLake API client functions. Handles fetching plugins, connections, projects, and blueprints from the API.
- **projects.py**: Project and blueprint creation logic. Creates individual projects and blueprints for repositories with webhook integration.
- **connection.py**: Connection management. Handles scope configurations, repository syncing, and connection setup.
- **scopes.py**: Repository/scope management. Manages DevLake scopes, remote groups, and filtering for supported plugins.
- **webhooks.py**: Webhook connection management. Handles creation and retrieval of webhook connections.
- **monitoring.py**: Blueprint monitoring and alerting. Checks pipeline status, detects failures, sends Telegram alerts for failures only, and automatically exits when all work is complete.

### Utilities (`utils/`)
- **http.py**: HTTP client utilities with retry mechanisms and response validation.
- **logging.py**: Logging configuration and setup utilities.  
- **datetime.py**: Date/time utilities including timezone handling and Saturday calculations.
- **blueprints.py**: Blueprint time management utilities for updating timeAfter values.

### Configuration
- **config.py**: Centralized configuration management with environment variable validation.




