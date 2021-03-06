---
layout: post
title: "Predicting college basketball winners with machine learning"
subtitle: 
author: "Ryan Berger"
date: "2019-02-11"
output: 
  html_document:
    theme: spacelab	
    highlight: tango	
    keep_md: true
preserve_yaml: true
---

After doing some machine learning projects using public datasets in the past I decided I needed a new challenge on a project I would build from scratch: making a predictive model for college basketball games. The end goal that I wanted to accomplish with this project was to use machine learning algorithms and advanced analytics metrics to determine:

1. Which team will win every basketball game between NCAA division I teams
2. Projected final scores for each team
3. A reporting system where the daily predictions would automatically be displayed in an interactive Tableau visualization

## Machine learning model

After trying out a few different algorithms (linear regression, k-nearest neighbors, neural network), the best performance was achieved by treating each game as a binary classification using logistic regression (will the home team win the game: Yes or No?). The model was trained using game results from every game from the last 5 years (26,000+ games) using adjusted offensive and defensive efficiency ratings from [Bart Torvik](http://www.barttorvik.com/#) from the day the game was played as predictors and correctly picked the winning team 72% of the time in an independent test set of over 5,000 games. 

For the current season the model is performing as expected; as of February 11 the winning team is being predicted with an accuracy rate of 72.3% rate in over 3,700 games. 

## Tableau visualization

Generating a system to display the predictions for today's games required:
- Writing a script for scraping the team ratings data for the current day from barttorvik.com using `rvest`
- Making predictions in R using the logistic regression model
- Saving the results as a `.csv` output file and pushing to Google Sheets from R
- Linking the Google Sheet to a Tableau Public visualization 
- Setting the Tableau viz to automatically refresh data every day

It sounds complicated but once assembled it requires one push of a button every day to update everything and push to Tableau. Here is the output with predictions for todays games:

<iframe src="https://public.tableau.com/views/college_basketball_predictions/Dashboard1?:embed=y&:display_count=yes&:showVizHome=no&:embed=true" width="90%" height="1000"></iframe>

## Notes

The challenges of this project were primarily in data wrangling and cleaning. Oh and web scraping, lots and lots of web scraping. 

Past game results and future schedules were scraped from [college baskeball reference](https://www.sports-reference.com/cbb/) using the `rvest` package in R by looping over every date of every season from 2014-2018 (26,000+ games). Similarly, to provide analytics metrics for each team from [BartTorvik.com](http://www.barttorvik.com/#) on the day the game was played required scraping and cleaning analytics data for 353 teams on every date from 2014-2018 (250,000+ team ratings were scraped!).

To see the source code for this project, check out my [Github repository](https://github.com/rberger997/college_basketball_predictions). 
