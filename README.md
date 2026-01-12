# Apiiro Guardian Agent MCP Server

Apiiro Guardian Agent enables AI coding assistants to leverage Application Security Posture Management (ASPM) capabilities through the Model Context Protocol (MCP). This integration allows developers to write secure code, query security risks, analyze repositories, trigger diff scans, and manage security findings directly from their AI assistant.

## Overview

This MCP server connects AI assistants to Apiiro's security analysis platform, providing real-time security insights during development. The integration exposes Apiiro's security capabilities through MCP tools that can be invoked by supported AI coding assistants.

## Supported Integrations

### Google Gemini Extension

The Gemini extension is configured through `gemini-extension.json` and provides access to Apiiro's security analysis tools within the Gemini CLI environment.

**Configuration:**
- Extension manifest: `gemini-extension.json`
- Server endpoint: `https://app.apiiro.com/mcp/server`
- Authentication: Requires `APIIRO_API_TOKEN` environment variable

## Security Capabilities

The integration provides access to:

- Write secure code with real-time security guidance
- Query security risks across repositories
- Trigger differential scans on code changes
- Analyze repository security posture
- Manage and remediate security findings
- Application Security Posture Management (ASPM) analysis

## How It Works

The Apiiro MCP server acts as a bridge between AI coding assistants and the Apiiro security platform. When invoked through MCP tools, the server communicates with Apiiro's API to retrieve security analysis, trigger scans, and provide actionable security insights within the developer workflow.

## Setup

To use the Apiiro Guardian Agent, you need:

1. An Apiiro account with API access
2. An API token set in the `APIIRO_API_TOKEN` environment variable
3. A supported AI assistant configured to use MCP servers

## License

Apache License 2.0
