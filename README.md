
# Statistical Distributions

![](images/distributions.png)

# Order of Business:
    
>    1. Describe the difference between discrete vs continuous variables
>    2. Describe the difference between PMFs, PDFs, CDFs
>    3. Introduce the bernouli and binomial distributions
>    4. Introduce the normal distribution and empirical rule

## What is a statistical distribution?

> After establishing the set of all possible outcomes, a statistical distribution is a representation of the relative frequency each event will occur.

The distributions we introduce today will reappear throughout the bootcamp.  They will:

1. Allow us to conduct statistical tests to judge the validity of our conclusions.  As a data scientist at your company, you may be asked to perform various scientific tests. For example, you may be asked to judge whether a certain change to the user interface of your website increases conversion rate. 
2. Provide the foundation for specific assumptions of linear regression.
3. Appear in the cost functions tied to logistic regression and other models.
4. Drive the classification decisions made in parametric models, such as Naive-Bayes. 



# 1. Discrete vs Continuous

We will learn about a variety of different probability distributions, but before we do so, we need to establish the difference between **discrete** and **continuous** variables.

## Discrete
>  With discrete distributions, the values can only take a finite set of values.  Take, for example, a roll of a single six-sided die. 

![](images/uniform.png)

> - There are 6 possible outcomes of the roll.  In other words, 4.5 cannot be an outcome. As you see on the PMF plot, the bars which represent probability do not touch, suggesting non-integer numbers between 1 and 6 are not possible results.

### Let's think back to our Phase 1 projects: What are some examples of discrete probability distributions in either the King County or Movie datasets?

your answer here

Let's take a moment to look back at the Divy data we encountered in our visualizations lesson.




```python
# ! curl https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2020_Q1.zip -o 'data/divy_2020_Q1.zip'
# ! unzip data/divy_2020_Q1.zip -d data
```


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns


%load_ext autoreload
%autoreload 2
from src.data_import import prep_divy
from src.student_caller import three_random_students
from src.student_list import student_first_names
divy_trips = prep_divy()

```


```python
# Let's create a probability distribution of the rides per day of the week.
```


```python
# code here
```

The above plot visualizes an **empirical** distribution. Empirical distributions are based on observations of real world phenomena. 

An a**nalytical** distribution is one which is created by a mathematical function.  We use analytical functions to model real world phenomena. 
[ThinkStats2e](http://greenteapress.com/thinkstats2/html/thinkstats2006.html)

#### Examples of analytical discrete distributions:

> 1. The Uniform Distribution:- occurs when all possible outcomes are equally likely.
> 2. The Bernoulli Distribution: - represents the probability of success for a certain experiment (binary outcome).
> 3. The Binomial Distribution - represents the probability of observing a specific number of successes (Bernoulli trials) in a specific number of trials.
> 4. The Poisson Distribution:- represents the probability of 𝑛 events in a given time period when the overall rate of occurrence is constant.



## Continuous

With a continous distribution, the set of possible results is an infinite set of values within a range. One way to think about continuous variables are variables that have been measured.  Measurement can always be more precise.

> - A common example is height.  Although we think of height often in values such as 5 feet 7 inches, the exact height of a person can be any value within the range of possible heights.  In other words, a person could be 5 foot 7.000001 inches tall. 
> - Another example is temperature, as shown below:

![](images/pdf.png)

#### Examples of analytical continuous distributions
> 1. Continuous uniform
> 2. The Normal or Gaussian distribution.
> 3. Exponential


### What are examples of continuous probability distributions in the SKC and movie datasets?

    # your answer here


```python
# Let's take the data above, and inspect and plot a continuous variable: ride time.

```


```python
# Boxplot
```


```python
# Boxplot no fliers
```


```python
# Histogram
```

The distinction between descrete and continuous is very important to have in your mind, and can easily be seen in plots. 

Let's do a quick exercise. There are two tasks.  

1. First, simply change the color of the plots representing descrete data to orange and the plots represent continous data to blue.
2. Attach the titles to the distributions you think reflect the data set described.


```python
from scipy import stats
from matplotlib import pyplot as plt
import seaborn as sns
import numpy as np
%matplotlib inline
```


```python

title_1 = "height_of_us_women in inches"
title_2 = 'outcomes of flipping a coin 100 times'
title_3 = 'outcomes of rolling a 20 sided die 1000 times'
title_4 = 'probability that a computer part lasts a certain amount of time from now.'
title_5 = 'probability that a picture is a chihauhua\n, a muffin, a bird, or a piece of pizza\n as would guess a neural network'
title_6 = 'probability of rolling a value equal to or below\n a certain number on a 20 sided dice'
no_title = 'no_title'

fig, ax = plt.subplots(2,3, figsize=(15,10))

sns.kdeplot(np.random.exponential(10, size=1000), ax=ax[0][0], color='purple')
ax[0][0].set_xlim(0,80)
ax[0][0].set_title(no_title)

sns.barplot(['outcome_1', 'outcome_2', 'outcome_3', 'outcome_4'], [.4,.5,.08,.02], ax=ax[1][0], color='yellow')
ax[1][0].tick_params(labelrotation=45)
ax[1][0].set_title(no_title)

sns.kdeplot(np.random.normal(64.5, 2.5, 1000), ax=ax[1][1])
ax[1][1].set_title(no_title)

sns.barplot(x=['outcome_1','outcome_2'], y=[sum(np.random.binomial(1,.5, 100)),100 - sum(np.random.binomial(1,.5, 100))], ax=ax[0][1], color='pink')
ax[0][1].set_title(no_title)

sns.barplot(x=list(range(1,21)), y=np.unique(np.random.randint(1,21,1000), return_counts=True)[1], ax=ax[0][2], color='teal')
ax[0][2].tick_params(labelrotation=45)
ax[0][2].set_title(no_title)

sns.barplot(list(range(1,21)), np.cumsum([1/20 for number in range(1,21)]), ax=ax[1][2])
ax[1][2].set_title(no_title)

plt.tight_layout()
```


![png](index_files/index_28_0.png)


# 2. PMFs, PDFs, and CDFs, oh my!

## PMF: Probability Mass Function


The $\bf{probability\ mass\ function\ (pmf)}$ for a random variable gives, at any value $k$, the probability that the random variable takes the value $k$. Suppose, for example, that I have a jar full of lottery balls containing:
- 50 "1"s,
- 25 "2"s,
- 15 "3"s,
- 10 "4"s

We then represent this function in a plot like so:


```python
# For each number, we calculate the probability that pull it from the jar by dividing

numbers = range(1,5)
counts = [50,25, 15, 10]
```


```python
# calculate the probs by dividing each count by the total number of balls.
probs = None
loto_dict = {}
```


```python
# Plot here!


x = list(lotto_dict.keys())
y = list(lotto_dict.values())

fig, ax = plt.subplots(1, 1, figsize=(6, 6))
ax.plot(x, y, 'bo', ms=8, label='lotto pmf')
ax.vlines(x, 0, y, 'r', lw=5)
ax.legend(loc='best');
```


![png](index_files/index_34_0.png)


### Expected Value/Mean

The expected value, or the mean, describes the 'center' of the distribution (you may hear this called the first moment).  The 'center' refers loosely to the middle-values of a distribution, and is measured more precisely by notions like the mean, the median, and the mode.

For a discrete distribution, working from the vantage point of a collected sample of n data points:

mean = $\Large\mu = \frac{\Sigma^n_{i = 1}x_i}{n}$

If we are working from the vantage point of known probabilities, the mean is referred to as the expected value. The expected value of a discrete distribution is the weighted sum of all values of x, where the weight is their probability.
 
The expected value of the Lotto example is:
${\displaystyle \operatorname {E} [X]= \Sigma^n_{i=1}p(x_i)x_i}$


## Let's calculate the expected value of the lotto example:



```python
# code
```

### Variance/Standard Deviation
Variance describes the spread of the data (it is also referred to as the second moment).  The 'spread' refers loosely to how far away the values are from the center.

Standard deviation is the square root of variance, and effectively measures the *average distance away from the mean*.

From the standpoint of a sample, the variance of a discrete distribution of n data points is:

std = $\Large\sigma = \sqrt{\frac{\Sigma^n_{i = 1}(x_i - \mu)^2}{n}}$


Variance is the expectation of the squared deviation of a random variable from its mean.

For our Lotto PMF, that means:

 $ \Large E((X-\mu)^2) = \sigma^2 = \Sigma^n_{i=1}p(x_i)(x_i - \mu)^2$


## Let's calculate the variance for the Lotto Ball example



```python
# Code
```

# Pair Program 7 minutes

The **uniform** distribution describes a set of discrete outcomes whose probabilities are all equally likely.

A common example is the roll of a die.  

![dice](https://media.giphy.com/media/3ohhwLh5dw0i7iLzOg/giphy.gif)



For the following pair programming challenge, you are tasked with:

    1. Calculating the expected value of a 12-sided die roll using the above equations.
    2. Calculating the variance and standard deviation of the 12-sided die roll using the above equations.
    3. Plot the pmf of the 12-sided die roll.


```python
# Your code here
expected_value = None
```


```python
# Your code here
variance = None
standard_deviation = None
```


```python
# Your code here
```

To check your answers, use the formulae below.


$\Large E(X)=\frac{a+b}{2}$

Where a is the lowest value and b is the highest. 




```python
# Let's check out that the two methods equal the same thing.
expected_value == (1+12)/2
```




    True



Variance can be calculated as follows:

$ \Large\sigma^2=\frac{(b-a+1)^2-1}{12} $


```python
# Again, let's check our math
round(variance,7) == round(((12-1+1)**2-1)/12, 7)
```




    True



The pmf of a discrete uniform distribution is simply:

$ f(x)=\frac{1}{n} $


## PDF: Probability Density Function
> Probability density functions are similar to PMFs, in that they describe the probability of a result within a range of values.  But where PMFs can be descibed with barplots, PDFs are smooth curves.  

![](images/pdf_temp.png)



We can think of a pdf as a bunch of bars of probabilities getting smaller and smaller until each neighbor is indistinguishable from its neighbor.


![](images/pdf_inter.png)

# Describing the PDF

Instead of calculating the mean and standard deviation by hand (this would require integration), we will rather get familiar with how they affect the shape of our PDF.


The mean of our PDF affects where it is centered on the x-axis.  In numpy and stats, mean is denoted by the loc parameter.

The two plots below have the same shape, but different centers.


```python
fig, ax = plt.subplots()

mean = 0
z_curve = np.linspace(stats.norm(mean,1).ppf(0.01),
             stats.norm(mean,1).ppf(0.99), 100)
ax.plot(z_curve, stats.norm(mean,1).pdf(z_curve),
     'r-', lw=5, alpha=0.6, label='z_curve')

mean = 1
z_curve = np.linspace(stats.norm(mean,1).ppf(0.01),
             stats.norm(mean,1).ppf(0.99), 100)
ax.plot(z_curve, stats.norm(mean,1).pdf(z_curve),
     'b-', lw=5, alpha=0.6, label='norm pdf')

ax.set_title("Two distributions differing only in mean")
```




    Text(0.5, 1.0, 'Two distributions differing only in mean')




![png](index_files/index_59_1.png)


The variance of our plots describes how closely the points are gathered around the mean.  Low variance means tight and skinny, high variance short and wide.


```python
# Mess around with the variance to see how the shape is altered.

fig, ax = plt.subplots()

mean = 1
var = 1
z_curve = np.linspace(stats.norm(mean,var).ppf(0.01),
             stats.norm(mean,var).ppf(0.99), 100)
ax.plot(z_curve, stats.norm(mean,var).pdf(z_curve),
     'r-', lw=5, alpha=0.6, label='z_curve')

mean = 1
var = 3
z_curve = np.linspace(stats.norm(mean,var).ppf(0.01),
             stats.norm(mean,var).ppf(0.99), 100)
ax.plot(z_curve, stats.norm(mean,var).pdf(z_curve),
     'b-', lw=5, alpha=0.6, label='norm pdf')

ax.set_title("Two distributions with different variance")
```




    Text(0.5, 1.0, 'Two distributions with different variance')




![png](index_files/index_61_1.png)


## Skew 

We will touch briefly on the third and fourth moments for the normal curve. Skew is a measure of assymemtry.  A skew of zero is perfectly symetrical about the mean.   
![skew](images/skew.png)


```python
# We can check skew with scipy
z_curve = np.random.normal(0,1, 1000)
print(stats.skew(z_curve))
```

    0.08141307654492642


To add right skew to the data, let's add some outliers to the left of the mean.

To learn about skew, let's take a normal distribution, and add values to skew it.


```python
# Update add right skew with data to skew it.
z_curve = np.random.normal(0,1, 1000)
add_right_skew = [0]
right_skewed_data = np.concatenate([z_curve, add_right_skew])

fig, ax = plt.subplots()
ax.hist(right_skewed_data)
ax.set_title(f"Right Skew {stats.skew(right_skewed_data)}");
```


![png](index_files/index_66_0.png)



```python
# Now, do the same for left skewed data

z_curve = np.random.normal(0,1, 1000)
add_left_skew = [0]
left_skewed_data = np.concatenate([z_curve, add_left_skew])

fig, ax = plt.subplots()
ax.hist(left_skewed_data)
ax.set_title(f"Left Skew {stats.skew(left_skewed_data)}");
```


![png](index_files/index_67_0.png)


# Pair Program

When we get to modeling, certain models may be improved by correcting the skew of our distributions to make them more normal.  below are a few different ways to correct for different types of skew.

### Transforming  Right/Positively Skewed Data

We may want to transform our skewed data to make it approach symmetry.

Common transformations of this data include 

#### Square root transformation:
Applied to positive values only. Hence, observe the values of column before applying.


#### The cube root transformation: 
involves converting x to x^(1/3). This is a fairly strong transformation with a substantial effect on distribution shape: but is weaker than the logarithm. It can be applied to negative and zero values too. Negatively skewed data.

#### The logarithm:
x to log base 10 of x, or x to log base e of x (ln x), or x to log base 2 of x, is a strong transformation and can be used to reduce right skewness.


```python
# np.log(array_like_object) will transform each element of the array.
```

## Left/Negatively Skewed Data

### Square transformation:
The square, x to x2, has a moderate effect on distribution shape and it could be used to reduce left skewness.
Another method of handling skewness is finding outliers and possibly removing them

Let's return to the Divy ride times.

Let's return to our Divy ride time example.  

Below is the original distribution of ride times.



```python
fig, ax = plt.subplots()

ax.hist(divy_trips.ride_time, bins=50);
ax.set_title("""Divy Bike Ride Time: 
                Heavy Right Skew = {}""".format(round(stats.skew(divy_trips.ride_time),3)));

```


![png](index_files/index_74_0.png)


With a partner, apply an appropriate transformation to reduce the skew of the distribution:
    
  - 1. Select and apply an appropriate transformation
  - 1. Plot transformed distribution
  - 3. Report transformed skew
    - Hint: certain transformations don't like zeros
    


```python
# your code here
```

# Kurtosis

![kurtosis](images/kurtosis.png)


## CDF: Cumulative Distribution Function

![](images/cdf.png)

The cumulative distribution function describes the probability that your result will be of a value equal to or below a certain value. It can apply to both discrete or continuous functions.

For the scenario above, the CDF would describe the probability of drawing a ball equal to or below a certain number.  

In order to create the CDF from a sample, we:
- align the values from least to greatest
- for each value, count the number of values that are less than or equal to the current value
- divide that count by the total number of values

The CDF of the Lotto example plots how likely we are to get a ball less than or equal to a given example. 

Let's create the CDF for our Lotto example



```python
lotto_dict = {0:0, 1:50, 2:25, 3:15, 4:10}
# align the values

# count the number of values that are less than or equal to the current value

# divide by total number of values

```

# Pair Program
Taking what we know about cumulative distribution functions, create a plot of the CDF of divy bike rides by hour of the day.

Take this in steps (no pun intended).
1. Count the number of rides per hour.  Hint: Use groupby.
2. Make sure the hours are arranged from earliest to latest.
3. Calculate the cumulative sum after each hour (hint: try np.cumsum())
4. Use a list comprehension or for loop to divide each hours cumsum by the total.
5. Create a bar plot in matplotlib.
6. Fix the x-ticks to be positioned at the beginning of each bar



```python
# Your Code here

```

- For continuous random variables, obtaining probabilities for observing a specific outcome is not possible 
- Have to be careful with interpretation in PDF

We can, however, use the CDF to learn the probability that a variable will be less than or equal to a given value.



Consider the following normal distributions of heights (more on the normal distribution below).

The PDF and the CDF look like so:



```python
r = sorted(stats.norm.rvs(loc=70,scale=3,size=1000))
r_cdf = stats.norm.cdf(r, loc=70, scale=3)
fig, (ax1,ax2) = plt.subplots(1,2, figsize=(10,5))
sns.kdeplot(r, ax=ax1, shade=True)
ax1.set_title('PDF of Male Height in US')

ax2.plot(r, r_cdf, color='g')
ax2.set_title('CDF of Male Height in the US')


```




    Text(0.5, 1.0, 'CDF of Male Height in the US')




![png](index_files/index_85_1.png)


If we provide numpy with the underlying parameters of our distribution, we can calculate: 



```python
# the probability that a value falls below a specified value
r = stats.norm(70,3)
r.cdf(73)

```




    0.8413447460685429




```python
# the probability that a value falls between two specified values
r = stats.norm(70,3)
r.cdf(73) - r.cdf(67)

```




    0.6826894921370859



We can also calculate the value associated with a specfic percentile:


```python
r.ppf(.95)
```




    74.93456088085442



And from there, the value of ranges, such as the interquartile range:


```python
print(f'interquartile range {r.ppf(.25)} - {r.ppf(.75)}')

# We can see that the boxplot's interquartile range aligns with our ppf calculation above
box = plt.boxplot(stats.norm.rvs(loc=70,scale=3,size=1000));
print(box['boxes'][0].get_data())

```

    interquartile range 67.97653074941175 - 72.02346925058825
    (array([0.925, 1.075, 1.075, 0.925, 0.925]), array([68.1099954 , 68.1099954 , 71.94071889, 71.94071889, 68.1099954 ]))



![png](index_files/index_92_1.png)


![break](https://media.giphy.com/media/mX3Pf78rXsfxrUDNwi/giphy.gif)

# 3. Bernouli and Binomial Distributions

The Bernouli distribution is the discrete distribution that describes a two-outcome trial, such as heads or tails.  The distribution is described by the probability of one random variable of the value 1 associated with the probability p, and its correlary, the probability q, associated with 0  and taking the probability 1-p. 

PMF: 
${\displaystyle {\begin{cases}q=1-p&{\text{if }}k=0\\p&{\text{if }}k=1\end{cases}}}$

The simplest example is, once again, a coin flip.  In this scenario, we define either heads or tails as a "success", and assume, if the coin is fair, the probability of success to be .5

![](images/bernouli.png)

Another example would be the chance a nohitter would occur in a baseball game.

![no_hitter](https://media.giphy.com/media/nTbCRLw5ZPWBa/giphy.gif)

We will scrape data from the web for this example:


```python
import requests 
response = requests.get('https://en.wikipedia.org/wiki/List_of_Major_League_Baseball_no-hitters')

# Scrape the wikipedia table corresponding to no-hitters
no_hit_table = pd.read_html('https://en.wikipedia.org/wiki/List_of_Major_League_Baseball_no-hitters')[1]

# for demonstration, look at games from 1998, which all had 162 games for 30 teams. The game count is approximately 2430 games
no_hit_table['Date'] = pd.to_datetime(no_hit_table['Date'])
no_hit_table['Year'] = no_hit_table.Date.apply(lambda x: x.year)

# Don't count 2020 given it is currently under way.
no_hit_table_1998= no_hit_table[(no_hit_table["Date"] >= "1998") & (no_hit_table["Date"] < "2020")]

no_hit_table_1998.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>#</th>
      <th>Date</th>
      <th>Pitcher</th>
      <th>Team</th>
      <th>RS</th>
      <th>Opponent</th>
      <th>RA</th>
      <th>League</th>
      <th>Catcher</th>
      <th>Notes</th>
      <th>Year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>246</th>
      <td>245.0</td>
      <td>1998-05-17</td>
      <td>David Wells</td>
      <td>New York Yankees</td>
      <td>4</td>
      <td>Minnesota Twins</td>
      <td>0</td>
      <td>AL</td>
      <td>Jorge Posada</td>
      <td>[notes 142]</td>
      <td>1998</td>
    </tr>
    <tr>
      <th>247</th>
      <td>246.0</td>
      <td>1999-06-25</td>
      <td>José Jiménez</td>
      <td>St. Louis Cardinals</td>
      <td>1</td>
      <td>Arizona Diamondbacks</td>
      <td>0</td>
      <td>NL</td>
      <td>Alberto Castillo</td>
      <td>[notes 143]</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>248</th>
      <td>247.0</td>
      <td>1999-07-18</td>
      <td>David Cone</td>
      <td>New York Yankees (AL)</td>
      <td>6</td>
      <td>Montreal Expos (NL)</td>
      <td>0</td>
      <td>Inter</td>
      <td>Joe Girardi (2)</td>
      <td>[notes 144]</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>249</th>
      <td>248.0</td>
      <td>1999-09-11</td>
      <td>Eric Milton</td>
      <td>Minnesota Twins</td>
      <td>7</td>
      <td>Anaheim Angels</td>
      <td>0</td>
      <td>AL</td>
      <td>Terry Steinbach (2)</td>
      <td>NaN</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>250</th>
      <td>249.0</td>
      <td>2001-04-04</td>
      <td>Hideo Nomo (2)</td>
      <td>Boston Red Sox</td>
      <td>3</td>
      <td>Baltimore Orioles</td>
      <td>0</td>
      <td>AL</td>
      <td>Jason Varitek (1)</td>
      <td>[notes 145]</td>
      <td>2001</td>
    </tr>
  </tbody>
</table>
</div>



To create a Bernouli distribution from the above data, we have to calculate the probability of a no-hitter occuring in a single trial.  In this scenario, a trial is a single game.`


```python
# plot Bernouli distribution of no hitter

# probability of scoring
p = p_no_hitter
# probability of missing
q = 1 - p_no_hitter

fig, ax = plt.subplots(figsize=(10,10))
ax.bar(['hitter', 'no hitter'],[q,p], color=['red','green'])
ax.set_title('Bernouli Distribution of No Hitters')

```




    Text(0.5, 1.0, 'Bernouli Distribution of No Hitters')




![png](index_files/index_100_1.png)


The expected value is the probability of success, i.e. **.001146**  
The variance is:  
$\sigma^2 = (p\_no\_hitter)*(1-p\_no\_hitter) = .001147 $

## Binomial

The Binomial distribution describes the number of successess of a set of bernouli trials. For example, say we have an unfair coin with a probability of landing heads of .8, if we designated our number of trials as 3, our PMF and CDF would look like what we see below:
![](images/binomial.png)

For the binomial our Expected Value and Variance can be calculated like so:
- Expected Value
> $E(X) = np$ <br>
- Variance
> $Var(X) = np(1-p)$<br>

If we want to see the probability of a certain number of successes, we use the pmf.
$\Large f(x) = {n \choose k}p^k(1 - p)^{n - k}$

Note: ${n\choose k} = \frac{n!}{k!(n - k)!}$, the number of ways of choosing $k$ objects from a total of $n$.

## Coin Flip Code Along
To get our feet wet with the multinomial, let's look at the traditional coin flip example.

![coin_flip](https://media.giphy.com/media/Q8gsDmBzmNkKE9DVsg/giphy.gif)

Let's code out the probability mass distribution of observing a certain number of heads in sets of 10 flips of a fair coin.




```python
#  What is the probability of a succesful trial (p)?

```


```python
#  How many trials constitute one round of our experiment (k)? 

```


```python
# What is our set of possible outcomes?

```

While it may be evident what the set of possible outcomes is, often that is not the case.  We can use stats.binom.ppf() to create a the set of outcomes. 

**stats.binom.ppf** returns the outcome associated with a given percentage of the cdf. We can then use np.arange to create the set of outcomes associated with a range of percentages.  In this case, we can reproduce the range of using a very low and high percentage.



```python
# Possible outcomes using ppf

```


```python
# What probability is associated with each outcome?

```


```python
# Plot the pmf

```

# Pair Programming (12 minutes)

We will expand on our no hitter example from above, modeling the probability of the number of no-hitters occuring over an entire season's worth of games.  

In pairs, you will create the PMF and CDF of the multinomial distribution of our no-hitter example.  

To get started, 

     1. calculate the expected value and the variance of the distribution.
       - to do so, you must define n (number of trials) and p (the probability of a no-hitter in one trial).  
       
     2. Create a range of results (i.e. an ordinal set of counts of nohitters per season) using np.arange and stats.binomial.ppf()
     
     3. Create probabilities associated with each result using stats.binom.ppf
     
     4. Create a bar plot of the probabilities associated with each no-hitter count
     





```python
# 1. calculate the expected value and the variance of the distribution.

n = None
p = None

expected_value = None
variance = None

print("We expect {} no hitters across a season".format(round(expected_value,5)))
print("The variance of the nohitter distribution is {}".format(round(variance, 5)))
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-221-ef10cd6a7190> in <module>
          7 variance = None
          8 
    ----> 9 print("We expect {} no hitters across a season".format(round(expected_value,5)))
         10 print("The variance of the nohitter distribution is {}".format(round(variance, 5)))


    TypeError: type NoneType doesn't define __round__ method



```python
# 2. Create a range of results using np.arrange and stats.binomial.ppf()
x = None
```


```python
# 3. Create probabilities associated with each result using stats.binom.ppf

p_binom = None
```


```python
# 4. Create a bar plot of the probabilities associated with each no-hitter count

```

# 4. Normal Distribution

The normal distribution describes many phenomena. Think of anything that has a typical range:
- human body temperatures
- sizes of elephants
- sizes of stars
- populations of cities
- IQ
- Heart rate

Among human beings, 98.6 degrees Fahrenheit is an _average_ body temperature. Many folks' temperatures won't measure _exactly_ 98.6 degrees, but most measurements will be _close_. It is much more common to have a body temperature close to 98.6 (whether slightly more or slightly less) than it is to have a body temperature far from 98.6 (whether significantly more or significantly less). This is a hallmark of a normally distributed variable.

Similarly, there are large elephants and there are small elephants, but most elephants are near the average size.

The normal distribution is _very_ common in nature and will arise often in your work. Get to know it well!

You will recognize it by its characteristic bell curve. 

![normal_curve](images/IQ_normal.png)

You may see the notation 

$ N(μ,σ2)$

where N signifies that the distribution is normal, μ is the mean, and σ2 is the variance. 


The PDF of the normal curve equals:

$\Large f(x) = \frac{1}{\sigma\sqrt{2\pi}}exp\left[\frac{-(x - \mu)^2}{2\sigma^2}\right]$



```python

fig, ax = plt.subplots()

mu = 0
sigma = 1
z_curve = np.linspace(stats.norm(mu,sigma).ppf(0.01),
             stats.norm(mu,sigma).ppf(0.99), 100)
ax.plot(z_curve, stats.norm(mu,sigma).pdf(z_curve),
     'r-', lw=5, alpha=0.6, label='z_curve')
```




    [<matplotlib.lines.Line2D at 0x11d1b8390>]




![png](index_files/index_123_1.png)


![](images/normal_2.png)

# Quick Solo Challenge

Turn off you cameras, turn them back on when you solved the problem, or when 1 minutes is up.

suppose the average height of an American woman is 65 inches with a standard deviation of 3.5 inches. 
Use numpy's random.normal to generate a sample of 1000 women and plot the histogram of the sample.



```python
# Code here
```

# Standard Normal Distribution

A standard normal distribution has a mean of 0 and variance of 1. This is also known as a z distribution. 


![norm_to_z](images/norm_to_z.png)


```python
# Let's transform the normal distribtion centered on 5 with a standard deviation of 2 into a z curve
normal_dist = np.random.normal(5,2,1000)
z_dist = [(x - np.mean(normal_dist))/np.std(normal_dist) 
          for x in normal_dist]

fig, ax = plt.subplots()
sns.kdeplot(z_dist, ax=ax)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11c2d70f0>




![png](index_files/index_129_1.png)


![](images/empirical_rule.png)

## Empirical Rule
> The empirical or 68–95–99.7 states that 68% of the values of a normal distribution of data lie within 1 standard deviation of the mean, 95% within 2 stds, and 99.7 within three.  
> The empirical rule has countless applications in data science, which we will expand upon in the next few lectures.

By calculating the z-score of an individual point, we can see how unlikely a value is.

Consider, once again, the distribution of heights of American women, with a mean of 65 inches and a standard deviatio of 3.5 inches.

Calculate the zscore of a height of 75inches. 

Based on the empirical rule, if you were sampling heights of American women, speculate as to how improbable would that height be?


```python
# Your code here
```

# Pair Program

Z score can be used to eliminate outliers.

Based on the empirical rule, if our data is normally distributed, we can remove outliers based on the zscore. 

In the example below, we will consider anything outside of 3 std's away from the mean an outlier.

Z score outlier removal wouldn't work on our original divy bike ride time dataset, because the original set was heavily right skewed.  However, our log transformed data set was approximately normal.  Let's remove outliers based on the log transformed data.

In order to do so, you need to:

    1. remove records with zero values for ride_time
    2. log transform the ride_times
    3. use stats.zscore to calculate the z-score for each logtransformed time
    4. Use np.absolute to subset the dataframe to include values above or below 3 standard deviations from the mean 
    5. Plot the boxplot of the ride times with time in seconds (i.e. do not plot the log-transformed distribution.  The boxplot, therefore, will still show outliers)


```python
# Remove zero values

```


```python
# log transform ride times
```


```python
# use stats.zscore to calculate the z-score for each logtransformed time

```


```python
# 4. Use np.absolute to subset the dataframe to include values above or below 3 standard deviations from the mean 
```


```python
# 5. Plot the boxplot of the ride times with time in seconds (i.e. do not plot the log-transformed distribution.  The boxplot, therefore, will still show outliers)
    
```
