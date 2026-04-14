# Discussions

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

