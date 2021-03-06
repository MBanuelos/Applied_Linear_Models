---
title: "MATH 106 - Applied Linear Statistical Models"
subtitle: "00 - Introduction to Linear Models"
author: <b> <a href="http://www.mbgmath.com"> Mario Banuelos </a> </b>
date: mbanuelos22@csufresno.edu
output: 
  # uncomment to toggle between presentation and html_notebook
  # html_notebook
  ioslides_presentation:
    # comment the lines below if knitting entire document
    incremental: true
    logo: mbg.png
    css: styles.css
editor_options: 
  chunk_output_type: console
---

## Introduction {.build}
Instructor: Prof. Mario Banuelos

Lecture: MWF 1:00 – 1:50 pm, Science I, Rm 242

Office Hours: MW 2:30 – 4:00 pm, Tu 4:30 – 5:30pm, and by appointment (PB 337)

Students: Introduction

## What are linear models? {.build}

A <b>linear model</b> is an equation that attempts to describe the <b> <font color=#dc322f>response</font></b> variable with a linear transformation of the <b><font color=#dc322f>predictor</font></b> variable using the following form


<b>Example 1:</b>
$$
	Y  \approx  mX + b,
$$
where $m$ is the slope and $b$ is the intercept.

## Why study linear models? {.build}

Linear models help us understand the world around us:

- Are daughters taller than their mothers?
- Does changing class size affect success of students?
- Do countries with higher per person income have lower birth rates than countries with lower income?

Linear regression is the backbone of data science and many more advanced machine learning methods.

<b> This class: </b> <font color=#dc322f> Model </font> real-world phenomena using linear regression and <font color=#dc322f> analyze </font> the resulting models.

## Syllabus: Textbook

<b> Textbooks: </b> <em> Applied Linear Regression (ALR4) </em>, 4th Edition, S. Weisberg, 2014.

<em> An Introduction to Statistical Learning with Applications in R (ISLR) </em>, G. James, D. Witten, T. Hastie, and R. Tibshirani, 2013. 

Both books are free and available online.

- On the course schedule, I list which chapters we will discuss. <em> You are responsible for reviewing these sections before lecture. </em>
- Homework is due one week after it is assigned.


## Syllabus: Quizzes, Midterms & Final Project

<b> Quizzes: </b> There will be a total of five quizzes. The <b>  lowest quiz  </b> will be dropped.

<b> Midterm Exams: </b>  There will be two exams,

- Exam 1: Monday, March 9
- Exam 2: Friday, April 24
- No calculators, 3 $\times$ 5 index cards allowed.

<b> Final Project Presentation: </b> 
Monday, May 11 from 1:15-3:15pm in Science I Rm 242 
Final project is mandatory and more details will be provided in the future.

## Syllabus: Grade determination 

Your grade will be based on the following

- Homework Assignments (20%, lowest score dropped)
- Class Participation (10%)
- Quizzes (10%, lowest score dropped)
- Midterms (20% each)
- Final Project (20%).

## Syllabus: Homework and Computing

- Homework will be assigned on a weekly basis.
- It is mandatory that you complete these assignments.
- Homework will be due <b> at 11:59pm </b> one week from assigned date.
- All homework <b> will be required to be typed and uploaded as a PDF or HTML </b> online through Canvas. <b> R </b> will also be required as a component of this course.
- You are encouraged to work in groups.
- Homework will be graded for completeness/effort as well as correctness. So do your best on every problem!

## Syllabus: Miscellaneous

- Bring 2 blue books next week to class.
- All portable electronic devices must be put away in class.
- You are responsible for all information discussed in class; if you skip class, make sure you get any important information from others.

## Course expectations
In this course, you are expected to

- Attend all lectures, and participate in this mathematical community (see course  community agreement)
- <b> give space, take space </b>

## Other Expectations
You should also be <b> very familiar </b> with the following:

- Confidence intervals
- Hypothesis testing (t-tests vs z-tests)
- Graphical displays of data
- Exploratory data analysis

<b> HW1 </b> will be assigned today after class. HW0 (already assigned) is an online survey which is a credit/no credit assignment.


## Introduction {.build}

- <b> Predictors/Dependent Variables: </b> Generally referred to as $X$ and is used to predict $Y$. 
- <b> Response/Independent Variables:</b>  Referred to as $Y$ 
- <b> Data:</b>  Consists of values $(x_i, y_i)$ for $i = 1, 2, \ldots n$ of observed $(X, Y)$, where $n$ are the number of observations, or cases. 
- <b> Linear Regression:</b>  $\rightarrow$ a straight line!

## Motivating Example 1 {.build}

- One of the first use of regression was during 1893 -- 1898 by Karl Pearson (1857 -- 1936) and Lee.
- Organized the collection of $n = 1375$ heights of mothers in the United Kingdom under the age of 65 and one of their adult daughters over the age of 18. 
- We are interested in predicting daughter height using mother height in `Heights` data.

## `Heights` data

```{r, message=FALSE, echo=FALSE, fig.dim = c(8,4)}
# load necessary libraries
library(alr4)
library(ggplot2)
library(cowplot)

p1 <- ggplot(data = Heights) + 
  geom_point(mapping = aes(x = mheight, y = dheight))

p2 <- ggplot(data = Heights) + 
  geom_point(mapping = aes(x = round(mheight), y = round(dheight)))

plot_grid(p1, p2, labels = c('a)','b)'))

```


## Motivating Example 1 (continued) 

In small groups, consider the following:

- What is wrong with the previous plot?
- What is the difference between plot a) and plot b)?
- Do taller mothers tend to have taller daughters?
- What is <em>  jittering </em> and what does it do?

## Motivating Example 2

- In an 1857, Scottish physicist James D. Forbes (1809 --1868) discussed a series of experiments that he had done concerning the relationship between atmospheric pressure and the boiling point of water.
-  The data for $n = 17$ locales are reproduced in the data `Forbes`.

## `Forbes` data

```{r, echo=FALSE, fig.dim=c(8,3)}
p1 <- ggplot(data = Forbes) + 
  geom_point(mapping = aes(x = bp, y = pres)) + labs(caption = 'test')

p2 <- ggplot(lm(pres ~ bp, data=Forbes)) +
  geom_point(aes(x=.fitted, y=.resid))

plot_grid(p1, p2, labels = c('a)','b)'))
```

<b> Q: </b>

- How is the residual calculated?
- Are there any patterns you observe in the residuals? 

## Scatterplots
Given a collection of one predictor $X$ and one response $Y$, scatterplots visually represent <em> potential </em> relationships between $X$ and $Y$.

- <b> Q: </b> If $X$ and $Y$ are uncorrelated, what would the resulting scatterplot look like?
- <b> Q: </b> How would you generate and plot such data?


## Mean Functions {.build}

Our interest centers on how the distribution of $Y$ changes as $X$ is varied, which is described by the <b>  mean function </b>,

\[
\text{E}(Y \;|\; X = x) = \text{ a function that depends on the value of }x
\]

In the case of the mother and daughter height, we have

\[
\text{E}( \texttt{dheight}\; |\; \texttt{ mheight}=x)=\beta_0 + \beta_1 x
\]


This particular mean function has 2 parameters, an intercept $\beta_0$ and a slope $\beta_1$. If we knew the values of the $\beta$s, then the mean function would be completely specified, but usually the $\beta$s need to be estimated from data. 

## Variance Functions {.build}

Another part of the distribution of $Y$ is described by the <b>  variance function </b> ,

\[
\text{Var}(Y \;|\; X = x)
\]

A frequent assumption in fitting linear regression models is that the variance function is the same for every value of x. 

\[
\text{Var}(Y \;|\; X = x)  = \sigma^2
\]

This is usually done for convenience but we will discuss general variance models in Ch. 7.
