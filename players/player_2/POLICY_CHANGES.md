# Group 2: replacement-aware discards

The idea is to ask whether replacing a sock would improve matching, rather than
discarding it simply because its shade is unusual.

## Which socks to wear

Keep the existing pair rule. If every pair has a penalty, wear the pair with the
smallest penalty. Among zero-penalty pairs, prefer the pair whose aging leaves
the projected same-colour shade distributions tightest.

## Which leftover to discard

Record the last 40 observed shades of each colour. For each leftover, compare:

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
