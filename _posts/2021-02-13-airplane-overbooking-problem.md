---
layout: post
mathjax: true
comments: true
title:  "Airplane Overbooking Problem"
date:   2021-02-13
desc: "A Probabilstic View of the Airplane Overbooking Problem"
keywords: "statistics,poisson,python,binomial,probability,airplane"
categories: [Machine Learning]
tags: [ML,Python]
icon: icon-html
---

I recently had the opportunity to solve an interesting optimization problem, the Airplane Overbooking Problem. I'll aim to use this this blog to introduce the problem, the stastical analysis used to solve it and a pythonic implementation of my methodology. Hopefully, the blog also presents an interesting insight into scientific programming and how standard stastical thinking translates itself into code. 

The airplane overbooking problem presents itself as an optimization problem where a typical airline company must maximize its revenue by considering the number of seats available on a standard aircraft and the number of tickets offered to potential customers. Assuming our aircraft accomodates 180 passengers ([Airbus A320](https://www.airbus.com/aircraft/passenger-aircraft/a320-family/a320ceo.html)), we assume that on a typical day tickets are priced at \$200. It is obvious to observe that in an ideal scenario, an airline company would like to offer 180 seats, and 180 passengers board the flight. This ideal scenario indicates our theoretical limit on the revenue the company can make for a given ticket price, in this case being   \$200x180, \$36000. However, in reality, there are a reasonable amount of no-shows, people who cancel the tickets, delays due to preceding connecting flights, and in some cases, an outright lower demand for tickets. 

In such cases, offering 180 tickets may not be optimal factoring lost revenue due to empty seats. Considering this, airlines often overbook flights, i.e. offer an additional "x" number of seats. This allows them to give those extra seats to the extra passangers who show up, thus making up for any potentially lost revenue. The trouble happens when there are there are no no-shows, and suddenly airlines have to compensate heavily for inconvenience caused to the extra passengers who show up, including hotel, transporation and re-booking charges. Altogether, these prices may run all the way up to \$700-1000. With enough number of compensations, the overall revenue begins taking a hit. 

Considering all the factors laid above, this problem becomes an interesting optimization problem, where we optimize for the number of overbooked seats in a manner as to maximize revenue. As such, we make certain assumptions before we jump into the mathematics. 
- It can be assumed that tickets are purchased independently from each other, and similarly the chance of a passenger showing up/not showing up is independent from another passenger.
- Should a passenger not show up of their own volition, we shall assume the \$200 ticket price shall not be refunded to them.

Starting of with modelling a basic structure for passengers that board the flight given the airline offering a certain number of tickets N, we can use the binomial distribution. Binomial Distribution is useful to model events which model an even similar to a coin toss, i.e. an event with a probability ***p*** of occuring, and a probability ***1-p*** of not occuring. In this case, that ***p*** is represented by a passenger showing up to board the flight. Data shows that percentage being around 95%, hence we set our ***p*** as 0.95.

Binomial distributions are represented as,

$$ P(N=n) =  {180 + overbook\choose n}p^n(1-p)^(180+overbook-n)$$

***180 + overbook*** represents the total number of tickets offered by the airline company, defined as the original 180 seats and a variable *overbook* to account for the number of extra seats offered by the airline. ***N*** represents the number of customers who show up at the ticketing counter. Here, binomial theorem allows us to optimize for the number of overbooked seats. Given the probabilistic aspect of running this experiment once, we convert our experiment into a Monte Carlo experimental setting to allow for more deterministic results.

