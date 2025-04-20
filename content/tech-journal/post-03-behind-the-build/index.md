---
title: "Behind the Build: Foundations, Tradeoffs & What’s Ahead"
date: 2025-04-25T09:00:00-05:00
draft: true
tags: ["youtube-digest"]
---

## Behind the Build: Foundations, Tradeoffs & What's Ahead

From the outset, YouTube Digest was designed as more than a simple demo; it serves as a solid foundation for a more comprehensive tool. The following design choices were made not just to deliver initial functionality, but to ensure the application can evolve effectively.

### 🧱 **Foundations That Scale**

- **Containerized from Day One:** The entire stack runs in Docker Compose—frontend (Next.js), backend (FastAPI), and database (PostgreSQL). That makes it reproducible, portable, and ready for production. A single `docker-compose up` is all it takes to spin up the full environment.
- **Mounted Volumes for Fast Dev:** To move fast, I mapped local volumes to my containers. No rebuild loops—just save and refresh. This cut iteration time drastically during early development and testing.

* * *

### 🔁 **Smart Tradeoffs (Not Shortcuts)**

- **yt-dlp &gt; YouTube API:** I bypassed the YouTube Data API completely. Instead, I use `yt-dlp` to extract metadata and transcripts reliably—no quota limits, no credential headaches. It’s battle-tested (100k+ stars) and does exactly what I need.
- **PostgreSQL as Cache + Source of Truth:** Every transcript and summary is stored, so nothing gets recomputed unnecessarily. This avoids repeated calls to the OpenAI API, which cuts latency and saves money.
- **Async-First, Always:** Heavy lifting (like fetching transcripts or summarizing long videos) happens in background tasks. The frontend stays responsive. If something takes 60 seconds, it won’t block anything else.

* * *

### 📈 **Performance and Observability**

- **Digest Polling & Status Updates:** The frontend polls the backend to check video and digest processing status, ensuring users always see real-time feedback without blocking the UI.
- **Token Usage & Cost Tracking:** Every OpenAI request logs tokens in/out. Right now it’s just internal, but the groundwork is there for per-user quotas, cost dashboards, or even billing in the future.

* * *

### 🛠️ **Engineered for What's Next**

- **Overbuilt Schema (On Purpose):** I designed the database with user accounts, content tracking, and digest history in mind—even though none of it’s visible in the UI yet. No rewrites later—just feature toggles when I need them.
- **Library View (WIP):** Digest persistence is live. You can already retrieve previous summaries. The “library view” isn’t fully polished yet, but the backend is ready for when it is.
- **Model-Agnostic Summarization:** The app started on GPT-4-turbo and now runs `o3-mini`. As new models drop, upgrades are plug-and-play. The MVP is quite literally the worst these summaries will ever be.

* * *

This isn’t just a working prototype—it’s a system designed to grow without crumbling. The early effort was intentional, and it sets the stage for rapid iteration without technical debt. Next stop: polish, UX upgrades, and user-facing features.
