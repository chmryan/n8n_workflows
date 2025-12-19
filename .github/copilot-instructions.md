# GitHub Copilot Instructions

## Repository Context

This repository contains n8n workflow automation files in JSON format. These workflows are designed to be imported into n8n instances for various automation tasks.

## Working with n8n Workflows

### Workflow Structure
- n8n workflows are stored as JSON files
- Each workflow contains `nodes` (individual processing steps) and `connections` (data flow between nodes)
- Credentials are referenced by ID but actual secrets are not stored in the workflow files

### When Creating or Modifying Workflows

1. **Node Structure**:
   - Each node has: `id`, `name`, `type`, `typeVersion`, `position`, and `parameters`
   - Common node types: `httpRequest`, `code`, `if`, `splitOut`, `webhook`, `wait`
   - Use proper typeVersion for each node (check n8n documentation)

2. **Connections**:
   - Format: `"Source Node": {"main": [[{"node": "Target Node", "type": "main", "index": 0}]]}`
   - IF nodes have multiple outputs: index 0 (true), index 1 (false)
   - Ensure all nodes are properly connected

3. **Credentials**:
   - Reference credentials by name, not actual values
   - Document required credentials in the README
   - Use generic placeholders like "Bearer Auth [Service Name]"

4. **Best Practices**:
   - Use descriptive node names that explain their purpose
   - Add comments in Code nodes to explain complex logic
   - Keep workflows modular and reusable
   - Document required setup steps
   - Include example values where appropriate

### When Documenting Workflows

- Explain the purpose and use case
- List all required credentials
- Provide step-by-step setup instructions
- Document any configurable parameters
- Include the workflow flow/execution order
- Add notes about dependencies or external services

### Common n8n Patterns

- **Polling Loop**: Use IF node with loop-back connection to Wait/Delay node
- **Error Handling**: Use error output branches on nodes
- **Data Transformation**: Use Code node for complex JavaScript operations
- **Webhooks**: Use webhook trigger for external API integrations
- **Batch Processing**: Use Split In Batches for handling large datasets

### Testing Workflows

- Always test with sample data before deploying
- Verify credential configurations
- Check error handling paths
- Validate output data formats
- Test loop conditions to avoid infinite loops

## Repository Guidelines

- One workflow per JSON file
- Use descriptive filenames (kebab-case)
- Update README.md when adding new workflows
- Include version information if the workflow has dependencies
- Tag releases when making significant updates
