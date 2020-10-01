---
title: "Effect of Smoking on Pre-term Birth"
date: 2020-09-20
tags: [regression, data science, logistic regression, health]
header:
  image: "/images/perceptron/percept.jpg"
excerpt: "Logistic regression, R, Healthcare"
mathjax: "true"
---


## Introduction


Our questions of interest is:
(1) Compared to non-smoking mothers, do smoking mothers have higher possibility of giving pre-term birth? 
(2) Can we find any evidence that the chances of giving pre birth differs by mother's race? 
(3) Besides the variables mentioned above, are there any other interesting findings?

In this report, logistic regression model was made to address the questions of interest. 


## Exploratory Data Analysis 

### Data
Before starting the analysis, the variable 'premature' was created based on gestation days: if the gestation is less than 270 days, it is categorized as 1, else 0. Predictors such as race, income bracket, education and whether mothers smoked or not were factored. Height, mother's pre-pregnancy weight, age and parity were considered as continuous variable. 

### Exploring Data
The boxplot for parity, age, height and pregnancy weight against the response variable showed that there is not much difference between two groups of mothers with regards to each numeric variable. The median parity value for moms who gave premature birth was similar to that of moms who did not give premature birth. This was same for all other numeric variables; the median value and the overall distribution was similar when comparing mothers who gave premature birth and mothers who did not. 

```r
#Parity vs. Pre-term birth
ggplot(smoke,aes(x=parity, y=premature_fac, fill=parity)) +
  geom_boxplot() + coord_flip() + scale_fill_brewer(palette="Reds") +
  labs(title="Parity vs Gestation", x="Parity",y="Gestation") + theme_classic() + theme(legend.position="none")

#Age vs. Pre-term birth
ggplot(smoke,aes(x=mage, y=premature_fac, fill=mage)) +
  geom_boxplot() + coord_flip() + scale_fill_brewer(palette="Reds") +
  labs(title="Age vs Gestation", x="Age",y="Gestation") + theme_classic() + theme(legend.position="none")

#Height vs. Pre-term birth
ggplot(smoke,aes(x=mht, y=premature_fac, fill=mht)) +
  geom_boxplot() + coord_flip() + scale_fill_brewer(palette="Reds") +
  labs(title="Height vs Gestation", x="Height",y="Gestation") + theme_classic() + theme(legend.position="none")

#Pre-pregnancy weight vs. Pre-term birth
ggplot(smoke,aes(x=mpregwt, y=premature_fac, fill=mpregwt)) +
  geom_boxplot() + coord_flip() + scale_fill_brewer(palette="Reds") +
  labs(title="Preg Weight vs Premature", x="Preg Weight",y="Premature") + theme_classic() + theme(legend.position="none")
```

The probabilities for premature birth given mother's race is white was 0.16, which was the lowest of all race categories. On the other hand, when mother's race is asian, odds of premature birth was 0.32, two times higher compared to white mothers. The p-value of premature and race was lower than 0.05, which indicated that these two variables are related and this variable be included in the model. 

```r
tapply(smoke$premature_fac, smoke$mrace, function(x) table(x)/sum(table(x)))
chisq.test(table(smoke[,c("premature_fac","mrace")]))
```

Another variable that showed p-value under 0.05 on their chi-squared test was pre-term birth and education. As the education level went up, the probability of giving premature birth decreased. 

```r
tapply(smoke$premature_fac, smoke$med, function(x) table(x)/sum(table(x)))
chisq.test(table(smoke[,c("premature_fac","med")]))
```

For the variable 'smoke', the probability of giving premature given that the mother smoked was higher than non-smoking mothers. But the results of chi-squared test showed that smoke and premature is not related to each other. 

The binned plots for pregnancy weight and premature birth showed a specific trend. The probability tended to decrease as pregnancy weight increased from 100 to 120, but the odds peaked again when the pregnancy weight was around 140 pounds, and decreased afterwards. 

```r
par(mfrow=c(2,2)) 
binnedplot(y=smoke$premature,smoke$mht,xlab="Height",ylim=c(0,1),col.pts="navy",
           ylab ="Premature baby?",main="Height and Premature",
           col.int="white")

binnedplot(y=smoke$premature,smoke$mpregwt,xlab="Pregnancy Weight",ylim=c(0,1),col.pts="navy",
           ylab ="Premature baby?",main="Pregnancy Weight and Premature",
           col.int="white")

binnedplot(y=smoke$premature,smoke$mage,xlab="Age",ylim=c(0,1),col.pts="navy",
           ylab ="Premature baby?",main="Age and Premature",
           col.int="white")

binnedplot(y=smoke$premature,smoke$parity,xlab="Parity",ylim=c(0,1),col.pts="navy",
           ylab ="Premature baby?",main="Parity and Premature",
           col.int="white")
```

## Final Model


## Conclusion



### H3 Heading


And here's some *italics*

Here's some **bold** text.

What about a [link](https://github.com/dataoptimal)?

Here's a bulleted list:
* First item
+ Second item
- Third item

Here's a numbered list:
1. First
2. Second
3. Third

Python code block:
```python
    import numpy as np

    def test_function(x, y):
      z = np.sum(x,y)
      return z
```

R code block:
```r
library(tidyverse)
df <- read_csv("some_file.csv")
head(df)
```

Here's some inline code `x+y`.

Here's an image:
<img src="{{ site.url }}{{ site.baseurl }}/images/perceptron/linsep.jpg" alt="linearly separable data">

Here's another image using Kramdown:
![alt]({{ site.url }}{{ site.baseurl }}/images/perceptron/linsep.jpg)

Here's some math:

$$z=x+y$$

You can also put it inline $$z=x+y$$
