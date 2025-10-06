
## Directory Structure

```
AAC_Templates/
├── templates/
│   ├── main_templates.json
│   └── sub_templates.json
└── README.md
```

## File Descriptions

### `templates/main_templates.json`
Contains the main agent templates with the following structure:
```json
[
  {
    "id": "unique_template_id",
    "name": "Display Name for Template",
    "agent_name": "AgentClassName",
    "orchestrator_prompt": "Main orchestrator prompt text...",
    "final_response_prompt": "Final response formatting prompt...",
    "description": "Brief description of what this agent does",
    "category": "Category Name"
  }
]
```

### `templates/sub_templates.json`
Contains sub-agent templates organized by main template ID:
```json
{
  "main_template_id": [
    {
      "id": "sub_agent_id",
      "name": "SubAgentName",
      "worker_prompt": "Specific worker prompt for this sub-agent...",
      "description": "What this sub-agent specializes in"
    }
  ]
}
```

## How It Works

1. **Automatic Loading**: The system automatically loads templates from your GitHub repository
2. **Fallback**: If GitHub is unavailable, it falls back to local hardcoded templates
3. **Refresh**: Use the `/admin/refresh_templates` API endpoint to reload from GitHub
4. **Info**: Use `/admin/template_info` to see current template counts and source

## Setup Instructions

1. Create the repository structure as shown above
2. Copy the example JSON files to your repository
3. Customize the templates with your specific content
4. Push to GitHub
5. The system will automatically load your templates

## API Endpoints

- `GET /admin/agent_templates` - Get all main templates
- `GET /admin/subagents/{template_id}` - Get sub-agents for a template
- `POST /admin/refresh_templates` - Refresh from GitHub
- `GET /admin/template_info` - Get template information





Backend Implementation
1. 
agent_templates.py
Key Components:
Pydantic Models:
AgentTemplate
: Defines the structure for main agent templates
SubAgentTemplate
: Defines the structure for sub-agent templates
Core Functionality:
Template Loading:
load_templates()
: Fetches templates from GitHub using github_loader
Initial loading of templates at startup
Automatic refresh on each API call to /agent_templates
API Endpoints:
GET /admin/agent_templates:
Returns all available agent templates
Automatically refreshes templates from GitHub
Includes detailed error handling and logging
GET /admin/agent_templates/categories:
Returns unique categories from available templates
GET /admin/subagents/{template_id}:
Returns sub-agents for a specific template
2. 
github_template_loader.py
Key Features:
GitHub Integration:
Loads templates from https://github.com/HLokeshwariEmids/AAC_Templates
Supports both main templates and sub-templates
Core Methods:
load_templates_from_github()
: Main method to load all templates
_load_json_file()
: Helper to load individual JSON files from GitHub
_get_file_content(): Fetches raw content from GitHub
Error Handling:
Comprehensive error handling for network issues
Detailed logging for debugging
No fallback to local templates (as per requirement)
Frontend Implementation
1. 
ProjectSetupPage.jsx
State Management:
templates: Stores loaded agent templates
loading: Tracks loading state
error: Stores any error messages
Key Functions:
fetchTemplates(): Fetches templates from the backend
handleTemplateSelect(): Handles template selection
handleProjectNameChange()
: Manages project name changes
UI Components:
Template selection dropdown
Loading and error states
Form fields that auto-populate based on selected template
2. 
apiservices.js
API Functions:
getAgentTemplates()
: Fetches templates from the backend
Includes caching control headers
Detailed error handling and logging
Data Flow
Initialization:
Backend loads templates from GitHub on startup
Frontend loads available templates when the page loads
Template Selection:
User selects a template from the dropdown
Form fields auto-populate with template data
User can modify the fields if needed
Template Refresh:
Backend checks for updates from GitHub on each request
Frontend can manually refresh templates if needed
Error Handling
Backend:
Detailed error messages for GitHub fetch failures
Logging of all operations
Clear error responses with status codes
Frontend:
Loading states during API calls
Error messages for failed requests
Fallback UI for empty states
