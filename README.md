# Study-Visualization-Storytelling (R)

__[R]__
 - I. Explore One Variable
 - II. Explore Two Variable 
 - III. Explore Multi-Variable
 
http://hci.stanford.edu/publications/2013/invisibleaudience/invisibleaudience.pdf

__Library:__ `ggplot2`

__Data:__ 'pseudo_facebook.tsv' 

__Story:__ People's perception of their audience on FB matches up to the reality? 
 - Well, FB believes who you think is in your audience affects how you present yourself. 
 - Who's actually seeing the content they are sharing?  
   - __Survey Q1.__ "How many do you think saw the post? 
     - There is a big mismatch b/w people's perceived audience size and their actual audience size.
     - People dramatically underestimate the size of their audience - (1/4). 

> Adjust a font size?
 - with the font size set to 20 ?
```
install.packages('ggthemes', dependencies = TRUE)
library(ggthemes)
theme_set(theme_minimal(20)) 

pf <- read.csv('C:/Users/Minkun/Desktop/classes_1/NanoDeg/1.Data_AN/L7/---New R/data/pseudo_facebook.tsv', sep = '\t')
```
<img src="https://user-images.githubusercontent.com/31917400/35759700-3fd3e9f2-0873-11e8-93d7-ff5e74cbefc6.jpg" />


### I. One Variable

>Phase_01. Understand the audience
 - [1) Histogram of Users' **Birthdays**]
   - 'date of birth' on x-axis
   - Adjust x-axis' bins, using 'scale' layers - `+ scale_x_continuous(breaks)`...JUST marking ticks - ("1 to 31")
   - 'binwidth argument' in the 'geom_histogram' layer is actually change bin size while 'breaks argument' in the 'scale' layer just change the tick marks!
```
ggplot(aes(x = dob_day), data = pf) + geom_histogram(binwidth = 1) + scale_x_continuous(breaks = 1:31)
```
 - Faceting, Splitting the data over 'dob_month' variable: `+ facet_wrap( ~ variable, ncol)`
<img src="https://user-images.githubusercontent.com/31917400/35760868-2c798ad0-087b-11e8-91e5-08aad97e0341.jpg" width="300" height="160" /> 

```
ggplot(data = pf, aes(x = dob_day)) + geom_histogram(binwidth = 1) + scale_x_continuous(breaks = 1:31) +
  facet_wrap(~dob_month, ncol = 4)
```
<img src="https://user-images.githubusercontent.com/31917400/35760701-e1ce5ce6-0879-11e8-808a-e6f293bc4f09.jpg" width="600" height="160" />

 - [2) Histogram of Users' **friend size**]
   - 'friends_count' on x-axis
   - Outliers? then 'Limiting axis': `+ scale_x_continuous(limits = c(n, n), breaks)`
   - Faceting by 'gender': `+ facet_wrap( ~ variable)` or '+ facet_grid(gender ~ .)' 
   - then ommiting NA values..it sould be dealt within the 'data' level: `data=subset(pf, !is.na(gender))`
   - IF the relationship include categorical variable (such as 'gender'), 
     - STATISTICS - which side has more friend on average?
     - To find average 'friend_count_by_gender', we use `by(target variable, categorical variable, function)`
     
```
ggplot(aes(x=friend_count), data=pf) + geom_histogram() 

ggplot(aes(x=friend_count), data=pf) + geom_histogram() + scale_x_continuous(limits = c(0,1000))

ggplot(aes(x = friend_count), data = pf) + geom_histogram(binwidth = 25) + scale_x_continuous(limits = c(0, 1000), breaks = seq(0, 1000, 50))
```
<img src="https://user-images.githubusercontent.com/31917400/35767164-d029a0c6-08de-11e8-9814-07da198c3bc2.jpg" width="1200" height="160" />

```
ggplot(aes(x = friend_count), data = pf) + geom_histogram(binwidth = 25) + scale_x_continuous(limits = c(0, 1000), breaks = seq(0, 1000, 50)) + facet_wrap(~gender)

ggplot(aes(x = friend_count), data = subset(pf, !is.na(gender))) + geom_histogram(binwidth = 25) + scale_x_continuous(limits = c(0, 1000), breaks = seq(0, 1000, 50)) + facet_wrap(~gender)
```
<img src="https://user-images.githubusercontent.com/31917400/35767303-de7caf80-08e1-11e8-9bdb-9f4b269e2486.jpg" width="700" height="200" />

```
table(pf$gender) 
by(pf$friend_count, pf$gender, summary)
```
<img src="https://user-images.githubusercontent.com/31917400/35767397-73a5e5bc-08e3-11e8-800c-4b8a94318713.jpg" width="600" height="80" />

 - [3) Histogram of Users' **tenure by year** - how many days they've used facebook so far?]
   - 'tenure/365' on x-axis
   - Determines the color of the outline, area_inside in a plot: `+ geom_histogram(binwidth, color, fill)`
   - How to set up bins? Using `summary`, find mix&max. In x-axis, the tick-marks ranged 0 to 9 (by 1), and the plot limit is 0 to 7..binwidth be 0.25 
   - How about gender difference?: `+ facet_wrap(~gender)`
```
ggplot(aes(x = tenure), data = pf) +
  geom_histogram(binwidth = 30, color = 'black', fill = '#099DD9')
  
summary(pf$tenure/365)   
  
ggplot(aes(x = tenure/365), data=subset(pf, !is.na(gender))) +
  geom_histogram(binwidth = 0.25, color = 'black', fill = '#F79420') + scale_x_continuous(breaks=seq(1,7,1), limits=c(0,7)) +
  facet_wrap(~gender) + xlab('Years using Facebook') + ylab('Number of users in sample')
```   
<img src="https://user-images.githubusercontent.com/31917400/35768532-0c5ef29c-08f5-11e8-944f-4e41967b2051.jpg" width="600" height="170" />   

 - [4) Histogram of Users' **age**]
   - 'age' on x-axis
   - In x-axis, the tick-marks ranged 0 to 113 (by 5), and..binwidth would be 1.
```
summary(pf$age) 

ggplot(aes(x = age), data = pf) +
  geom_histogram(binwidth = 1, color='black', fill = '#5760AB') + scale_x_continuous(breaks = seq(0, 113, 5)) + 
  xlab('Ages using FB') + ylab('NO.users in sample')
```
<img src="https://user-images.githubusercontent.com/31917400/35768286-4383a3e8-08f1-11e8-9164-94dbea6dfa95.jpg" width="300" height="170" /> 
 
 - [5) Data Transformation of Users' **friend size**]
   - To get a better look at the data... 
   - If we have 'Over-Dispersed' data (the term is always relative to some particular posited distribution..Poisson? Gamma?) 
     - **too long tailed shape**: Some have 100 times the median value...some have an order of magnitude..
     - **Shorten the tail**: to see SD, Orders of magnitude, the pattern more clearly, etc
       - Take 'log' or 'sqrt' in the variable
       - Take 'log' or 'sqrt' in a scaling layer: `+ scale_x_log()`, `+ scale_x_sqrt()`
         - what's the difference?: **x-axis labeling**. A scaling layer method returns actual 'friend_count' on x-axis. 
   - If we need to use regression
     - Take 'normalization' ?

Create all three histograms on "one single plot."
```
p1 <- ggplot(aes(x=friend_count), data=pf) + geom_histogram()
p2 <- ggplot(aes(x=log(friend_count + 1)), data=pf) + geom_histogram()
p3 <- ggplot(aes(x=sqrt(friend_count)), data=pf) + geom_histogram()

install.packages('gridExtra')
library(gridExtra)
grid.arrange(p1,p2,p3, ncol=1)

p1 <- ggplot(aes(x=friend_count), data=pf) + geom_histogram()
p2 <- p1 + scale_x_log()
p3 <- p1 + scale_x_sqrt()

grid.arrange(p1,p2,p3, ncol=1)
```
<img src="https://user-images.githubusercontent.com/31917400/35769867-c082c712-0909-11e8-964c-7b9d82821158.jpg" /> 

 - [6) Comparison I. **Frequency Polygon**, Connecting all counts and comparing each distribution]
   - `+ geom_freqpoly(aes(color = categorical_variable), binwidth)`
   - The shape of the frequency polygon depends on how our bins are set up - the height of the lines are the same as the bars in individual histograms, but the lines are easier to make a comparison with since they are on the same axis. 

With `facet_wrap(~gender)`, Check the each 'friend_count'.
```
ggplot(aes(x = friend_count), data = subset(pf, !is.na(gender))) + 
  geom_histogram(aes(color = gender), binwidth = 10) + 
  scale_x_continuous(limits = c(0, 1000), breaks = seq(0, 1000, 50)) + 
  facet_wrap(~gender)

ggplot(aes(x = friend_count), data = subset(pf, !is.na(gender))) + 
  geom_freqpoly(aes(color = gender), binwidth = 10) + 
  scale_x_continuous(limits = c(0, 1000), breaks = seq(0, 1000, 50)) + 
  facet_wrap(~gender)

by(pf$friend_count, pf$gender, sum)
```
<img src="https://user-images.githubusercontent.com/31917400/35770970-4515ab8a-091d-11e8-9b71-2e596b39152e.jpg" width="700" height="230" />

But the plots above do not show which gender has more friends on avg. We need proportion !
 - See the **proportion**:  Which side has more friends on average?
   - `sum(..count..)` will sum across color, so the proportions displayed are based on total users. 
   - `y = ..density...` will plot these proportions within each group
 - Change y-axis to show proportion instead of raw counts: `ggplot(aes(x = variable, y = ..count../sum(..count..))` 
 - Without `facet_wrap(~gender)`
 - Adding xlab(), ylab()
```
ggplot(aes(x = friend_count, y = ..count../sum(..count..)), data = subset(pf, !is.na(gender))) +
  geom_freqpoly(aes(color = gender), binwidth=10) +
  scale_x_continuous(limits = c(0, 1000), breaks = seq(0, 1000, 50)) +
  xlab('Friend Count') +
  ylab('Proportion of users with that friend count')
```
<img src="https://user-images.githubusercontent.com/31917400/35770870-a88164cc-091b-11e8-8f25-ba37273f8647.jpg" width="300" height="190" />

 - [7) Comparison II. **Boxplot** and comparing each distribution]
   - `+ geom_boxplot()`
   - In the boxplot, y-axis is no longer row count, but it should be the variable normally distributed. So we pass in the **categorical** to x, and the **numerical** to y.
   - 1)So many outliers, then focus on the box only(limiting 'friend_counts'(y-axis) between 0 and 1000) 
   - 2)But they remove our data from calculation and our boxes were affected, then go with the **"coord_cartesian layer"** instead of scale_layer !
   - 3)Let's try boxes only..
<img src="https://user-images.githubusercontent.com/31917400/35805199-c4becc8a-0a72-11e8-9978-4cb3468f7097.jpg" width="700" height="300" />

```
ggplot(aes(x=gender, y=friend_count), data = subset(pf, !is.na(gender))) + 
  geom_boxplot() + 
[...] + scale_y_continuous(limits = c(1,1000))

[...] + coord_cartesian(ylim = c(1,1000))
by(pf$friend_count, pf$gender, summary) 
[...] + coord_cartesian(ylim = c(0,250))
```
<img src="https://user-images.githubusercontent.com/31917400/35804020-e14a897a-0a6d-11e8-98cc-dcc327622762.jpg" />

> Useful Tips: To get a better look at the data 
 - What if the variable holds too many '0'sss ? 'mobile_likes' shows how often each user is giving 'likes' via their mobile phones. 
<img src="https://user-images.githubusercontent.com/31917400/35808590-31a105b0-0a7e-11e8-89fc-492a6350bc05.jpg" width="600" height="50" />

 - We want to simplify it. check_in or not...
 - Then **we create a factor!** First, let's create a new variable('mobile_check_in')that we funnel NA to..
 - `ifelse(cond, 1, 0)` if the cond is true, assign 1, otherwise assign 0, and convert it to a factor variable.
```
pf$mobile_check_in <- NA  
pf$mobile_check_in <- ifelse(pf$mobile_likes>0, 1, 0) 
pf$mobile_check_in <- factor(pf$mobile_check_in)

summary(pf$mobile_check_in)
```
<img src="https://user-images.githubusercontent.com/31917400/35807437-b58fdf26-0a7a-11e8-8a0a-fd59cfd427fd.jpg" width="600" height="40" />

 - What percent of users check_in, using mobile? 
   - The `sum()` will not work since mobile_check_in is a **factor variable**. 
   - Use the `length()` to determine the number of values in a vector.
```
sum(pf$mobile_check_in==1)/length(pf$mobile_check_in)
```
[1] 0.6459097

### II. Two Variable

>Phase_02. Tackle the issue - plotting 'actual_audience_size'(x) vs 'perceived_audience_size'(y)
 - Usually it's best to use a scatter plot to examine the relationship b/w two continuous variables. 
 - Here we have these scatter plots, where we're showing people's raw guesses of their perceived audience size against their actual audience size. And you can see they all fall below the perfect accuracy line. So people underestimate. But it's still kind of hard to get the big picture from these scatter plots and so, we have some histograms to capture more summary data. Using the histogram showing how much people over/underestimate their audience size. histogram ==> the #users vs (actual% - perceived%)
<img src="https://user-images.githubusercontent.com/31917400/35930965-49d990ee-0c2b-11e8-86c7-fb9aff037977.jpg" />

 
 - [1) Scatter of **'friend_count'** vs **'age'**]
   - `+ geom_point(alpha=1/20)`
   - `+ xlim(n, n)`
   - Note: OverPlotting - Some of these pt are **spread out from one another(verically)** while others are stacked right on top of each other. Some are overplotted. We can't see how many pt are in each region. so we set the transparency of pt using `geom_point(alpha)`.
```
summary(pf$age)
ggplot(aex(x=age, y=friend_count), data = pf) + geom_point() + xlim(13, 90)
ggplot(aes(x=age, y=friend_count), data = pf) + geom_point(alpha=1/20) + xlim(13, 90)
ggplot(aes(x=age, y=friend_count), data = pf) + geom_jitter(alpha=1/20) + xlim(13, 90)
```
<img src="https://user-images.githubusercontent.com/31917400/35819924-0fb29f7c-0a9c-11e8-840a-efd1e9b1a0e6.jpg" />

We want to do some **transformation on y-axis** to get a better visualization of the data. 
 - `+ scale_y_sqrt()` and `+ geom_smooth(method = 'lm')`
 - `+ coord_trans(y='sqrt')`
 - The difference between transforming the 'scales' and transforming the 'coordinate system' is that:
   - Scale transformation occurs BEFORE statistics.
   - Coordinate transformation afterwards. Coordinate transformation also changes the shape of geoms.
 - If we want 'jitter', then use `+ geom_point(position = position_jitter(h=0))` where we set the min-height of jitter as '0' because we have '0' value on y-axis, and in jitter, the value is trembling and might end up with negative value and those sqrt would be imaginary. 
```
ggplot(aes(x=age, y=friend_count), data=pf) + geom_point(alpha=1/20) + xlim(13, 90) 
  + scale_y_sqrt() + geom_smooth(method = 'lm') 

ggplot(aes(x=age, y=friend_count), data=pf) + geom_point(alpha=1/20) + xlim(13, 90) 
  + coord_trans(y='sqrt')

ggplot(aes(x=age, y=friend_count), data=pf) + geom_point(alpha=1/20, position = position_jitter(h=0)) + xlim(13, 90) 
  + coord_trans(y='sqrt')
```
<img src="https://user-images.githubusercontent.com/31917400/35880379-afd913c6-0b75-11e8-88b1-334d11681924.jpg" />

 - [2) Further thought about **'friend_count'** vs **'age'**]

Normally, it's impossible to judge important qualities from a scatter display (just plotting every single point) only..coz we need to see mean, median, variance varies with another variables.  For example, we ask..**how does the "average" friend count varies over age?** (sounds like a moving averages smoothing out?) To do this, we could start by creating a table that for each age gives us the mean, median for friend count(conditional mean, median)
 - Library: `dplyr`
   - It allows us to split up the dataframe and apply a function to SOME parts of the data !!!!
   - It provides a set of tools for efficiently manipulating dataframes. 
     - `group_by()` 
     - `summarise()`: reduces multiple, scattered values down to a grouped, single value..just like a pivot table? 
       - This func is typically used on grouped data created by `group_by()`. The output will have one row for each group. we used this when the expected groups are SO FUCKING a lot !
       - the `n()` is only useful in `summarise()` function and it reports how many people are in each group ^..^..discrete? categorical?
     - `mutate()`
     - `filter()`
     - `select()`
     - `arrange(data, by=desc(variable))`
```
install.packages('dplyr')
library(dplyr)
```
 - want to group our data by 'age'? Create a new dataset! 
 - create new dataset, using: `summarise(data, variable that i want to creat, the NO.of users in each group)`
 - so...it creates a...pivot table...then sort out in an ascending order
```
age_groups <- group_by(pf, age)
pf_fcount_by_age <- summarise(age_groups, f_count_mean = mean(friend_count), f_count_median = median(friend_count), n=n())
pf_fcount_by_age <- arrange(pf_fcount_by_age, by=age)
head(pf_fcount_by_age, 50) 
```
<img src="https://user-images.githubusercontent.com/31917400/35917431-05e374b0-0c06-11e8-84bd-f0af369197aa.jpg" />

 - Plot mean friend count vs. age using a line graph.
```
ggplot(aes(x=age, y=f_count_mean), data=pf_fcount_by_age) + geom_line() 
```
<img src="https://user-images.githubusercontent.com/31917400/35918378-7f84d554-0c09-11e8-868e-43284f1f84bf.jpg" />

>well..in older ages, our estimates are highly variable for friend_count_mean..in ages 30 to 60, the mean count is hovering about over 100...

 - We can overlay our original scatter plot with the summary plots on its top. 
```
ggplot(aes(x=age, y=friend_count), data=pf) + 
  geom_point(alpha=1/20, position = position_jitter(h=0), color='orange') +
  xlim(13, 90) + coord_trans(y='sqrt') +
  
  geom_line(stat = 'summary', fun.y = mean) + 
#Black: this is new!! the y is mean 'friend_count' by age over my raw data. we can see how dispersed the data is around the means..
  geom_line(stat = 'summary', fun.y = quantile, fun.args=list(probs = 0.1), linetype=2, color='blue') + 
#Blue: 10% quantile: the value that cuts off the first 10 percent of the data values when it is sorted in ascending order. 
  geom_line(stat = 'summary', fun.y = quantile, fun.args=list(probs = 0.9), linetype=2, color='red') +
#Red: 90% quantile 
  geom_line(stat = 'summary', fun.y = quantile, fun.args=list(probs = 0.5), linetype=2, color='green') 
#Green: 50% quantile("median")
```
 - We can zoom in. If we use `coord_cartesian(xlim = c(13, 90))` instead of `xlim()`and `coord_trans()`, we can zoom in. 
```
ggplot(aes(x=age, y=friend_count), data=pf) + 
  geom_point(alpha=1/20, position = position_jitter(h=0), color='orange') +
  
  coord_cartesian(xlim = c(13, 70), ylim = c(0,1000)) + 

  geom_line(stat = 'summary', fun.y = mean) + 
  geom_line(stat = 'summary', fun.y = quantile, fun.args=list(probs = 0.1), linetype=2, color='blue') + 
  geom_line(stat = 'summary', fun.y = quantile, fun.args=list(probs = 0.9), linetype=2, color='red') +  
  geom_line(stat = 'summary', fun.y = quantile, fun.args=list(probs = 0.5), linetype=2, color='green') 
```
<img src="https://user-images.githubusercontent.com/31917400/35929570-b00c38f2-0c27-11e8-81b6-cdb2ffc7d278.jpg" />

>The first plot: well..the 90% of data values are still below 1000. The second plot: well..in b/w 35 to 50, the friend count falls below 250, so 90% of users in this group have.. < 250 friends

 - Correlation: 'age' and 'friend_count'..Let's see the strength of the relationship!
   - The `with()` function applys an expression to a dataset.
   - The `by()` function applys a function to each level of a factor or factors.
```
cor.test(pf$age, pf$friend_count, method = 'pearson')
with(pf, cor.test(pf$age, pf$friend_count, method = 'pearson'))
```
<img src="https://user-images.githubusercontent.com/31917400/35931338-46f1e7f4-0c2c-11e8-80f9-8f916a0f67a4.jpg" width="600" height="120" />

>-0.027 so... there is no significant relationship...

 - The relationship is not linear or not monotonic...i mean...not flat..and we don't want to include the older ages, since they are likely to be incorrect. Let's recalculate the corr.
```
with(subset(pf, age<=70), cor.test(age, friend_count, method = 'pearson')) #as the age increases, friend count decreases...but it's just a descriptive statistics..we don't think age is not only factor that causes the decrease of friend count...so we need inferential statistics..-0.17

with(subset(pf, age<=70), cor.test(age, friend_count, method = 'spearman')) #spearman(the rank correlation measure) gives us 'rho' instead of 'cor'...that also measures "monotonic relationship"...-0.25
```
<img src="https://user-images.githubusercontent.com/31917400/35931723-640b1cec-0c2d-11e8-9b94-c64f80ede048.jpg" width="800" height="120" />

 - [3) Scatter of **likes_received**(y) vs. **www_likes_received**(x)]
```
ggplot(aes(x=www_likes_received, y=likes_received), data = pf) + geom_point()
```
Let's ignore some funky outlier..so limit the x, y-axis..using `quantile()`
```
ggplot(aes(x=www_likes_received, y=likes_received), data = pf) + geom_point() +
  xlim(0, quantile(pf$www_likes_received, 0.95)) +
  ylim(0, quantile(pf$likes_received, 0.95)) +
  geom_smooth(method = 'lm', color='red')
```
<img src="https://user-images.githubusercontent.com/31917400/35943037-b6cffe4e-0c4f-11e8-903c-017c0434fc19.jpg" width="800" height="210" />

The correlation coefficient measures the "tightness" of linear relationship between two variables and is bounded between -1 and 1, inclusive.

The regression slope measures the "steepness" of the linear relationship between two variables and can take any value from −∞ to +∞.

The correlation gives you a bounded measurement that can be interpreted independently of the scale of the two variables. The closer the estimated correlation is to ±1, the closer the two are to a perfect linear relationship. The regression slope, in isolation, does not tell you that piece of information.
<img src="https://user-images.githubusercontent.com/31917400/35944238-aeffa6fc-0c53-11e8-8fc8-218a781716c1.jpg" />

>But the correlation coefficient is invariant under a linear transformation of either X or Y, and the **slope** of the regression line when both X and Y have been transformed to **z-scores** is the **correlation coefficient**. 

That is, the **slope** of the regression and the **correlation** coincide when SD(Y) = SD(X). They only coincide when the two variables are on the same scale, in some sense. The most common way of achieving this is through **standardization**. When you are doing multiple regression, this can be more complicated due to multicollinearity.  

 - Let's quantify this relationship! 
```
cor.test(pf$www_likes_received, pf$likes_received, method = 'pearson') 
```
This gives us a correlation of 0.948. This is a strong positive correlation, and in reality most variables are not correlated that closely. The correlation that we just found was an artifact of the nature of the variables. One of them was really a superset of the other.
























































