# Wearable Stress Classification

Testing whether the sensor-fusion approach I used in my TENS patch (sEMG + GSR for pain detection) generalizes to a broader physiological signal classification problem, using a public dataset instead of private device data.

## Data

WESAD (Wearable Stress and Affect Detection) — 15 subjects, chest-worn sensors recording EDA, ECG, temperature, and respiration during baseline, stress, and amusement conditions.

## Approach

Split each subject's recording into 60-second windows, extracted statistical features (mean, std, min, max, range) per signal per window, and trained a Random Forest classifier to predict the state label for each window.

## What I found

A standard random train-test split gave 91% accuracy, but that's misleading — windows from the same subject can land in both train and test sets, so the model might just be memorizing individual baselines. Using leave-one-subject-out cross-validation (train on 14 subjects, test on the one left out, repeat for each) gave a more honest number: 71.2% accuracy (+/- 13.2%).

That standard deviation matters — the model works well for some people and poorly for others, which tracks with known individual variability in how stress shows up physiologically. Worth flagging directly rather than hiding behind the higher number from the naive split.

ECG variability and EDA variability were consistently the strongest predictors, which lines up with the sensor choices in my TENS patch design.

## TODO

- Try subject-level normalization (z-score each subject's signals individually) to see if it reduces the cross-subject variability
  
- Test whether adding wrist-worn signals (currently only using chest sensors) improves generalization

- Apply the same LOSO validation approach back to the TENS patch's own classifier
