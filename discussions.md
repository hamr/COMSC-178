# Discussions

## Discussion 4: Time-series data

### Prompt
This week we are looking at how forecasting can be used with time-series data.  One of the common problems with observations over time is missing data and how to handle it before running a forecast.  We looked at a few different methods:

* Forward fill
* Backward fill
* Moving average
* Interpolation

Imagine you are modeling someone's daily screen time and are making a forecast.  The time-series data file contains a screen time measurement in rows like this:

| Date | Screen Time (Minutes) |
| --- | --- |
| 2026-04-03	| 248 |
| 2026-04-04 | 143 |
| 2026-04-05 | |
| 2026-04-06 | 221 |

What would be an effective way to handle the missing entries?  You might use one of the four options above, a combination of them, or something else.  Provide a justification for your answer.

### Response

The row for 2026-04-05 is missing data in the “Screen Time” column.

The methods listed in the prompt would produce the following values:
* forward fill uses the previous row’s value:  143
* backward fill uses the following row’s value:  221
* moving average takes the average of values from a time window.  Since there aren’t many values in this set, we’d take the average off all the values, excluding the one that’s missing:   204
* linear interpolation would use the distance formula $\frac{143 + 221}{\texttt{2026-04-06} - \texttt{2026-04-04}} = \frac{364}{2} = 182$

Of these methods, I think linear interpolation is safest because captures the trend between the previous and following points.

However, since there isn’t much information about trends in the dataset, I expect that it would be safer to drop the row from the data.  It might not be safe to assume that there are trends between previous and following points.  It’s possible that screen time is always lower or higher on Sundays, for example.


## Discussion 3: Evaluating the model's predictions

### Prompt

In this week's notebooks we are using an xgboost model to predict whether a patient is 'Normal' or 'Abnormal' in the vertebral column dataset.  Each observation is classified as either "Normal" or "Abnormal" (represented by a target value of 0 or 1).

There are several ways to measure "how well" the model predicts whether a patient belongs in class 0 or class 1.  In Lab 3.6 you will calculate the following metrics based on the model's predictions:

* Sensitivity
* Specificity
* Precision
* False positive and false negative rates
* False discovery rate
* Overall accuracy

Choose any one of these metrics and explain why it is important in this scenario.  Answer these questions in your post:

* What does the metric measure, and how is it calculated?
* What is a real-world situation where relying on this metric would be useful?

### Response

metric: specificity

Questions:
1. Why is specificity important in the scenario of predicting whether a patient is 'Normal' or 'Abnormal' in the vertebral column dataset?

2. What does the metric measure, and how is it calculated?
3. What is another real-world situation where relying on this metric would be useful?

Answers:

1. Specificity is important in the scenario of predicting whether a patient is 'Normal' or 'Abnormal' in the vertebral column dataset because it brings the problem of avoiding false positive classifications into focus.

    While avoiding false *negatives* is highly relevant to initial medical screenings, avoiding false *positives* is more relevant to followup diagnoses, where it is important not to tell healthy patients they have a serious problem when they don’t.

2. Specificity is the ratio of true negative identifications (TN) to the total of actual negatives (TN + FP)

    $$\textrm{specificity} = \frac{\textrm{TN}}{\textrm{TN} + \textrm{FP}}$$

    It answers the question, “Of all actual negative instances, how many does the model correctly reject as negative?”

    Note that specificity gets higher when the number of false positives gets lower.

3. Another real-world situation where relying on specificity is useful is in forensic identification in the context of criminal justice.  A false positive could mean wrongly identifying an innocent person as a suspect if a DNA or facial recognition match is wrong, for example.


### Notes

**Sensitivity** is the ratio of true positive identifications (TP) to the total of actual positives (TP+FN).  It is the percentage of positives correctly identified.  Of all actual positive instances, how many does the model correctly find?

$$\textrm{sensitivity} = \frac{\textrm{TP}}{\textrm{TP} + \textrm{FN}}$$

What else is it called?
* true positive rate
* recall

When is it important?  Sensitivity is important in contexts where missing a positive case is more costly than raising a false alarm.  

Example scenarios:
* fraud detection
* threat detection
* Cancer/serious-disease screening: you want to catch as many true cases as possible, even if it causes follow-up testing for some false alarms.

Common trade-off:
* Lower threshold → recall increases (fewer misses).
* Higher threshold → recall decreases.



**Specificity** is the ratio of true negative identifications (TN) to the total of actual negatives.  It is the percentage of negatives correctly identified.  Of all actual negative instances, how many does the model correctly reject as negative?

$$\textrm{specificity} = \frac{\textrm{TN}}{\textrm{TN} + \textrm{FP}}$$

What else is it called?
* true negative rate
* selectivity

When is it important?  Specificity is important when false positives -- incorrectly labeling a negative case as positive -- lead to serious consequences.

Example scenarios:
* Confirmatory medical testing after a screening test:  once you’re down to a smaller set of people, you want high specificity to avoid telling healthy patients they have a serious illness when they don’t
* criminal justice and forensic identification:  wrongly identifying an innocent person as a suspect (e.g., through DNA matching or facial recognition) can lead to wrongful convictions.
* Drug testing in employment or sports: a false positive can ruin someone's career or reputation, so confirmatory tests are designed with very high specificity.

Common trade-off:
* Lower threshold → specificity decreases (more false positives).
* Higher threshold → specificity increases.

**Precision** is the ratio of correctly classified positive results to all classified positive results.  It ignores negative predictions.  It answers the question, “How many predicted positives are actually correct?”

$$\textrm{precision} = \frac{\textrm{TP}}{\textrm{TP} + \textrm{FP}}$$

What else is it called?
* positive predictive value

When is it important?  Precision is important in contexts where false positives are costly.  
Examples scenarios:
* Spam/phishing detection: if you label a message as “phishing,” you want that label to be right most of the time.  You want to avoid marking legitimate emails as spam.

Common trade-off:
* Lower threshold (more predicted positives) → precision often decreases (more false positives).
* Higher threshold → precision often increases.

**Accuracy**  is the ratio of correct predictions to the total number of predictions.  Of all instances, how many are predicted correctly (either positive or negative)?

$$\textrm{accuracy} = \frac{\textrm{TP}+\textrm{TN}}{\textrm{TP}+\textrm{TN}+\textrm{FP}+\textrm{FN}}$$

What else is it called?
* model’s score

When is it important?  Accuracy is typically more informative than precision/recall when class imbalance is mild and false positives vs false negatives are about equally harmful.   If those conditions don’t hold, precision/recall (and specificity, or F1/ROC/PR-AUC) usually tell the better story.  Like if the observations are significantly weighted toward negatives as in a medical diagnosis.

Examples include:
* Quality control / defect detection with similar costs:  If the cost of a “miss” (false negative) and a “false alarm” (false positive) are comparable—e.g., both lead to similar rework or inspection—then overall accuracy is a good summary.
* Multiclass tasks with balanced categories e.g., categorizing documents into a handful of classes where each class is well represented and misclassifying any class is similarly bad. Accuracy gives a straightforward “how often it’s right” measure.
* Routine classification with roughly balanced classes: e.g., labeling emails into “billing vs non-billing” when both types are common and the two error types are similarly undesirable.

Common trade-off:
* Not monotonic / no guaranteed trade-off direction. Accuracy can go up or down depending on class balance and which errors dominate.

**False Positive Rate (FPR)** is the ratio of false positive identifications to the total of actual negatives.  Among actual negatives, how often does the model incorrectly predict positive?

$$\textrm{FPR} = \frac{\textrm{FP}}{\textrm{FP} + \textrm{TN}} = 1 - \textrm{specificity}$$

FPR tracks when false alarms matter among the negative class.

Common trade-off:
* Lower threshold → FPR increases.
* Higher threshold → FPR decreases.

Example scenarios:
* Security/threat alerting: how often normal/background events trigger alerts (alert fatigue / wasted investigation effort).


**False Negative Rate (FNR)** is the ratio of false negative identifications to the total of actual positives.  Among actual positives, how often does the model incorrectly predict negative (misses positives)?

$$\textrm{FNR} = \frac{\textrm{FN}}{\textrm{FN} + \textrm{TP}} = 1 - \textrm{recall}$$

FNR tracks when false alarms matter among the positive class.

FNR answers: Among truly positive cases, how often do we mistakenly miss them?

Example scenarios:
* Fraud detection: how often truly fraudulent transactions slip through unflagged (missed losses).

FPR and FNR are not sufficient when you care about “how many predicted positives are actually correct?”  That question is precision, not FPR/FNR.  Precision depends strongly on prevalence (base rate).
You can have:
* low FPR
* but still poor precision if positives are rare

Common trade-off:

Lower threshold → FNR decreases.
Higher threshold → FNR increases.

**False Discovery Rate (FDR)** is the expected fraction of false positives among all predicted positives—i.e., among the “discoveries” your model called positive.  Among all instances your model predicts as positive, what fraction are actually false positives?

$$\text{FDR} = \frac{FP}{TP+FP} = 1 - \text{Precision}$$

Common trade-offs:
* Lower threshold → model predicts more positives
    * TP may increase (you catch more real positives)
    * FP typically increases too
    * FDR usually increases (a larger share of your positive calls are wrong)
* Higher threshold → model predicts fewer positives
    * FP tends to decrease
    * FDR usually decreases (your positive calls become “cleaner”)
    * but you usually miss more real positives (recall goes down)

Example scenarios:
* Systems that take action on flagged items: e.g., when a tool automatically quarantines or escalates items—FDR tells you the “expected wrong actions rate” among everything you acted on.

**lowering and raising the threshold**  Typical direction when you lower the decision threshold (predict more positives):

* Precision: usually down
* Sensitivity / Recall (TPR): up
* Specificity (TNR): down
* FPR: up
* FNR: down
* Accuracy: uncertain (can go either way)

And when you raise the threshold (predict fewer positives):
* Precision: usually up
* Recall (TPR): down
* Specificity (TNR): up
* FPR: down
* FNR: up
* Accuracy: uncertain

## Discussion 2: data features

### Prompt

This week we learned about the characteristics of ML problems and features in the data.
Data formatted for ML is usually structured as a series of observations,
with each row representing one observation (a customer, a purchase, etc.).

Suppose you are working on a model for a music streaming service (e.g. Apple Music or Spotify).
The model should be able to scan a list of songs and
recommend whether each song should be added to a personalized playlist.

As input into the model, you have a series of observations about the customer's behavior
(such as what type of music they listen to the most, how long they listen, etc.).

What are three features that would be useful in the data?
Give a short description of each and how it might inform a recommendation.


### Response

I expect any song recommendation application needs to be able to accomplish the basic task of assessing how “close” each song is to other songs that the customer enjoys.

To help identify tracks that the customer enjoys, I would include a count of how many times the customer has listened to a given song in the data.

To compare songs, official track metadata like musical genre or subgenre would be indispensable.  I expect that genres would be divided into “dummy” features like `genre_jazz` or `genre_pop`.

Features that describe songs’ acoustic properties would be important.   For example, a customer might have preferences for specific instruments, so boolean features for instruments like `has_guitar` or `has_drum_machine` could be useful.


### Notes

model input:  a series of observations about the customer's behavior
 - what type of music they listen to the most
 - how long they listen, etc.

model output:  scan a list of songs and recommend whether each song should be added to a personalized playlist

What are three features that would be useful in the data?
Give a short description of each and how it might inform a recommendation.

type of song
 - lots of ways to classify songs: instruments, singing style, tempo

or people with overlapping interests, what songs do they listen to frequently that customer hasn't listened to
 - score correlation with 

sometimes, you don't want more of the same.
you want some that contrasts: fast vs slow, silly vs serious


## Discussion 1: introductions

### Prompt

Write a short post introducing yourself.
Let us know your preferred name and anything else you'd like us to know about you.

To reflect on the first module, discuss one or more of these questions in your post:
* What is one question you hope to answer in this course?
* How do you see machine learning impacting a field or industry that matters to you?
* Describe any prior experience you have with machine learning, cloud computing, or AWS.

### Response

Hello, my name is Robert (Rob is also fine).  I work in Linux system engineering in my day job, doing a mix of system administration, troubleshooting, and automation.

I have no prior experience with machine learning and my experience with AWS is limited to taking COMSC-175 this semester.  I’d like to increase my foundational knowledge of cloud computing in general and machine learning in particular for career growth and to deepen my toolkit for solving on-job problems.

The question I hope to answer in this class is, how can one go about creating custom, purpose-built ML solutions to a narrowly defined problems?  Artificial Intelligence is everywhere in our world now and I expect that understanding how to craft solutions to small problems is a good way to begin understanding how it works more broadly.

