---
layout: post
title:  "Implementation of a Custom Multidimensional Array in C++"
date:   2020-12-06
desc: "Creating a custom multidimensional array in C++ emulating a NumPy array in Python"
keywords: "array,c++,python,numpy,multidimensional,variadic"
categories: [C++]
tags: [C++,Python]
icon: icon-html
---

I have recently been really interested in learning C++, and the learning experience has brought forth a lot of fundamental programming challenges to me that you usually skirt over when you invest a majority of your programming time coding in high level of languages like Python and Java. With both these languages, a relatively large number of abstractions exist that don't expose us to the ugly pit fires of low level code formative of C/C++ (looking at you, pointers).

To short circuit my C++ learning, I decided to undertake a relatively intensive project in C++. Since I have worked rather extensively within the circles of Scientific Computing, I figured it would be a worthy goal as any to begin emulating the popular NumPy library in C++. Any engineer working with scientific programming using NumPy can attest to the sheer power the library offers, both via functionality, and performance (NumPy achieves this through SIMD instructions for array operations). Emulating this in C++ is a very non-trivial task, but the consequent learning and utilites would be an excellent goal to pursue.

