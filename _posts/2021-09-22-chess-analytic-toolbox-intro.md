---
layout: post
title: "The Beginings of a Chess Game Analytics Project"
author: Nathaniel S. Keplinger
categories: blog
date: 2021-09-22
---

Those who have known me for a while would know that I am an avid chess player. Throughout my undergrad, I've slowly built my chess skills and opening repertoire. Playing white, I have taken to 1 e4 to enter the dynamic and combative Vienna Gambit or System. As black, the carro-kann has been my weapon of choice. These openings are my opening of choice because, intuitively, I have found the most success with their playstyle and approach.

The problem is that as of late, my rating on lichess.com has plateaued at around 2020 ELO. Perhaps it is due to general nature reaching this level where opening tricks and traps might be becoming less effective. Or rather, further improvement requires some serious study and practice that I have neither the time nor motivation at the moment to dedicate myself toward. Either way, I see my chess rating plateau as an opportunity to develop statistical analysis tools to gain insight into my "chess behaviors."

Thus, I am beginning a project that will look at all of the chess games I have ever played on lichess.com. This project aims to clean all data and metadata I can extract from each game and detect trends in my gameplay and performance. The current plan is to use the lichess.com API to export all of my games and related metadata. The initial analysis and statistical framework I plan to develop would closely emulate the "chess insights" page on lichess. This chess insights page provides analytics for each of you where you can filter your chess data to answer questions like "How often do I punish blunders made by my opponent during each game phase?" or "When I trade queens, how do games end?". I would like to build a similar program in python to answer these same questions.

In the latter stages of this project, I would like to extend this statistical toolbox to implement some machine learning algorithms on all the data related to my chess game. Currently, I am not sure how these predictive models will manifest themselves. Still, there are interesting questions that could be answered by applying unsupervised learning algorithms to my gameplay data.

Maybe I'll be able to predict the likelihood of me winning a game, given some openings, opponent rating, and time of day! We shall see.

In the meantime, follow me on Lichess -- [MeaningfulMonkey](https://lichess.org/@/MeaningfulMonkey)
