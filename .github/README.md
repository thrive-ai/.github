# PR and issue templates for Thrive repositories

Standard GitHub templates for the Thrive organisation. Deliberately **lean** — a long template gets mostly-deleted, which is worse than no template. Every field below is there because it supports engineering quality *and* serves as evidence for one or more ISO 27001 controls.

## Contents

| File | Path in a GitHub repo | Purpose |
|---|---|---|
| `PULL_REQUEST_TEMPLATE.md` | `.github/PULL_REQUEST_TEMPLATE.md` | Default PR body — summary, type, linked issue, how verified, security/privacy, breaking changes, checklist |
| `ISSUE_TEMPLATE/bug_report.yml` | `.github/ISSUE_TEMPLATE/bug_report.yml` | Structured bug intake |
| `ISSUE_TEMPLATE/feature_request.yml` | `.github/ISSUE_TEMPLATE/feature_request.yml` | Structured feature/enhancement intake — includes data-class and trust-boundary prompts |
| `ISSUE_TEMPLATE/security_concern.yml` | `.github/ISSUE_TEMPLATE/security_concern.yml` | Non-vulnerability security discussion (redirects real vulns to the private Security Advisory flow) |
| `ISSUE_TEMPLATE/config.yml` | `.github/ISSUE_TEMPLATE/config.yml` | Disables blank issues; adds contact links for security vulnerabilities |

## Rollout — two options

### Option A (recommended) — organisation-default via `thrive-ai/.github`

GitHub has a convention: if a repository exists at **`https://github.com/thrive-ai/.github`** and contains a `.github/` folder with these template files, **every repository in the org** inherits them as defaults without any per-repo setup. A repository can still override a given template by having its own file of the same name.

Steps:

1. Create a repository `thrive-ai/.github` (if it does not already exist). It should be **public** or **internal** — private `.github` repos do not propagate templates. A small risk to accept; the repo holds no secrets, only templates and org-level community health files.
2. Give `thrive-admin` Admin on the repo; all other teams **no access** (same model as `thrive-ai/iso-27001-evidence` — integrity of the templates is itself a control).
3. Customise placeholders in `ISSUE_TEMPLATE/config.yml` — search for `REPLACE_ME` and substitute the real Security Advisory and customer-support URLs (see "Customisation required before rollout" below).
4. Run **`.github/scripts/install_org_templates.sh`** from the root of this repo. It uploads the contents of `github-templates/` into `thrive-ai/.github/.github/` via the GitHub Contents API, with one commit per file. The script refuses to run if any `REPLACE_ME` placeholders remain or if the destination repo doesn't exist or is private. Re-running is safe (it updates existing files in place using their current sha).

If you'd rather do it by hand: clone `thrive-ai/.github`, copy `github-templates/.` into the clone's `.github/`, commit, push.

Settings-as-code extensions:
- Also put `SECURITY.md`, `CODE_OF_CONDUCT.md`, `SUPPORT.md` in this repo to have them apply org-wide.
- When the settings-as-code rollout (DESIGN-03 §3.1 gap) lands, `thrive-ai/.github` can hold the Terraform/Flux-style config for branch protection, CODEOWNERS, and environment protection rules too.

### Option B — per-repo copy

For each repository:

1. Copy the files from this `github-templates/` folder into the repo's `.github/` directory, preserving the subfolder structure.
2. Commit, push.

Drawback: each repo is a separate place to maintain the templates, and drift is easy. Prefer Option A.

## Customisation required before rollout

| File | What to update |
|---|---|
| `ISSUE_TEMPLATE/config.yml` | Replace the **`https://REPLACE_ME`** URLs with the Thrive security-vulnerability reporting URL (ideally the GitHub Security Advisory path of each repo — `../../security/advisories/new` — or a canonical org page) and the customer-support URL |

## ISO 27001 control mapping

The templates are not just engineering hygiene — they are evidence artefacts.

| Template field | Control(s) | Evidence role |
|---|---|---|
| PR — "Type" | A.8.32 | Records change category for the change log |
| PR — "Linked issue" | A.8.25, A.8.32 | Ties change back to a captured requirement / defect |
| PR — "How verified" | A.8.29, A.8.32 | Records that testing happened beyond CI (per DESIGN-03 §3.5 — manual verification in `dev`) |
| PR — "Security / privacy notes" | A.8.25, A.8.27, A.8.28 | Forces the dev to consider auth/authz/data/trust-boundary impact at PR time |
| PR — "Breaking changes" | A.8.32 | Captures breaking changes for the change record |
| PR — Checklist (ADR-0001, POL-A8.27) | A.8.27, A.8.32 | Confirms CODEOWNERS and architectural-review obligations have been met where relevant |
| Feature — "Data class impact" | A.8.25 §6.1 | Captures data considerations at requirements stage |
| Feature — "Trust-boundary impact" | A.8.27 §7.1 | Triggers architectural review when needed |
| Security concern — redirect to Security Advisory | Responsible disclosure | Prevents accidental public disclosure of vulnerabilities |

## Maintenance

- Changes to the templates go through the normal PR flow on `thrive-ai/.github` (or each repo for Option B). Branch protection applies.
- These files are reviewed alongside the POL-A8.25 and POL-A8.32 annual review (the templates are the operational expression of those policies).
