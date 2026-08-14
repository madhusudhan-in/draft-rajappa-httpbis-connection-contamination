# draft-rajappa-httpbis-connection-contamination

**Mitigating HTTP/3 Connection Contamination in Multi-Tenant and CDN-Fronted Deployments**

IETF Internet-Draft · BCP · [httpbis](https://datatracker.ietf.org/wg/httpbis/about/) Working Group

---

## Abstract

HTTP/3 clients commonly reuse ("coalesce") an existing QUIC connection for requests to a second origin when the TLS certificate on that connection is also valid for the second origin. A security-relev[...] 

This document defines the underlying mechanism, characterizes the attacker model, distinguishes connection contamination from related QUIC exposures, and provides normative operational guidance for im[...] 

---

## Document Status

| Field | Value |
|---|---|
| Docname | `draft-rajappa-httpbis-connection-contamination-02` |
| Category | Best Current Practice (BCP) |
| Submission | IETF |
| Area | Applications and Real-Time |
| Working Group | httpbis |
| Author | Madhusudhan Rajappa (IBM) |

---

## Repository Contents

| File | Description |
|---|---|
| [`draft-rajappa-httpbis-connection-contamination-02.mkd`](draft-rajappa-httpbis-connection-contamination-02.mkd) | Current draft source (mmark / xml2rfc Markdown) |
| [`draft-rajappa-httpbis-connection-contamination-00.mkd`](draft-rajappa-httpbis-connection-contamination-00.mkd) | Initial submission (-00) |

---

## Building the Draft

The source uses [mmark](https://mmark.miek.nl/) and [xml2rfc](https://xml2rfc.tools.ietf.org/).

```bash
# Install toolchain (once)
pip install xml2rfc
brew install mmark          # macOS; or see https://mmark.miek.nl

# Build HTML and text outputs
mmark draft-rajappa-httpbis-connection-contamination-02.mkd \
      > draft-rajappa-httpbis-connection-contamination-02.xml

xml2rfc draft-rajappa-httpbis-connection-contamination-02.xml \
        --html --text
```

---

## Document Structure

| Section | Title |
|---|---|
| §1 | Introduction |
| §2 | Terminology (RFC 2119 / RFC 8174 key words) |
| §3 | Applicability |
| §4 | Background: Connection Coalescing |
| §5 | Connection Contamination: Threat Description |
| §6 | Recommendations (normative) |
| §7 | Security Considerations |
| §8 | IANA Considerations |

### §6 Recommendations at a Glance

- **§6.1 Per-Request Host Revalidation** — routing layers MUST evaluate `:authority` on every request; MUST respond `421` when they cannot serve it
- **§6.2 Certificate Scope Minimization** — avoid certificates spanning isolation boundaries; if unavoidable, §6.1 applies without exception
- **§6.3 SNI / `:authority` Consistency Enforcement** — mismatch SHOULD trigger revalidation; inability to serve MUST yield `421`
- **§6.4 Trust-Boundary Segmentation** — isolate high-trust origins onto separate listeners as defense-in-depth
- **§6.5 Cache Contamination Mitigation** — cache keys MUST include `:authority`; entries MUST NOT be served across origins
- **§6.6 Detection Guidance** — authorized testing procedure for enumerating and verifying coalescing-eligible pairs

---

## Key Normative References

| RFC | Role |
|---|---|
| [RFC 9114](https://www.rfc-editor.org/rfc/rfc9114) | HTTP/3 — defines connection coalescing (§3.3) and 421 opt-out |
| [RFC 9000](https://www.rfc-editor.org/rfc/rfc9000) | QUIC transport |
| [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110) | HTTP Semantics — 421 Misdirected Request (§15.5.20) |
| [RFC 8446](https://www.rfc-editor.org/rfc/rfc8446) | TLS 1.3 |
| [RFC 6454](https://www.rfc-editor.org/rfc/rfc6454) | Web Origin definition |
| [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) / [RFC 8174](https://www.rfc-editor.org/rfc/rfc8174) | Key word conventions |

---

## Revision History

| Version | Date | Notes |
|---|---|---|
| `-00` | 2026-08-13 | Initial submission |
| `-01` | 2026-08-13 | Fixed stale cross-references (§5.4, §6.2, §6.3); added §6.5 cache contamination mitigation; expanded HTTP/2 parallel note (§4.2) with parallel-BCP pointer; justified revali[...] |
| `-02` | 2026-08-13 | Corrected two reference errors: the [KETTLE2022] entry was updated to cite the original PortSwigger research post that introduced the connection contamination terminology used throughout this document; the [COMSNETS2024] author list was corrected to list only the paper's actual two authors. |

---

## Contributing / Feedback

This draft is targeting the [httpbis working group](https://datatracker.ietf.org/wg/httpbis/about/). Discussion and substantive technical feedback should be directed to the working group's public mailing list (ietf-http-wg@w3.org).

Public archives of that list are available at:

- https://lists.w3.org/Archives/Public/ietf-http-wg/
- https://mailarchive.ietf.org/arch/browse/httpbisd/

Issues and pull requests against this repository are welcome for editorial corrections. Editorial or repository-specific issues are fine to raise here; substantive technical feedback is best discussed on the mailing list.

---

## License

This document is submitted under the IETF Trust's [Legal Provisions Relating to IETF Documents](https://trustee.ietf.org/license-info) (BCP 78, `trust200902`).
