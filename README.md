# MicroClaw Custom Setup

This repository contains custom configuration files and skills for MicroClaw that are set up manually and don't come out of the box.

## Contents

- **skills/** - Custom agent skills
- **config/** - Configuration files
- **mcp_servers.yaml** - MCP server configurations

## Skills Included

- audio-transcriber - Transcribe audio messages to text
- composio-cli - Composio CLI integration
- docx - Word document manipulation
- github - GitHub CLI integration
- pdf - PDF file operations
- pptx - PowerPoint manipulation
- skill-creator - Skill creation guide
- xlsx - Excel spreadsheet operations
- weather - Weather information
- find-skills - Skill discovery

## Setup

1. Copy skills to `~/.microclaw/skills/`
2. Copy config files to `~/.microclaw/`
3. Update API keys in `microclaw.config.yaml` and `mcp_servers.yaml`
4. Restart MicroClaw

## Notes

- Remove sensitive API keys before committing
- This is a backup/template of custom setup files
