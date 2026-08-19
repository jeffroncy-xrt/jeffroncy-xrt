## Hey, I'm Jeff 👋

I build systems that run without me.

Media pipelines that generate, assemble and publish video on a schedule. Trading
infrastructure that reconciles against a venue instead of trusting its own memory.
And the operational layer that keeps all of it alive on one small cloud instance.

![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=flat-square&logo=python&logoColor=white)
![systemd](https://img.shields.io/badge/systemd-timers%20%2B%20cgroups-1793D1?style=flat-square&logo=linux&logoColor=white)
![ffmpeg](https://img.shields.io/badge/ffmpeg-render%20pipeline-007808?style=flat-square&logo=ffmpeg&logoColor=white)
![Hetzner](https://img.shields.io/badge/Hetzner-Cloud-D50C2D?style=flat-square&logo=hetzner&logoColor=white)
![GPU](https://img.shields.io/badge/rented%20GPU-open--weights%20video-76B900?style=flat-square&logo=nvidia&logoColor=white)
![pytest](https://img.shields.io/badge/pytest-green?style=flat-square&logo=pytest&logoColor=white)

### The shift

I used to think the hard part was writing the code. It isn't. The hard part is
everything that happens at 05:00 when nobody is watching — the render that stalls
without failing, the upload that runs twice, the job that quietly eats the host.

So most of what I build now is the part that decides whether the rest is allowed
to run.

### The setup

**Five systems. One small VPS. No operator.**

|  |  |
|---|---|
| **Host** | one Hetzner VPS — 4 vCPU, 8 GB RAM |
| **Running** | 2 always-on services + 3 scheduled video pipelines |
| **Track record** | 39 days continuous on the previous host, before migrating 2026-08-14 |
| **Output** | 3 pipelines publishing daily, unattended |
| **Bursts to** | rented GPU, per-minute, for open-weights video generation |

Everything about how these are scheduled, capped and prioritised follows from that
one constraint. They run on the instance they cost.

### Repositories

Each is a **portfolio excerpt** — a reduced, runnable subset of a private
production system, with credentials, identity and the parts I still operate
commercially removed. Each README says what was left out.

| Repository | What it shows |
|---|---|
| [**unattended-ops**](https://github.com/jeffroncy-xrt/unattended-ops) | systemd units with cgroup memory ceilings and priority tiers, drift-proof deploys verified by checksum, two incident postmortems, and a case study on migrating the estate across three providers |
| [**docs-video-pipeline**](https://github.com/jeffroncy-xrt/docs-video-pipeline) | Timeline planning and ffmpeg assembly for a daily documentary render, inside a hard memory ceiling |
| [**news-video-pipeline**](https://github.com/jeffroncy-xrt/news-video-pipeline) | LLM script generation in Spanish, editorial linting of generated copy, and a publish gate that can refuse |
| [**repost-pipeline**](https://github.com/jeffroncy-xrt/repost-pipeline) | Crash-safe scheduled publishing with a rolling buffer, reconciled against the platform API |
| [**prediction-market-bot**](https://github.com/jeffroncy-xrt/prediction-market-bot) | Event-sourced execution layer for a prediction market — replayable state, tick constraints, precondition guards |
| [**perps-news-bot**](https://github.com/jeffroncy-xrt/perps-news-bot) | News ingestion, cross-source deduplication and risk limits for a news-driven trading system |

### What I actually think about

**Unattended software is judged on how it fails.** Anything here that looks
defensive is there because the alternative already happened once. A marker written
before every upload, because a crash mid-upload otherwise posts a second copy. A
publish gate that refuses a short episode, because one published itself before
anyone looked. A ceiling on every batch job, because a render without one took an
entire host down.

**Silent failure is the expensive kind.** A stalled video render produces a
full-length file with repeated frames, so every duration check passes while the
output is broken. A process sitting between a soft and a hard memory limit gets
throttled forever and never killed, and the supervisor reports it as healthy. Both
are in the postmortems.

**Measure before you tune.** A local model I added to cut an API bill turned out to
score true matches *below* random noise — an inverted margin — because I'd compared
single words against a sentence encoder. The fix was visible in ten minutes of
measurement and invisible in any amount of reasoning about it.

**A migration is not a file copy.** Moving this estate across three providers —
managed serverless, then one VM, then a cheaper and larger VPS — cut the bill by
74% while quadrupling the RAM. None of the difficulty was in the code. It was in the
publish ledgers and cached SDK credentials that live outside version control, a
hand-built `ffmpeg` whose distro replacement would have changed render output
without erroring, and a firewall posture that turned out to be a property of the
old provider rather than of anything I had configured. Written up in
[unattended-ops/migrations](https://github.com/jeffroncy-xrt/unattended-ops/blob/main/migrations/three-hosts-one-year.md).

**Rent the expensive thing.** Replacing a managed video-generation API with an
open-weights model (22B, fp8) on rented GPU was justified by one hour of rented
card time costing about $0.70. It answered whether the model fit in 32 GB, how long a
clip took, and what one unit of real work cost — **$0.17 against €5.60** — before
anything was committed to. Owning a GPU for a job that uses it 4% of the day
costs ~$245/mo; renting it per-minute costs ~$5.

**Constraints are a design input.** These systems run on one small instance because
that is what they cost. Everything else follows.

### Stack

Python · systemd (timers, cgroup resource control) · ffmpeg · yt-dlp · LLM APIs ·
open-weights video models on rented GPU · Hetzner Cloud, previously AWS EC2 and
Azure · Tailscale · ufw/AppArmor · bash · pytest

### Connect

[![Email](https://img.shields.io/badge/Email-jeff.roncy%40gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:jeff.roncy@gmail.com)
