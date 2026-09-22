# Group 2: replacement-aware discards

The idea is to ask whether replacing a sock would improve matching, rather than
discarding it simply because its shade is unusual.

## Which socks to wear

Keep the existing pair rule. If every pair has a penalty, wear the pair with the
smallest penalty. Among zero-penalty pairs, prefer the pair whose aging leaves
the projected same-colour shade distributions tightest.

## Which leftover to discard

Record the last 10 observed shades of each colour. For each leftover, compare:

- its average mismatch against recent socks of the same colour;
- a pristine sock's average mismatch against those same observations.

Mismatch is zero for a shade gap of at most 6, otherwise the full gap. A pristine
white sock has shade 255; a pristine black sock has shade 0. Discard at most one
leftover: the one whose replacement improves the average by more than 6 points.
Require at least 10 same-colour observations before using this estimate.

For example, if recent white socks are near 250, replacing a leftover at 200
with one at 255 should help. If recent white socks are near 200, keep it.

## Budget protection

Keep a reserve for estimated future hole replacements plus two safety packs.
Permit voluntary discards only when the fraction of budget remaining is more
than 0.05 ahead of the fraction of time remaining. Disable voluntary discards
when five or more socks are offered.

The reserve estimates available wear from recent observed shades, assumes about
C-4 socks split evenly between colours, and uses 68 expected wears per new sock.
These are approximations because the actual drawer and discard counters are
hidden. Replacement happens only after six household discards of a colour;
the matching estimate treats the eventual replacement as pristine and does not
predict the exact delay. It also averages individual pair costs rather than
predicting the best pair in a future hand. Budget protection is a heuristic,
not a survival guarantee against arbitrary roommate spending.

## Comparisons

28 socks, 4 players, 4-sock hands, 730 days; shared household budgets. Each entry
averages the same 30 seeds (4001–4030). Scores are cumulative embarrassment per
player: all four seats averaged for clones, only our player for mixed households.
The gain cutoff is >6 throughout. The window affects both matching and reserve estimates.

| Household | Budget | Window 40, limit 1 | Window 40, limit 2 | Window 10, limit 1 | Window 10, limit 2 |
|---|---:|---:|---:|---:|---:|
| Four copies of our policy | $120 | 1167.0 | 1084.4 | 453.9 | 461.2 |
| Four copies of our policy | $400 | 582.1 | 552.8 | 51.2 | 57.9 |
| Our policy + 3 RandomPlayers | $120 | 1777.9 | 1759.7 | 1414.4 | 1304.5 |
| Our policy + 3 RandomPlayers | $400 | 1469.8 | 1431.9 | 946.5 | 970.6 |
| Our policy + 3 Group 7 players | $120 | 1018.0 | 993.0 | 796.2 | 781.4 |
| Our policy + 3 Group 7 players | $400 | 502.3 | 500.4 | 332.3 | 300.8 |
| Our policy + 3 Group 10 players | $120 | 1829.2 | 1697.3 | 1380.5 | 1327.8 |
| Our policy + 3 Group 10 players | $400 | 837.5 | 840.2 | 596.7 | 591.9 |
| Our policy + Group 7, Group 10, RandomPlayer | $120 | 1622.2 | 1622.2 | 1287.2 | 1273.4 |
| Our policy + Group 7, Group 10, RandomPlayer | $400 | 818.2 | 815.5 | 610.7 | 592.8 |

The shorter window improved all ten comparisons. Allowing two discards gave mixed
results; the paired 95% intervals for its effect at window 10 all included zero.
We kept window 10 and the one-discard limit. No sockless days or faults occurred
in these 1,200 games. Individual runs are in `comparison_results.csv`.

## Running experiments

Run from the repository root with Python 3.12+. Player codes are group numbers,
`r` for RandomPlayer, and `g` for GreedyPlayer. Each comma-separated roster lists
the players in seat order. Use `--help` to change seeds, budget, drawer size, hand
size, game length, scoring seats, gain cutoff, window, or discard limit.

To reproduce the saved comparison, first fetch the September 21 opponent snapshot:

```bash
git fetch https://github.com/ananyakapoor19/coms4444-socks.git 8ddbf1da444305a57c12cf2b9c60b04f5ffbdd6b
python players/player_2/run_experiments.py \
  --rosters 2,2,2,2 2,r,r,r 2,7,7,7 2,10,10,10 2,7,10,r \
  --windows 40 10 --discard-limits 1 2 --budgets 120 400 \
  --capacity 28 --hand-size 4 --days 730 --seed-start 4001 --num-seeds 30 \
  --opponent-revision 8ddbf1d --workers 6 --output /tmp/socks_comparison.csv
```

The CSV output path must be new. Each row records settings, scored seats,
embarrassment, household spending, sockless player-days, faults, opponent revision,
source hash, and Python version. Both simulator and random-opponent RNGs are seeded.

Omit `--opponent-revision` to use opponents from your current checkout. That option
pins numbered opponents' `player.py` files; helper imports still use the checkout.
Group 2 and simulator code always come from the checkout. The two Group 2 knobs
are changed only for experimental instances; the normal player still defaults to
window 10 and at most one discard.
