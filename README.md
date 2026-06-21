NFC Relay Attack Case Study — Contactless Payment Security


A technical case study on how NFC relay attacks bypass contactless payment tokenization, and how detection engineering can catch what cryptography can't.

Contactless payments rely on tokenization and cryptographic challenge-response to stay secure — and that cryptography genuinely works. The problem this project investigates is different: NFC relay attacks don't break the crypto, they bypass the assumption that the card is physically next to the terminal.

This repository contains a full technical breakdown of why that gap exists at the protocol level (ISO/IEC 14443), how it's exploited, and — most importantly — how a SOC or fraud-detection team can actually catch it, with working detection logic and simulated trace logs.

📄 Read the full case study (PDF)


🎯 Why This Project

Most write-ups on NFC relay attacks stop at "here's the vulnerability." This one goes further and asks the question a hiring manager actually cares about:


If the cryptography can't stop this, what can — and how would you build that into a detection pipeline?

That framing is intentional. The deliverable isn't just security research — it's a detection-engineering artifact: telemetry design, a SIEM-style correlation rule, and a synthetic trace-log walkthrough comparing a legitimate transaction to a relayed one, frame by frame.


🧩 What's Inside

SectionWhat It Covers1–3Problem background, ISO/IEC 14443 protocol fundamentals, mobile wallet tokenization architecture4–6Annotated NFC handshake walkthrough, cryptogram generation mechanics, timing constraints (FWT/WTX)7–9Relay attack models & threat actors, RTT/distance-bounding theory, effectiveness evaluation10–11Layered countermeasures (terminal / issuer / client), detection telemetry & rule design12Simulated trace logs — legitimate tap vs. relayed tap, side by side13–14Recommendations checklist, glossary, references, lab extension ideas

The centerpiece: Section 12 simulates two transactions with identical-looking cryptograms — one legitimate, one relayed over Wi-Fi Direct — and shows exactly why a naive timeout check misses the attack, while a correlation rule combining RTT drift + geo-velocity + device-lock state catches it.


🔑 Key Findings
Tokenization ≠ relay-proof. Tokens, limited-use keys, and dynamic cryptograms remain cryptographically valid even when relayed — the attack subverts proximity, not secrecy.
Native protocol timeouts (FWT) are too generous. Standard EMV timing budgets (tens–hundreds of ms) comfortably absorb the latency added by Wi-Fi Direct or Bluetooth LE relay links.
RTT alone isn't enough. Tight distance-bounding helps, but real-world deployment faces backward-compatibility and false-positive tradeoffs — effective defense requires a layered model, not one silver bullet.
Detection is achievable today. Combining per-frame RTT anomalies with geo-velocity and device-state signals — even without new hardware — flags relay attempts that pure timeout logic misses.



🛠️ Tech Stack
PurposeToolReport generationPython 3, ReportLabStructureModular content build (helpers.py, decorations.py, content_*.py)OutputClickable in-document TOC + full PDF bookmark/outline navigation

🚀 Extending This Project

Section 14.4 of the report outlines concrete next steps, including:
Implementing the Section 11.2 detection rule against a synthetic transaction dataset
Building the monitoring dashboard concept from Section 11.3
Writing a cost/benefit memo for distance-bounding terminal hardware adoption



⚠️ Scope & Ethics Note

This project is written for a defensive / detection-engineering audience. Hardware and tooling categories are discussed at the same level of detail as published academic literature — no build instructions, firmware, or attacker-usable configuration is included anywhere in this repository.
