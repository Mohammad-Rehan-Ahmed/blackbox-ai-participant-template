# Experiment: comorbidity_ratio and years_registered

## Team
BB-002

## Round
round-1

## Objective
Determine whether changing comorbidity_ratio and years_registered while keeping the other baseline inputs fixed produces meaningful changes in the model score.

## Baseline inputs

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

## Input strategy

We used controlled one-variable-at-a-time comparisons.

For comorbidity_ratio, we changed the value from 0 to 1 while keeping the other baseline inputs fixed.

For years_registered, we compared years_registered = 0 with values around 5–20 while keeping the other inputs otherwise similar.

## Results

Changing comorbidity_ratio from 0 to 1 changed the model score from 0.9786 to 0.8177, a decrease of 0.1609.

For years_registered, the score was 0.9684 at years_registered = 0, while values around 5–20 produced approximately 0.9786.

The comorbidity_ratio change was substantially larger than the observed years_registered change.

## Conclusion

The experiment supports a strong negative effect of comorbidity_ratio on the model score.

The years_registered observations suggest possible threshold-like or nonlinear behaviour near the low end, but they are not sufficient to establish an exact threshold.

This experiment does not establish causality beyond the tested input comparisons or establish interactions with other inputs.
