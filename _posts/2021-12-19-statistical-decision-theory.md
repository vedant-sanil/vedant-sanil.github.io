---
layout: post
mathjax: true
title:  "The Statistical Decision Theory"
date:   2021-12-19
desc: "An in-depth mathematical explanation and motivations of the Statistical Decion Theory"
keywords: "statistics,probability theory"
categories: [Science]
tags: [ML]
icon: icon-html
---

This blog is inspired by an excellent introduction to the Statistical Decision Theory presented in ([The Elements of Statistics Learning](https://hastie.su.domains/ElemStatLearn/)). I aim to provide a more detailed explanation of this theory with rigorous mathematical derivations to support the author's solutions. 

We begin by presenting a general framework for making predictions based on a real-valued input vector. Let $X \in \mathbb{R}^{p}$ be a real-valued random input vector (covariates), and $Y \in \mathbb{R}$ be the real-valued random output vector. The joint probability distribution for $X$ and $Y$ is given as $Pr(X,Y)$. We seek a function $f(X)$ that uses covariates $X$ for predicting an output that matches $Y$ as closely as possible. 

The theory requires a loss function $L(Y,f(X))$ for penalizing errors in prediction. For convenience, we will choose the *Squared Error Loss* as loss function, which is defined as, $L(Y, f(X)) = (Y-f(X))^{2}$. We define the *Expected Prediction Error* as the criterion for choosing $f$. 

$$EPE(f) = E(Y-f(X))^{2}$$
$$EPE(f) = \int (y-f(x))^{2}Pr(dx, dy)$$

We can convert the joint distribution above to a density function according to the property $P(X,Y \in \mathbb{R})=\int \int f_{XY}(x,y)\,dx\,dy$, where $f_{XY}$ is the density function. We now define $EPE$ as,

$$EPE(f)=\int \int (y-f(x))^{2}\,f_{XY}(x,y)\,dx\,dy$$

Using Bayes rule, $f_{XY}(x,y) = f_{Y|X}(y|x)f_{X}(x)$. We plug this into the formulation above as,

$$EPE(f) = \int \int (y-f(x))^{2}\,f_{Y|X}(y|x)\,f_{X}(x)\,dy\,dx$$
$$EPE(f) = \int \int f_{X}(x)\,[(y-f(x))^{2}\,f_{Y|X}\,dy]\,dx$$
$$EPE(f) = \int f_{X}(x) \bigg[\int (y-f(x))^{2}\,f_{Y|X}\,dy\bigg]\,dx$$

We can use the definitions of Expectations $E_{X}$ and $E_{Y|X}$ to simplify the equation above as,
$$EPE(f) = E_{X} \int (y-f(x))^{2}\,f_{Y|X}\,dy$$
$$EPE(f) = E_{X} E_{Y|X}([Y-f(X)]^{2}|X)$$

Note that for each point $x \in X$, when we condition on $X=x$ we observe that we only need to minimize $E_{Y|X}$. This is done as,

$$ EPE(f) = E_{Y|X}([Y-f(X)]^{2}|X=x)$$

We also note here that the modelling function $f$ can be any arbitrary function that can be formed to generate any output value. In the equation above, we therefore don't require $f(X)$ to be specified and instead can replace it with a constant $c$. The constant $c$ has a value that minimizes the $EPE$ pointwise. 

Later, we will observe that whatever optimal value we obtain for the constant $c$, we can select different modelling functions to obtain this optimal value. As an example I will include later in this post, in the context of designing a Linear Regressor as the modelling function, the function $f$ will look like $f(x) = x^{T}\beta$. It may already be apparent to readers the usefulness of substituting the predictions of the function with a constant representing the optimum value for minimizing $EPE$.

We now represent the problem as,

$$EPE(f) = argmin_{c}E_{Y|X}([Y-c]^{2}|X=x)$$

Using the linearity of expectations, we can represent this as,

$$EPE(f)=argmin_{c}(E_{Y|X}(Y^{2}|X=x) -2cE_{Y|X}(Y|X=x)+c^{2})$$

We differentiate w.r.t $c$ and set the right hand side to 0,

$$0 = -2E_{Y|X}(Y|X=x) +2c$$
$$c = E_{Y|X}(Y|X=x)$$

We obtain the solution to the Decision Theory as,

$$f(x) = E_{Y|X}(Y|X=x)$$

This tells us that the best prediction of $Y$ at any point $X=x$ is the conditional mean, given that the predictions are measured using average squared error.