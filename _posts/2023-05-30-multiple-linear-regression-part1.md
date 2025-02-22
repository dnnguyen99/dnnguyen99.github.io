---
layout: post
title: "Multiple Linear Regression (Part 1)"
author: "Diep Nguyen"
tags: [linear regression, ml]
categories: journal
image: lr_bg.jpg
---
> This article explores multiple linear regression, its matrix representation, and the Ordinary Least Squares method. We delve into the math behind obtaining the normal equation and deriving the closed-form solution for coefficient estimates. Furthermore, we look at a small example that demonstrates the use of this closed-form solution for making predictions.

## What is Multiple Linear Regression?
Linear regression is a statistical learning technique used to model the relationship between a quantitative response variable and one or more predictors. The goal is to find a linear model that captures the relationship between the independent (predictor) variables and the dependent (response) variable. This model can then be used to predict new, unseen data. When there are multiple predictors involved, we use the term "multiple linear regression" to denote the extension of simple linear regression to accommodate more than one predictor variable. 

For example, there might be a linear relationship between a person's age and their height and IQ. We can fit a linear regression model using the available age, height, and IQ data to capture this relationship. Then, the response (denoted as $y$) will be the age we try to predict. There are two predictors: height (denoted as $X^{(1)}$) and IQ (denoted as $X^{(2)}$). Linear regression uses a linear equation with coefficients $\beta$ to capture the relationship between the predictors 
and the response:

  $$age = \beta_0 + \beta_1 (Height) + \beta_2 (IQ) + \epsilon$$
  
In general, if there are $p$ predictors:

  $$y = \beta_0 + \beta_1X^{(1)} + \beta_2X^{(2)} + \cdots + \beta_pX^{(p)} + \epsilon, \text{ where }  \beta_0 \text{ is the intercept term. }$$
 
Note that we added an error term $\epsilon$ to indicate the unexplained variations or errors present in the data. 

## Linear Regression in Matrix Form

In many cases, the dataset we work with contains multiple observations, such as data on N individuals including their age, height, and IQ. Writing out the linear regression model for each individual can become cumbersome and notation-heavy. For example, if we have data on $100$ observations, then our regression model is:

$$\begin{align}
&y_1 = \beta_0 + \beta_1X_1^{(1)} + \beta_2X_1^{(2)} + \cdots + \beta_pX_1^{(p)} + \epsilon_1\\
&y_2 = \beta_0 + \beta_1X_2^{(1)} + \beta_2X_2^{(2)} + \cdots + \beta_pX_2^{(p)} + \epsilon_2\\
&\cdots\\
&y_{100} = \beta_0 + \beta_1X_{100}^{(1)} + \beta_2X_{100}^{(2)} + \cdots + \beta_pX_{100}^{(p)} + \epsilon_{100}
\end{align}$$

Writing out all $100$ equations is a very inefficient way to express our model. Therefore, it is customary to formulate the linear regression model in matrix form. 

We let $y$ be an $N \times 1$ vector that contains all the ages of person $1$ upto person $N$. Usually, we say $y \in \mathbb{R}^{(N \times 1)}$. We will create a matrix $X \in \mathbb{R}^{N \times (p+1)} $, where $p$ is the number of predictor variables. Usually, this matrix is called the design matrix that contains $1$'s in its
first column to account for the intercept term $\beta_0$, and the predictor variables in its remaining columns. Each row of this matrix represents one observation. In our example, the design matrix contains $1$'s 
in its first column and the height and IQ of person 1 up to person N in its second and third columns,
respectively. We will have a vector containing the coefficients called $\beta \in \mathbb{R}^{(p+1) \times 1}$. The dimension of this vector depends on the number of predictors. 
Since our example has two predictors (height and IQ), we will have  $\beta = [ \beta_0 \quad \beta_1 \quad \beta_2]^T$, where $\beta_0$ is the intercept term. Lastly, we have a vector $\epsilon \in \mathbb{R}^{N \times 1}$ that represents the errors. Then, the linear regression model in matrix form is:

$$y = X\beta + \epsilon$$\\
  $$\begin{bmatrix}
  y_0\\
  y_1\\
  \cdot\\
  \cdot\\
  \cdot\\
  y_N
  \end{bmatrix} = X = \begin{bmatrix}
  1 & X_1^{(1)} & X_1^{(2)} & \cdots & X_1^{(p)}\\
  1 & X_2^{(1)} & X_2^{(2)} & \cdots & X_2^{(p)}\\
  \cdot & \cdot & \cdot & \cdot & \cdot \\
  \cdot & \cdot & \cdot & \cdot & \cdot \\
  \cdot & \cdot & \cdot & \cdot & \cdot \\
  1 & X_N^{(1)} & X_N^{(2)} & \cdots & X_N^{(p)}\\
  \end{bmatrix} \begin{bmatrix} \beta_0 \\
  \cdot \\
  \cdot \\
  \cdot \\
  \beta_p \end{bmatrix} +\begin{bmatrix} \epsilon_1 \\
  \cdot \\
  \cdot \\
  \cdot \\
  \epsilon_N \end{bmatrix} $$
 
The regression model aims to find coefficients $\beta$ that, when multiplied by the design matrix X, give a good estimate for $y$. Fitting the linear regression model will give us the fitted or predicted value called $\hat{y}$:

  $$\hat{y} =\beta_0 + \sum_{j=1}^{p} X^{(j)} \beta_j = X \beta $$
 
Note that $\hat{y}$ is only an estimate of the true $y$. We almost always expect that the predicted and the actual value are different. 

## The Loss Function

To reiterate, we do not expect the predictors and the response to have a perfect linear relationship. So, $\hat{y}$ will not be the same as $y$. Our goal is to fit a model that gives $\hat{y}$ as close to $y$ as possible. Intuitively, the perfect model will give $\beta_0, \beta_1, \cdots, \beta_p$ estimates such that, when using these estimates to calculate $\hat{y}$, the overall distance/difference between $\hat{y}$ and $y$ is minimized.

We use the Ordinary Least Squares (OLS) method to estimate the coefficients of the regression model. OLS aims to find the best-fit line that minimizes the overall difference between the predicted values and the actual values (also known that the residual sum of squares or RSS). We define the loss function $L(y,\hat{y})$ that measures the overall difference between the actual $y$ and the predicted $\hat{y}$. For a particular observation $n$, the sum of squares difference between $y$ and $\hat{y}$ is:

  $$ (y_n - \hat{y_n})^2 = (y_n - X_n \beta)^2$$
 
 If we have $N$ observations,the overall difference, or the loss function is:

  $$L(y,\hat{y}) = \sum_{n=1}^N(y_n - X \beta)^2$$

 
One can show that $\sum_{n=1}^N(y_n - X \beta)^2$ is equivalent to $\lVert y-X \beta \rVert_2^2$. Usually, $\lVert.\rVert_2$ is referred to as the L2 norm or the Euclidean norm. We want to find coefficients $\beta$ that will minimize this loss:

  $$\hat{\beta} = argmin_{\beta} L(y,\hat{y})$$
 
this means that $\hat{\beta}$ should be the values that minimize the loss function (the minimizer of $L(y,\hat{y})$.

## Deriving the coefficient estimates

Recall from Calculus, for a convex function $f(x)$, the derivative of $f(x)$ gives us the slope of the line tangent to the point $x$ on the curve. Since the function is convex, at the minimum, the slope of the tangent line is equal to 0. To find the minimum point of the function, we find $x$ such that the derivative of the function at $x$ is equal to $0$. In other words, we take the derivative of $f(x)$, set it equal to $0$, and solve for $x$. 

We can apply the same concept to find $\hat{\beta}$ that minimizes the loss function. Note that the loss function will have to be convex. We will omit the proof for this, but one can show that any lp norm is convex using the definition of a convex function and the triangle inequality. To find $\hat{\beta}$, we take the (partial) derivative of $L(y,\hat{y})$ with respect to $\beta$, set it equal to $0$, and solve for $\hat{\beta}$. 

Let us first compute the derivative of the loss function: 

  $$\frac{\partial}{\partial \beta} L(y,\hat{y}) =\frac{\partial}{\partial \beta} \lVert y-X \beta \rVert_2^2 $$

Recall from linear algebra that $\lVert x \rVert_2^2 = x^T x$ and $(AB)^T = B^TA^T$. Then,

$$\begin{align}
\frac{\partial}{\partial \beta} \lVert y-X \beta \rVert_2^2  &= \frac{\partial}{\partial \beta} (y-X \beta)^T(y-X \beta)\\
&= \frac{\partial}{\partial \beta} (y^T - \beta^T X^T) (y - X \beta)\\
&= \frac{\partial}{\partial \beta} y^Ty  -y^TX \beta - \beta^T X^Ty + \beta^T X^T X \beta\\
&=\frac{\partial}{\partial \beta} y^Ty - 2 \beta^T X^T y + \beta^T X^T X \beta\\
&= -2X^Ty + 2X^T X \beta
\end{align}$$

Note that since $y^TX \beta \text{ is a scalar value, } y^TX \beta = (y^TX \beta)^T = \beta^TX^Ty$. So, $-y^TX \beta - \beta^T X^Ty = - 2 \beta^T X^T y$.

Setting this derivative equal to 0 gives us the Normal Equation. This equation is used to find the closed-form solution for $\hat{\beta}$ that minimize the loss function. 

  $$-2X^Ty + 2X^TX = 0$$\\
  $$2X^TX = 2X^Ty$$\\
  $$X^TX \beta = X^T y$$
 
If $X^TX$ is invertible, we can solve for $\hat{\beta}$:

  $$\hat{\beta} = (X^TX)^{-1}X^T y$$

## Interpretation
We have derived a closed-form expression for $\hat{\beta}$, but how can we apply this to fit a regression model? Let's go back to our 
example. Let's say we have data on the age, height, and IQ of four people:

Person | Age  | Height | IQ
:-----:|:----:|:------:|:-------:
1      | 20   | 170    | 99
2      | 6    | 45     | 20
3      | 40   | 160    | 101
4      | 12   | 150    | 45

Now, given a fifth person with a height of $135$ cm and
IQ $30$, how do we use the available data to predict this new person's age? We can use the closed form equation $\hat{\beta}= (X^TX)^{-1}X^T y$ to find the coefficient estimates. First, let's put our data into matrix form:

  $$y = \begin{bmatrix}
  y_1\\
  y_2\\
  y_3\\
  y_4
  \end{bmatrix} = \begin{bmatrix}
  20\\
  6\\
  40\\
  12
  \end{bmatrix}, X = \begin{bmatrix}
  1 & X_1^{(1)} & X_1^{(2)}\\
  1 & X_2^{(1)} & X_2^{(2)} \\
  1 & X_3^{(1)} & X_3^{(2)} \\
  1 & X_4^{(1)} & X_4^{(2)}\\
  \end{bmatrix} = \begin{bmatrix}
  1 & 170 & 99\\
  1 & 45 & 20\\
  1 & 160 & 101\\
  1 & 150 & 45\\
  \end{bmatrix} , \beta = \begin{bmatrix} \beta_0 \\
  \beta_1 \\
  \beta_2 \end{bmatrix}$$

Before we proceed, we should normalize the design matrix. A person's height is measured in cm and a person's IQ is a score. The two variables have different magnitudes (e.g., height ranging from 45 cm to 170 cm and IQ ranging from 20 to 101). Normalization ensures that the variables are on a similar scale, preventing one variable from dominating the regression model's results simply due to its larger magnitude. The new, normalized design matrix $X$ is

  $$X = \begin{bmatrix}
  1 & 0.7704 & 0.9385\\
  1 & -1.7148 & -1.3254\\
  1 & 0.5716 & 0.9958\\
  1 & 0.3728 & -0.6089\\
  \end{bmatrix}$$

We will also need to normalize the new person's height and IQ using the mean and standard deviation of the height and IQ from our dataset. For the fifth person, their normalized height is $0.0746$ and their normalized IQ is $-1.0388$.

Now, we can use the equation $\hat{\beta} = (X^TX)^{-1}X^T y$ to find the coefficients of the regression model. First, we need to find the inverse of $X^TX$. Recall from our derivation above that $X^TX$ has to be non-singular (i.e., $X^TX$ has an inverse).  

$$(X^TX)^{-1} = 
  \begin{bmatrix} 
  2.5\text{e}-01 & 1.1429\text{e}-17 & -9.5382\text{e}-18\\
  -1.1429\text{e}-17 & 8.2358\text{e}-01 & -6.8730\text{e}-01\\
  -9.5382\text{e}-18 & -6.8730\text{e}-01 & 8.2358\text{e}-01
  \end{bmatrix}$$

Then, 
  $$\hat{\beta} = \begin{bmatrix} 
  2.5\text{e}-01 & 1.14\text{e}-17 & -9.54\text{e}-18\\
  -1.14\text{e}-17 & 8.24\text{e}-01 & -6.87\text{e}-01\\
  -9.54\text{e}-18 & -6.87\text{e}-01 & 8.24\text{e}-01
  \end{bmatrix} \begin{bmatrix} 
  1 & 1& 1& 1\\
  0.7704 & -1.7148 & 0.5716 & 0.3728\\
  1.1832 & -1.5213 & 0.5071 &-0.1690
  \end{bmatrix} \begin{bmatrix}
  20\\
  6\\
  40\\
  12
  \end{bmatrix}
  $$\\
  $$\hat{\beta} = \begin{bmatrix} 19.5\\
  -3.0588\\
  13.3886
  \end{bmatrix}$$
  
Given the fifth person's height $(0.0746)$ and IQ $(-1.0388)$, we can predict their age using the estimates we just calculated:

  $$\hat{y} = \hat{\beta_0} + 0.0746 \hat{\beta_1} + (-1.0388) \hat{\beta_2} $$\\
  $$\hat{y} =  19.5 + 0.0746 * (-3.0588)   -1.0388 * 13.3886 $$\\
  $$\hat{y} = 5.3637 = 6$$

Given the dataset containing the age, height, and IQ score of four observations, we fit a linear regression model and used the coefficients to predict the age of a new person from (unseen) height and IQ data. 

## Limitations
Linear regression is a widely used statistical technique for modeling the relationship between a dependent variable and one or more independent variables. However, it has certain limitations that should be considered. Recall when deriving the closed form solution of $\hat{\beta}$, we need $X^TX)$ to be invertible. This condition does not always hold. It is proven that

  $$(X^TX) \text{ is singular} \iff rank(X) < \text{ number of columns of } X $$
 
For the design matrix $X$, $rank(X)$ represents the number of columns that are "unique". So, when the number of unique columns is less than the total number of columns, $X^TX$ will not have an inverse. A trivial way this can happen is when we accidentally enter the same predictor more than once (the design matrix has columns that are linearly dependent). Another way $X^TX$ is not invertible is when  $N \ll p$ (when we have more predictors than observations). Having a singular $X^TX$ means that there's no unique best solution OLS can find.

Even when $X^TX$ has an inverse, we should not always fit a linear regression model using all the available predictors. Overfitting occurs when the model follows the training dataset too closely, leading to poor generalization to new, unseen data. Including all predictors in the model increases the complexity and flexibility of the regression equation, allowing it to capture even small variations in the training data. However, this can result in an overly complex model that fails to generalize well to new observations.

To solve these problems, we might want to consider using a regularized linear regression technique. Ridge and Lasso are two well-known regularized regression methods that add a penalization term to the loss fucntion. Because of this added term, the solution to Ridge and Lasso will always be unique. Further, we can use dimensionality reduction techniques to ensure $p \ll N$. Principal Component Analysis (PCA) is a well-known technique. PCA takes the original predictor variables and transforms them into new variables called principal components. These components are created by combining the original variables in a smart way to capture the most important patterns and variations in the data. By choosing the principal components that explain the most variation, we can reduce the number of predictors while preserving important information. 

<!-- PCA transforms the original predictor variables into uncorrelated variables known as principal components. These components capture the maximum variance in the data and are obtained by linearly combining the original variables. By selecting the principal components that explain the most variance, we can effectively reduce the dimensionality of the data while preserving crucial information. -->


