## Compare Methods for Mitigating Bias in Word Embeddings

**Geometric Debiasing**

Geometric debiasing, as developed by Bolukbasi et all in "Man is to Computer Programmer as Woman is to Homemaker? Debiasing Word Embeddings" consists of geometrically manipulating the embedding to maintain a geometric definition of unbiased.

The model debiased for comparison using this method were created using the authors' repository:
https://github.com/tolga-b/debiaswe

**Probabilistic Debiasing*

This method consists of enforcing a probabilitistic definition of unbiased. For examples, P(he|doctor) = P(she|doctor). These probabilities are constructed using the definition of log conditional probability in Mikolov et al's "Distributed Representations of Words and Phrases and their Compositionality." Rather than geometrically manipulating the vectors, a shallow neural network is used with a loss function being the difference between conditional probabilities of neutral words.

**Cluster Debiasing**

This method of debiasing aims to mitigate a type of bias suggested in Goldberg et al's "Lipstick on a Pig:
Debiasing Methods Cover up Systematic Gender Biases in Word Embeddings But do not Remove Them." They suggest that one method of measuring bias: "the percentage of male/female socially-biased words among the k nearest neighbors of the target word." Similar to probabilisitc debiasing, this method uses a shallow neural network to adjust the vectors, where the loss function is the difference between the distance to the k/2 male socially-biased words and the the distance to the k/2 male socially-biased words, as if these distances are similar, the percentage will settle around 50%. 

## Results


