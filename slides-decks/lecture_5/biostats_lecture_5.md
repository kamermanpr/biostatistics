
Lecture 5 and 6
===============
css: ../custom.css
transition: none
width: 960
height: 720
autosize: false



## Cookbook of commonly used statistical tests
- Comparing two groups
- Comparing more than two groups

<div style="position:fixed;bottom:10%">
    <h3 style="margin:0;">
        Introduction to Biostatistics
    </h3>
    <p style="font-style:italic;font-size:80%;margin-top:1%;margin-bottom:1%;">
        By: Peter Kamerman &nbsp&nbsp (view at
        <a href="//painblogr.org/biostatistics/" target="_blank">painblogR</a>)
    </p>
    <img src="./resources/cc-by.png" width="128" style="margin:0;"/>
</div>

Recap
=====
title: hide
type: aside

A quick recap on:<br>_p-values & hypothesis testing_

Definition of a p-value
=======================

<div>
    <p style="font-size:150%;font-style:italic;text-align:center;margin-top:60px;">
    "The probability of observing a result as great as (or greater than) you observed if the null hypothesis is true."
    </p>
    <br>
    <p style="text-align:center;">
    If the data are unlikely under the null hypothesis (small p-value), then either we observed a low probability event, <strong>or</strong> it must be that the null hypothesis is not true.<br><br>...only one of these can be correct.
  </p>
</div>

<div class="footer" style="font-size:60%;">
    Source: <a href="http://bayesianbiologist.com/2011/08/21/p-value-fallacy-on-more-or-less/">Corey Chivers, bayesianbiologist</a>
</div>

Hypothesis testing
==================

**Jerzy Neyman and Egon Pearson:**
- Works by setting a threshold $(\alpha)$ that the p-value must cross.

- You state a _null hypothesis_ and an _alternative hypothesis_ and use the _threshold p-value_ as a decision rule.

- The _p-value threshold_ is chosen to control false-positive inference (usually set at $\alpha$ = 0.05).

- You have to abide by the statistical test's 'decision' if you wish to protect against false-positive errors.

Which test?
===========

<img src="biostats_lecture_5-figure/cheatsheet_1-1.png" title="plot of chunk cheatsheet_1" alt="plot of chunk cheatsheet_1" style="display: block; margin: auto;" />

Which test?
===========

<img src="biostats_lecture_5-figure/cheatsheet_2-1.png" title="plot of chunk cheatsheet_2" alt="plot of chunk cheatsheet_2" style="display: block; margin: auto;" />

Parametric tests
================

**Experimental groups may differ for two reasons:**

1. Real effect of intervention

2. Random variation between samples drawn from the same population

<h3 style="text-align:center;">You must decide whether:</h3>
<p style="text-align:center;"><bold>[1]</bold> is large enough relative to <bold>[2]</bold> to conclude<br>that a treatment had an effect.</p>

Parametric tests
================

**Calculate the ratio of variances**

1. between-group variance $(\sigma^2_{bet})$

2. within-group variance $(\sigma^2_{with})$

<p style="text-align:center;">If samples are from the same population,<br>the variances will be similar, and...</p>

<p style="text-align:center;">$\frac{\sigma^2_{bet}}{\sigma^2_{with}} \rightarrow 1$</p>

<p style="text-align:center;">Degrees of Freedom <em>(df)</em> determine the critical value the ratio <em>(test statistic)</em> must reach for the null hypothesis to be rejected.</p>

Assumptions for parametric tests
=================================
class: vcenter

- The distribution of the data in the population is _Gaussian_

- Equal variance across groups<br>_(the basis on which the test statistic is calculated)_

- The errors are independent<br>_(the 'error' refers to the difference between each value and the mean)_

- Data are unmatched _(for unpaired data)_ / matching is effective _(for repeated measures data)_

Student's t-test
================
type: twocol

First have a look at the data

```r
data(sleep)
head(sleep)
```

```
  extra group ID
1   0.7     1  1
2  -1.6     1  2
3  -0.2     1  3
4  -1.2     1  4
5  -0.1     1  5
6   3.4     1  6
```

****
<br>

```r
boxplot(extra~group, data = sleep)
```

![plot of chunk t_test_2](biostats_lecture_5-figure/t_test_2-1.png)

Student's t-test
================
Run _t-test_


```r
# When you data are in long format:
t.test(extra ~ group, # formula
       data = sleep,  # data source
       paired = TRUE) # paired or unpaired

# If your data are in wide format,
# you can use the default format:
> t.test(x, y, ...)
```

Student's t-test
================

Output

```

	Paired t-test

data:  extra by group
t = -4.0621, df = 9, p-value = 0.002833
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 -2.4598858 -0.7001142
sample estimates:
mean of the differences 
                  -1.58 
```

One-way ANOVA
=============
Use when the grouping factor has $\geq$ 3 levels.

**One-way ANOVA**

```r
foo <- aov(outcome ~ factor_1,
           data = dataframe)
summary(foo)
```

**One-way repeated-measures ANOVA**

```r
bar <- aov(outcome ~ factor_2 +
               Error(subjectID), # rep measure
           data = dataframe)
summary(bar)
```

Two-way ANOVA
=============

**Two-way ANOVA**

```r
foo <- aov(outcome ~ factor_1 * factor_2,
           data = dataframe)
summary(foo)

# For main effects only:
> aov(outcome ~ factor_1 + factor_2,...)

# For repeated measures:
> aov(outcome ~ factor_1 + factor_2 +
          Error(subjectID), ...)
```

**NB:** `aov` uses _Type I SS_, so order matters.<br>
**Equivalent to Type III SS:** `drop1(foo, ~., test = 'F')`

Example: One-way ANOVA
======================
type: twocol


```r
data("chickwts")
head(chickwts)
```

```
  weight      feed
1    179 horsebean
2    160 horsebean
3    136 horsebean
4    227 horsebean
5    217 horsebean
6    168 horsebean
```

****


```r
boxplot(weight ~ feed,
        data =
            chickwts)
```

<img src="biostats_lecture_5-figure/anova_example_2-1.png" title="plot of chunk anova_example_2" alt="plot of chunk anova_example_2" style="display: block; margin: auto;" />

Example: One-way ANOVA
======================


```r
# One-way ANOVA
foo <- aov(weight ~ feed,
           data = chickwts)
summary(foo)
```

```
            Df Sum Sq Mean Sq F value   Pr(>F)    
feed         5 231129   46226   15.37 5.94e-10 ***
Residuals   65 195556    3009                     
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```


Post-hoc tests
==============

**NB:** Need to correct $(\alpha)$ for multiple comparisons<br>to avoid inflation of type I error.

$Bonferroni = \frac{target~p-value}{number~of~comparisons~(n)}$<br><br>
$Holm = \frac{target~p-value}{n - rank~number~in~terms~of ~degree~of~significance + 1}$


```r
# Pairwise post-hoc tests
pairwise.t.test(chickwts$weight, chickwts$feed,
                p.adjust.method = 'holm',
                paired = FALSE)

# Several correction methods are available.
```

Post-hoc tests
==============


```

	Pairwise comparisons using t tests with pooled SD 

data:  chickwts$weight and chickwts$feed 

          casein  horsebean linseed meatmeal soybean
horsebean 2.9e-08 -         -       -        -      
linseed   0.00016 0.09435   -       -        -      
meatmeal  0.18227 9.0e-05   0.09435 -        -      
soybean   0.00532 0.00298   0.51766 0.51766  -      
sunflower 0.81249 1.2e-08   8.1e-05 0.13218  0.00298

P value adjustment method: holm 
```


Post-hoc tests...contd
==============

What if you don't need to compare all pairs?

Pre-planned _(non-orthogonal)_ comparisons

```r
# Make a vector of p-values from each of
# the planned comparisons
p <- c('[test 1]' = 0.001, '[test 2]' = 0.211,
       '[test 3]' = 0.013)

# Adjust the p-values accordingly
p.adjust(p,
         method = 'holm')
```

```
[test 1] [test 2] [test 3] 
   0.003    0.211    0.026 
```

Non-parametric test
===================
Ranking methods

<img src="biostats_lecture_5-figure/data_rank-1.png" title="plot of chunk data_rank" alt="plot of chunk data_rank" style="display: block; margin: auto;" />

Assumptions for non-parametric tests
====================================

**Distribution free does not mean assumption free**

- The errors are independent<br>_(the 'error' refers to the difference between each value and the median)_

- Data are unmatched _(for unpaired data)_ / matching is effective _(for repeated measures data)_

- Samples are drawn from populations with the _same shape distributions_.

Non-parametric tests
====================

**Comparing two groups**

```r
# Wilcoxon signed-rank tests (paired t-test)
# Wilcoxon rank-sum test (unpaired t-test)
wilcox.test(value ~ group,
            data = dataframe,
            paired = TRUE)
```

Non-parametric tests
====================

**Comparing more than two groups**

```r
# Kruskal-Wallis rank sum test (One-way ANOVA)
kruskal.test(value ~ group,
            data = dataframe)

# Friedman test (Repeated measures ANOVA)
friedman.test(value ~ group | subjectID,
              data = dataframe)
```

Categorical data
================

**Contingency tables**

The relationship between categorical variables can be examined by cross-tabulating the data into contingency tables.


```r
# Dummy dataset
head(foo)
```

```
  intervention outcome
1    untreated    died
2      treated    died
3    untreated   lived
4    untreated    died
5    untreated    died
6    untreated    died
```

Categorical data
================
class: center

Cross-tabulate with `table`

```r
table(foo$intervention, foo$outcome)
```

```
           
            died lived
  treated     57    43
  untreated   63    37
```

Categorical data
================
class: center

Cross-tabulate with `xtabs`

```r
xtabs(~intervention + outcome, data = foo)
```

```
            outcome
intervention died lived
   treated     57    43
   untreated   63    37
```

Categorical data
================
class: center

**What if you already have totals?**


```r
print(bar)
```

```
  Intervention Outcome Count
1      treated   lived    43
2    untreated   lived    37
3      treated    died    57
4    untreated    died    63
```

Categorical data
================
class: center

**...Use `xtabs` with the 'count' on the left of the formula**


```r
xtabs(Count~Intervention + Outcome, data = bar)
```

```
            Outcome
Intervention died lived
   treated     57    43
   untreated   63    37
```

Categorical data
================
class: center

## $\chi^2$ test

Examines whether there is an association between the row variable and the column variable.

_That is, is the distribution of individuals among categories of one variable independent of their distribution among the categories of the other variable?_

Categorical data
================
class: center

The $\chi^2$ test compares the *observed* and the *expected* _(assuming no association)_ frequencies across the row and column variables.

$\chi^2 = \sum\frac{(Observed - Expected)^2}{Expected}$,

where Expected cell frequency $~= \frac{Row~total ~*~Column~total}{Grand~total}$

- If _Observed_ is close to _Expected_, then $\chi^2$ = small

- If _Observed_ is far from _Expected_, then $\chi^2$ = large

Test assumptions
================
class: vcenter

- Random sampling

- Observations are independent _(unpaired)_

- Large sample, with adequate _expected_ cell counts
    - 2 x 2 table: Expected $\geq$ 5 in all cells;
    - $\geq$ 2 x 3 table: Expected $\geq$ 5 in 80% of cells;
    - Expected $\neq$ 0 in any cell.

- Assumes the discrete probability of observed frequencies in the table can be _approximated_ by the continuous $\chi^2$ distribution.

Solutions
=========
class: vcenter

- At low sample sizes, p-values are too low _(risk of type I error)_, so introduce a p-value correction: **Yates' Correction for Continuity**.

- $\chi^2_{Yates} = \sum\frac{(|Observed - Expected|~- \frac{1}{2})^2}{Expected}$

- ...but Yates' correction may overcorrect the p-value and increase risk of Type II errors

Solutions
=========
class: vcenter

- **Fisher's Exact Test**.

- Doesn't use an approximation of the  $\chi^2$.

- _Exact_ p-values are calculated.

- Computationally intense, therefore traditionally limited to 2 x 2 tables.

- _R_ provides mechanisms to larger tables.

Examples
========


```
            outcome
intervention died lived
   treated     57    43
   untreated   63    37
```

```r
chisq.test(baz, correct = FALSE)
```

```

	Pearson's Chi-squared test

data:  baz
X-squared = 0.75, df = 1, p-value = 0.3865
```

```r
# TRUE for Yates' correction
```

Examples
========
class: center


```

	Fisher's Exact Test for Count Data

data:  baz
p-value = 0.4706
alternative hypothesis: true odds ratio is not equal to 1
95 percent confidence interval:
 0.4243409 1.4267683
sample estimates:
odds ratio 
  0.779501 
```

Examples
========

**What about bigger tables?**

```
        Blood_group
Sex       A  B  O
  Female 20 13 10
  Male   15 30 20
```

```r
fisher.test(fred)
```

```

	Fisher's Exact Test for Count Data

data:  fred
p-value = 0.04334
alternative hypothesis: two.sided
```

Examples
========
class: center

**Divide and conquer bigger tables**

```
        Blood_group
Sex       A  B
  Female 20 13
  Male   15 30
```

```r
tidy(# broom::tidy results to save space
    fisher.test(waldo) # A vs B blood group
    )
```

```
  estimate    p.value conf.low conf.high
1 3.030327 0.02191256  1.09882  8.702412
```

Examples
========
class: center

**Divide and conquer bigger tables**

```
        Blood_group
Sex       A  O
  Female 20 10
  Male   15 20
```

```r
tidy(
    fisher.test(bar) # A vs O blood group
    )
```

```
  estimate    p.value conf.low conf.high
1 2.625328 0.08076709 0.869242  8.336033
```

Examples
========
class: center

**Divide and conquer bigger tables**

```
        Blood_group
Sex       B  O
  Female 13 10
  Male   30 20
```

```r
tidy(
    fisher.test(qux) # A vs O blood group
    )
```

```
   estimate   p.value  conf.low conf.high
1 0.8683794 0.8027563 0.2858022  2.681916
```

Examples
========
class: center

**Multiple non-orthogonal tests, so need $\alpha$ correction**

```r
AB <- fisher.test(waldo)$p.value # A vs B
AO <- fisher.test(bar)$p.value # A vs O
BO <- fisher.test(qux)$p.value # B vs O

p.adjust(c(AB, AO, BO), method = 'holm')
```

```
[1] 0.06573767 0.16153418 0.80275628
```

Ordinal data
============

**Order has meaning, so use it**

```
       Disease_severity
Outcome  I II III
  Died  13 10  30
  Lived 30 20   5
```

```r
# install and load vcdExtra package
```

Ordinal data
============

**Order has meaning, so use it**

```r
# Run Cochran-Mantel-Haenszel Tests
# Assign scores to the ordered rows (rscores)
# or columns (cscores)
CMHtest(qux,
        types = 'cmeans', # compare columns
        cscores = c(1, 2, 3)) # column scores
```

```
Cochran-Mantel-Haenszel Statistics for Outcome by Disease_severity 

                AltHypothesis  Chisq Df       Prob
cmeans Col mean scores differ 27.626  2 1.0026e-06
```

What is my data are paired?
==========================

**Use the McNemar Test**

```
            2nd Survey
1st Survey   Approve Disapprove
  Approve        794        150
  Disapprove      86        570
```

```r
mcnemar.test(Performance)
```

```

	McNemar's Chi-squared test with continuity correction

data:  Performance
McNemar's chi-squared = 16.818, df = 1, p-value = 4.115e-05
```

Assignment
==========
class: vcenter

Practice applying the significance tests you learned about in this lecture by completing [assignment 4](https://painblogr.org/biostatistics#assignments).

The assignment also gives you a chance to practice using skills acquired in earlier lessions (_rmarkdown_, _git_, _GitHub_, _data cleaning_, and _plotting_).


Web resources
=============
class: vcenter

_**Basic statistics**_
- Quick-R: [basic statistics](http://www.statmethods.net/stats/index.html), by Robert Kabacoff.

- r-statistics.co: [statistical tests](http://r-statistics.co/Statistical-Tests-in-R.html), by Selva Prabhakaran


tl;dr
===================================

<div class="center", style="width:80%;">
    <p style="font-size:150%;font-style:italic;text-align:center;margin-top:60px;">
    "But the shocking discovery was that 50% were below the median age."
    </p>
    <p style="text-align:center">
     Dogbert<br>
        <span style="font-style:italic;font-size:80%;">(megalomaniac alter ego of Dilbert, by Scott Adams)</span>
    </p>
</div>
