# Jefferson Health Plans — Member Consent Portal (Demo, v2)

A clickable, front-end-only prototype of JHP's **internal customer-service portal** for managing member
data-sharing consent under **CMS-0057** (the CMS Interoperability & Prior Authorization Final Rule).

Built for the **Learning & Development team** to click through the live experience and create training
content for the customer-service team. No backend, no real member data — everything runs in the browser.

The original demo was built by **Beno Mathew**. This v2 copy applies the three-choice consent model
approved by the plan's leadership on 2026-11-03. The original stays live at its own URL for the existing course.

**Live demo (v2):** `https://hike-health.github.io/jhp-consent-portal-v2/` *(available once Pages is enabled — see below)*

---

## What it covers

The portal implements the two member-consent models CMS-0057 requires payers to support. Each consent is now
**three choices: Full / Partial / None**. Purposes, on both: **treatment, payment and operations**.

| Screen | CMS-0057 mapping | Consent model | Choices |
| --- | --- | --- | --- |
| **Share data with healthcare providers** | Provider Access API | **Opt-out** — default **Partial** for a member who has never chosen | **Full** = share all data with the member's treating in-network providers, including behavioral health / substance use treatment (42 CFR Part 2) and other specially protected data · **Partial** = all data except those · **None** = do not share |
| **Transfer data from other health plans** | Payer-to-Payer API | **Opt-in** — no default; "Not asked" is a valid state | Per previous plan: **Full** = request all data, including behavioral health / substance use treatment and specially protected data · **Partial** = all data except those · **None** = do not request |

**Full needs an active, informed choice.** When the rep selects Full, the screen shows the consent text and a
state-law notice (HIV-related information; for New Jersey members, genetic information) for the rep to read to
the member before confirming. The notice text is starting language; the plan's language owners will refine it.

Every change requires the rep's attestation ("I confirm that the member explicitly requested this change ...
during this interaction"), shows a toast, and adds an entry to that consent's **history** (choice, date,
captured by "Member services").

### Clickable flows
- **Sign in** → CSR landing page
- **Member search** — by name, member ID, or phone, with live-filtered results and recent members
- **Member profile** — demographics + current consent status for both consent types
- **Provider consent** — Full / Partial / None radios, read-aloud text for Full, required attestation, confirmation + toast, consent history
- **Payer-to-Payer transfer** — multi-select prior plans, a Full / Partial / None choice per plan, read-aloud text for Full, attestation, submission with reference ID
- **Manage member consents** — plans grouped by Full / Partial / None / Not asked / Expired
- **Review consent** — per-plan detail (current choice, date, last transfer, transfer status, consent history) and a Full / Partial / None change with a confirmation modal and attestation
- **Plan transfer-status** detail page
- **Search by plan** within a member's consents
- **Toast notifications** for every consent change
- **Guided tour** — three walkthroughs:
  1. *Change Provider Access sharing* (Partial → None)
  2. *Payer-to-Payer data exchange* (request a prior plan's records with a Partial choice)
  3. *Member asks to share everything (Full)* (Provider Access Partial → Full, reading the consent text and state notice)
- **Reset demo** — restores the sample data

> Sample members: **John Doe** (ID `123456`; Provider Access at the default Partial; plans at Full, Partial,
> None, Not asked and Expired) and **Sarah Brown** (ID `456789`; Provider Access None; Humana Full, Aetna Partial).
> All data is fictional and resets on logout/refresh.

---

## What changed from v1

- Provider Access: the opted-in / opted-out toggle is replaced by **Full / Partial / None** radios. Default is **Partial** (shown as "Partial (default)" until the member chooses).
- Payer-to-Payer: Opt-in / Opt-out per plan is replaced by **Full / Partial / None** per plan. Status per plan is Full / Partial / None / Not asked / Expired. The profile shows a count per status.
- **Full** shows the consent text and the state-law notice for the rep to read before confirming.
- Purposes text on both: "treatment, payment and operations".
- Attestation wording on every change: "I confirm that the member explicitly requested this change to … during this interaction". A new choice clears the attestation, so it always matches the choice being saved.
- New **consent history** per consent (choice, date, "Member services").
- Guided tour: flows 1 and 2 use the new controls; new flow 3 "Member asks to share everything (Full)".
- Small fix: the plan-search button no longer causes horizontal scroll at phone width.

---

## Deploy to the HIKE-HEALTH org (one step)

This folder is a **ready-to-push static site** — it's already a committed git repo (it has no remote, so it
cannot push to the v1 repo by accident). Pick one path.

### Option A — GitHub CLI (fastest)
Requires the [`gh` CLI](https://cli.github.com/) signed in to an account with access to the `HIKE-HEALTH` org.

```bash
cd "jhp-consent-portal-v2"

# Create the repo under HIKE-HEALTH and push this folder
gh repo create HIKE-HEALTH/jhp-consent-portal-v2 --public --source=. --remote=origin --push

# Turn on GitHub Pages (serves the root of the main branch)
gh api -X POST repos/HIKE-HEALTH/jhp-consent-portal-v2/pages -f "source[branch]=main" -f "source[path]=/"
```

Your shareable link (live in ~1 minute):
**https://hike-health.github.io/jhp-consent-portal-v2/**

### Option B — plain git + the web UI
```bash
cd "jhp-consent-portal-v2"
git remote add origin https://github.com/HIKE-HEALTH/jhp-consent-portal-v2.git
git push -u origin main
```
Then on GitHub: **Settings → Pages → Build and deployment → Source: "Deploy from a branch" → Branch: `main` / `(root)` → Save.**
The link appears at the top of that page after ~1 minute.

> Private org? Either make the repo public (Pages link is public regardless), or use a GitHub
> Team/Enterprise plan that allows Pages on private repos.

---

## Run locally
It's a single static file — just open `index.html` in any browser, or:
```bash
cd "jhp-consent-portal-v2" && python3 -m http.server 8080
# visit http://localhost:8080
```

## Files
- `index.html` — the entire app (HTML + CSS + JS, no dependencies)
- `.nojekyll` — tells GitHub Pages to serve files as-is
- `README.md` — this file

---
*Original demo by Beno Mathew. Internal training demonstration only. Not a production system and not connected to any member data source.*
