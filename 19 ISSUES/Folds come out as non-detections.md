---
type: issue
created: 2026-01-18
system: ptuse
symptom: Folds come out as non-detections even at the known period and DM
status: solved
project: 
---
> [!limitation] Symptom
> A source that was detected last epoch folds to nothing this epoch, same period, same DM,
> same pipeline. No error anywhere.

> [!hypothesis] Cause
> The band was recorded the other way round, so the frequency axis is inverted and the
> fold smears out.

> [!result] Fix
> Re-clean with the flip flag before anything else:
> ```bash
> filtool -v -z "kadane 4 8 zdot" --flip -o cleaned raw.fil
> ```

> [!source] Came up in
> [[EXAMPLE-0000 search]]

The field that matters here is `symptom`, not `system`. A year from now you will search
for "non-detections", because if you already knew it was a sideband inversion you would
not be searching at all.
