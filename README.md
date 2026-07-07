# AI Scribe — Tablet Concept Prototype

An interactive, self-explanatory concept for a governed AI clinical-documentation ("AI scribe") tablet app. Built for engineers who do **not** know EHRs or AI scribes: every screen carries numbered component labels and a walkthrough panel explaining what each part is, why it exists, and what to try.

**All patient data is synthetic.**

## Run it

Open `index.html` in any modern browser. No build step, no server, no dependencies — it is a single self-contained file (fonts and runtime inlined). It also works offline.

## What the prototype demonstrates

The core loop: **listen → transcribe → draft → explain → verify → sign → write back**, with nothing entering the legal record until flagged uncertainty is resolved by a human.

### The 8 screens (tabs)

1. **Home** — safe-work triage. One card per encounter with a live pipeline strip (consent → recorded → drafted → signed → in EHR). Work is sorted by clinical risk, not recency.
2. **Consent & Capture** — five separately-logged consent scopes, device/room readiness, and the recording control. Recording cannot start without core consent.
3. **Transcript + Live verification** — the visit as a mobile-style chat (patient left, clinician right). Every bubble replays its own audio span and links to the note sentences built from it. A docked panel raises safety flags in real time; the clinician resolves them beside the evidence.
4. **Note** — the AI-drafted SOAP note. Every sentence carries a provenance token; tapping opens its evidence drawer. The Sign button is locked while flags remain.
5. **Evidence** — the provenance reference: the 5-class system, the utterance-to-signature pipeline, and a per-sentence inspector.
6. **Patient** — the premium portal summary the patient reads, plus her action rail: counter-sign, push to EHR, and tracked correction requests.
7. **Write-back** — field-level EHR mapping (FHIR), the six-item sign-off gate, clinician signature (content hash), and the EHR push.
8. **Governance** — safety metrics, error taxonomy, model/template version registry, patient-correction queue, and fiscal reporting.

### The provenance system (the core data contract)

Every note sentence is exactly one of:

| Token | Class | Sign-off meaning |
|-------|-------|------------------|
| `TG` | Transcript grounded | Verifiable against the recording |
| `MS` | Model summarised | Clinician must check the abstraction |
| `TP` | Template / copied forward | Clinician must confirm it still applies |
| `CA` | Clinician authored | Human is the source |
| `CV` | Clinician verified | Review action is on the audit ledger |

Provenance is a graph: **audio span → transcript turn → note sentence → review event → signed hash**. Every arrow is stored data.

### Safety mechanics engineers should preserve

- **Sign gate**: signing is blocked while mandatory flags are open; blocked attempts are counted as governance data.
- **Dual signature**: clinician signs (hash + identity frozen) → patient counter-signs → only then can the note be pushed to the EHR.
- **Everything is logged**: consent scopes, pauses, flag resolutions, signatures, pushes, corrections — with identity and timestamp.
- **Patient corrections**: tracked requests with a 5-working-day SLA; serious errors auto-escalate.

## Source

`AI Scribe Tablet.dc.html` in the design project is the editable source; `index.html` here is the bundled build. Concept derived from the "AI scribe system design workbook".
