# Statistics Questions

## Levels
- 🟢 [Easy](https://github.com/PhilZambri/DataLemur-Interview-Questions/edit/main/Statistics.md#-easy)
- 🟠 [Medium](https://github.com/PhilZambri/DataLemur-Interview-Questions/edit/main/Statistics.md#-medium)
- 🔴 [Hard](https://github.com/PhilZambri/DataLemur-Interview-Questions/edit/main/Statistics.md#-hard)

***

## 🟢 Easy

### 📌 [Facebook | Easy | Coin Fairness Test](https://datalemur.com/questions/coin-fairness-test)

Say you flip a coin 10 times and observe only one heads. What would be your null hypothesis and p-value for testing whether the coin is fair or not?

Null Hypothesis $H_0$: The coin is fair where p of heads(success) = 0.5  
When we observe only one head we might guess that maybe this coin is biased towards tails right?  
so our Alt Hypothesis $H_1$: biased coin towards tails meaning p < 0.5				= one tailed test  

The p-value is the probability, under the null hypothesis, of observing a result as extreme or more extreme than 1 head:  
p-value = P(0 heads) + P(1head)

Binomial(n=10,p=0.5)  
p-value = P(X <= 1) =  

from scipy.stats import binom  
binom.cdf(1, 10, 0.5)	= 0.0107421875

Using a typical significance level of 𝛼 = 0.05,   
- We reject the null hypothesis.
- We conclude there is statistically significant evidence that the coin is biased toward tails.

## 🟠 Medium


## 🔴 Hard
