<!--
Lean PR template for Thrive repositories.
Keep answers short. If a section is genuinely N/A, write "None" or "N/A" — do not delete the heading.
-->

## Summary

<!-- 1–2 sentences. What changed and why. -->

## Type

- [ ] Feature
- [ ] Fix
- [ ] Refactor
- [ ] Security
- [ ] Infra / CI
- [ ] Docs

## Linked issue

<!-- e.g. Closes #123 — or a short description if no issue exists -->

## How verified

<!-- Concrete check you did. "CI green" is necessary but not sufficient on its own — note the manual verification in `dev` or locally. -->

## Security / privacy notes

<!--
Answer even if "None". Consider:
- Does this touch authentication, authorisation, or tenant routing?
- Does this introduce or change a data class (telemetry, auth, PII, curation config)?
- Does this cross, add, or change a trust boundary (DESIGN-01 §6)?
- Does this introduce a new external dependency or inbound integration?
-->

## Breaking changes

<!-- API, MQTT topic, config format, DB schema, public contract, etc. "None" if none. -->

## Checklist

- [ ] CI is green
- [ ] I verified the behaviour in `dev` (or locally for changes that don't need a cluster)
- [ ] If this touches a CODEOWNERS path (ADR-0001), the right reviewers have been requested
- [ ] If this is a significant architectural change (POL-A8.27 §7.1), an ADR is attached and the relevant design document has been updated in the same PR
