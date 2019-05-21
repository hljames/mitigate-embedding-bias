### Debiasing Word Embeddings

**Goal:** Create debiased word embeddings using a probabilistic framework, specifically by interpolating between two distributions that should be the same using wasserstein distance.

Procedure:

1. Create a matrix of reshaped distributions along some axis of bias (ex: gender)

2. Use this matrix to create new word embeddings

3. Measure the bias using WEAT statistics (https://github.com/hljames/compare-embedding-bias)
