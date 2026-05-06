---
name: youtube-channel-reporter
description: Scrape YouTube channels, create detailed Google Docs with video catalogs, and email reports via Gmail
deps:
  - yt-dlp
  - curl
platforms: []
---

# YouTube Channel Reporter

This skill scrapes YouTube channels for complete video catalogs, creates detailed Google Docs via Composio, and sends email reports via Gmail.

## When to Use

Activate this skill when the user wants to:
- Extract all videos from a YouTube channel
- Create a comprehensive catalog/report of YouTube videos
- Generate a Google Doc with video titles, URLs, view counts, and metadata
- Email the report to someone

## Prerequisites

- `yt-dlp` installed (for YouTube scraping)
- `curl` installed (for API calls)
- Composio MCP connection with Google Docs and Gmail enabled
- Consumer API key configured

## Workflow

### Step 1: Scrape YouTube Channel

Use `yt-dlp` to extract all video metadata from the channel:

```bash
yt-dlp --flat-playlist --print-json "https://youtube.com/@CHANNEL_HANDLE" > tmp/youtube_data.json
```

Parse the JSON to extract:
- Video titles
- Video IDs (construct URLs as `https://youtube.com/watch?v=VIDEO_ID`)
- View counts
- Upload dates (if available)

Count total videos and identify top videos by view count.

### Step 2: Create Google Doc via Composio

Use Composio MCP to create a Google Doc with the catalog:

```bash
curl -s -X POST \
  -H "x-consumer-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "COMPOSIO_MULTI_EXECUTE_TOOL",
      "arguments": {
        "tools": [{
          "tool_slug": "GOOGLEDOCS_CREATE_DOCUMENT",
          "arguments": {
            "title": "CHANNEL_NAME - Complete Video Catalog (N Videos)"
          }
        }],
        "session_id": "SESSION_ID"
      }
    }
  }' \
  https://connect.composio.dev/mcp
```

Extract the `documentId` from the response.

### Step 3: Format Content with Markdown

Build markdown content with:
- Document title
- Summary statistics (total videos, channel URL, date)
- Top videos by views (top 5-10)
- Complete video list with titles, URLs, and view counts
- Use markdown formatting: headings, lists, links

### Step 4: Add Content to Google Doc

Use `GOOGLEDOCS_BATCH_UPDATE_DOCUMENT` to insert the formatted content:

```bash
curl -s -X POST \
  -H "x-consumer-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 2,
    "method": "tools/call",
    "params": {
      "name": "COMPOSIO_MULTI_EXECUTE_TOOL",
      "arguments": {
        "tools": [{
          "tool_slug": "GOOGLEDOCS_BATCH_UPDATE_DOCUMENT",
          "arguments": {
            "document_id": "DOCUMENT_ID",
            "requests": [
              {
                "insertText": {
                  "location": {"index": 1},
                  "text": "YOUR_MARKDOWN_CONTENT"
                }
              }
            ]
          }
        }],
        "session_id": "SESSION_ID"
      }
    }
  }' \
  https://connect.composio.dev/mcp
```

### Step 5: Send Email via Gmail

Use `GMAIL_SEND_EMAIL` to send the report:

```bash
curl -s -X POST \
  -H "x-consumer-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 3,
    "method": "tools/call",
    "params": {
      "name": "COMPOSIO_MULTI_EXECUTE_TOOL",
      "arguments": {
        "tools": [{
          "tool_slug": "GMAIL_SEND_EMAIL",
          "arguments": {
            "recipient_email": "RECIPIENT_EMAIL",
            "subject": "CHANNEL_NAME - Complete Video Catalog (N Videos)",
            "body": "EMAIL_BODY_WITH_SUMMARY_AND_GOOGLE_DOC_LINK",
            "is_html": false
          }
        }],
        "session_id": "SESSION_ID"
      }
    }
  }' \
  https://connect.composio.dev/mcp
```

Email should include:
- Summary statistics
- Google Doc link (format: `https://docs.google.com/document/d/DOCUMENT_ID/edit`)
- Top videos by views
- Brief description of what's in the doc

## Key Points

- Always use `tmp/` directory for temporary files
- Parse JSON carefully - handle missing fields gracefully
- Format large numbers (e.g., 22.4M for 22,400,000 views)
- Include clickable links in both Google Doc and email
- Verify email sent successfully by checking response for `successful: true` and email ID
- Use session_id consistently across all Composio calls
- Extract documentId from Google Docs API responses correctly

## Error Handling

- If yt-dlp fails, check channel URL format (should be `@handle` or channel ID)
- If Composio API fails, check API key and connection status
- If email fails, verify recipient email format
- Always report specific errors to the user with next steps

## Example Output Structure

**Google Doc:**
```
# Channel Name - Complete Video Catalog (164 Videos)

## Summary
- Total Videos: 164
- Channel: https://youtube.com/@channelhandle
- Report Generated: [Date]

## Top Videos by Views
1. Video Title 1 - 22.4M views
   https://youtube.com/watch?v=VIDEO_ID1
2. Video Title 2 - 7.3M views
   https://youtube.com/watch?v=VIDEO_ID2
...

## Complete Video List
1. Video Title - View Count
   https://youtube.com/watch?v=VIDEO_ID
...
```

**Email:**
- Subject: Channel Name - Complete Video Catalog (N Videos)
- Body: Summary + Google Doc link + Top videos + Call to action
