<div align="center">
  <h1>🪐 PtiCalin</h1>
  <p><strong>Building modular knowledge systems, developer tooling, and creative automation — learning in public with disciplined iteration.</strong></p>
</div>

## Overview
I explore the intersection of personal knowledge systems, scripting, and creative engineering. My current focus: evolving an extensible Obsidian-based stack (VaultOS + Awesome Test Vault) to prototype structured thinking, automated organization, and reproducible workflows.

## Current Focus
- VaultOS: plugin orchestration, validation, and structured vault automation.
- Awesome Test Vault: experimental second-brain patterns & modular templates.
- Tooling experiments in Rust, Python & TypeScript for automation and content ops.
- Systems thinking applied to worldbuilding, game design, and data curation.

## Featured Projects
| Project | Domain | Brief | Core Stack | Status | Repo |
|--------|--------|-------|-----------|--------|------|
| Awesome Test Vault | PKM / Obsidian | Modular vault workflows & knowledge patterns | Obsidian, YAML, Git | WIP | [Repo](https://github.com/PtiCalin/Awesome-Test-Vault) |
| UNCHAINED | Audio Library / DJ | Local-first music library & DJ studio | FastAPI, SQLite, React, Tauri | Phase 1 | [Docs](#unchained) |
| VaultOS | Obsidian Plugin | Orchestrates sub-plugins & validates structure | TypeScript, Node.js | WIP | [Repo](https://github.com/PtiCalin/VaultOS) |
| Veridonia Universe | Worldbuilding | Modular RPG + narrative system design | Markdown, Obsidian | WIP | [Repo](https://github.com/PtiCalin/Veridonia-Universe) |
| Calin Coin | Blockchain Experiment | Wallet + consensus exploration (PoW/PoA hybrid) | Rust, Docker | Prototype | [Repo](https://github.com/PtiCalin/vault_nuggets) |
| Security Suite | Privacy Lab | Local stack (VPN, DNS, security tooling) | Bash | Draft | Private |
| Survivor Encyclopedia | Data Archiving | Structured TV metadata & templates | Python, Obsidian | WIP | Private |
| AI Image Describer | Vision AI | Multi-mode image description + tagging | Python, Ollama | Prototype | [Repo](https://github.com/PtiCalin/vault_image-description) |
| Image Inspiration Pipeline | Creative Sourcing | Pinterest API + local asset pipeline | Python | PoC | [Repo](https://github.com/PtiCalin/vault_image-description_temp) |
| Simple Game Engine | Learning Engine | Minimal game dev fundamentals | C++ | Prototype | [Repo](https://github.com/PtiCalin/simple-game-engine) |
| Template Repo Generator | Dev Productivity | Consistent project scaffolding CLI | Python | Draft | [Repo](https://github.com/PtiCalin/temp_repo-gen) |

<details>
<summary><strong>Extended Notes (click to expand)</strong></summary>

- <strong>Vault Architecture:</strong> Emphasis on declarative metadata, validation layers, and reproducible knowledge flows.
- <strong>Experiment Rhythm:</strong> Small scope → measured outcome → refactor → document pattern.
- <strong>Design Drivers:</strong> Maintainability, transparency, portability, low cognitive load.
- <strong>Learning Style:</strong> Build-first, synthesize later. Prefer real artifacts over speculative diagrams.

</details>

### UNCHAINED

Local-first, privacy-respecting music library + DJ studio designed to work offline, then scale to the web without rewrites.

<details>
<summary><strong>Quick Start (Phase 1)</strong></summary>

Backend setup (Windows PowerShell):
```powershell
powershell -ExecutionPolicy Bypass -File scripts\setup-dev.ps1
powershell -ExecutionPolicy Bypass -File scripts\run-backend.ps1
# Health check
Start-Process http://127.0.0.1:8000/health
```
Import a demo track:
```powershell
powershell -ExecutionPolicy Bypass -File scripts\import-demo.ps1 -FilePath "C:\path\to\song.mp3"
```

Frontend (React + Tailwind + Tauri):
```powershell
powershell -ExecutionPolicy Bypass -File scripts\run-frontend.ps1
```

Download worker (queued jobs):
```powershell
powershell -ExecutionPolicy Bypass -File scripts\run-download-worker.ps1
```

</details>

<details>
<summary><strong>Architecture Overview</strong></summary>

- Backend: FastAPI (`backend/app`), SQLite (`library/db/library.sqlite`), metadata extraction (Mutagen).
- Library Storage: `library/audio`, `library/covers`, `library/metadata`.
- Configuration: `config/settings.json`, `.env`.
- Frontend: React + Tailwind + Tauri (desktop), Vite dev server.
- Dev Scripts: `scripts/setup-dev.ps1`, `run-backend.ps1`, `run-frontend.ps1`, `run-download-worker.ps1`, `create-shortcuts.ps1`.
- Env Vars (backend `config/.env`): `API_HOST`, `API_PORT`.
- Env Vars (frontend optional): `VITE_API_BASE`, `VITE_ENABLE_NOTIFICATIONS`.

Repository layout:
```
backend/       # FastAPI app, services, models
library/       # Audio, covers, metadata, db
frontend/      # React + Tailwind + Tauri
config/        # .env, settings.json
scripts/       # Helper scripts
```

</details>

<details>
<summary><strong>Metadata & Sources Workflow</strong></summary>

Supported source imports (legal use only): Spotify (metadata/playlists), iTunes XML, Bandcamp (authorized URLs), MusicBrainz, Discogs, SoundCloud (public metadata), local folder scans.

Quality aggregation:
```powershell
$Body = @{ artist = "Boards of Canada"; album = "Music Has the Right"; title = "Roygbiv"; path_audio = "library/audio/roygbiv.flac" } | ConvertTo-Json
Invoke-RestMethod -Method Post -ContentType "application/json" -Body $Body -Uri http://127.0.0.1:8000/sources/metadata/quality
```
Apply candidate:
```powershell
$Apply = @{ candidate_id = 12; track_id = 5 } | ConvertTo-Json
Invoke-RestMethod -Method Post -ContentType "application/json" -Body $Apply -Uri http://127.0.0.1:8000/sources/metadata/apply
```
Revert field:
```powershell
$Revert = @{ track_id = 5; field_name = "album" } | ConvertTo-Json
Invoke-RestMethod -Method Post -ContentType "application/json" -Body $Revert -Uri http://127.0.0.1:8000/sources/metadata/revert
```

Artwork variants & relations endpoints also support multiple covers and semantic track links (`remix`, `edit`, `alternate_version`, `sample_of`, `part_of_release`).

</details>

<details>
<summary><strong>Performance Features</strong></summary>

- Samples: time-ranged slices for pads (`/sources/tracks/{id}/samples`).
- Multiple artwork variants per track.
- Relations: remix/edit/version/sample linking.
- SSE Events: `/sources/events/stream` for real-time notifications (upload_complete, download_finished).
- Desktop integration: system tray actions, toast host, update status.

</details>

<details>
<summary><strong>UI Modes & Layout</strong></summary>

Modes: Player, Library, Collection, Focus, Dashboard, Studio.
Core shell components: `AppShell.tsx`, `TopBar.tsx`, `Sidebar.tsx`, `BottomPlayer.tsx`.
Global state: `useAppStore.ts` (Zustand). Notifications: `services/notifications.ts` (SSE bootstrap).
Interaction rules: click = open, double-click = play, space = play/pause, Ctrl/Cmd+F = search, drag files = import.

</details>

<details>
<summary><strong>Vision & Roadmap</strong></summary>

Phases: Foundation → Analysis → UI Modes → Data Science → DJ Engine → Mixing Suite → Web Deployment.
Long-term: Local-first extensibility + optional cloud sync; provenance-aware metadata with reversible attribution; modular analysis engine (Librosa → Essentia).

</details>

<details>
<summary><strong>Contributing & License</strong></summary>

- Issues & PRs welcome (see PR template).
- Read `CONTRIBUTING.md` before submitting changes.
- Licensed under MIT (`LICENSE`).

</details>


## Technology Landscape
<strong>Core:</strong> `TypeScript` · `Python` · `Rust` · `C++` · `Markdown` · `YAML` · `Git`  
<strong>Platforms:</strong> `Obsidian` · `Node.js` · `Docker` · `Ollama`  
<strong>Automation & Scripting:</strong> `Bash` · `PowerShell` · `CLI tooling`  
<strong>Data & Analysis:</strong> `Jupyter` · `SQLite` · `PostgreSQL` · `MySQL`  
<strong>Creative & Worldbuilding:</strong> `Figma` · `Godot` · `Unity` · `Narrative tooling`  
<strong>Exploration:</strong> Consensus design · Knowledge graph structuring · Semantic tagging workflows

## Engineering & Knowledge Principles
- Clarity over cleverness; explicit metadata beats implicit convention.
- Systems > goals: invest in reusable processes that compound.
- Automate the boring; narrate the non-obvious.
- Prototype fast, archive learnings, refactor with intention.
- Layered validation reduces entropy in growing knowledge bases.

## Metrics & Highlights (Optional)
<details>
<summary>Show GitHub Stats</summary>

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=PtiCalin&theme=calm&show_icons=true&hide_border=true" alt="GitHub Stats" />
  <br><br>
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=PtiCalin&theme=calm&layout=compact&hide_border=true" alt="Top Languages" />
</p>

</details>

## Support / Collaboration
If you find value in these experiments or want to collaborate on structured thinking / vault automation, feel free to open an issue or reach out.  
<a href="https://buymeacoffee.com/pticalindop">☕ Buy Me a Coffee</a>

## Roadmap Snapshot
- Short-term: strengthen VaultOS validation layer & test harness.
- Mid-term: modular knowledge graph export strategy (queryable model).
- Long-term: interoperable vault schema supporting multi-tool ingestion.

## Contact
For queries, collaboration ideas, or constructive feedback: open a discussion or reach out via GitHub Issues.

<p align="center"><em>🌱 Iteration compounds. Build with curiosity, refactor with care.</em></p>

---

<sub>Previous, more badge-heavy profile version has been simplified for readability and professional tone.</sub>
