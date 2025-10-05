
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
