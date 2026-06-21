# Domain 12 — Project & Behavioral Stories (STAR)

## How to use this
- Lead with the one-line **headline**, then give the detail. Stop when answered.
- For "tell me about a challenging task," use Story 1 or 4.
- For "tell me about a project," use Story 3 or 5.
- For "architecture, scaling, or distributed systems," use Story 2.
- For "real-time systems or performance," use Story 6.
- For "leadership," use Story 4.
- For "what architecture/tools and why," use the Architecture Decisions section.
- Keep it ~60-90 seconds spoken. Go deeper only if they probe.

---

## Story 1 — Dashboard Performance Optimization (challenging task)

**Headline:** Cut dashboard load times by roughly 80% (from ~30s to 5-7s) on a multi-tenant BI platform.

**Situation:** On our BI SaaS platform, dashboards were taking around 30 seconds to load. Users were frustrated and it was hurting the product.

**Task:** I owned diagnosing and fixing it as the technical escalation point.

**Action:** I profiled the load path and found the time wasn't in one place. First, several user-data fetch and validation methods were running *before* the dashboard even needed them, I removed those pre-load calls so they only run when a dashboard item actually requires the data. Second, a caching API was being called on every dashboard load when it was only needed on save, so I deferred it to the save action. Then I split oversized endpoints (which fetched and processed large data into many tables at once) into smaller targeted endpoints the frontend calls only when needed, dropping processing to milliseconds. Finally I rewrote nested multi-level queries into flatter ones and switched lookup-heavy Lists to HashSets.

**Result:** Load times dropped from about 30 seconds to 5-7 seconds, roughly an 80% reduction, and I measured it before and after to confirm.

**If probed on "why HashSet?":** Lists do an O(n) scan for lookups; HashSets give O(1) lookups, which matters when you're checking membership repeatedly inside a loop.

---

## Story 2 — Stateless Scaling with Redis + SignalR (architecture + why)

**Headline:** Architected a Redis-backed SignalR backplane so the platform scales statelessly across Kubernetes pods.

**Situation:** Our BI platform runs on multiple Kubernetes pods, and it uses SignalR for real-time updates. The problem: a user connected to one pod wouldn't receive real-time signals generated on another pod, and in-memory session state broke when requests hit different pods.

**Task:** Make the platform horizontally scalable and stateless so any pod can serve any request.

**Action:** I introduced Redis as a SignalR backplane, so real-time signals publish through Redis and reach every pod regardless of which one the user is connected to. I also moved session state, token caching, and AI working-copy state out of pod memory and into Redis. That made the pods stateless, no pod holds anything another pod needs.

**Result:** The platform scales horizontally cleanly, you can add pods and any of them can handle any request, because state lives in Redis, not in memory.

**Why Redis:** It's fast, in-memory, supports pub/sub (which is exactly what a SignalR backplane needs), and it doubled as our distributed cache and session store, so one component solved three problems.

---

## Story 3 — Multi-Provider LLM Integration Gateway (interesting project + AI)

**Headline:** Built an LLM integration gateway that let users query and modify dashboards in natural language.

**Situation:** We wanted users to interact with their BI dashboards using plain language instead of manually building queries.

**Task:** Design and build the integration so the platform could talk to multiple AI providers and turn natural language into dashboard actions.

**Action:** I built a multi-provider LLM integration gateway connecting the OpenAI API and our in-house models behind a single interface, so the platform could route requests and switch providers without changing the frontend. When a user described a change in natural language, the gateway interpreted it and updated the dashboard. The updated dashboard was held as a working copy in Redis so the user could iterate in real time across the multi-pod environment, and it was only persisted to the database when they chose to save.

**Result:** Users could query and adjust dashboards conversationally, and the working-copy approach kept it fast and avoided writing unsaved changes prematurely.

**Honest scope note (say if asked):** This is LLM integration, secure API design, multi-provider routing, and state management, not agent frameworks or RAG from scratch.

---

## Story 4 — Zero-Downtime Migration of 20+ Apps (challenging delivery + leadership)

**Headline:** Led a zero-downtime migration of 20+ production government applications while leading a team of 6.

**Situation:** At the UAE Ministry of Foreign Affairs, 20+ production applications needed migrating from Windows Server 2012 to 2022, with no acceptable downtime for government systems.

**Task:** As team lead, I owned the delivery end-to-end and the team of 6 executing it.

**Action:** I planned the migration in sequenced stages, set up on-premises DevOps infrastructure from scratch (repositories, pipelines, automated deployments, internal NuGet/NPM hosting), and ran the migrations with rollback plans so we could revert if anything failed. I also ran code reviews and gave hands-on technical guidance throughout.

**Result:** All 20+ applications migrated with zero downtime, on time, and we came out of it with proper DevOps infrastructure that didn't exist before.

**Leadership angle:** I balanced hands-on technical work with owning timelines, sprints, and accountability for the team's delivery.

---

## Story 5 — Built a Ride-Sharing Platform from Scratch (zero-to-one + full-stack + mobile)

**Headline:** Built a corporate ride-sharing platform end-to-end, React web app, React Native driver mobile app, and real-time dispatcher communication.

**Situation:** At FMS-Tech, we needed a corporate ride-sharing platform built from the ground up, covering both riders and drivers, with live communication between them and a dispatcher.

**Task:** I built it end-to-end across web, mobile, and the real-time communication layer.

**Action:** I built the rider-facing web app in React and the driver mobile app in React Native, so drivers had a proper native experience on the road. For dispatcher communication I implemented low-latency audio and text using WebRTC together with SignalR, so the dispatcher, drivers, and system could talk in real time. I owned it across the full stack, frontend, mobile, backend services, and the real-time layer.

**Result:** A working corporate ride-sharing platform delivered from scratch, with live driver-dispatcher communication.

**Why React Native:** I needed a real native mobile experience for drivers (background location, responsiveness) without maintaining two separate native codebases, so a single cross-platform codebase made sense for the timeline and team size.

**Why WebRTC + SignalR together:** WebRTC handles the low-latency peer audio; SignalR handles signaling, text messaging, and coordination with the backend. They complement each other, WebRTC for the media stream, SignalR for the messaging and control channel.

---

## Story 6 — Polling to Real-Time SignalR at Scale (real-time + performance)

**Headline:** Rebuilt a vehicle-monitoring dashboard from polling to real-time SignalR streaming, handling hundreds of vehicles live.

**Situation:** The existing fleet dashboard used polling to get vehicle data, which was inefficient and didn't scale well as the number of vehicles grew, the UI lagged and the server took unnecessary load from constant polling.

**Task:** Move it to a real-time model that could handle live GPS and journey data for hundreds of vehicles at once.

**Action:** I rebuilt the dashboard to use SignalR streaming instead of polling, so the server pushes updates to the client only when data changes, rather than the client repeatedly asking. This let us stream live GPS positions and journey management for hundreds of vehicles simultaneously without the overhead of constant polling.

**Result:** Live, real-time fleet monitoring for hundreds of vehicles at once, with far less server load and a responsive UI.

**Why SignalR / push over polling:** Polling wastes resources, every client repeatedly asks even when nothing changed, and it adds latency. A push model (SignalR) sends data only when there's something new, which scales much better for many concurrent live clients and gives a real-time feel.

---

## Architecture & Tools — What I Used and Why

Use these when asked "why did you choose X?" Interviewers want reasoning, not a tool list.

- **Redis** — Used as SignalR backplane (pub/sub across pods), distributed cache, session/token store, and AI working-copy state. Chosen because it's in-memory (fast), supports pub/sub natively, and solved multiple problems with one component.
- **SignalR** — Real-time updates to the UI. With the Redis backplane it works correctly across multiple pods.
- **Kubernetes (stateless design)** — Pods are stateless because all shared state lives in Redis; this is what makes horizontal scaling work, any pod can serve any request.
- **Quartz.NET (clustered)** — Scheduled ETL jobs across distributed nodes. Clustering ensures a job runs reliably even with multiple nodes, without double-execution.
- **EF Core + Dapper** — EF Core for standard data access and migrations (productivity, change tracking); Dapper for performance-critical queries where I want raw SQL control and speed.
- **OAuth2 / JWT / OpenID Connect / Keycloak / Azure Entra ID** — Enterprise SSO and secure APIs. Token validation paired with Redis caching so auth scales across pods.
- **CData providers + custom REST/RSS provider** — To integrate 200+ data sources; I built a custom REST/RSS provider to overcome single-URL table limits and enable multi-endpoint federation in one connection schema.
- **SonarQube** — Continuous code quality and security scanning, enforced in CI/CD.

---

## Quick STAR index (for fast recall)
- **Performance / debugging question** → Story 1 (dashboard 80%)
- **Architecture / distributed systems / scaling** → Story 2 (Redis + SignalR)
- **AI / interesting project** → Story 3 (LLM gateway)
- **Leadership / hard delivery / challenge** → Story 4 (zero-downtime migration)
- **Zero-to-one / full-stack / mobile project** → Story 5 (ride-sharing platform)
- **Real-time / performance** → Story 6 (polling to SignalR)
