## Automation and AI engineering

I build systems that run without me. Media pipelines that generate, assemble
and publish video on a schedule; trading infrastructure that reconciles against
a venue instead of trusting its own memory; and the operational layer that
keeps five of them alive on one small cloud instance.

**One small cloud instance:** two always-on services and three scheduled video
pipelines, sharing the host under explicit memory ceilings and priority tiers,
all publishing daily and unattended. The estate has since been migrated between
cloud providers to cut running cost by roughly 60%, with no change to the
schedule it publishes on.

### Repositories

Each is a **portfolio excerpt** — a reduced, runnable subset of a private
production system, with credentials, identity and the parts I still operate
commercially removed. Each README says what was left out.

| Repository | What it shows |
|---|---|
| [**unattended-ops**](https://github.com/jeffroncy-coder/unattended-ops) | systemd units with cgroup memory ceilings and priority tiers, drift-proof deploys verified by checksum, and two incident postmortems |
| [**docs-video-pipeline**](https://github.com/jeffroncy-coder/docs-video-pipeline) | Timeline planning and ffmpeg assembly for a daily documentary render, inside a hard memory ceiling |
| [**news-video-pipeline**](https://github.com/jeffroncy-coder/news-video-pipeline) | LLM script generation in Spanish, editorial linting of generated copy, and a publish gate that can refuse |
| [**repost-pipeline**](https://github.com/jeffroncy-coder/repost-pipeline) | Crash-safe scheduled publishing with a rolling buffer, reconciled against the platform API |
| [**prediction-market-bot**](https://github.com/jeffroncy-coder/prediction-market-bot) | Event-sourced execution layer for a prediction market — replayable state, tick constraints, precondition guards |
| [**perps-news-bot**](https://github.com/jeffroncy-coder/perps-news-bot) | News ingestion, cross-source deduplication and risk limits for a news-driven trading system |

### What I actually think about

**Unattended software is judged on how it fails.** Anything here that looks
defensive is there because the alternative already happened once. A marker
written before every upload, because a crash mid-upload otherwise posts a
second copy. A publish gate that refuses a short episode, because one published
itself before anyone looked. A ceiling on every batch job, because a render
without one took an entire host down.

**Silent failure is the expensive kind.** A stalled video render produces a
full-length file with repeated frames, so every duration check passes while the
output is broken. A process sitting between a soft and a hard memory limit gets
throttled forever and never killed, and the supervisor reports it as healthy.
Both are in the postmortems.

**Constraints are a design input.** These systems run on one small instance
because that is what they cost. Everything about how they are scheduled,
capped and prioritised follows from that.

### Stack

Python · systemd (timers, cgroup resource control) · ffmpeg · yt-dlp ·
LLM APIs · Linux cloud instances (Azure, AWS EC2, current host) · bash · pytest
