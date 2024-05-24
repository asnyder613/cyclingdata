# From Physiology to Psychology
## An exploration of personal cycling data
Personal project to practice data science skills on my own cycling workout data from 2018-2023

Initial questions include:
- What is the general relationship between workout duration, average power, average heart rate, TSS, and rpe?
- Does rpe increase linearly with average power and average heart rate?
- Is it possible to predict rpe based on average power, average heart rate, and workout duration?

## Background
As a data scientist and elite athlete, I have always found the data collected during endurance training fascinating. On one hand, there is the concrete data: the physiological markers of power, heart rate, speed, distance, time, etc. But on the other hand, many endurance athletes also document subjective data such as the rate of perceived exertion (RPE). In short, my focus question is: What is the relationship between physiological metrics such as power and heart rate with the subjective metric of RPE?

## The Data
Since I first began training, in 2018, I have been using TrainingPeaks to manage my workout data and share with my coaches. The TrainingPeaks platform allows users to download their workout history in .csv format. This project was carried out using my personal cycling training data from 2018 through 2023, a total of 4,154 observations. 

## Terms
Training Stress Score (TSS) is a composite number that takes into account the duration and intensity of a workout to arrive at a single estimate of the overall training load and physiological stress created by that training session. (TrainingPeaks.com)

Rate of Perceived Effort (RPE) is a subjective rating of how hard one is exercising at a given moment, on a scale of 1-10. (TrainingPeaks.com)

## Initial Observations
Based on exploratory data analysis, there does seem to be somewhat linear relationship between physiological metrics (TSS, power average, heart rate average) and the rider’s reported RPE. As physiological effort increases, RPE also tends to increase.

## Digging In
After cleaning the data to drop any observations that did not include data for RPE, the size of the dataset dropped considerably, exacerbating the pre-existing “small data problem” caused by using only my own training data. This means two things: (1) all conclusions are based on the data of only one athlete and may not be representative of the greater endurance athlete population, and (2) even the data from this one athlete is limited, and any correlations may not hold true when tested on a larger data set with more observations. 

My first question was to determine the correlation between the physiological metrics of total ride time, average heart rate, and average power with the subjective metric of RPE. In doing this, I chose to use a simple regression model, with the understanding that average heart rate and average power are generally highly correlated themselves (as average power increases, so does average heart rate). In the model summary, the R-squared of .55 indicates that 55% of the variability in RPE can be explained by workout length, average heart rate, and average power. The F-statistic is 227.9 with a low p-value, showing that the model as a whole is statistically significant. When controlling for time and heart rate, average power is statistically significant and positively correlated with RPE. Interestingly, average heart rate (despite being itself highly correlated with average power), and total time, however are not statistically significant when controlling for the other variables. 

In endurance sport, we often use “tempo” as a differentiating point between relatively easy aerobic / endurance work and harder efforts that start to move towards lactate threshold and even anaerobic work. My next question, in an attempt to refine my understanding of the correlation between physiological metrics and RPE, was whether workouts with average power or heart rate above tempo tended to have a higher RPE. To do this, I created binary variables for workouts with average power over 220 and average heart rate over 150 (the bottom end of my current tempo (zone 3) ranges, which, of course, makes the assumption that these values remain static over time). The resulting model, using these new variables, to predict RPE, had an R-squared of only .25, and neither variable was statistically significant. A quick value count also highlighted a data imbalance, due to very few observations having an average power or average heart rate above the zone 3 cut-off. I made one more attempt, with the consideration that a warm-up and cool-down could impact average power, and so re-created the binary variables, this time using the bottom end of my endurance (zone 2) range as the cut-off point for average heart rate and average power, which created a more balanced dataset for modeling purposes. Again, neither variable was statistically significant in the resulting model, though the model as a whole had a R-squared of .4, which was an improvement over the prior iteration.

My next question was whether following a rest day had any significant effect on RPE when controlling for TSS. To do this, I created a binary variable for each observation to label whether it followed a rest day, then used a simple regression model to predict RPE based on that new binary variable and TSS. The model resulted in a F-statistic of 249.4 with a low p-value, indicating that the model as a whole is statistically significant. With a R-squared of .3, only 30% of the variability in RPE can be explained by the variables in the model. More specifically, the coefficient of 'follows_rest_day' has a p-value close to .5, which is not statistically significant, indicating that when controlling for TSS, whether a workout follows a rest day or not is not a significant indicator of RPE. On the other hand, TSS, when controlling for 'follows_rest_day', does seem to be positively correlated with RPE, which makes sense, considering earlier observations regarding the effect of average power and average heart rate (factors that, like TSS, indicate physiological intensity) on RPE.

## Conclusions
The relationship between the physiological metrics of average heart rate and average power with the subjective metric of RPE is not as cut and dried as one might think. While average power and increased TSS do seem to be positively correlated with RPE, workouts with higher average power and heart rate do not consistently result in higher RPE, nor do workouts following a rest day have any statistically significant effects on RPE. 

[Tableau Dashboard](https://public.tableau.com/views/PhysiologytoPsychology/Dashboard2?:language=en-US&publish=yes&:sid=&:display_count=n&:origin=viz_share_link)

Due to the limitations of working with a single athlete’s data, there is plenty of opportunity for more research and exploration in this space. Some questions for further exploration and consideration in this space include:
With more athletes and more data, do these conclusions remain consistent?
Do mental performance practices affect RPE when controlling for TSS?
With data for 10-minute power and heart rate, would there be a clearer correlation with workouts containing 10-minute efforts over tempo and a higher RPE?
