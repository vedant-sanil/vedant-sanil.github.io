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

<br/><br/>

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

<br/><br/>

## Confidence Intervals for Bernoulli Distributed Random Variables
Let $X_{1}, ...., X_{n} ~ Bernoulli(p)$ be $n$ Bernoulli distributed independent random variables parametrized by $p$. The parameter $p$ is also the expected value of this distribution. We can therefore ask the question, what is the maximum and minimum value that the average of the **observed** data deviates from the expected value $p$. In other words, can we find an upper and lower bound for the value of the observed average's deviation from the expected value? Let's represent this statement mathematically. For any $\epsilon > 0$, we quantify the magnitude of the observed average $ \bar{X} = n^{-1} \sum_{i=1}^{n}X_{i}$ from the expected value $p$ as,

$$\mathbb{P}(\vert \bar{X}-p \vert > \epsilon)$$

We attempt to rewrite this inequality in a different manner as shown below,

$$\mathbb{P}(\vert \bar{X}-p \vert >  \epsilon) = \mathbb{P}(\vert \frac{1}{n} \sum_{i=1}^{n}X_{i} - p \vert > \epsilon)$$

$$\mathbb{P}( \vert \bar{X}-p \vert > \epsilon) = \mathbb{P}( \vert \sum_{i=1}^{n}X_{i} - np \vert > n \epsilon)$$

Coincidentally, we observe that $np$ can be written as $\mathbb{E}[\sum_{i=1}^{n}X_{i}]$. This is because for $n$ independent Bernoulli trials, 

$$\mathbb{E}[\sum_{i=1}^{n}X_{i}] = \sum_{i=1}^{n}\mathbb{E}[X_{i}] = \sum_{i=1}^{n}p = np$$

Therefore, we can now rewrite the inequality as,

$$\mathbb{P}( \vert \bar{X}-p \vert > \epsilon) = \mathbb{P}\left( \bigg\vert \sum_{i=1}^{n}X_{i} - \mathbb{E}[\sum_{i=1}^{n}X_{i}] \bigg\vert > n \epsilon\right) \longrightarrow (1)$$

We can now use Hoeffding's inequality to create an upper bound for this inequality. I'll briefly introduce Hoeffding's Inequality here, but you can refer to the [Wikipedia page](https://en.wikipedia.org/wiki/Hoeffding%27s_inequality) for a more detailed definition, along with a proof. Do it give it a read, it's quite interesting! 

### Hoeffding's Inequality
For a sum of independent random variables $X_{1}, ...., X_{n}$, where $a_{i} \le X_{i} \le b_{i}$ Hoeffding's inequality states that,

$$\mathbb{P}\left( \bigg\vert \sum_{i=1}^{n}X_{i} - \mathbb{E}[\sum_{i=1}^{n}X_{i}] \bigg\vert > \epsilon\right) < 2*exp \left( \frac{-2 t^{2}}{\sum_{i=1}^{n}(b_{i} - a_{i})^{2}} \right)$$

where $t > 0$ is a constant.

We now apply Hoeffding's Inequality to our problem. You can already see our reasoning for using this inequality to set upper bounds for equation $(1)$ based on the definition above. Applying Hoeffding's inequality to the RHS of equation $(1)$,

$$\mathbb{P}\left( \bigg\vert \sum_{i=1}^{n}X_{i} - \mathbb{E}[\sum_{i=1}^{n}X_{i}] \bigg\vert > n \epsilon\right) < 2*exp \left( \frac{-2 n^{2}\epsilon^{2}}{\sum_{i=1}^{n}(b_{i} - a_{i})^{2}} \right) \longrightarrow (2)$$

We also observe that for any Bernoulli random variable, the r.v. takes only two values, $1$ and $0$, since a Bernoulli distribution is defined as, $\mathbb{P}(X_{i}=1)=p$ and $\mathbb{P}(X_{i}=0)=(1-p)$. In the context of Hoeffding's Inequality, the bounds for each r.v. is, 

$$a_{i} \le X_{i} \le b_{i} \implies a_{i}=0, b_{i}=1, \: \forall \: i \in [0, N)$$

We can now substitue the values for $a_{i}$ and $b_{i}$ in equation $(2)$, 

$$\mathbb{P}\left( \bigg\vert \sum_{i=1}^{n}X_{i} - \mathbb{E}[\sum_{i=1}^{n}X_{i}] \bigg\vert > n \epsilon\right) < 2*exp \left( \frac{-2 n^{2}\epsilon^{2}}{n} \right)$$

$$\mathbb{P}\left( \bigg\vert \sum_{i=1}^{n}X_{i} - \mathbb{E}[\sum_{i=1}^{n}X_{i}] \bigg\vert > n \epsilon\right) < 2*exp \left( -2 n \epsilon^{2} \right)$$

To recap our derivations until this point, we now understand that for a certain Bernoulli distribution $X_{1}, ...., X_{n}$, the probability that the observed average of this distribution of random variables deviates from their expected value $p$ by an amount $\epsilon$ is bounded by,

$$\mathbb{P}( \vert \bar{X}-p \vert > \epsilon) = \mathbb{P}\left( \bigg\vert \sum_{i=1}^{n}X_{i} - \mathbb{E}[\sum_{i=1}^{n}X_{i}] \bigg\vert > n \epsilon\right) < 2*exp \left( -2 n \epsilon^{2} \right)$$

$$ \mathbb{P}( \vert \bar{X}-p \vert > \epsilon) < 2*exp \left( -2 n \epsilon^{2} \right) \longrightarrow (3)$$

Consider the expression $\vert \bar{X_{n}} - p \vert > \epsilon$, from which we can derive,

$$ \implies \bar{X_{n}} - p > \epsilon, \bar{X_{n}} - p < -\epsilon$$
$$ \implies \bar{X_{n}} - \epsilon > p, \bar{X_{n}} + \epsilon < p$$

Pay attention to this derivation. With the second implication, we can define a range for $p$ such that it lies outside the interval $(\bar{X_{n}} - \epsilon, \bar{X_{n}} + \epsilon)$. We now formally define this interval as,

$$C_{n} = (\bar{X_{n}} - \epsilon, \bar{X_{n}} + \epsilon) \longrightarrow (4)$$

With the definition of $C_{n}$ defined in equation ${4}$, we can express the Left Hand Side of equation ${3}$ using our newly derived range for $p$ as,

$$\mathbb{P}( \vert \bar{X}-p \vert > \epsilon) = \mathbb{P}( \bar{X_{n}} - \epsilon > p, \bar{X_{n}} + \epsilon < p) = \mathbb{P}(p \notin C_{n}) $$

and, 

$$\mathbb{P}( \vert \bar{X}-p \vert > \epsilon) \leq 2*exp \left( -2 n \epsilon^{2} \right)$$

$$ \implies \mathbb{P}(p \notin C_{n}) \leq 2*exp \left( -2 n \epsilon^{2} \right) $$

Let $\alpha = 2*exp \left( -2 n \epsilon^{2} \right)$, we can now write the equation above as, 

$$ \mathbb{P}(p \notin C_{n}) \leq \alpha$$

$$ \implies \mathbb{P}(p \notin C_{n}) \geq 1-\alpha$$

We are finally ready to bridge the definition of a confidence interval with the parameter $p$. From the equation above, we can state that the random interval $C_{n}$ becomes a $(1-\alpha)$ confidence interval $C_{n}$ when it traps the value of parameter $p$ with the interval $C_{}n$ with a probability greater than $(1-\alpha)$. 

### Bounding Values of Confidence Intervals

For our example, it is very easy to compute the bounding value $\epsilon$ such that it traps the parameter $p$ within a $C_{n}$ with a probability $(1-\alpha)$. This is done using our defination of $\alpha$, 

$$ \alpha = 2*exp \left( -2 n \epsilon^{2} \right)$$

$$ \implies exp \left( -2 n \epsilon^{2} \right) = \frac{\alpha}{2}$$

$$ \implies exp \left( 2 n \epsilon^{2} \right) = \frac{2}{\alpha}$$

$$ \implies 2 n \epsilon^{2} = log \left( \frac{2}{\alpha} \right)$$

$$ \implies \epsilon = \sqrt{\frac{1}{2n} log \left( \frac{2}{\alpha} \right)}$$

Therefore, the confidence interval $C_{n}$ can be expressed as,

$$ C_{n} = (\bar{X_{n}} - \epsilon, \bar{X_{n}} + \epsilon) = \left(\bar{X_{n}} - \sqrt{\frac{1}{2n} log \left( \frac{2}{\alpha} \right)}, \bar{X_n} + \sqrt{\frac{1}{2n} log \left( \frac{2}{\alpha} \right)} \right)$$

The value of $\alpha$, as mentioned previously, is the user's choice. A common choice is setting $\alpha$ to $0.05$, allowing for a confidence of $95$ percent that our selected confidence interval traps the parameter $p$. 

We now have a clearer understanding of confidence intervals. A confidence interval is not a probability distribution over the parameter $\theta$, which may appear intuitive when we first look at the definition of CIs, nor is it a fixed range of values that $\theta$ can take. Rather, it is more intuitive to define CIs as an interval that bounds the value of $\theta$ with a certain probability $(1-\alpha)$, hence giving it the name $(1-\alpha)$ Confidence Interval. 