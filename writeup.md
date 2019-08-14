### Related Work

- Kalai / Bolukbasi
- Goldberg
- Caliskan


### Methods:

#### Mitigating Bias:

The original embedding used for comparison is the fastText embedding, 1 million word vectors trained on Wikipedia 2017, UMBC webbase corpus and statmt.org news dataset (16B tokens) as described in "Advances in Pre-Training Distributed Word Representations" by Mikolov et al. For simplicity, only the first 22000 tokens are used in all embeddings.

For the methods of mitigating bias other than geometric debiasing, a shallow neural network to adjust the vectors. The single layer of the model is an embedding layer with weights initialized to those of the original embedding. A batch of token indices is fed into the model, which are then embedded and a loss value is calculated based on the embedded tokens, allowing backpropogation to adjust the embeddings accordingly. For each of the models, a fixed number of iterations is used to prevent overfitting, which will eventually hurt performance on the embedding benchmarks.

##### Geometric Debiasing:

Geometric debiasing, as developed by Bolukbasi et all in "Man is to Computer Programmer as Woman is to Homemaker? Debiasing Word Embeddings" consists of mathematically manipulating the embedding to maintain a geometric definition of unbiased. The authors implicitly define bias to mean a geometric dissymmetry between words when projected onto a subspace, such as the gender subspace. The present two methods for constructing the gender subspace, one by substracting gender word pair vectors such as "he" and "she" and the second through PCA. They denote the projection of a vector $v$  onto $B$ (the subspace) by

$$v_B = \sum_{j=1}^{k} (v \cdot b_j) b_j$$

The model debiased for comparison using this method were created using the authors' repository.

##### Probabilistic Debiasing:

Probabilistic debiasing relies on a probabilistic definition of unbiased, or enforcing that the probability of prediction or outcome cannot depend on a protected class such as gender (citation needed). For example, $p(doctor|she) = p(doctor|he)$. As calculating the exact conditional probability includes summing over conditional probability of all the tokens in the vocabulary, we  we use the following estimation of log conditional probability, per Mikolov et al's paper "Distributed Representations of Words and Phrases and their Compositionality":

$$ \log p(w_O|w_I) \approx \log \sigma ({v'_{wo}}^T v_{wI}) + \sum_{i=1}^{k} [\log {\sigma ({{-v'_{wi}}^T v_{wI}})}] $$

Here $v'$ refers to the input vector while $v$ refers to the output or context vector. As described in the original work, for the k sample words $w_i$ is drawn from the corpus using the Unigram distribution raised to the 3/4 power.

We then separate the tokens into appropriately gendered and inappropriately gendered tokens, using the same list created by Bolukbasi et al. for geometric debiasing. Appropriately gendered words include words like "stepdaughter", "niece", "prince", and "fatherhood" while inappropriately gendered words are all other words, including words like, "nurse", "doctor", and "marriage." Additionally, we use the list of gender word pairs for which the conditional probability should remain equal from the same source, consisting of pairs like "he" and "she", "man" and "woman", "king" and "queen", etc. At each step of training, we generate a batch of inappropriately gendered words and samples from which to calculate the conditional probability,  and calculate the loss as the difference between the conditional probability of the target word given the two different words from the gender pairs.

$$loss = \sum_{p1,p2 \in pairs} p(target|p1) - p(target|p2)$$


##### Neighborhood Debiasing:

Neighborhood or KNN debiasing aims to mitigate a type of bias suggested in Goldberg et al's "Lipstick on a Pig:
Debiasing Methods Cover up Systematic Gender Biases in Word Embeddings But do not Remove Them." The authors demonstrate that despite even when enforcing a geometric definition of unbiased, significant bias still remains when considering the neighborhood of each target word. Specifically, they create groups of the most socially-biased male and female words (using a geometric definition of bias) and then measure the portion of male and female socially-biased words in the neighborhood of a target word. They find that previously biased male words are surrounded almost entirely by socially-biased male neighbors and vice versa. For example, (insert example). They suggest an alternative definition of bias as "the percentage of male/female socially-biased words among the k nearest neighbors of the target word."

To mitigate bias in embeddings according to this definition, we then find the distances to the $k/2$ nearest neighbors from the male socially-biased words as well as the distances to the $k/2$ female socially-biased words (in sorted order, from smallest to largest). If the word is unbiased according to this alternative definition, these distances shouldn't been too far apart. Thus our loss function becomes:

$$loss = \sum_{i = 0}^{k/2} dist(t,m_i) - dist(t,f_i)$$


Where $m$ and $f$ represent the male and female neighbors sorted by distance to the target word. Here $dist$ denotes the $L1$ distance.

#### Measuring Bias

We evaluate remaining bias in the embeddings using two methods, WEAT statistic and the neighborhood metric.

##### WEAT

##### Neighborhood Metric
