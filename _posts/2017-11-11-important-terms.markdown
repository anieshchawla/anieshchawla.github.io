---
layout: post
title:  "Starting Terms ML"
date:   2017-11-11 00:08:12 -0500
categories: jekyll update
---
We will start with some very common terms that have find it use in ML

### Loss functions :
This is finally what you would expect to be low. There are many loss functions that are used:

  - L1 Loss - This is basically absolute value of difference of input and output (I have never used) :

  $$Loss = \sum_{i=0}^n |y_i - f(x_i)|$$

  - L2 Loss - This is square of difference of input and output (used extensively) :

  $$Loss = \sum_{i=0}^n (y_i - f(x_i))^2$$

  - Softmax loss - This is loss that comes from softmax classifiers (will discuss it in next post)
  basic equation for this is given by following equation. If $$y_i$$ is the expected output for the given input.

  $$Loss = -\log(\frac{\exp(y_i)}{\sum_{k=0}^n \exp(y_k)}) $$


  {%highlight python %}
  # Code For Softmax Loss function calculation - used very frequently in Deep Neural Networks
  # Training Inputs:
   # W: A numpy array of shape (D, C) containing weights.
   # X: A numpy array of shape (N, D) containing a minibatch of data.
   # y: A numpy array of shape (N,) containing training labels; y[i] = c means
   # that X[i] has label c, where 0 <= c < C.
   for i in range(num_train):
      scores = X[i].dot(W)
      # doing the following to maintain numerical stability
      scores-=np.max(scores)

      scores = np.exp(scores)
      expscore = np.sum(scores)
      scores/=expscore
      loss-=np.log(scores[y[i]])
   #=> Please note this is not the final loss output - have to take mean and take into account regularisation
   #=> Gives non-vectorized loss for softmax classifier
   {% endhighlight %}

Since we are using Numpy, we should do this in much smarter way i.e. vectorized solution

      {%highlight python %}
      #Inputs are same as above
      scores = X.dot(W)
      # following is done to maintain the numerical stability by subtracting the max values
      scores = (scores.T - scores[np.arange(len(scores)),scores.argmax(axis=1)]).transpose()
      scores = np.exp(scores)
      expval = scores.sum(axis=1)
      scores = (scores.T/expval).transpose()
      #scores[np.arange(len(scores)),y] - this is basically taking the values only belonging to classifier
      loss = np.log(scores[np.arange(len(scores)),y])
      #=> Gives vectorized loss for softmax
      #=> Please note this is not the final loss output - have to take into account regularisation
      {% endhighlight %}


### Hyper parameters:

The parameters that are not learned by during the training process but are required to make your model
learn and converge. Some of the common hyper parameters are :
1. Learning rate - This tell how fast do you want your model to train. A high value can explode your data
very quickly and a low value will lead to very slow convergence. To select the right value, a number of
iterations are required. A good value to start with is 1e-3. A snippet of how to use learning rate:
  ```python
    #self.W is weight matrix
    #grad is gradient for iteration - will talk more about it on later posts
    self.W -=  (learning_rate*grad)
  ```
2. Regularisation Strength - Regularisation is responsible to keep your model simple and not to become complex. Since we are dealing with non-linear functions, we need to keep the keep the model in check to not to become super
complex. Generally we use L2 regularization
$$\sum_i\sum_k W_iW_k$$. A good value to start with for regularisation strength is 1e-5. Code snippet of its use :
```python
  #W is weight matrix obtained during this iteration
  #dW - gradient of the weight matrix
  #loss - loss seens during this iteration
  # reg - regularisation strength
  loss += 0.5*reg*np.sum(W*W)
  dW += (reg*W)
  #=> please note regularisation here is W*W and not reg. reg is the hyper parameter
```
3. Number of Iterations - this is also hyper parameter as it depends on how long do you want to run your model.
4. Many more based on the model you are using for eg. KNN has K as hyper parameter, etc.


### Cross Validation
This technique is used to remove overfitting in the learning algorithm. The training dataset is randomly partitioned in k-folds. One fold is chosen at random and other data is use for training. The algorithm is made to learn on the randomly chosen dataset and is validated on validation data set. This is performed k times.

There are two ways to go after cross validation:
1. You can either select average of all the output of the `k` trials
2. You can penalise those models which tries to overfit (better result on training dataset and bad result on validation dataset)

### Performance
Performance of any machine learning algorithm generally uses terms like `confusion matrix`, `precision`, `recall`, etc. We will understand their meaning by taking a simple example. Lets suppose our algorithm predicts whether a particular credit card transaction is fraud or not. We ran our algorithm on 20 transactions, out of which it predicted 4 being as fraud and 16 as not fraud. Out of the 4 transactions predicted, only 3 were actually fraud. We know what there were total of 6 fraud transactions and 14 regular transactions.

$$
\begin{array}{|c|c|c|}
\hline
 n=& Predicted & Predicted \\
 20& Fraud(4) & \text{Not Fraud(16)} \\ \hline
\text{Actual Fraud(6)}& 3 & 3 \\ \hline
\text{Actual Not Fraud(14)} & 1 & 13 \\ \hline
 \hline
\end{array}
$$

Now we will define our those terms:
* **Confusion Matrix** : The matrix shown above is the confusion matrix. This is the main performance matrix for classification algorithms.
* **Precision**: As the name suggests, it tells how precise is the algorithm. In our case it is ratio of (actual fraud in predicted)/ (total fraud predicted) i.e 3/4. In technical terms it is ratio of true positives to total predicted. That is why Precision is also called positive predictive value.
* **Recall**: This term tell us about the sensitivity of prediction. It tell us about how much was the algorithm actually able to predict properly. In our case it is ratio of (actual fraud in predicted)/(total fraud in transactions) i.e. 3/6 = 1/2. Technically, it is ratio of true positives to all positive
* **True Positive Rate**: When it is fraud, how often does the algorithm predict a transaction to be fraud. In our example, it is 3/6.
* **False Negative Rate**: When it is not fraud, how often does the algorithm predict a transaction to be fraud. In our example, it is 1/13.
* **Accuracy**: This tells us, how accurate our algorithm is. In our case it is (3+13)/20 = 80%.
* **Misclassification Rate**: This is opposite of Accuracy i.e. missclass_rate = 1 - accuracy = 20%.
* **Prevalance**: It tells us the whether our data is sparse or not for classification. In our example it is percentage of transaction that are fraud in dataset i.e. 6/20 = 30%.




#### Common commands in Jekyll for Mac:
1. bundle exec jekyll serve
2. open $(bundle show minima) -  if your theme is minima -  check config.yml file for it.


<!-- You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight python %}
def print_hi(name)
  print ("Hi", name)

print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
