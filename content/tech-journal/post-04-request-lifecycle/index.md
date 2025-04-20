---
title: "Request Lifecycle: From URL to AI-Generated Digest"
date: 2025-05-02T09:00:00-05:00
draft: true
tags: ["youtube-digest"]
---

## System Architecture & Flow

The system employs a decoupled, asynchronous architecture designed for resilience and responsiveness. The API offloads intensive work (transcript fetching, AI analysis) to background workers, preventing blocked requests and ensuring a smooth user experience.

{{< mermaid >}}
flowchart TD
  %% User Interface
  subgraph UI["User Interface"]
      U["User"] -->|Submit URL| FE["Next.js Frontend"]
      FE -->|Poll Digest & Status| U
  end

  %% API & Queueing
  subgraph API["FastAPI Backend"]
      FE -->|POST /videos/| VP["VideoProcessor"]
      VP -->|Validate & Create Video| VTASK["Queue Video Processing"]

      FE -->|POST /videos/:id/digests| DIG["Digest Request"]
      DIG -->|Check Transcript Status| CHK{"Transcript Ready?"}
      CHK -->|Yes| DTASK["Queue Digest Generation"]
      CHK -->|No| WAIT["Return Pending"]
  end

  %% Video Processing Worker
  subgraph VW["Video Background Worker"]
      VTASK -->|Process Video| TSVC["TranscriptService (yt-dlp)"]
      TSVC -->|Transcript Processed| VDB["Update Video Status"]
  end

  %% Digest Processing Worker
  subgraph DW["Digest Background Worker"]
      DTASK -->|Generate Summary| SUM["OpenAISummarizer"]
      SUM -->|Save Digest| DIGDB["DigestModel"]
  end

  %% Data Layer
  DB[("PostgreSQL Database")]
  
  %% Storage Flows
  VDB --> DB
  DIGDB --> DB
  DB --> FE
{{< /mermaid >}}

### **Request Lifecycle: From URL to AI-Generated Digest**

The architecture separates concerns effectively: the user interacts with the **UI layer**, which communicates with the **API layer**. The API then manages **background tasks** (like fetching data and calling the AI), interacts with the **AI engine**, and stores/retrieves information from the **data layer**.

1. **User Submission**

    - User submits a YouTube URL via the Next.js frontend.
    - URL is immediately sent to the FastAPI backend (`POST /api/v1/videos/`).
2. **Video Validation and Queuing**

    - FastAPI validates the URL and extracts initial metadata (`title`, `duration`, `channel details`) via `yt-dlp`.
    - A new video record (`VideoModel`) is created in PostgreSQL with status `PENDING`.
    - The backend queues a background job (`process_video_background`) to handle intensive tasks asynchronously.
3. **Transcript Retrieval and Processing (Background Task)**

    - A background worker initiates processing:

        - Updates video status to `PROCESSING`.
        - Uses `TranscriptService` and `yt-dlp` to fetch the video’s transcript.
        - Upon successful retrieval, transcript data is processed and stored (`TranscriptModel` set to `PROCESSED`).
        - Video status updates to `COMPLETED` upon successful transcript processing, or `FAILED` on errors.
4. **Digest Request and Task Scheduling**

    - Frontend initiates a digest request (`POST /api/v1/videos/{video_id}/digests`).
    - API checks if the video has a fully processed transcript available.

        - If available, a new digest record (`DigestModel`) is created in PostgreSQL with empty content.
        - A summarization task (`generate_digest_background`) is queued for asynchronous processing.
        - If not yet available, the client receives a pending status to retry later.
5. **AI Summarization (Background Task)**

    - Background summarization worker retrieves transcript data and invokes the AI summarization provider (`OpenAISummarizer`).
    - AI summarizer generates concise, structured summaries.
    - Generated summary, along with metadata (`tokens used`, `cost`, `model info`), is saved to PostgreSQL.
6. **Digest Retrieval and Display**

    - Frontend polls or directly fetches digest data (`GET /api/v1/videos/{video_id}` or `GET /api/v1/digests/{digest_id}`).
    - Backend retrieves and returns video information alongside the completed AI-generated summary (digest).
    - Frontend displays the summary, providing a seamless, responsive user experience.