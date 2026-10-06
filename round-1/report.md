# round-1 — Observe

Team: BB-002
Queries used: 79 / 150

## What we concluded

The strongest observed effect was from `comorbidity_ratio`.

With the other baseline inputs held fixed, changing `comorbidity_ratio` from 0 to 1 changed the score from 0.9786 to 0.8177. This is a decrease of 0.1609, indicating that this feature has a strong negative effect on the model output.

We also observed a smaller change associated with `years_registered`. At `years_registered = 0`, the score was 0.9684, while values around 5 to 20 produced approximately 0.9786 under otherwise similar baseline inputs. This suggests possible nonlinear or threshold-like behaviour near the low end.

## How we got there

We used controlled black-box queries, keeping the other input values fixed while changing one feature at a time.

Our main baseline used:

- age = 73
- baseline_score = 750
- comorbidity_ratio = 0
- dependants = 0
- prior_visits = 20
- recent_admissions = 0
- requested_beds = 100
- vitals_index = 100
- ward = B
- years_registered = 10

For `comorbidity_ratio`, changing the value from 0 to 1 produced a large score decrease from 0.9786 to 0.8177.

For `years_registered`, a value of 0 produced 0.9684, while values in the 5–20 range were around 0.9786.

## What we ruled out

We did not establish that every input has a large effect. Our experiments focused on identifying features with observable changes in the output.

We did not establish an exact threshold for `years_registered`, because we did not perform a sufficiently fine sweep around the transition.

We also did not establish interactions between features in this round.

## What we are still unsure about

The exact functional relationship between `years_registered` and the score remains uncertain.

The magnitude of the `comorbidity_ratio` effect was established from the observed 0-to-1 comparison, but its behaviour at intermediate values was not fully mapped.

We were unable to recover the individual query identifiers after the black-box server became inaccessible, so the findings record the observations without fabricating query IDs.
