# Case Study: User Rewards and In-Game Purchases Prediction

## Overview
This notebook analyzes user behavior data from a mobile game to predict future user rewards and in-game purchases. We examine user progression patterns, engagement metrics, and spending behavior to answer specific business questions about future user performance.

## Notebook Structure
This notebook is divided into 4 parts. Each section is linked to the question being asked and has more detailed comments on what has been done in them. These parts are:
* Read Data - just to read and create the dataframes
* Rewards Probability
* In App Purchases
* Suggested Adjustments to Reward Structure

## Assumptions:
- **User Behavior Consistency**: We assumed that user progression patterns and purchase behaviors would remain consistent for users starting July 1st onwards and not influenced by seasonal factors or special events.
- **Game Remains the Unchanged**: During the entire duration of data provided, we assume that there were no significant changes in game difficulty, economy, or marketing.
- **Recent trends will continue**: We assume that the recent completion rates would be more presentative of future behavior than the overall 3 month average.
- **Correlation implies causation in purchases**: We assume that strong correlation between level progression and purchase behavior indicates a causal relationship, i.e. encouraging users to reach higher levels would lead to increased purchases, rather than simply reflecting that high-spending users tend to progress further.


---

## Question 1: Predicted Average User Rewards (July 1st onwards)
### Grain: 
user_id - because the data shows users have reinstalled and reached the same level multiple times. In order to keep the process clean, the assumption here is the user can only achieve each milestone once, and keeping it at the user grain and using the earliest action_datetime would give us a cleaner timeline.

### Results:
For users starting from July 1st, their expected rewards in:
Month 1 average: 41.31
Month 2 average: 37.14
Month 3 average: 34.14

The rewards calculated uses expectation for better explainability. Rewards are defined in separate ways for users who complete the challenge in time, and users who are too early to know if they will or will not get there based on level progression and time left.

### Methodology:
Calculate the expected reward payout for each user based on their progression patterns. For each reward level, we compute:
The probability of a user reaching that level within the time limit
The expected reward value (probability x reward amount)
This approach uses expectation for better explainability.
 

---

## Question 2: Predicted In-Game Purchase (within 50 days)
### Grain: 
offer_start_id (given)

### Results:
Given the limited data sample we have and lack of other non-game attributes, this model doesn’t perform too well and can be improved with other metadata.
Expected in-game purchase per offer: $0.98

### Methodology:
* Used two predictive models to predict expected value of in-game purchaes. The approach is:
    * Probability model to predict the likelihood of a user making a purchase
    * For users that make a purchase, prediction model to understand the amount value being spent based on game-level progression.
    * Combine the two predictions using a median value across the dataset to simulate July to get expected in-game purchase for that month.

* To maximize performance of the model, we use SMOTE to balance classes, and GridSearch across hyperparameters, using fbeta tuned for precision on the purchased rows as the scoring mechanism.


---

## Question 3: Recommended Task Structure Adjustments

### Current Structure Limitations
- Large gaps between reward tiers (levels 35, 100, 175) lead to user drop-off
- Final reward at level 175 requires significant time investment (50 days average)
- Linear reward structure doesn't account for different player types

### Recommended Adjustments
1. **Add Intermediate Reward Tiers**: Insert rewards at levels 60 and 140 to maintain engagement
2. **Implement Early Engagement Bonuses**: Provide small rewards for consistent daily play in first 14 days
3. **Weekend Multipliers**: Offer enhanced rewards for weekend play to capitalize on higher engagement
4. **Personalized Reward Paths**: Create parallel reward structures for different player types (casual vs. hardcore)


