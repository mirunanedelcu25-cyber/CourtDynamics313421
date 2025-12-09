# CourtDynamics313421

*Team The powerpuff girls*

* Nedelcu Ioana-Miruna 313421  
* Kovaleva Darya 322681  
* Akbota Kablatin 321281

# SECTION 1

## INTRODUCTION

In this project, we investigate how NBA players develop over time and how their roles evolve within different team contexts. Although the basketball dataset contains multiple tables with player and team statistics, our analysis focuses specifically on the player_regular_season table, as it provides the most detailed and consistent season performance indicators. This table includes key metrics such as scoring efficiency, assist ratios, defensive contributions and playing time, allowing us to examine how individual performance evolves across multiple seasons.  
Rather than building a predictive model or classifying players, our goal is to design a comprehensive analytical framework that captures performance patterns, identifies anomalies and traces progression trajectories throughout a player’s season. By analyzing how these indicators change over time and how they relate to player responsibilities on the court, we aim to generate interpretable insights that are meaningful for coaches, analysts and scouts. Ultimately, our findings are intended to support data-informed decisions in player development, role assignment and long-term team-building strategies.

# SECTION 2

## METHODS

### Choosing the table

We first explored all 12 tables available in the basketball database by inspecting their structure with PRAGMA table\_info. Among them, the player\_regular\_season table provided the most complete and relevant information for our analysis, including minutes played, points, rebounds, assists and other key performance statistics. Other tables either aggregated data at the career level or contained incomplete or redundant information, so they were not suitable for our goals.

### Data cleaning

To ensure data quality, we applied several cleaning steps:

* Visualized missing values using missigno.bar() and missigno.heatmap()  
* Filled all missing values in numerical columns with 0  
* Removed rows containing logically impossible values, such as:  
  * fgm\> fga (field goals made\> field goals attempted)  
  * ftm\> fta  (free throws made\> free throws attempted)  
  * tpm\> tpa (three-point made \> three-point attempted)  
  * reb \< oreb \+ dreb (total rebounds\< offensive \+ defensive rebounds)

### Data validation

We performed additional checks to confirm the consistency of the dataset:

* No years earlier than 1946 or later than 2025  
* No duplicated rows  
* No negative values in any numerical feature

These steps ensured that the dataset was coherent and ready for further analysis.

## Environment

Regarding the environment, we used a version of Python3 and we worked on colab.  
The libraries that we used are: sqlite3, pandas, missingno, seaborn, matplotlib.pyplot. 

### KNN part

 In the second part of the analysis, we applied a K-Nearest Neighbors (KNN) regression model to predict how efficiently a player will perform in the following season. Unlike Random Forest or Linear Regression, KNN makes predictions by comparing each player to the most similar players from previous seasons. Because KNN relies heavily on distance calculations, we first standardized all numerical features using StandardScaler to ensure that variables measured on different scales (minutes, rebounds, points, etc.) contributed fairly. To find the most suitable configuration, we performed a Grid Search using different combinations of k (3, 5, 10, 20\) and weighting strategies (“uniform” vs. “distance”). The search was performed with a K-Fold cross-validation on the training set only. This allowed us to evaluate multiple versions of KNN and identify the one that consistently produced the smallest validation error. After testing all candidates, the model with the lowest MAE on the internal CV was selected as the final KNN model. We then evaluated this model on the validation set (1995–1999) to check how well it generalizes to unseen data. We also visualized the predictions by plotting actual vs. predicted values, inspecting residual patterns, and examining the distribution of errors. These visual checks help us understand whether the model systematically over- or underestimates certain players and whether the prediction errors follow a stable pattern.

# SECTION 3

##   Experimental Design

### Purpose

The goal of the cleaning process was to obtain a reliable dataset that accurately reflects player performance and can support meaningful exploratory data analysis and modeling.

### Baseline

We explained the distribution of points per game (PPG) after cleaning to understand the overall shape of the data and identify potential outliers. This served as a baseline for evaluating the quality of the cleaned dataset.

### Evaluation Metrics

We used several visual tools to assess the dataset:

* Histogram to inspect the distribution of PPG  
* Boxplots to detect outliers  
* A correlation heatmap to understand relationships between features

These visual metrics helped us evaluate whether the cleaned data behaved as expected.

### Comparison on the validation test 

To understand how well each model performs, we compared all three on the same validation set. For each model, we computed the Mean Absolute Error (MAE), which measures how far the predictions deviate from the true next-season efficiency. We summarized the results in a comparison table and visualized them using a bar plot. This makes it easy to see which model performs best in practice. The model with the lowest MAE is selected as the “winner” on the validation set. Identifying the strongest model at this stage is essential, as it determines which model we trust most before moving on to the final evaluation. Additionally, we provided detailed visual diagnostics for the KNN model to illustrate how well it predicts individual values and whether its errors follow a consistent trend.

# SECTION 4

##   Results

### 

### Final test evaluation

After determining the best-performing model on the validation set, we used that model to generate predictions on the final test set (2000–2005), which contains completely unseen seasons. The idea here is to simulate a real-world scenario: if we had built our model before 2000, how well would it predict player development in the upcoming seasons? 
We evaluated the selected model on this test set using MAE once again. This final number represents the true out-of-sample performance and gives the most realistic picture of the model’s predictive ability. Ultimately, this step tells us whether the model that performed best on validation data can still maintain its accuracy when applied to completely new players and seasons.

# SECTION 5

## Conclusions

In this project, we set out to understand whether a player’s seasonal statistics can help predict their performance in the following year. By cleaning and aggregating NBA player statistics, engineering meaningful features such as EFF, and applying three different regression models—Random Forest, Linear Regression, and KNN—we built a full pipeline that moves from raw data to actionable predictions. Our analysis showed that different models capture different aspects of player development, but overall, the validation results clearly highlighted which model generalizes best to new seasons. Through systematic comparisons, visual diagnostics, and a final test on completely unseen data, we were able to evaluate not only the accuracy of our models but also the consistency and reliability of their predictions. Overall, the project demonstrates how statistical learning techniques can be effectively applied to sports analytics. By combining domain intuition with data-driven modeling, we were able to extract meaningful insights into player performance trends and assess how well early-career indicators translate into future outcomes. This framework can easily be extended and refined in future work, offering a solid foundation for more advanced predictive systems in basketball analytics.

### Future work

While our analysis offers a clear picture of how player performance evolves across regular NBA seasons, there are still several questions we weren’t able to fully address. Since we focused only on the player\_regular\_season table, we didn’t explore how players behave in different competitive environments, such as playoffs or All-Star games. Tables like player\_playoffs, player\_playoffs\_career or player\_allstar could reveal whether certain players step up under pressure or take on different roles when the stakes are higher. We also didn’t incorporate team-level information from tables such as teams or team\_season, which might help explain coaching strategies, roster changes, or team dynamics influence individual development.

For future work, it would be interesting to combine these additional tables, compare performance across contexts and track how roles shift over time. This could lead to a more complete picture of player evolution and might even support more advanced analyses, such as detecting breakout seasons, identifying role transitions, or understanding how team structure shapes player growth.
