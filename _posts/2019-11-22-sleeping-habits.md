---
layout: post
title: Does school improve my quality of sleep?
description: Using Python and Sleep Cycle data to find out if academics affected my sleep quality.
date: 2019-11-21
---

According to the Center for Disease Control and Prevention (CDC), 35.5% of men in the United States suffered from Short Sleep Duration (SSD) in 2014; which is defined by "[Getting] less than 7 hours of sleep over a 24–hour period." A higher proportion of individuals suffering from chronic SSD also reported a slew of other health risk behaviors, as well as an increased risk for chronic health conditions. A few of the health risk behaviors were obesity, physical inactivity, and excessive drinking. These same individuals were also considered to be at greater risk of suffering from chronic health conditions including heart attacks, coronary heart disease, strokes, depression, and diabetes. I marginally fall into the category of individuals having SSD, but luckily for me I maintain an otherwise healthy lifestyle. Over the next few minutes and paragraphs you'll see how academics has played a role in this aspect of my life.

But before we get into that, let me outline the parameters used in combination to measure Qs (measurements are per night recorded):

1. Duration of time spent in bed
2. Duration of time spent in deep sleep
3. Frequency of motion and intensity for each movement
4. Duration of time registered as fully awake

So, how can someone measure a combination of these parameters? If you just guessed "there's an app for that!"; well, there is! Back in 2018 I discovered the Sleep Cycle app, which is a functional health app that tracks sleeping patterns and behaviors, and up to this point (July, 2020) I've been using it daily. By the time you're reading this, there will easily over 600 data points collected, and counting. With Python, knowledge of statistics, and a few visualization libraries, I was able to gain some insight into my Qs so as to provide support in favor or against the question, has Lambda School (academics) affected my Qs?

## Exploring

The first obstacle in attempting to answer any question is always understanding the data. Having a better general understanding of data allows you to refine your questions regarding exploration, and your hypotheses. In order to understand this dataset and before moving forward into any real analytics I first looked at the general characteristics of each feature, namely distribution, datatype, missing/null values, and shape. Out of the 8 available features, I determined only five would be useful in my analysis; nothing fancy here, the columns eliminated were mostly missing values, or entirely composed of 0s like the Heart rate feature (I don't own a heart rate monitoring device).

I then looked at the distribution of two important parameters, Time in Bed and Qs. As you can see, the distribution in my Qs seems to be normally distributed between 0 and 1; 0 being poorest Qs and 1 being best Qs. Some of the data contained very small Qs values and thus were omitted from analysis and treated as outliers.

![Sleep Quality Distribution](/img/pop_sleep_qs_dist.png)
![Time in Bed Distribution](/img/pop_time_in_bed_dist.png)

With respect to the rest of the world's Qs (0.76) I rank low, with an average Qs of 0.55. Naturally, I would also expect the amount of time I spend asleep to rank proportionally low (5.24 hrs/night); if you remember, Time in Bed is one of the contributing features to the overall measurement in Qs.

Another feature worth exploring as it relates to Qs and Time in Bed is weekday. Using the 'Start' and 'End' date-time columns I engineered a column containing the respective day of the week I went to bed for each element in the data. The following figures illustrate this analysis but show no clear trend, other than Friday nights typically resulted in better and more sleep.

![Qs vs Weekday](/img/pop_qs_wkdy.png)
![Time in Bed vs Weekday](/img/pop_t_to_b_wkdy.png)

## Analysis

But the question remains, is there a difference in sleep quality during academic stints? To answer this let's start by looking at the distributions in a more rigorous way. This will necessitate splitting the dataframe based on the date classes started (October 23, 2019). I will then test for independence with the SciPy library using:

```python
from scipy import stats
stats.ttest_ind(df['before'], df['during'])
```

The t-test for independence generates p-values, which provide insight on whether the averages between the two groups are statistically different, or statistically the same. A finding that is necessary to concretely say that Qs and time in bed did change during these two periods of time.

The results show that while holding everything else constant, my academics did in fact have an effect on my quality of sleep and time in bed. According to the distribution means there was an increase of 7% in Qs, and an average of 30 min more time spent in bed during my time as a student.

![Before vs During - Qs Distribution](/img/before_during_qs_dist.png)
![Before vs During - Time in Bed Distribution](/img/before_during_time_in_bed_dist.png)

The figures also contain information regarding weekday trends as they pertain to time before and during academics. Again, there is no clear trend nor difference between the two time periods, but there is one day that stands out. Over both observations, Mondays show a significant difference when compared between academic periods (before and during). This result makes me curious — what is happening on Mondays that causes the notable difference in sleep quality? I'll come back to this question in the near future.

![Before vs During - Qs by Weekday](/img/before_during_qs_wkdy.png)
![Before vs During - Time in Bed by Weekday](/img/before_during_time_in_bed_wkdy.png)

## Conclusion

With the results obtained, visualizations generated, and statistical analysis we can confidently say that sleep quality was different, and provide the evidence necessary to reject the null hypothesis. We see that the longer I find myself in bed, the higher Qs I'm able to achieve. Moving forward I will work on getting at least 7 hours of sleep per night, so as to improve my sleeping habits.

If you would like to further explore my analysis and code, [check it out here](https://colab.research.google.com/drive/1bG0QlMhdhw_3MY-sey6ncWhkpGedILzx).
