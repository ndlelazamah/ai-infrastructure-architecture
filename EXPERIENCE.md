# Experience & Skills Summary

**Director, Digital Dappa**
Designed, built, and operate a self-hosted AI infrastructure and automation platform from the ground up — architecture, security, AI systems, and business automation, end to end, as a solo/lean technical lead.

## Core competencies

- **Infrastructure architecture** — virtualization platform design, network segmentation, perimeter security
- **Security hardening** — reduced an audited environment from High to Low risk; VPN-gated admin access, TLS/cipher hardening, standard security headers, SSH hardening
- **AI systems integration** — deployed local LLM inference, a RAG/knowledge-retrieval service, and a chat-driven automation agent with human-in-the-loop safeguards
- **Automation & reliability engineering** — built a 13-job monitoring/alerting suite with self-healing (auto-restart) behavior and zero-touch backups
- **Media processing pipelines** — FFmpeg/Blender render pipeline triggered by an AI agent rather than manual jobs
- **Business systems** — client-facing lead capture, automated invoicing/PDF generation, and branded client communications

## Selected achievements

- Designed and deployed a Proxmox-based virtualization platform running 8 isolated VMs and a 12-container Docker AI stack
- Took an infrastructure audit from **High risk to Low risk** by redesigning the admin-access model around a VPN gateway and removing broad public exposure
- Built and tested a 13-job automation suite covering health monitoring, backups, lead alerts, SSL expiry checks, and self-restart — infrastructure that surfaces and resolves its own issues without manual checking
- Integrated a local LLM + RAG pipeline to power a client-facing chatbot and internal knowledge lookup, without relying on third-party AI APIs
- Built an automated render pipeline (FFmpeg + Blender) driven by an AI agent that classifies and routes incoming creative work
- Produced client-facing deliverables end to end — branded PDF documents, invoicing flows, and multi-variant business communications

## Technical stack

`Proxmox VE` · `pfSense` · `Docker / Docker Compose` · `Nginx` · `PostgreSQL` · `Ollama` (local LLM) · `Botpress` · `RAG / vector search` · `FFmpeg` · `Blender` · `WireGuard` · `fail2ban` · `Let's Encrypt / certbot` · `CheckMK` · `Bash / cron automation` · `ReportLab (PDF generation)`

## Projects

- [`ai-infrastructure-architecture`](https://github.com/) — full architecture case study, including the security hardening and automation write-ups

---
Figures and technical details here describe real, deployed work; live credentials, IPs, and internal configuration are intentionally omitted (see individual case studies).
