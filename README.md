# Cheat Detection in Video Games Using Behavioral Biometrics and Machine Learning

Bachelor thesis, Faculty of Technical Sciences, University of Novi Sad (2026).

**Author:** Ivana Bajić
**Mentor:** Prof. Aleksandar Kovačević, PhD

**Video walkthrough (~6 min):** (https://youtu.be/uPhUOlf3xJI)

The slides are in Serbian; English subtitles are provided as a translation
(enable them with the CC button). The video is a visual overview and does not
cover everything in the work. It shows the game environment, how the human
player and the three bots play, and the main results. Further details are in
the sections below.

## What this is about

Traditional anti-cheat systems look for known software signatures, so they
always lag behind new cheating methods. This thesis investigates whether
cheating can be detected from player behavior alone (mouse movement and
aiming precision), without any access to the game's code or the player's
machine.

## Approach

- **Unity simulation environment:** a first-person shooter where the player
  shoots randomly spawned targets using only the mouse. The aim trajectory is
  logged at 60 samples per second, along with every shot.
- **Cheating simulated with three bots** of increasing "humanization":
  - `Basic`: instant and perfectly consistent aim
  - `Medium`: adds subtler movement variation
  - `Advanced`: calibrated against the fastest and smoothest human players
- **Dataset:** 7 players, 48 sessions, 3,915 human hits and 2,267 bot hits.
  Labels are known by construction, so no manual annotation was needed.
- **Three models, two input representations:**
  - Random Forest on hand-engineered features (reaction time, jerk, jitter,
    overshoot, path efficiency, ...)
  - LSTM and GRU on raw trajectory sequences
- **Evaluation:** leave-one-person-out cross-validation, which measures how
  well the models generalize to players they have never seen.

## Results

| Model         | Accuracy | Precision | Recall | F1    |
|---------------|----------|-----------|--------|-------|
| Random Forest | 0.993    | 0.990     | 0.993  | 0.991 |
| LSTM          | 0.976    | 0.933     | 0.990  | 0.958 |
| GRU           | 0.981    | 0.944     | 0.991  | 0.965 |

All three models reliably detect the bots, including the `Advanced` profile.
Random Forest is the most accurate overall, while LSTM and GRU do slightly
better than Random Forest on the `Advanced` bot.

## Key finding

The boundary between the most skilled players and a well-calibrated bot is
not sharp. For two of the seven players, precision is much lower across
all three models: their legitimate play is misclassified as cheating, while
recall stays high. In a real system this would mean punishing the best honest
players, so any sanction should depend on repeated detections over time
rather than a single one.

## Limitations

- Small number of participants (7)
- Cheating is simulated with bots, not real cheat software
- Simple environment (static targets, no movement or team play)
- This is a proof of concept, not a production-ready system

## Tech

Unity (C#), Python

## Note

The full thesis is written in Serbian and is not published. It is available
on request.
