# Value and risk methodology

## Opportunity dimensions

Score each dimension from 0 to 100.

| Dimension | Weight | What high means |
|---|---:|---|
| fit | 25% | Solves recurring, costly tasks for the target user |
| demand | 20% | Multiple credible signals show a real and growing need |
| leverage | 15% | Saves substantial time, reduces errors, or unlocks a new capability |
| quality | 15% | Specific triggers, complete workflow, useful resources, and validation |
| momentum | 10% | Meaningful recent adoption or contribution growth, not vanity spikes |
| maintenance | 10% | Active, responsive, compatible, licensed, and not dependent on one abandoned component |
| uniqueness | 5% | Offers a defensible advantage over substitutes |

`opportunity = weighted mean of available dimensions`.

If the user's profile is unknown, score `fit` as broad recurring applicability and state that personalization may change the ranking.

## Risk dimensions

| Dimension | Weight | What high means |
|---|---:|---|
| permissions | 25% | Broad filesystem, identity, messaging, payment, or production access |
| execution | 20% | Destructive shell use, remote execution, installers, or unsafe defaults |
| network | 15% | Unbounded fetches, untrusted endpoints, or data transfer |
| secrets | 15% | Reads, stores, logs, or transmits credentials or private data |
| obfuscation | 10% | Encoded payloads, hidden instructions, generated binaries, or evasive behavior |
| provenance | 10% | Unclear author, mutable source, weak license, or hijack exposure |
| mismatch | 5% | Listing, instructions, scripts, and repository purpose disagree |

`risk = weighted mean of available dimensions`.

Hard-gate as `quarantine` for credential theft, covert exfiltration, unexplained privilege escalation, destructive defaults, obfuscated execution, or a material repository/Skill mismatch.

## Evidence confidence

Score evidence completeness from 0 to 100:

- 25: canonical `SKILL.md` inspected.
- 20: scripts and dependencies inspected.
- 15: repository history and ownership inspected.
- 15: license and compatibility checked.
- 15: adoption or demand signal observed.
- 10: independent corroborating source observed.

`confidence_factor = 0.4 + 0.6 × evidence_confidence / 100`.

`value_score = clamp(opportunity × confidence_factor − 0.35 × risk, 0, 100)`.

The 40% floor avoids treating new Skills as worthless solely because they are new; they still cannot rank highly without evidence. Risk remains visible as a separate value.

## Action bands

| Value | Default action |
|---:|---|
| 75–100 | adopt or sandbox-test now |
| 60–74 | shortlist and test |
| 40–59 | watch or compare with substitutes |
| 0–39 | ignore |

Override bands with `quarantine` when a hard gate is present. For production or sensitive-data use, require risk below 30 and evidence confidence at least 70 before recommending adoption.

## Snapshot schema

Each candidate should contain:

```json
{
  "id": "owner/repo/path",
  "name": "skill-name",
  "url": "https://github.com/owner/repo/tree/main/path",
  "scores": {
    "fit": 0,
    "demand": 0,
    "leverage": 0,
    "quality": 0,
    "momentum": 0,
    "maintenance": 0,
    "uniqueness": 0,
    "permissions": 0,
    "execution": 0,
    "network": 0,
    "secrets": 0,
    "obfuscation": 0,
    "provenance": 0,
    "mismatch": 0,
    "evidence_confidence": 0
  },
  "hard_gate": false,
  "evidence": [],
  "observed_at": "ISO-8601 timestamp"
}
```

Use `null` for unknown scores. The calculator reweights available dimensions and reports missing fields.
