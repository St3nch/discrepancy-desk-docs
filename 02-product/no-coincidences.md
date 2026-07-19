# No Coincidences

## Concept

**No Coincidences** is a future module that scans the Anomaly Vault for strange overlaps and creates pattern candidates for human review.

It does not declare truth.

It opens drawers.

## Canon Line

> Crumbs open drawers. They do not close cases.

## What It Detects Later

Possible overlaps:

- repeated names
- repeated dates
- repeated organizations
- repeated locations
- repeated phrases
- source structure reuse
- story template reuse
- symbol repetition
- timeline coincidence
- topic/entity clusters

## Candidate Scores

- Weirdness score
- Confidence score
- Source independence score
- Content potential score
- Risk level

## Human Review Labels

- Open Drawer
- Park It
- Coincidence Likely
- Source Copying
- Brain Soup Gold
- Thread Candidate
- Needs Primary Source

## Early Implementation

Start simple:

- exact entity matching
- date matching
- organization matching
- topic overlap counts
- manual review

Later:

- embeddings
- Qdrant similarity
- source independence scoring
- graph relationships
- alias detection
- anomaly scoring

## Example Output

```markdown
# No Coincidences Flag 0007

The same phrase pattern appears across 6 giant skeleton stories.

Finding:
Several sources appear to reuse the same “discovered by workmen, reported by local paper, remains disappeared” structure.

Possible explanations:
- syndicated newspaper filler
- folklore drift
- hoax template
- actual repeated reporting pattern

Clerk note:
Not proof. But the phrase has fingerprints.
```
