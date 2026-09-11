# Security Hardening Case Study

A structured hardening pass that moved this infrastructure's risk rating from **High** to **Low**, based on a third-party security audit.

## Starting point

An internal audit flagged the original setup as high-risk: several admin interfaces (firewall GUI, SSH, monitoring dashboards) were reachable directly from the public internet on known or predictable ports, with no VPN gate in front of them.

## What changed

**Perimeter access model.** Every administrative surface — firewall GUI, hypervisor console, SSH to any host, the monitoring dashboard — was moved behind a VPN tunnel. Nothing administrative is reachable from the open internet anymore; only the actual public-facing application ports (web, HTTPS) remain exposed, and only to the services that need them.

**Removed redundant exposure.** A batch of legacy port-forwarding rules — several direct SSH forwards, a misconfigured rule pointing at the wrong internal address, a couple of forgotten HTTP endpoints — were audited and deleted once VPN access covered the same need more safely. Fewer open ports, less attack surface, nothing lost functionally.

**IP-allowlisted the remaining sensitive endpoint.** One analytics dashboard needed to stay reachable outside the VPN for convenience; it was restricted to a named allowlist of admin IPs instead of being open to the world.

**TLS/cipher hardening.** Disabled legacy TLS versions, restricted cipher suites to modern authenticated-encryption ciphers only, enabled forward-secrecy settings, and turned off version-fingerprinting responses from the web server.

**Standard security headers applied globally:** HSTS with preload, clickjacking protection, MIME-sniffing protection, a restrictive referrer policy, and a permissions policy locking down camera/mic/geolocation by default.

**SSH hardening:** disabled root login and password authentication across the board — key-only access — with a low max-retry count before lockout, and brute-force protection (fail2ban-style banning) enabled and tuned so it doesn't lock out the VPN's own address range.

**Snapshot discipline:** a full system snapshot was taken immediately after the hardening pass, so the known-good hardened state can be restored in one step if a future change regresses it.

## Result

| | Before | After |
|---|---|---|
| Admin surfaces reachable from internet | Several | Zero (VPN-gated) |
| Open inbound ports | Broad | Minimal, purpose-scoped |
| TLS configuration | Default | Modern ciphers only, HSTS enforced |
| SSH auth | Password allowed | Key-only, rate-limited |
| Audit outcome | High risk | Low risk |

## What's still open

Hardening is treated as ongoing, not a one-time event — the honest next-steps list includes rolling the same header/version-hiding hardening out to a WordPress front-end, extending brute-force protection to every host (not just the primary one), and scheduling a re-audit to confirm the fixes hold up under a second pass.

---
