---
applyTo: '**'
---
Provide project context and coding guidelines that AI should follow when generating code, answering questions, or reviewing changes.

## Tool Usage Guidelines

### MCP Tools Priority
- **Always prioritize MCP (Model Context Protocol) tools when available** for any operation that can be performed through them
- MCP tools provide enhanced capabilities and better integration with external services
- When multiple tools can accomplish the same task, prefer MCP tools over standard tools
- Examples of MCP tool categories to prioritize:
  - Git operations: Use `mcp_git-mcp-serve_*` tools instead of terminal git commands
- Only fall back to alternative approaches if MCP tools are unavailable or insufficient for the specific task

### Required MCP Tools Usage
- **Always use `mcp_sequentialthi_sequentialthinking`** for complex problem-solving, analysis, and multi-step tasks
- **Always use memory MCP tools** (`mcp_memory_*`) to store and retrieve important context, entities, and relationships
- These tools should be used proactively to enhance reasoning and maintain context across conversations

## Language Guidelines

### Code Language Standards
- **All code must be written in English (en-US)** including:
  - Variable names, function names, class names
  - Comments and documentation
  - Method signatures and parameters
  - Configuration keys and constants
  - File names and folder names
  - API endpoints and routes
  - Database column names and table names
- **Only user-facing strings should be in Portuguese (pt-BR)** including:
  - Text displayed in the UI (labels, buttons, messages)
  - Error messages shown to users
  - Validation messages
  - Help text and tooltips
  - Content in templates and components that users see
- **Technical strings remain in English** including:
  - Log messages for developers
  - Console outputs
  - Technical error codes
  - Configuration values
  - API response keys (unless specifically for user display)

## Code Modification Guidelines

### Comment Management
- **Do not add comments** when modifying or creating code
- **Always remove existing comments** when modifying code
- Keep code clean and self-documenting without inline comments
- Only preserve comments if explicitly requested by the user
- Focus on writing clear, readable code that doesn't require comments to understand

## Documentation Guidelines
