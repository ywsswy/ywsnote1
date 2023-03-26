It takes all the inputs as a big vector,that will denote X,and multiplies them with a matrix to generate its predictions,one per output class.
Throughout,we'll denote the inputs by X,the weights by W,and the biased by B.
Here is an example solution.We take the exponential of the scores and we divide by the sum of the exponential of the scores across the other categories.
Notice that the probabilities do sum to one.

