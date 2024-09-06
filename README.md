# Introduction

**robozao** is a xgboost model that calculates the probability of a shot resulting in a goal, given the current situation of the game (tracking and some event information).

# Dataset

The _data/enhanced_frames.pkl_ file contains the PFF frames and events of all shots of the FIFA 2022 World Cup. The model was trained and tested using only this dataset. For now, the dataset is private, but when PFF makes it available I will publish it here too.

# Features

To feed the model, some features were extracted from the raw data. Are they:

- _shot distance_ : The euclidean distance from the ball to the center of the goal.
- _shot angle_ : The relative angle from the bal to the goal (see [this paper](https://pubmed.ncbi.nlm.nih.gov/27119201/) to math details).
- _goalkeeper distance_ : The euclidean distance from the ball to the goalkeeper.
- _goalkeeper angle_ : The relative angle from the ball to the goakeeper.
- _num_blocking_players_ : Number of enemy players inside the triangle from the ball to the both goal posts.
- _blocking_angle_ : The sum of all relative angles from the ball to the enemy players inside the triangle from the ball to the both goal posts.
- _pressure_type_ : A categorical variable to show whether the shooting players is under pressure.
- _shot_height_ : yes...the shot height.
- _header_ : A bool variable to explicit if the shot was a header.

Checkout the notebook itself to see more details about feature extraction.

# Results

Due to the small amount of data and many features, the first models really suffered from overfitting. To solve this problem, L1 and L2 regularization was used, in addition to limiting the depth of the tree. GridSearchCV was used to hyperparameter tuning.
Using a train-test set with 10% of the data to testing, the model achieve a ROC-AUC score of **0.8483** in the test set.

# Conclusion
The model performed well, managing to process the features in order to generate xG's consistent with the shot situation. 
Working in this goal expectation generation model definitely doesn't make me a better football player, but now I have more excuses to justify all the goals I miss in matches with my friends.
