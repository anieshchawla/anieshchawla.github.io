---
layout: post
title:  "Decision Trees"
date:   2017-11-14 00:15:12 -0500
categories: jekyll update
---

One of the most basic algorithm which is easier to understand yet very powerful is Decision Trees. There are two most popular algorithm associated with training the Decision Trees are :
1. C4.5, ID3
2. CART

### C4.5
This was developed by Quinlan and is extension of its earlier algorithm ID3. Consider Samples of data s1,s2, ....., sn. Each sample is k-length vector - which means that each sample is represted by k -columns or k variables.

At each node C4.5 chooses the best attribute on which it can be split. The splitting criterion is information gain, which means that whichever attribute gives the largest information gain after splitting, that would be chosen. First, we should what is gain.

$$Gain(S,A) = H(S) - \sum_{a\subset A} \frac{|a|}{|S|} * H(a)$$

where H is entropy, S is sample and A is the attribute chosen.
Entropy is given by :

$$H(P) = \sum_i-p_i*log_{2}(p_i)$$

Let's take an example to make it more clear. The table below shows, age, income and whether that person buys a certain thing or not.

$$
\begin{array}{|c|c|c|}
\hline
Age & Income & Buys \\ \hline
<30& high & yes \\ \hline
>30& medium & yes \\ \hline
>30& low & no \\ \hline
>30& high & no \\ \hline
<30& high & yes \\ \hline
<30& medium & yes \\ \hline
>30& high & no \\ \hline
<30& low & yes \\ \hline
<30& low & yes \\ \hline
 \hline
\end{array}
$$

1st we find the information stored in this table:

 $$ \begin{align}
 H[buys]& = -(probability of yes)* log_2(probability of yes) + -(probability of no)* log_2(probability of no) \\
& = -6/9 * log_2(6/9) - 3/9*log_2(3/9) \\
& = 0.91829
  \end {align}
  $$

Lets consider now that we split based on income. So we have three categories - low(3), medium(2) and high(4).

So Entropy associated with this attribute is as follows:
1. For high (2 yes and 2 no) : -2/4 log 2/4 -2/4 log 2/4 = 1
2. For medium (two yes) : -2/2 * log 2/2 = 0
3. For low (2 yes and 1 no) : -1/3 log (1/3) - 2/3 log(2/3) = 0.9182

Therefore, we can now find the gain as follows:

$$\begin{align}
Gain & = 0.91829 - (4/9 * (1) + 2/9 * 0 + 3/9 * 0.9182)\\
  & = 0.167
  \end {align}
 $$

 Similarly, the information gain for splitting on age attribute is = 0.5577. As we can see that the information gain is more in case of age, thus it will first choose to split via age attribute. Intuitively, this makes sense, if you see the table again, for age < 30 the person is buying the product.

### CART

This is similar to C4.5 but uses Gini index instead of entropy for gain. Gini index is defined as :

$$
Gini(X) = 1 - \sum_x p(x)^2
$$

And there Gain is defined in this case as :

$$
Gain(S,A) = Gini(S) - \sum_{a\subset A} \frac{|a|}{|S|} * Gini(a)
$$

### When Should we stop growing the Tree?
We should be vary of the depth of the decision tree that we are making. More depth generally means that we are overfitting the data. There are two basic ways to avoid overfitting in Decision Trees:
1. Pre - Pruning - as the name suggests, we start making decision trees only after making some initial decisions. It can have two major criteria :
  - Depth -  which means that we define the maximum depth till which we will allow tree to grow
    - Pre defined Conditions - which means, we will make certain conditions and can make our decision trees grow to certain depth till that condition is reached on the leaf node - Following are couple of examples of such pruning conditions:
      * if all attribute value on the leaf node are same
      * number of attributes in the leaf node are x (where x < total attributes provided initially)
      * attributes reaches certain threshold values
      * Chi-square feature is not statistically significant
2. Post - Pruning - This is pruning mechanism in a bottoms-up approach.

C4.5 and CART both uses different pruning mechanism to remove overfitting. C4.5 uses "Reduce Error Pruning" whereas CART uses "Cross validation for selecting Gini Threshold"

### Reduce Error Pruning
In this mechanism, We start pruning in an bottom's up approach in the following fashion:
Let T be subtree whose root node is v. We define gain from pruning as :

$$ Gain(pruning) = \#misclassification_T - misclassification_v $$

If this gain is +ve, it means that there were fewer misclassification when we started with and have more misclassification in the subtree T. Thus we will start removing those nodes which have Gain +ve and thereby keeping only -ve pruning gain nodes.

### Cross validation for selecting Gini Threshold

For cross validation technique you can look at [my previous post]({{ site.url }}//jekyll/update/2017/11/11/important-terms.html). This is pre-pruning mechanism with condition testing. In this method, for each validation test set, define a Gini threshold $$G_t$$ for the decision tree. This means that whenever the leaf node reaches Gini index at $$G_t$$, you should stop. The best $$G_t$$ is chosen from these validation sets and is finally used during testing.
