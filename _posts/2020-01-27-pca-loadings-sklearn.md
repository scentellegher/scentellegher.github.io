---
layout: post
title:  "How to compute PCA loadings and the loading matrix with scikit-learn"
date:   2020-01-27 00:00:00
subtitle: How to interpret Principal Components (PCs) and compute the correlations between the original variables in a dataset and the PCs (Loadings Matrix) with scikit-learn.
categories: machine-learning
img:  # Add image post (optional)
tags: [scikit-learn, pca, loadings] # add tag
---

Suppose that after applying Principal Component Analysis (PCA) to your dataset, you are interested in understanding which is the contribution of the original variables to the principal components. How can we do that?

### Loadings
In PCA, given a mean centered dataset \\(X\\) with \\(n\\) sample and \\(p\\) variables, the first principal component \\(PC_1\\) is given by the linear combination of the original variables \\(X_1, X_2, ..., X_p\\)  

\\[PC_1 = w_{11}X_1 + w_{12}X_2 + ... + w_{1p}X_p\\]

The first principal component \\(PC_1\\) represents the component that retains the maximum variance of the data. \\(\mathbf{w_1}\\) corresponds to an **eigenvector** of the covariance matrix 

\\[\mathbf{\Sigma} = \frac{1}{N-1}\mathbf{X}^\top \mathbf{X}\\] 

and the elements of the eigenvector \\(w_{1j}\\), and are also known as **loadings**.

> **PCA loadings are the coefficients of the linear combination of the original variables from which the principal components (PCs) are constructed.**

#### Loadings with scikit-learn
Here is an example of how to apply PCA with scikit-learn on the Iris dataset.

```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

from sklearn import decomposition
from sklearn import datasets
from sklearn.preprocessing import scale

# load iris dataset
iris = datasets.load_iris()

X = scale(iris.data)
y = iris.target

# apply PCA
pca = decomposition.PCA(n_components=2)
X = pca.fit_transform(X)
```

To get the loadings, we just need to access the attribute **components_** of the *sklearn.decomposition.pca.PCA* object.

```python
loadings = pd.DataFrame(pca.components_.T, columns=['PC1', 'PC2'], index=iris.feature_names)
loadings

                        PC1       PC2
sepal length (cm)  0.521066  0.377418
sepal width (cm)  -0.269347  0.923296
petal length (cm)  0.580413  0.024492
petal width (cm)   0.564857  0.066942
```

The columns of the dataframe contain the eigenvectors associated with the first two principal components. Each element represents a loading, namely how much (the weight) each original variable contributes to the corresponding principal component.  

**Note:** In R we have the same resulting matrix accessing the element of the outputs call *rotation* returned by the function [prcomp()][r-link].

### Loadings Matrix 
Another useful way to interpret PCA is by computing the correlations between the original variable and the principal components. How can we do that?  

To compute PCA, available libraries first compute the singular value decomposition (SVD) of the original dataset 

\\[\mathbf{X} = \mathbf{U} \mathbf{S} \mathbf{V}^\top\\]  

The columns of \\(\mathbf{V}\\) contains the principal axes, \\(\mathbf{S}\\) is a diagonal matrix containing the singular values, and the columns of \\(\mathbf{U}\\) are the principal components scaled to unit norm.  
Standardized PCs are given by \\(\sqrt{N-1}\mathbf{U}\\). 

As we have seen before, the covariance matrix is defined as  

\\[\mathbf{\Sigma} =\frac{1}{N-1}\mathbf{X}^\top \mathbf{X} = \mathbf{V}\frac{\mathbf{S}^2}{N-1}\mathbf{V}^\top=\mathbf{VEV}^\top\\]
  
This means that the principal axes \\(\mathbf{V}\\) are eigenvectors of the covariance matrix and \\(\mathbf E=\frac{\mathbf S^2}{N-1}\\) are its eigenvalues.  

To compute the **Loading matrix**, namely the correlations between the original variable and the principal components, we just need to compute the cross-covariance matrix:

\\[Cov(\mathbf{X}, \mathbf{Y}) = \frac{\mathbf{X}^\top \mathbf{Y}}{N-1}\\]

In our case, \\(\mathbf{X}\\) contains the original variables, and \\(\mathbf{Y}\\) contains the standardized principal components, so  

\\[Cov(\mathbf{X}, \mathbf{Y}) = \frac{\mathbf{X}^\top(\sqrt{N-1}\mathbf{U})}{N-1} = \frac{\mathbf{V}\mathbf{S}\mathbf{U}^\top\mathbf{U}}{\sqrt{N-1}} = \mathbf{V}\frac{\mathbf{S}}{\sqrt{N-1}}=\mathbf{V}\sqrt{\mathbf E}=\mathbf{L}\\] 

(**Note**: derivation adapted from [here][stats-stack]).

> **As you can see, from a numerical point of view, the loadings \\(L\\) are equal to the coordinates of the variables divided by the square root of the eigenvalue associated with the component.**

Therefore, if we want to compute the loading matrix with scikit-learn we just need to remember that 
* \\(\mathbf{V}\\) is stored in **pca.components_.T** 
* \\(\sqrt{\mathbf E}\\) is given by **np.sqrt(pca.explained_variance_)**



```python
loadings = pca.components_.T * np.sqrt(pca.explained_variance_)

loading_matrix = pd.DataFrame(loadings, columns=['PC1', 'PC2'], index=iris.feature_names)
loading_matrix

                        PC1       PC2
sepal length (cm)  0.893151  0.362039
sepal width (cm)  -0.461684  0.885673
petal length (cm)  0.994877  0.023494
petal width (cm)   0.968212  0.064214
```
Here each entry of the matrix contains the correlation between the original variable and the principal component. For example the original variable **sepal length (cm)** and the first principal component **PC1** have a correlation of \\(0.89\\).

You can find the code [here][code-snippet].

[stats-stack]: https://stats.stackexchange.com/a/104640
[r-link]: http://www.sthda.com/english/articles/31-principal-component-methods-in-r-practical-guide/118-principal-component-analysis-in-r-prcomp-vs-princomp/
[code-snippet]: https://github.com/scentellegher/code_snippets/blob/master/pca_loadings/pca_loadings.ipynb
