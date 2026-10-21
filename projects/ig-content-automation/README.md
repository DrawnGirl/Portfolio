🇬🇧 English | 🇵🇱 [Polski](README_PL.md)

[← Back to portfolio](../../README.md)

# Instagram content automation (n8n)

An n8n workflow that turns a topic from a Notion database into a finished Instagram post: a carousel or a reel. It writes the copy, picks photos, generates graphics or video, publishes the post, adds the first comment and logs the result in Notion. Every step is reported on Discord.

**Scale:** 45 nodes · runs daily at 9:00 · live on the @calmyourtech account

![Workflow in the n8n editor](workflow.png)
*The full workflow in the n8n editor: top branch = carousel, second branch = reel.*

## How it works

```mermaid
flowchart TD
    A[Schedule 9:00<br/>or webhook] --> B[Fetch next topic<br/>from Notion queue]
    B --> C{Duplicate?}
    C -- yes --> D[Skip and notify<br/>on Discord]
    C -- no --> E{Post type}
    E -- carousel --> F[GPT-4o writes 5 slides]
    F --> G[Pexels: pick photos]
    G --> H[Render slide graphics]
    H --> I[Upload to Cloudinary]
    I --> J[Carousel container<br/>via Instagram Graph API]
    E -- reel --> K[GPT-4o writes scenes]
    K --> L[Pexels: scene photos]
    L --> M[Render video]
    M --> N[Upload to Cloudinary]
    N --> O[Reel container<br/>via Instagram Graph API]
    J --> P[Publish to IG]
    O --> P
    P --> Q[First comment]
    Q --> R[Log in Notion<br/>+ mark topic as used]
    R --> S[Discord notification]
```

## What this project shows

- **Working with APIs:** Instagram Graph API, OpenAI, Notion, Pexels, Cloudinary, Discord webhooks; HTTP requests with headers and parameters
- **Data consistency:** duplicate check before publishing, marking used topics, logging every publish and every skip
- **Logic and branching:** separate paths for carousels and reels, handling an empty queue, manually skipping a topic
- **JavaScript code:** parsing GPT responses, mapping Notion data, assembling slide and scene data
- **Prompt engineering:** a prompt with an example of target content and an enforced JSON format
- **Async handling:** waiting for Instagram to process media before publishing

## Related workflows

This workflow is part of a larger system:
- **Post Analytics – 24h Pull:** a day after publishing, pulls post metrics and follower count and saves them in Notion
- **Weekly Format Report:** compares reels vs carousels every week and sends a report to Discord
- **Trend Scout:** generates new topics every week and adds them to the Notion database

## Workflow file

[`workflow.json`](workflow.json) can be imported into n8n. All API keys, tokens, account and database IDs have been removed and replaced with placeholders like `YOUR_IG_ACCESS_TOKEN`.
