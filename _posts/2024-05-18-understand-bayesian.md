---
title: "Easy and Intuitive Understanding of Bayesian Statistics - part 1: The Quantification of Beliefs"
# categories:
#   - stats
tags:
  - statistics
  - bayesian
  - intuitions
excerpt: "If you try to understand Bayesian Statistics, do not start with the Bayes theorem."
---

The common trap when trying to understand Bayesian Statistics is to start with the Bayes theorem.

I felt into this trap last year when I joint a project leveraging Bayesian Statistics to develop some advanced Experimentation features. While I was somehow comfortable with basic statistics, I had little knowledge or intuition about the Bayesian approach. My understanding and my ability to form critical opinions were, as a result, quite hindered. I spent some time after to self educate and had some good “gotcha” moments that I would like to share, in order to help other engineers like me to better succeed when working on Bayesian stuffs. 

In this note I will explain, in my words, the essential intuitions behind Bayesian statistics.

## It is about beliefs, and the quantification of beliefs.

Again, the common trap when trying to understand Bayesian Statistics is to start with the Bayes theorem. I find it much more helpful to start with the notion of “beliefs”.

**In everyday life and work, we each hold different beliefs, at various degrees**. When we say “It is likely to rain this afternoon”, we express our degree of belief about the weather to come. Beliefs can also be about things that already happened, such as “I think I left the key on the kitchen table, or maybe it is still in my bag”.

**Now, we can quantify the degree of beliefs.** The term “quantify” here can sound a bit strange, but anyone who did any sport betting actually did quantify their beliefs. “I bet 3 against 1 that France will win the next football game against Italy” means my belief about France’s victory is 3 times higher than otherwise. (If you are an Italian football fan, please switch the team names)

Again, the beliefs and the quantification can hold on things in the past or just general facts. “I bet 3$ against 1 that I already replied to your email!”. “I bet 100$ against 1 that whales can not breath under water”.

If we make a very dumb presentation of our degree of belief, we get the following graph. You can note that in this case, the beliefs are “discrete”. We can very well imagine beliefs that are continuous, such as tomorrow’s higher bound temperature.

![Understand Bayesian Statistics for Engineers part](/assets/images/understanding_bayesian/raw_degree_beliefs.png)

That’s it.

**That’s the first key intuition behind Bayesian Statistics: there are beliefs and the quantification of those beliefs.**

## In the “Frequentist” approach, you don’t have beliefs, just likelihoods

The statement above about Bayesian Statistics also depicts its fundamental difference with the Frequentist Statistics (also called Classical or Non-Bayesian Statistics). Frequentists don’t have the concept of “belief”, but only “likelihood”.

Likelihood is quite simple to understand. It represents this: **if we suppose some hypothesis to be true, then how likely are the observations we see?** For example, if we throw a coin 10 times and got 8 Heads and 2 Tails, we can then compute the Likelihood of these observations under particular hypothesis such as “the coin is fair”. 

It is not that Bayesians don’t have Likelihoods, as one term in the Bayes theorem is the Likelihood itself, but Bayesian Statistics compute and manipulate “Priors” and “Posteriors” that represent Beliefs.

Likelihoods are objective and depend totally on the observed data. Beliefs are not fully objective, which makes Bayesian Statistics both powerful but controversial.

## The probabilistic distribution of beliefs

If we return to our quantification of belief of “I bet 3$ against 1 that I already replied to your email” and normalize it so that the sum makes 1, we have a probabilistic distribution of beliefs, or “hypotheses”, as in the canonical nomenclature.

![Probabilistic distribution of beliefs](/assets/images/understanding_bayesian/probabilistic_distribution_beliefs.png)

This is the next important piece of intuition behind Bayesian Statistics: probabilities can be attributed to beliefs, aka hypotheses. This type of distribution has different interpretations than the classic probabilistic distribution of “events”.

Just like the probability of events, the probabilistic distribution of beliefs can be discrete or continuous. For example, weather forecasters must have a certain distribution of belief about the peak temperature of tomorrow which might let them say something like “we are 99% sure that the peak temperature will be between 17 and 18.5°C.”

Something we very usually want to hypothesize about is a “conversion rate” of an ad, of a mailing list, or more generally, the parameter of a binomial process. For this, the most basic way to go is the Beta binomial distribution, that we will observe next.

## Case in point: the Beta binomial distribution

The Beta binomial distribution is so important and popular that it is represented in the icon for the [Wikipedia’s series on Bayesian Statistics](https://en.wikipedia.org/wiki/Bayesian_statistics).

![Beta binomial representing Bayesian Stats in wikipedia](/assets/images/understanding_bayesian/beta_binomial_wiki.png)

Let’s say we have an ad that we want to estimate the click rate. We printed it 20 times, got 3 clicks and 17 non-clicks. The observed click rate is 0.15, but what can we say intuitively about the true click rate?

- Can it be between 0.12 and 0.18? Yeah, quite likely.
- Can it be between 0.01 and 0.07? Less plausible.
- Can it be above 0.4? Extremely unlikely, while nothing is impossible, in principle.

What we describe here corresponds to the distribution Beta(3, 17). I’ll leave the Math for later, but this distribution gives the following quantification of the terms “quite likely”, “less plausible” and “extremely unlikely” above.

![Beta binomial in 3 zones](/assets/images/understanding_bayesian/beta_binomial_3zones.png)

green: P(0.01 ≤ true click rate ≤ 0.07) = 0.148 (less plausible)\
red: P(0.12 ≤ true click rate ≤ 0.18) = 0.284 (quite likely)\
purple: P(0.4 ≤ true click rate) = 0.005 (extremely unlikely)

Now, let’s say we have printed the ad 100 times and got 15 clicks. Can we be more confident about the range of 0.12 and 0.18? Yes, we should, as we get more data in this direction. Here, we will draw the beta binomial with a = 15 and b = 85. We notice that the density around 0.15 is much much higher. As the total area under the curve is still 1, we get P(0.12 ≤ true click rate ≤ 0.18) = 0.577. Our degree of belief in this range of true value is now close to 60%.

![Beta binomial more samples](/assets/images/understanding_bayesian/beta_binomial_more_samples.png)

To better observe the dynamic when we change from 20 impressions to 100 impressions, we can plot the 2 distribution together, and get this:

![Beta binomial differences](/assets/images/understanding_bayesian/beta_binomial_difference.png)

We can make the following observations:

- As we get more data with the same click rate, the probabilistic distribution of beliefs get “narrower” around the observed rate.
- For the specific range of rate between 0.12 and 0.18, our degree of belief is around 2x stronger, with the additional amount of belief being the green area.

To go into details, let’s look at the formula for the Beta Binomial distribution:

$$
PDF(p,a,b)=\frac
{p^{a-1}\times(1-p)^{b-1}}
{beta(a,b)}
$$

As the PDF in Bayesian Statistics represents “degree of belief”, we see that the degree of belief on a given “true positive rate” $$p$$ is proportional to $$p^{a-1}\times(1-p)^{b-1}$$, which is exactly the likelihood of $$p$$ to explain the data that includes $$a$$ positive and $$b$$ negative samples.

---

To recap this first article:

- Beliefs and the degrees of beliefs are ubiquitous in our reasoning and communication. Bayesian Statistics is first and foremost about how to represent and quantify them.
- This leads to the concept of probabilistic distribution of belief, which has a different meaning than the probability of “events”.
- One of the most important probabilistic distribution in Bayesian Statistics is the Beta Binomial distribution, which quantifies the belief in ranges of value for a binomial process given a number of hits and misses.

In the eventual next article(s), I will describe how the Bayes theorem allows us to “update” our degrees of beliefs, explain the concept of “prior” that is both very powerful and controversial, and analyze the difference between the Bayesian and Frequentist approaches in Hypothesis Testing.

---

Thank you for your readership! Hope you find this useful.