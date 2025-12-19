# GitHub Copilot Instructions

## Repository Context

This repository contains n8n workflow automation files in JSON format. These workflows are designed to be imported into n8n instances for various automation tasks.

## MCP Permissions

- You are permitted to read, validate, and update the corresponding workflow on the connected n8n instance via MCP tooling.
- Specifically, you may update or create any workflow ID on the instance at https://n8n.iosys.ch using the latest JSON files from this repository. If a workflow ID does not exist on the instance, you may create it. If you are unsure about the specific workflow ID, ask the user for clarification.
- **IMPORTANT**: Direct API access to n8n is strictly forbidden. You MUST use MCP tooling exclusively for all n8n operations.
- Use MCP list/validate tools when available, and perform updates via the instance's supported MCP mechanisms only.

## Working with n8n Workflows

### Workflow Structure
- n8n workflows are stored as JSON files
- Each workflow contains `nodes` (individual processing steps) and `connections` (data flow between nodes)
- Credentials are referenced by ID but actual secrets are not stored in the workflow files

### Workflow Update Workflow (Critical)

**ALWAYS follow this process when updating a workflow on the n8n instance:**
1. **Fetch Latest Version**: Before modifying any workflow JSON, use MCP `n8n_get_workflow` to fetch the latest version from the n8n instance for the specific workflow ID
2. **Compare with Local JSON**: Compare the fetched version with the local JSON in the repository to identify what has changed since the last deployment
3. **Merge Changes**: If the instance version has modifications not in local JSON, incorporate those changes into your update or ask the user for clarification
4. **Update Local JSON**: After fetching and comparing, update the local JSON file in the repository with your changes
5. **Deploy to Instance**: Use MCP `n8n_update_full_workflow` or `n8n_update_partial_workflow` to deploy the updated workflow
6. **Verify**: Validate the workflow via MCP tools before and after deployment to ensure integrity

This ensures the instance workflow never loses any concurrent modifications and maintains a single source of truth.

### Error Diagnosis and Debugging

**When a user reports an error or issue with a workflow:**
1. **Fetch Execution History**: Use MCP `n8n_executions` with `action='list'` and the workflow ID to retrieve recent executions
2. **Identify Failed Execution**: Look for executions with status `'error'` to find the most recent failure
3. **Get Full Error Details**: Use MCP `n8n_executions` with `action='get'` and the execution ID to fetch complete error information, including:
   - Full error message and stack trace
   - Node where the error occurred
   - Input/output data for that node
   - Error context and timing information
4. **Analyze Root Cause**: Use the execution output to diagnose the issue:
   - Check if it's an API error (HTTP status code, response body)
   - Verify node parameters and expressions
   - Confirm credential configuration
   - Look for data type mismatches
5. **Fix and Re-test**: After making corrections, trigger a test execution and verify the fix

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
