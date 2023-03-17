# IMDB-movie-reviews
In this notebook I have focused on overfitting, demonstrating the phenomenon and studying techniques to address it.
The dataset we shall use is the IMDB movie reviews dataset, composed of 25,000 movies reviews, labeled by sentiment (positive/negative).

To prevent overfitting, the best solution is to use more training data. When that is not a viable possibility, you can try to use regularization techniques, constraining the quantity and quality of information stored by the model. If a network can only afford to memorize a small number of patterns, the optimization process will force it to focus on the most prominent ones, which have a better chance of generalizing well.

In keras, the dataset is preprocessed, and each review is encoded as a sequence of word indexes (integers). For convenience, words are indexed by overall frequency in the dataset, so that for instance the integer "3" encodes the 3rd most frequent word in the data.

The representation has a variable length dimension, that is not very stuitable for a neural network.

Now it is transformed it into a multi_hot encoding of of dimension equal to num_words. In this representation, a word gets index 1 if it appears in the document. It is essentially a bag-of-words encoding.

Looking at the initial part of the encoding for the first review.

Defining our first model, that is just a concatenation of three dense layers.

Compiling the model using adam as optimizer, and binary crossentropy (log likelyhood) as loss function. The fit function returns a history of the training, that can be later inspected. In addition to the loss function, that is the canonical metric used for training, we also ask the model to keep trace of accuracy.

Our base model is modified adding regularizers.

A common way to mitigate overfitting is to reduce the complexity of the network by forcing its weights to only take small values, making the distribution of weights more “regular”. This is called “weight regularization”, and it is done by adding to the loss function of the network an additional cost associated with having large weights.

Dropout is an alternativeregularization techniques for neural networks. It consists of randomly “dropping out” (i.e. set to zero) a number of output features of the layer during training.

At test time, no units are dropped out, but the layer’s output values are scaled down by a factor equal to the dropout rate, so as to balance for the fact that more units are active than at training time.

Added a couple of dropout layers in our IMDB network and see how it performs.

Early stopping can be simply implemented in keras using callbacks. A callback is a function taht is called at specific stages of the training procedure: start/end of epochs, start end of minibatches, etc.

You can use callbacks to get a view on internal states and statistics of the model during training. A list of callbacks can be passed to the .fit() function using the keyword argument "callbacks".
