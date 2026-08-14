# CampaignRunner – Post-MVP Vision & Roadmap

**Status:** Draft for discussion  
**Date:** 2026-08-14  
**Depends on:** Phase 6 (Memory Trust & Shared Control) complete → validation → Solo Mode MVP → Absent Player  

---

## 1. Guiding Principles (what we protect)

Even as the product expands, these remain non-negotiable:

1. **Memory is the source of truth.** Structured entities + events + summaries in the database remain authoritative. The LLM narrates and suggests around retrieved state; it does not invent campaign canon.
2. **Human DM stays sovereign.** The bot is a co-pilot / co-planner / continuity engine, never a replacement for the human’s creative authority at the table.
3. **Consent & agency for players.** Features that act on or as a character (especially Absent Player) are opt-in by the specific player. Actions taken while absent are tagged and reviewable.
4. **Continuity over simulation.** We prioritize features that make the campaign feel continuous, personal, and alive across sessions and absences. Full combat engines, complete rules adjudication, and VTT replacement are secondary.
5. **Progressive enhancement, not rewrite.** The local-first Discord + Ollama path remains viable. New surfaces (web app, integrations) share the same source of truth rather than replacing the original core.

These principles keep CampaignRunner differentiated from generic AI chatbots and from full VTTs.

---

## 2. Product Pillars (Post-MVP)

### Pillar A – Continuity & Immersion (highest leverage)
These amplify the unique memory advantage built in Phases 0–6 and Solo.

- **Session 0 Intake**  
  Structured capture of campaign setting, theme, tone, and each player’s character goals, backstory, bonds, ideals, flaws, and open threads. The bot continuously weaves these into narration, suggestions, NPC reactions, and summaries so the story feels personal.

- **DM Planning Session**  
  Pre-session collaborative mode. DM (and optionally Co-DMs) work with the bot to shape encounters, inject complications or random encounters, refine NPCs, and surface open plot threads — all grounded in the actual party capabilities, history, and current campaign state.

- **Absent Player (opt-in)**  
  Already designed: specific-player consent required. Diegetic absence + optional light, conservative, vetoable AI control of the missing PC + soft encounter-adjustment suggestions. All bot-mediated actions tagged in memory.

### Pillar B – Table Convenience
High-frequency quality-of-life that becomes powerful because it lives inside the same memory system.

- Campaign advancement mode: **XP vs Milestone** (configurable per campaign) with tracking and announcements.
- Dice roller (Discord-native + later web).
- Lightweight homebrew rules storage (searchable notes first; later soft constraints in prompts).
- Basic rules / lookup helper (against stored content + homebrew, not a full SRD engine).

### Pillar C – Surface Expansion
How people interact with the memory outside the live Discord table.

- Cloud-hosted web app (planning UI, Session 0 flows, memory dashboard/timeline/entity editor, multi-device access).
- Future integrations / plugins: Notion, OneNote, NotebookLM-style tools (export, sync, or embed views). Discord remains the primary live-play client.

### Pillar D – Simulation (deliberately later / modular)
- Combat *assistant* first (track state the DM describes, log outcomes into memory, light suggestions).  
- Full Combat Engine only if real usage demands it and after the continuity pillars are solid. Prefer integration with existing tools (Foundry, etc.) over building a competing engine early.

---

## 3. Recommended Sequencing (after current roadmap)

```
Current → Phase 6 (Memory Trust & Shared Control)
       → Phase 6.5 Validation / dogfooding (real tables)
       → Phase 7: Thin Solo Mode MVP
       → Phase 8: Absent Player (opt-in, thin-first)
       → Phase 9+: Continuity & Immersion (Session 0 + DM Planning Session)
       → Parallel / interleaved: Table Convenience (advancement, dice, light homebrew)
       → Web surface (hybrid dashboard) once planning & Session 0 need better UX
       → Combat assistant / deeper simulation only when demanded
       → External note-tool integrations once web surface exists
```

**Rationale:**  
Session 0 and DM Planning directly multiply the value of the trusted memory and the narrative agency developed for Solo/Absent. They should come before heavy simulation or a full architectural leap. The web app is introduced as a first-class client of the same data store, not a replacement for the Discord live loop.

---

## 4. Feature Backlog (prioritized)

### High priority (near-term Post-MVP)
| Feature                        | Notes / MVP shape                                      | Depends on          |
|--------------------------------|--------------------------------------------------------|---------------------|
| Session 0 intake               | Structured forms + continuous weaving into prompts     | Solid memory        |
| DM Planning session            | Pre-session co-planning mode, memory-grounded          | Solo narrative muscle |
| Absent Player                  | Opt-in, diegetic + light control + suggestions         | Solo                |
| Advancement mode               | XP vs Milestone setting + simple tracking              | Campaign settings   |
| Dice roller                    | Discord slash + later web                              | Low                 |

### Medium priority
| Feature                        | Notes                                                  | Depends on          |
|--------------------------------|--------------------------------------------------------|---------------------|
| Lightweight homebrew notes     | Searchable, injectible into context                    | Memory toolkit      |
| Basic rules / lookup helper    | Against stored + homebrew content                      | Homebrew notes      |
| Web dashboard (v1)             | Memory timeline, entity editor, Session 0 UI, planning | Shared data store   |
| Combat assistant               | Track / log / suggest; not full engine                 | Planning + memory   |

### Later / modular
| Feature                        | Notes                                                  |
|--------------------------------|--------------------------------------------------------|
| Full Combat Engine             | Only if assistant proves insufficient; consider VTT integration first |
| Deep rules engine / full SRD   | Legal + content scope; prefer external sources         |
| Notion / OneNote / NotebookLM  | After web surface; export or bidirectional sync        |
| Always-on hosting as default   | Railway or equivalent; keep local option               |
| Cloud LLM router               | Optional fallback / higher quality; preserve local path|

---

## 5. Tech Stack Evolution & Recommendation

### Current (MVP → Phase 6+)
- **Live interface:** Discord bot (discord.js, TypeScript)
- **Inference:** Local Ollama only
- **Source of truth:** Supabase (Postgres + pgvector)
- **Host:** Local machine (Windows)

### Recommended Post-MVP direction: Hybrid

**Core principle:** One source of truth, multiple clients.

```
┌─────────────────────────────────────────────────────────┐
│                    Supabase (source of truth)            │
│         campaigns • entities • events • summaries        │
│                    + pgvector embeddings                 │
└────────────────────────────┬────────────────────────────┘
                             │
           ┌─────────────────┴─────────────────┐
           │                                   │
           ▼                                   ▼
┌─────────────────────┐             ┌─────────────────────┐
│  Discord Bot Client │             │  Cloud Web App      │
│  (live table play)  │             │  (planning, Session │
│  local or always-on │             │   0, dashboard,     │
│  Ollama or cloud    │             │   multi-device)      │
│  LLM option         │             │                     │
└─────────────────────┘             └─────────────────────┘
           │                                   │
           └─────────── future ────────────────┘
                 Notion / OneNote / etc.
```

### Concrete recommendations

| Layer              | Recommendation                                                                 | Rationale |
|--------------------|--------------------------------------------------------------------------------|-----------|
| Data store         | Keep Supabase as primary source of truth                                       | Already in place; excellent for structured + vector; free tier viable early |
| Live play client   | Discord bot remains primary real-time interface (local or hosted)              | Lowest friction at the table; preserves existing investment |
| Planning / UI      | New cloud-hosted web app (Next.js or similar + Supabase Auth)                  | Session 0 forms, visual timeline, entity editing, planning boards need real UI |
| Inference          | Local Ollama primary; optional cloud LLM router (DeepSeek / Groq / etc.) for quality or when local is unavailable | Preserves privacy & zero-cost path; adds flexibility |
| Hosting            | Discord bot: local still supported + optional Railway/Fly/etc. always-on<br>Web app: Vercel / Railway / similar | Hybrid serves both power users who want local and groups who want availability |
| Auth               | Discord identity for bot; Supabase Auth (or Discord OAuth) for web app         | Clean multi-user / Co-DM story |
| External tools     | Later: official export, Notion API, or embeddable views                        | Do not make Notion the source of truth |

**Why hybrid over pure cloud web-app rewrite?**
- Protects the local-first, private, zero-cost path that is currently working.
- Discord remains the best live-table interface for most groups.
- Web app solves the exact friction points the user listed (planning, Session 0, memory management) without forcing every interaction through slash commands.
- Same memory substrate means Solo, Absent, Planning, and live play all stay consistent.

---

## 6. Success Criteria for the Post-MVP Horizon

- A DM can run a full Session 0, feed character goals/backstories, and see those elements naturally appear in later sessions and Solo play.
- Pre-session planning with the bot produces tighter, more reactive encounters that respect party history and open threads.
- When a consented player is absent, the table continues with diegetic continuity and optional light AI support that the returning player can review and correct.
- The DM has a web surface for planning and memory work that is more comfortable than pure Discord commands, while the live session still happens in Discord.
- Local-first + Ollama path remains fully functional for users who want it.
- Combat and rules features, if present, feel like assistants rather than a competing VTT.

---

## 7. Open Questions

1. How structured should Session 0 character intake be (free text vs guided form with bonds/ideals/flaws/goals)?
2. For DM Planning sessions, is a dedicated Discord thread / voice channel mode enough for v1, or do we need the web UI first?
3. How aggressively should we pursue always-on hosting vs keeping local as the happy path?
4. Legal / content strategy for any rules lookup (SRD only? user-provided only? links to external sources)?
5. Priority of multi-campaign / multi-DM web dashboard features once the single-campaign web surface exists?

---

## 8. Summary Recommendation

**Near-term focus after Solo + Absent:** Session 0 + DM Planning Session. These two features most directly turn the trusted memory into an immersive, personal, low-prep experience.

**Architecture:** Evolve to a hybrid model — Discord (local or hosted) for live play + cloud web app for planning, Session 0, and memory management — sharing the same Supabase source of truth. Preserve the local Ollama path.

**Defer:** Full combat engine and deep rules simulation until the continuity pillars are proven and demanded by real use.

This path stays true to the original magic (“the bot actually remembers and I can trust it”) while systematically removing the real friction points a weekly DM feels.

---

*This document is the starting brief for Post-MVP planning. It can be refined through the same multi-AI review workflow used for Phase 6.*
