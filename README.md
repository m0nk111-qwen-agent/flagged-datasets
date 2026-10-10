# flagged-datasets

A machine-readable registry of **public datasets that peer-reviewed literature has flagged** — for unverifiable authenticity, missing provenance metadata, or documented misuse patterns — with every flag quoting and citing its published source.

**This registry makes no claims of its own.** Each row aggregates what a dated, citable publication says about a dataset. Where a source says a dataset's authenticity could not be verified, the row says exactly that, with the citation. Where a dataset's authenticity is *not* in question (see the misuse-pattern rows), the row says so explicitly.

## Why

Papers flag datasets. The flags live in PDFs. A researcher who is about to build a model on a dataset has no single place to check whether someone has already published concerns about it — and the cost of missing a flag is real: one 2026 study traced two popular Kaggle health datasets to **125 clinical prediction-model studies, 3 models used in clinical practice, 1 cited in a medical device patent, and 86 review articles**, while concluding the datasets' authenticity could not be verified (Gibson et al., BMC Medicine 24:386, 2026-06-04, [doi:10.1186/s12916-026-04981-y](https://doi.org/10.1186/s12916-026-04981-y)). Separately, Springer Nature retracted and removed ~40 publications trained on one Kaggle dataset (The Transmitter, 2025, [doi:10.53053/XFKN9077](https://doi.org/10.53053/XFKN9077)), and NHANES/GBD misuse triggered 2025 policy changes at PLOS and Frontiers.

Analogies exist for papers (Retraction Watch) and reporting standards (TRIPOD+AI), but not, as far as we could find as of 2026-10-10, for **datasets** — machine-readable, cite-first, cross-repository.

## Files

- [`registry.jsonl`](registry.jsonl) — one JSON object per flagged dataset. Fields: `id`, `name`, `uploader`, `url`, `domain`, `status`, `flags[]` (each with `type`, `claim` — the source's own wording — and a full `source` block with venue/date/doi), `downstream_impact`, `registered`, `notes`.

### Flag types (and what they deliberately do NOT say)

| type | meaning | honesty note |
|---|---|---|
| `unverifiable-authenticity` | a peer-reviewed source states the dataset's authenticity could not be verified | the registry's strongest class; always cite-backed |
| `no-provenance-metadata` | no information on when/where/why/how/by whom the data were collected | metadata absence, not proof of fabrication |
| `retraction-wave` | publications using the dataset were retracted/removed in numbers | row may point at a pattern even when the dataset itself is not publicly named |
| `formulaic-research-magnet` | documented explosion of formulaic secondary research, inappropriate study designs, false discoveries | **the dataset itself is usually fine** — the flag is about the research pattern around it; rows say this explicitly |

## Method and rules

0. **Snapshot semantics.** Rows are *as-of their citation dates* — this is a dated, versioned snapshot, not a live feed. A flag may later be resolved, retracted, or superseded; the git history preserves the record either way. Corrections beat deletions.
1. **Cite-led only.** A row exists because a dated, citable publication flags the dataset. No rumor, no "vibes", no LLM guesses. If the source is a news report of a publisher action (e.g., the retraction wave), the row says so.
2. **Quote, don't paraphrase, the key claim** — `flags[].claim` carries the source's own wording (abridged with `[...]` where needed).
3. **Separate authenticity flags from misuse-pattern flags.** NHANES is a real CDC survey; GBD is a real IHME program. Their rows exist because the *research pattern* around them is documented. Confusing the two classes would be exactly the kind of unverifiable claim this registry exists to prevent.
4. **Corrections and additions by pull request**, each with a citation. Rows can be improved (e.g., the unnamed retraction-wave dataset) but never deleted — history stays in git.
5. **No legal or defamatory intent.** Flagged ≠ fraudulent; every row defers to its sources, and dataset maintainers who resolve concerns publicly should open a PR citing the resolution.

## Scope and limits (v1)

- Seeded from the primary literature available and read in full on 2026-10-10: Gibson et al. (BMC Medicine 24:386) and its cited prior cases. Five rows. This is a seed, not a survey — no systematic literature sweep has been run yet.
- Health/clinical + ML-training domains only so far.
- No downstream-impact tracking per dataset beyond what sources state.
- Maintained by [Pennyforge](https://dev.to/pennyforgehq) (a one-person studio). CC0 1.0 — copy it, fork it, improve it.
- [Announced on dev.to, 2026-10-10](https://dev.to/pennyforgehq/when-a-paper-flags-your-training-data-the-warning-lives-in-a-pdf-14je).

## Related work

- [Retraction Watch](https://retractionwatch.com/) — retractions of *papers* (our rows cite it where relevant).
- [TRIPOD+AI](https://doi.org/10.1136/bmj-2023-078378) — reporting checklist whose provenance items this registry borrows as flag vocabulary.
- PLOS (2025-09) and Frontiers (2025-09-15) fast-churn data policies — publisher-side responses this registry documents as downstream impact.
- Vines et al., Current Biology 2014 — data loss from unavailability (~17%/yr), the access half of the problem this registry does *not* track.

## Verification record

- 2026-10-10: all three seed citations resolve (doi.org HTTP 200 for 10.1186/s12916-026-04981-y, 10.53053/XFKN9077, 10.1371/journal.pbio.3003152); Crossref metadata confirms the PLoS Biology row's authors (Suchak, Aliu, Harrison, Zwiggelaar, Geifman, Spick) and date (2025-05-08); the BMC Medicine 24:386 paper was read in full (PDF) — the two Kaggle dataset URLs in rows 1–2 come from its reference list (refs 21–22). A skeptic review (independent delegate, 2026-10-10) judged the artifact BUILD-ON-conditional: verified citations, snapshot semantics, and a demand-side probe gate any v2 expansion.
- 2026-10-10: Kaggle-side surface check (do the dataset pages themselves surface flag/discussion signals?) is BLOCKED by Kaggle's bot wall — open, cheapest with a logged-in session.
