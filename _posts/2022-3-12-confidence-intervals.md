---
layout: post
mathjax: true
title:  "Confidence Intervals"
date:   2022-3-13
desc: "Understanding the intuition behind Confidence Intervals"
keywords: "statistics,probability theory"
categories: [Science]
tags: [ML, Statistics]
icon: icon-html
---


I begin this blog by giving a formal definition to Confidence Intervals, and then a detailed mathematical derivation to provide a more detailed insight into the significance of Confidence Intervals (CIs). 

## Formal Definition of Confidence Intervals
A definition of Confidence Intervals can be found in Larry Wasserman's [All of Statistics](https://link.springer.com/book/10.1007/978-0-387-21736-9), Ch. 6: Models, Statistical Inference and Learning,

A $1-\alpha$ **confidence interval** for a parameter $\theta$ is an interval $C_{n} = (a,b)$ where $a = a(X_{1}, ...., X_{n})$ and $b = b(X_{1}, ...., X_{n})$ are functions of the data such that, 
$$\mathbb{P}_{\theta}(\theta \in C_{n}) \geq 1 - \alpha, \; \forall \; \theta \in \Theta$$

In words, $(a,b)$ traps $\theta$ with a probability $1-\alpha$. We call $1-\alpha$ the **coverage** of the confidence interval.

By choosing an $\alpha$ of $0.05$, we say that the parameter $\theta$ lies within the confidence interval $C_{n}$ with a 95 percent probability.

**Note** $C_{n}$ is random and $\theta$ is fixed.

While the formal definition provides us with a basic understanding of Confidence Intervals, we are still left a little vague on certain aspects of this definition. For example, 
- Given a distribution of data $X_{1}, ....., X_{n}$, how are the bounds $a$ and $b$ for the CI defined? 
- What relation do these bounds have with $\alpha$ ? How does the choice of $\alpha$ define these bounds?
- How exactly is parameter $\theta$ trapped within $C_{n} = (a,b)$ ?
- How do we interpret a confidence interval? Clearly a CI cannot be a probability statement about $\theta$ since $\theta$ is a fixed quantity, not a random variable. 

We now explore a detailed derivation of the CI of $n$ random variables, each R.V. is modelled by the Bernoulli distribution. In the pursuit of this derivation, we will try cover all 4 points raised above. I will try to be as elaborate as possible with the derivation, and add explanations where necessary. 

## Confidence Intervals for Bernoulli Distributed Random Variables
Let $X_{1}, ...., X_{n} ~ Bernoulli(p)$ be $n$ Bernoulli distributed independent random variables parametrized by $p$. The parameter $p$ is also the expected value of this distribution. We can therefore ask the question, what is the maximum and minimum value that the average of the **observed** data deviates from the expected value $p$. In other words, can we find an upper and lower bound for the value of the observed average's deviation from the expected value? Let's represent this statement mathematically. For any $\epsilon > 0$, we quantify the magnitude of the observed average $ \bar{X} = n^{-1} \sum_{i=1}^{n}X_{i}$ from the expected value $p$ as,
$$\mathbb{P}(\vert \bar{X}-p \vert > \epsilon)$$

We attempt to rewrite this inequality in a different manner as shown below,
$$\mathbb{P}(\vert \bar{X}-p \vert >  \epsilon) = \mathbb{P}(\vert \frac{1}{n} \sum_{i=1}^{n}X_{i} - p \vert > \epsilon)$$
$$\mathbb{P}( \vert \bar{X}-p \vert > \epsilon) = \mathbb{P}( \vert \sum_{i=1}^{n}X_{i} - np \vert > n \epsilon)$$

Coincidentally, we observe that $np$ can be written as $\mathbb{E}[\sum_{i=1}^{n}X_{i}]$. This is because for $n$ independent Bernoulli trials, 
$$\mathbb{E}[\sum_{i=1}^{n}X_{i}] = \sum_{i=1}^{n}\mathbb{E}[X_{i}] = \sum_{i=1}^{n}p = np$$

Therefore, we can now rewrite the inequality as,
$$\mathbb{P}( \vert \bar{X}-p \vert > \epsilon) = \mathbb{P}( \vert \sum_{i=1}^{n}X_{i} - \mathbb{E}[\sum_{i=1}^{n}X_{i}] \vert > n \epsilon) \longrightarrow \textcircled{1}$$

We can now use Hoeffding's inequality to create an upper bound for this inequality. I'll briefly introduce Hoeffding's Inequality here, but you can refer to the [Wikipedia page](https://en.wikipedia.org/wiki/Hoeffding%27s_inequality) for a more detailed definition, along with a proof. Do it give it a read, it's quite interesting! 

For a sum of independent random variables $X_{1}, ...., X_{n}$, where $a_{i} \le X_{i} \le b_{i}$ Hoeffding's inequality states that,
$$\mathbb{P}( \vert \sum_{i=1}^{n}X_{i} - \mathbb{E}[\sum_{i=1}^{n}X_{i}] \vert > \epsilon) < 2*exp \left( \frac{-2 t^{2}}{\sum_{i=1}^{n}(b_{i} - a_{i})^{2}} \right)$$