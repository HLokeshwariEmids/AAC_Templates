
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
2. **Refresh**: Use the `/admin/refresh_templates` API endpoint to reload from GitHub
3. **Info**: Use `/admin/template_info` to see current template counts and source

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


## Backend Implementation

### 1. `agent_templates.py`
- **Pydantic Models:**  
  - `AgentTemplate`: Structure for main agent templates  
  - `SubAgentTemplate`: Structure for sub-agent templates

- **Core Functionality:**  
  - `load_templates()`: Loads templates from GitHub  
  - Loads at startup and refreshes on every request  
  - Endpoint `/admin/agent_templates` always fetches latest from GitHub

- **API Endpoints:**  
  - `GET /admin/agent_templates`: List all templates  
  - `GET /admin/agent_templates/categories`: Get all unique categories  
  - `GET /admin/subagents/{template_id}`: Get sub-agents for a template

### 2. `github_template_loader.py`
- **GitHub Integration:**  
  - Loads both main and sub-templates from [AAC_Templates GitHub](https://github.com/HLokeshwariEmids/AAC_Templates)

- **Methods:**  
  - `load_templates_from_github()`  
  - `_load_json_file()`  
  - `_get_file_content()`

- **Error Handling:**  
  - Detailed error/logging for network issues  
  - No fallback to local files (unless specified)

## Frontend Implementation

### 1. `ProjectSetupPage.jsx`
- **State Management:**  
  - `templates`, `loading`, `error`
- **Key Functions:**  
  - `fetchTemplates()`, `handleTemplateSelect()`, `handleProjectNameChange()`
- **UI:**  
  - Template dropdown, loading/error states, auto-populated form fields

### 2. `apiservices.js`
- **API Functions:**  
  - `getAgentTemplates()` – fetches with error handling, logging, and cache control

## Data Flow

1. **Initialization:** Backend loads templates from GitHub; frontend loads them on page load
2. **Template Selection:** User chooses template; fields auto-populate
3. **Template Refresh:** Backend fetches latest from GitHub on each API request; frontend can manually refresh  
4. **Error Handling:**  
   - Backend: error/logging, clear HTTP error responses  
   - Frontend: loading/error states, empty UI on failure

## API Endpoints

- `GET /admin/agent_templates` — Get all main templates
- `GET /admin/subagents/{template_id}` — Get sub-agents for a template
- `POST /admin/refresh_templates` — Refresh from GitHub
- `GET /admin/template_info` — Template info and counts
- `GET /admin/agent_templates/categories` — Unique template categories





