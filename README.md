# F1 Podium Predictor

A machine learning project that predicts which three drivers will finish on the podium in a Formula 1 race.

## Overview
Formula 1 results are shaped by car pace, driver form, track characteristics and a fair amount of randomness. This project builds a dataset of past races and trains a gradient boosting model to estimate each driver's probability of finishing in the top 3. The three drivers with the highest probabilities are picked as the predicted podium.

## Data sources
- **Jolpica-F1 API:** race results, grid positions, finishing status and qualifying positions (2018-2025).
- **FastF1:** race-day weather (air and track temperature, humidity, wind, rain). Available from 2021 onward; earlier seasons are left empty.

## Features
- Grid and qualifying position
- Driver form (rolling average of last 5 finishes and points)
- Driver DNF rate and historical win rate
- Driver's average finish at the circuit
- Constructor form (rolling team points)
- Race-day weather

All rolling features use only races that happened *before* the one being predicted, so the model never sees future information.

## Approach
- **Target:** whether a driver finished in the top 3 (binary).
- **Model:** XGBoost classifier.
- **Validation:** time-based split by season (train: 2018-2023, validation: 2024, test: 2025+).
- **Baseline:** the three drivers starting on the front row of the grid. The model is only useful if it beats this.
- **Metrics:** average podium drivers correct out of 3, exact podium set, AUC and log loss.

## Project status
- [x] Data collection from Jolpica-F1 and FastF1
- [x] Dataset merging and feature engineering
- [ ] Model training and evaluation
- [ ] Prediction script for the next race

## Setup
```bash
pip install -r requirements.txt
```
Run the notebook in `notebooks/` to fetch the data and build the dataset. FastF1 limits requests to 500 per hour, so weather collection may need to be resumed after a short wait.

## Limitations
F1 has a lot of randomness (safety cars, crashes, strategy), so perfect predictions are not expected. Predictions can only be made after qualifying, since grid position is the strongest feature.
