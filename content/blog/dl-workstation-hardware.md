+++
date = 2023-12-29T22:00:00Z
lastmod = 2023-12-29T22:00:00Z
author = "default"
title = "Building a Deep Learning Workstation"
subtitle = "First part: motivation and hardware choice."
tags = ["gpu", "rtx 4090"]
categories = ["deep learning", "hardware"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
draft = true
+++

## Introduction and Motivation

I have wanted to build a custom PC for a while. One of my first experiences with computers comes from the early 2000's when I was tinkering with my buddy in his garage to upgrade components ourselves to save some bugs. During university and my PhD, I had little incentive to do such a thing as I just bought a Macbook, which offers limited potential for customization. In the last two years though, I have more and more been playing with the idea to go back to old habits and assemble a workstation for deep learning experiments, CUDA programming, and continuous integration tests. 

While I have access to a cluster and a powerful workstation in the office, I would not like to use this hardware for personal projects. Vis-à-vis deep learning projects, I understand the potential of cloud solutions but also find them quite costly (especially GPU instances) and sometimes cumbersome to use. AWS, Azure, and GCP are non-trivial for a quick experiment and Google Colab comes with all the positives and negatives of a notebook environment plus timeouts and cumbersome data transfer. Running CUDA code in the cloud is of course possible but local development within an IDE like Visual Studio or CLion is prefered. Lastly, I have some cross-platform programming projects using C++ and Swift, which I would like to test on all major operating systems and compilers. Doing so with Github Actions is quite a pleasant experience. However, even with a paid Github membership, compiling C++ on multiple platforms quickly eats up the free compute budget. I also got a picture of my friend's new deep learning machine in the beginning of December, which made very excited to work on such a project myself. So between this year's Christmas and New Year's Eve, I decided to go ahead and start planning a new build.

## Research for the Hardware

### GPU

The most important piece of hardware 

### CPU