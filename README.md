## Compare Methods for Mitigating Bias in Word Embeddings

Relying on a single definition of “unbiased” may not be sufficient for mitigating bias in word embeddings – we present a novel method of mitigating bias that builds combines multiple methods of mitigating bias, each grounded in a separate definition of bias. In 2016, Bolukbasi et al published “Man is to Computer Programmer as Woman is to Homemaker? Debiasing Word Embeddings,” which demonstrated that word embeddings, even when constructed from large, supposedly unbiased sources, encode negative societal stereotypes, including gender bias. The authors propose a method for “debiasing” the word embeddings, based on a geometric definition of bias. The results of the method do mitigate bias in the embedding according to this geometric definition, but as noted in Honen and Golderg’s paper, does not eliminate bias on other axes or according to alternative definitions. In the same paper, the authors suggests another definition of bias, one that relies on the composition of the neighborhood of a target word. Another commonly used definition of unbiased is a probabilistic definition of bias, wherein an outcome cannot change conditional on flipping a protected class. In word embeddings this could mean P(doctor|he) = P(doctor|she). We construct methods of mitigating bias based on these two more recent definitions. We measure the results using the established WEAT statistic to quantify gender bias as well as a new metric based on the neighborhood definition of bias. We compare these three methods of mitigating bias: geometric, probabilistic, and k-nearest neighbors as well as their combinations. We demonstrate that effective mitigation of bias cannot be achieved by relying only on one methods alone, but that a combination of these methods proves most effective, and note that multiple definitions of bias can be additive rather than competing.

**Geometric Debiasing**

Geometric debiasing, as developed by Bolukbasi et all in "Man is to Computer Programmer as Woman is to Homemaker? Debiasing Word Embeddings" consists of geometrically manipulating the embedding to maintain a geometric definition of unbiased.

The model debiased for comparison using this method were created using the authors' repository:
https://github.com/tolga-b/debiaswe

**Probabilistic Debiasing**

This method consists of enforcing a probabilitistic definition of unbiased. For examples, P(he|doctor) = P(she|doctor). These probabilities are constructed using the definition of log conditional probability in Mikolov et al's "Distributed Representations of Words and Phrases and their Compositionality." Rather than geometrically manipulating the vectors, a shallow neural network is used with a loss function being the difference between conditional probabilities of neutral words.

**Cluster Debiasing**

This method of debiasing aims to mitigate a type of bias suggested in Goldberg et al's "Lipstick on a Pig:
Debiasing Methods Cover up Systematic Gender Biases in Word Embeddings But do not Remove Them." They suggest that one method of measuring bias: "the percentage of male/female socially-biased words among the k nearest neighbors of the target word." Similar to probabilisitc debiasing, this method uses a shallow neural network to adjust the vectors, where the loss function is the difference between the distance to the k/2 male socially-biased words and the the distance to the k/2 male socially-biased words, as if these distances are similar, the percentage will settle around 50%. 

## Results

Each of the embeddings performs reasonably well on the validated embedding benchmarks in comparison with the orginal. **note:** the values currently displayed for probabilistic debiasing are incorrect -- the real values are similar to scores for the original embedding.

![Benchmarks](https://github.com/hljames/mitigate-embedding-bias/blob/master/resources/benchmarks.png)


Results for embedding bias according to the WEAT statistic

![WEAT](https://github.com/hljames/mitigate-embedding-bias/blob/master/resources/weat_scores.png)

Results for embedding bias as measured by KNN. The values are obtained by summing the absolute value of the difference between the proportion of socially-biased male nearest neighbors and .5 (as ideally, each profession should have around half socially-biased male nearest neighbors and half socially-biased female neighbors).

![KNN](https://github.com/hljames/mitigate-embedding-bias/blob/master/resources/knn_scores.png)

Several of the professions are shown as an example. For example, note that in the original embedding, the profession "dancer" has 0% socially-biased male nearest neighbors, while "architect" has 100% socially-biased male nearest neighbors.


![KNN](https://github.com/hljames/mitigate-embedding-bias/blob/master/resources/profession_examples.png)

