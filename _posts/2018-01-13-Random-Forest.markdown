---
layout: post
title:  "Random Forest"
date:   2018-01-13 00:15:12 -0500
categories: jekyll update
mathjax: true
---

This algorithm belong the category of **supervised learning** algorithms. This in a very crude way be considered as ensemble classifier for the Decision Trees. Just like Decision Trees, this algorithm can be used both for classification as well as regression. Please read about [Decision Trees]({{ site.url}}//jekyll/update/2017/11/14/decision-trees.html) before we get on to Random Forest.

### Random Forest
As the name suggests, this is a forest i.e. group of many decision trees. The `Random` in  Random Forest is the random selection of parameters at each node, we will see in a while what does it mean. Random Forest is a very powerful algorithm. When I was working in American Express in fraud analytics team, we shifted our focus from building traditional Linear Regression model to building models using this algorithm. We also used this  for fraud classification and building rules.

### Algorithm
The algorithm is very simple. But first we will assume the following:
* The depth of decision tree used in Random Forest is a hyper parameter.
* Each data point has k parameters in total
* There are N Decision Trees that we expect in this algorithm. This is also a hyper parameter.

The algorithm is as follows:
1. Select m parameters out of k parameters, where m << k.
2. Select the best parameter out of these `m` to get the best split at each node. The best split selection is in the same way as in [Decision Trees]({{ site.url}}//jekyll/update/2017/11/14/decision-trees.html).
3. Continue the process from 1-2 till you reach the depth of decision tree.
4. Do 1-3 for each Decision Tree in Random Forest.
5. Select the most frequent outcome of all the Trees.

## Other Benefits
After the algorithm is complete. You can also rank order the number of parameter which appeared during the split based on the times they are used for finding the best split. When I was working in fraud analytics team in American Express, we often used the parameters that came in rank order to create rules for finding frauds. We directly couldn't use the results from Random Forest because we had to justify the parameters used for creating rule not only to the senior leaders but also to the auditing firms.
