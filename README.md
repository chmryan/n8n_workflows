# n8n Workflows

Collection of n8n workflows for automation tasks.

## Workflows

### Firecrawl to Nextcloud

**File**: `firecrawl-to-nextcloud.json`

Crawls a website using the Firecrawl.dev API and uploads the extracted markdown documents to Nextcloud.

**Features:**
- Parametrized URL via JavaScript code node
- Polls Firecrawl API until crawl is completed
- Splits crawled documents into individual items
- Uploads each document to Nextcloud as markdown files

**Required Credentials:**
- Firecrawl.dev API (Bearer Auth)
- Nextcloud account

**Setup:**
1. Import the workflow JSON file into n8n
2. Configure your Firecrawl.dev API credentials
3. Configure your Nextcloud credentials
4. Update the URL in the "Set URL" node
5. Adjust the Nextcloud path in the "Upload to Nextcloud" node

**How it works:**
1. Manual trigger starts the workflow
2. Sets the URL to crawl (configurable)
3. Initiates crawl via Firecrawl API
4. Polls the status endpoint until crawl completes
5. Splits the crawled documents
6. Uploads each document to Nextcloud

## Import Instructions

1. Open your n8n instance
2. Go to **Workflows** → **Add workflow** → **Import from file**
3. Select the JSON file
4. Configure credentials as needed

## License

MIT
Collection of n8n workflows
