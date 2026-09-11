# AI-Triggered Media Rendering Pipeline

An automated video/3D render pipeline that takes a creative brief from intake to finished output with no manual job-launching.

## The problem

Creative render jobs (video encoding, 3D rendering) were being triggered manually — someone had to notice a new brief had arrived, log in, and kick off the right job by hand. That doesn't scale and introduces delay between "client sends brief" and "render starts."

## Architecture

- **Intake** — creative briefs and source files land via a secure file-transfer drop folder rather than a public upload form, keeping the intake surface small.
- **Job classification** — an AI agent inspects the incoming job and decides its type (2D video encode vs. 3D render) rather than a human manually sorting the queue.
- **Render API** — a small internal API receives the classified job and dispatches it to the right engine: FFmpeg for video encoding, Blender for 3D rendering.
- **Output handoff** — completed renders land in a dedicated output folder, ready for pickup, with the original request traceable end to end.
- **Queue monitoring** — a scheduled check watches for jobs stuck in the intake queue beyond a time threshold and alerts if the pipeline stalls, so a stuck job doesn't sit silently for hours.

## Design decisions worth calling out

**Agent-driven, not webhook-driven.** The deliberate choice was to route job classification through an AI agent rather than a simple webhook/rules engine — new job types and edge cases can be handled by adjusting the agent's instructions rather than rewriting routing logic every time a new format shows up.

**Drop-folder intake over a public form.** Keeping the intake surface to an authenticated file-transfer folder rather than a public web upload reduces the attack surface while still being simple for clients to use.

**Watch the queue, not just the job.** Monitoring isn't just "did this job succeed" — it's "is anything stuck," which catches a different, quieter class of failure (a job that never started) that per-job monitoring alone would miss.

**Internal-only by default.** The render API itself isn't directly internet-facing; it's reached only through the reverse proxy with authentication, or from inside the network — external access is deliberately narrow.

## Outcome

A creative brief now moves from intake to finished render with zero manual job-launching, and a stalled queue surfaces itself as an alert rather than as a client complaint days later.
