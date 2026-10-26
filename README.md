# Yeeflow

Yeeflow helps Claude work with business applications, records, workflow tasks and AI agents through Yeeflow's hosted MCP connection and 14 guided skills. This is the Claude directory edition: it pairs with the Yeeflow connector (https://claude.ai/directory/yeeflow) and focuses on work you do through the live connection.

## What the skills cover

- Choosing and configuring Yeeflow controls, actions, workflow steps, reports and portals, grounded in official Help Center references.
- Building and changing live applications step by step through the connector, with a confirmation before every write and read-back after each save.
- Workflow stage UI, single-attachment OCR and document archive patterns.
- Data reports and knowledge sources for AI Agents and Copilots.
- Designing, templating, importing and exporting AI Agents and Copilots.

Skills: `yeeflow-agent-import-export-operator`, `yeeflow-agent-prompt-designer`, `yeeflow-ai-agent-template-builder`, `yeeflow-ai-agent-ui-operator`, `yeeflow-attachment-ocr-orchestrator`, `yeeflow-copilot-import-export-operator`, `yeeflow-copilot-instruction-designer`, `yeeflow-copilot-template-builder`, `yeeflow-data-report-generator`, `yeeflow-document-archive-lifecycle`, `yeeflow-feature-configuration-guide`, `yeeflow-knowledge-source-operator`, `yeeflow-mcp-incremental-application-builder`, `yeeflow-workflow-stage-ui`.

## Requirements and connection

You need a Yeeflow account with permission for the workspace and operation you choose. Connect Yeeflow through Claude's connector sign-in (OAuth); the endpoint is https://api.yeeflow.com/v1/mcp. Do not paste passwords, tokens or API keys into chat. Signing in does not expand your Yeeflow permissions, and Claude asks before any change.

## Full plugin

Package generation (.yap/.yapk), dashboard and form layout templates, and local validators are in the full Yeeflow plugin: https://github.com/Yeeflow/app-builder-plugin. Install the full plugin instead of this edition if you build complete application packages with Claude Code or Claude Desktop.

## Privacy and support

Privacy policy: https://www.yeeflow.com/privacy. Support: support@yeeflow.com. Setup guide: https://support.yeeflow.com/en/articles/16391707-connect-yeeflow-mcp-servers-to-your-ai-tool
