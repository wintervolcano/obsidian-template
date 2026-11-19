---
type: candidate
target: EXAMPLE-0000
project: "[[_Example Project]]"
p0_ms: 1.0322
f0: 968.80
dm: 426.037
acc: -3.1
snr: 14.2
epoch: 2026-01-15
beam: ptuse4
segment: full
cand_id: cand_0417
status: new
same_as: 
---
> [!method] Fold command
> ```bash
> psrfold_fil -v --nbin 128 --nsubband 64 -c 1.0322e-3 --dm 426.037 --acc -3.1 \
>   -o cand_0417 EXAMPLE-0000_ptuse4.fil
> ```

> [!result] Plot

> [!source] Found in
> [[EXAMPLE-0000 search]]

The name carries the period and DM, so typing `EXAMPLE P1.03` in the quick switcher pulls
up every neighbour without opening a Base. Sort `Candidates.base` by period vault-wide and
the same period in two different fields is interference, not a pulsar.
