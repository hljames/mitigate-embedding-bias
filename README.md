### Compare Methods for Mitigating Bias in Word Embeddings

**Method 1**

Bolukbasi debiasing -- mathematically manipulating embedding to maintain a geometric definition of unbiased

**Method 2**

Retraining embedding to maintain a probabilistic definition of unbiased

EX: P(he|doctor) = P(she|doctor)

**Method 3**

Retraining vectors to maintain a clustering definition of bias (see Goldberg paper) -- can be combined with either method 1 or method 2.
