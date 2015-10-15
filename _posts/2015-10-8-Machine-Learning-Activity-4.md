---
layout: post
title: Regularization with Linear and Logistic Regression 
---


```python
%load_ext oct2py.ipython
```

## Objective

● To implement regularized linear regression and regularized logistic regression.

● To apply regularized logistic regression to predict which passengers survived the Titanic shipwreck tragedy.

## Regularized linear regression

### Data Plot


```python
%%octave -f svg

x = load('ML4/ml4Linx.dat');
y = load('ML4/ml4Liny.dat');

m = length(x);

plot(x,y,'go', 'MarkerFaceColor','auto')
xlabel('x');
ylabel('y');
```


    



![_config.yml]({{ site.baseurl}}/images/ml4_6_1.svg)


### Hypothesis

$$ h_\theta(x) = \theta_0 + \theta_1 x + \theta_2 x^2 + \theta_3 x^3 + \theta_4 x^4 + \theta_5 x^5 $$

### Six Polynomial Features

$$ x^0, x^1, x^2, x^3, x^4, x^5  $$

### Six Model Parameters

$$ \theta_0, \theta_1, \theta_2, \theta_3, \theta_4, \theta_5 $$

### Linear Regression Cost Function

$$ J(\theta) = \dfrac{1}{2m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})^2  $$

### Linear Regression with Regularization Cost Function (To guard against overfitting!)

$$ J(\theta) = \dfrac{1}{2m}  \Big[ \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})^2  + \lambda \sum_{i=1}^{n} \theta_j^2   \Big]  $$

where $\lambda$ is the regularization parameter

### Regularization parameter

● The regularization parameter λ is a control on your fitting parameters.

● As the magnitudes of the fitting parameters increase, there will be an increasing penalty on the cost function.

● This penalty is dependent on the squares of the parameters as well as the magnitude of λ.

### Normal equations

Now we will find the best parameters of our model using the normal equations. Recall that the normal equations solution to regularized linear regression is

$$  \theta = (X^TX + \lambda \underbrace{\begin{bmatrix} 
		0 & 0 & \cdots & 0 \\
		0 & 1 & \ddots & \vdots \\
		\vdots & \ddots & \ddots & 0 \\
		0 & \cdots & 0 & 1
	\end{bmatrix}}_{n + 1} )^{-1} X^T \vec{y} $$ 

### Problem

Using the Normal equation, find values for θ using the three regularization parameters below:

    a. λ = 0 (this is the same case as nonregularized linear regression)
    
    b. λ = 1
    
    c. λ = 10

a. λ = 0 (this is the same case as nonregularized linear regression)


```python
%%octave -f svg

% Regularization parameter
lambda = 0;

x = load('ML4/ml4Linx.dat');
y = load('ML4/ml4Liny.dat');

m = length(x);

%Plot
plot(x,y,'go', 'MarkerFaceColor','auto');
hold on;

% Features
x = [ ones(m,1), x, x.^2, x.^3, x.^4, x.^5 ];
theta = zeros(size(x(1,:))); % Theta size is equivalent to the no. of x columns

% Closed form solution (Normal Equations)
L = lambda.*eye(6);
L(1) = 0;   % intercept term don't need regularization'
theta = (x' * x + L)\x' * y;

% Denser x values
x_vals = (linspace(-1,1,1000))';
features = [ ones(size(x_vals)), x_vals, x_vals.^2, x_vals.^3, x_vals.^4, x_vals.^5 ];

size(features)
size(theta)

% Hypothesis
h = features*theta;

%Plot
plot(x_vals, h, 'r--', 'Linewidth', 3);
xlabel('x');
ylabel('y');
title('Hypothesis at λ = 0');
legend('Training data', '5th order fit');
```


    ans =
    
         1000        6
    
    ans =
    
            6        1



![_config.yml]({{ site.baseurl}}/images/ml4_26_1.svg)


   b. λ = 1


```python
%%octave -f svg

% Regularization parameter
lambda = 1;

x = load('ML4/ml4Linx.dat');
y = load('ML4/ml4Liny.dat');

m = length(x);

%Plot
plot(x,y,'go', 'MarkerFaceColor','auto');
hold on;

% Features
x = [ ones(m,1), x, x.^2, x.^3, x.^4, x.^5 ];
theta = zeros(size(x(1,:))); % Theta size is equivalent to the no. of x columns

% Closed form solution (Normal Equations)
L = lambda.*eye(6);
L(1) = 0;   % intercept term don't need regularization'
theta = (x' * x + L)\x' * y;

% Denser x values
x_vals = (linspace(-1,1,1000))';
features = [ ones(size(x_vals)), x_vals, x_vals.^2, x_vals.^3, x_vals.^4, x_vals.^5 ];

size(features)
size(theta)

% Hypothesis
h = features*theta;

%Plot
plot(x_vals, h, 'r--', 'Linewidth', 3);
xlabel('x');
ylabel('y');
title('Hypothesis at λ = 1');
legend('Training data', '5th order fit');
```


    ans =
    
         1000        6
    
    ans =
    
            6        1



![_config.yml]({{ site.baseurl}}/images/ml4_28_1.svg)


c. λ = 10


```python
%%octave -f svg

% Regularization parameter
lambda = 10;

x = load('ML4/ml4Linx.dat');
y = load('ML4/ml4Liny.dat');

m = length(x);

%Plot
plot(x,y,'go', 'MarkerFaceColor','auto');
hold on;

% Features
x = [ ones(m,1), x, x.^2, x.^3, x.^4, x.^5 ];
theta = zeros(size(x(1,:))); % Theta size is equivalent to the no. of x columns

% Closed form solution (Normal Equations)
L = lambda.*eye(6);
L(1) = 0;   % intercept term don't need regularization'
theta = (x' * x + L)\x' * y;

% Denser x values
x_vals = (linspace(-1,1,1000))';
features = [ ones(size(x_vals)), x_vals, x_vals.^2, x_vals.^3, x_vals.^4, x_vals.^5 ];

size(features)
size(theta)

% Hypothesis
h = features*theta;

%Plot
plot(x_vals, h, 'r--', 'Linewidth', 3);
xlabel('x');
ylabel('y');
title('Hypothesis at λ = 10');
legend('Training data', '5th order fit');
```


    ans =
    
         1000        6
    
    ans =
    
            6        1



![_config.yml]({{ site.baseurl}}/images/ml4_30_1.svg)


## Regularized logistic regression

Implement regularized logistic regression using Newton's Method.

### Data Plot


```python
%%octave -f svg

x = load('ML4/ml4Logx.dat');
y = load('ML4/ml4Logy.dat');

% Distinguish positive and negative samples
negative = find(y == 0);
positive = find(y == 1);

plot( x(positive,1), x(positive,2),'r+');
hold on;
plot( x(negative,1), x(negative,2),'bo');
xlabel('u');
ylabel('v');
legend(' y = 1 ', ' y = 0 ');
```


    



![_config.yml]({{ site.baseurl}}/images/ml4_34_1.svg)



```python
%cat ML4/map_feature.m
```

    function out = map_feature(feat1, feat2)
    % MAP_FEATURE    Feature mapping function for Exercise 5
    %
    %   map_feature(feat1, feat2) maps the two input features
    %   to higher-order features as defined in Exercise 5.
    %
    %   Returns a new feature array with more features
    %
    %   Inputs feat1, feat2 must be the same size
    %
    % Note: this function is only valid for Ex 5, since the degree is
    % hard-coded in.
        degree = 6;
        out = ones(size(feat1(:,1)));
        for i = 1:degree
            for j = 0:i
                out(:, end+1) = (feat1.^(i-j)).*(feat2.^j);
            end
        end
        

### Recall the Logistic Regression Cost function:

$$ J(\theta) = \dfrac{1}{m} \sum_{i=1}^{m}\big[ -y^{(i)} \log (h_{\theta} (x^{(i)})) - (1 - y^{(i)}) \log(1 - h_{\theta} (x^{(i)}))  \big]   $$

### Logistic Regression with Regularization Cost function:

$$ J(\theta) = \dfrac{1}{m} \sum_{i=1}^{m}\big[ -y^{(i)} \log (h_{\theta} (x^{(i)})) - (1 - y^{(i)}) \log(1 - h_{\theta} (x^{(i)}))  \big]  + \dfrac{\lambda}{2m} \sum_{i=1}^{n} \theta^2_j $$

### Newton's Method Update Rule

$$ \theta^{(t+1)} = \theta^{t} - H^{-1} \nabla_\theta J  $$

### Cost function plot


```python
%%octave -f svg

x = load('ML4/ml4Logx.dat');
y = load('ML4/ml4Logy.dat');

% Distinguish positive and negative samples
negative = find(y == 0);
positive = find(y == 1);

addpath ML4/

% Add polynomial features
x = map_feature(x(:,1), x(:,2)); % Increase the no. of features from 2 to 28
[m, n] = size(x)
[m n]

% Initialize theta
theta = zeros(n, 1);

% Sigmoid
g = inline('1.0 ./ (1.0 + exp(-z))');

% Regularization parameter
lambda = 0

% Newton's method

MAX_ITER = 16;
J = zeros(MAX_ITER, 1);

for i = 1:MAX_ITER

    % hypothesis
    h = g(x*theta); % Sigmoid
 
    % cost function
    J(i) = (1/m)*sum(-y.*log(h) - (1-y).*log(1 - h)) + (lambda/(2*m)) * norm(theta([2:end]))^2;

    % gradient 
    G = (lambda/m).*theta;
    G(1) = 0; % Note: intercept term is not included
    grad = ((1/m).*x' * (h - y)) + G;
    
    % hessian
    L = (lambda/m).*eye(n);
    L(1) = 0;
    H = ((1/m).*x' * diag(h) * diag(1-h) * x) + L;        
    
    %theta update
    theta = theta - H\grad;
    
endfor
         
plot(0:MAX_ITER-1, J, 'ro--','MarkerFaceColor','auto')
xlim([0 15]);
xlabel('Iteration');
ylabel('J')
```


    m =  117
    n =  28
    ans =
    
          117       28
    
    lambda = 0



![_config.yml]({{ site.baseurl}}/images/ml4_43_1.svg)


a. Decision Boundary at λ = 0 (this is the same case as nonregularized linear regression)


```python
%%octave -f svg

x = load('ML4/ml4Logx.dat');
y = load('ML4/ml4Logy.dat');

% Distinguish positive and negative samples
negative = find(y == 0);
positive = find(y == 1);

plot( x(positive,1), x(positive,2),'r+');
hold on;
plot( x(negative,1), x(negative,2),'bo');
xlabel('u');
ylabel('v');
legend(' y = 1 ', ' y = 0 ');

addpath ML4/

% Add polynomial features
x = map_feature(x(:,1), x(:,2)); % Increase the no. of features from 2 to 28
[m, n] = size(x)
[m n]

% Initialize theta
theta = zeros(n, 1);

% Sigmoid
g = inline('1.0 ./ (1.0 + exp(-z))');

% Regularization parameter
lambda = 0

% Newton's method

MAX_ITER = 16;
J = zeros(MAX_ITER, 1);

for i = 1:MAX_ITER

    % hypothesis
    h = g(x*theta); % Sigmoid
 
    % cost function
    J(i) = (1/m)*sum(-y.*log(h) - (1-y).*log(1 - h)) + (lambda/(2*m)) * norm(theta([2:end]))^2; 

    % gradient 
    G = (lambda/m).*theta;
    G(1) = 0; % Note: intercept term is not included
    grad = ((1/m).*x' * (h - y)) + G;
    
    % hessian
    L = (lambda/m).*eye(n);
    L(1) = 0;
    H = ((1/m).*x' * diag(h) * diag(1-h) * x) + L;        
    
    %theta update
    theta = theta - H\grad;
    
endfor

         
u = linspace(-1, 1.5, 300);
v = linspace(-1, 1.5, 300);
         
h = zeros(length(u), length(v));
         
for i = 1:length(u)
         for j = 1:length(v)
             z(i,j) = map_feature(u(i), v(j))*theta;
         endfor
endfor
         
contour(u, v, z', [0, 0],'m-','LineWidth', 4)
legend('y = 1', 'y = 0', 'Decision Boundary')
```


    m =  117
    n =  28
    ans =
    
          117       28
    
    lambda = 0



![_config.yml]({{ site.baseurl}}/images/ml4_45_1.svg)


b. Decision Boundary at λ = 1


```python
%%octave -f svg

x = load('ML4/ml4Logx.dat');
y = load('ML4/ml4Logy.dat');

% Distinguish positive and negative samples
negative = find(y == 0);
positive = find(y == 1);

plot( x(positive,1), x(positive,2),'r+');
hold on;
plot( x(negative,1), x(negative,2),'bo');
xlabel('u');
ylabel('v');
legend(' y = 1 ', ' y = 0 ');

addpath ML4/

% Add polynomial features
x = map_feature(x(:,1), x(:,2)); % Increase the no. of features from 2 to 28
[m, n] = size(x)
[m n]

% Initialize theta
theta = zeros(n, 1);

% Sigmoid
g = inline('1.0 ./ (1.0 + exp(-z))');

% Regularization parameter
lambda = 1

% Newton's method

MAX_ITER = 16;
J = zeros(MAX_ITER, 1);

for i = 1:MAX_ITER

    % hypothesis
    h = g(x*theta); % Sigmoid
 
    % cost function
    J(i) = (1/m)*sum(-y.*log(h) - (1-y).*log(1 - h)) + (lambda/(2*m)) * norm(theta([2:end]))^2; 

    % gradient 
    G = (lambda/m).*theta;
    G(1) = 0; % Note: intercept term is not included
    grad = ((1/m).*x' * (h - y)) + G;
    
    % hessian
    L = (lambda/m).*eye(n);
    L(1) = 0;
    H = ((1/m).*x' * diag(h) * diag(1-h) * x) + L;        
    
    %theta update
    theta = theta - H\grad;
    
endfor

         
u = linspace(-1, 1.5, 300);
v = linspace(-1, 1.5, 300);
         
h = zeros(length(u), length(v));
         
for i = 1:length(u)
         for j = 1:length(v)
             z(i,j) = map_feature(u(i), v(j))*theta;
         endfor
endfor
         
contour(u, v, z', [0, 0],'m-','LineWidth', 4)
legend('y = 1', 'y = 0', 'Decision Boundary')
```


    m =  117
    n =  28
    ans =
    
          117       28
    
    lambda =  1



![_config.yml]({{ site.baseurl}}/images/ml4_47_1.svg)


c. Decision Boundary at λ = 10


```python
%%octave -f svg

x = load('ML4/ml4Logx.dat');
y = load('ML4/ml4Logy.dat');

% Distinguish positive and negative samples
negative = find(y == 0);
positive = find(y == 1);

plot( x(positive,1), x(positive,2),'r+');
hold on;
plot( x(negative,1), x(negative,2),'bo');
xlabel('u');
ylabel('v');
legend(' y = 1 ', ' y = 0 ');

addpath ML4/

% Add polynomial features
x = map_feature(x(:,1), x(:,2)); % Increase the no. of features from 2 to 28
[m, n] = size(x)
[m n]

% Initialize theta
theta = zeros(n, 1);

% Sigmoid
g = inline('1.0 ./ (1.0 + exp(-z))');

% Regularization parameter
lambda = 10

% Newton's method

MAX_ITER = 16;
J = zeros(MAX_ITER, 1);

for i = 1:MAX_ITER

    % hypothesis
    h = g(x*theta); % Sigmoid
 
    % cost function
    J(i) = (1/m)*sum(-y.*log(h) - (1-y).*log(1 - h)) + (lambda/(2*m)) * norm(theta([2:end]))^2; 

    % gradient 
    G = (lambda/m).*theta;
    G(1) = 0; % Note: intercept term is not included
    grad = ((1/m).*x' * (h - y)) + G;
    
    % hessian
    L = (lambda/m).*eye(n);
    L(1) = 0;
    H = ((1/m).*x' * diag(h) * diag(1-h) * x) + L;        
    
    %theta update
    theta = theta - H\grad;
    
endfor

         
u = linspace(-1, 1.5, 300);
v = linspace(-1, 1.5, 300);
         
h = zeros(length(u), length(v));
         
for i = 1:length(u)
         for j = 1:length(v)
             z(i,j) = map_feature(u(i), v(j))*theta;
         endfor
endfor
         
contour(u, v, z', [0, 0],'m-','LineWidth', 4)
legend('y = 1', 'y = 0', 'Decision Boundary')
```


    m =  117
    n =  28
    ans =
    
          117       28
    
    lambda =  10



![_config.yml]({{ site.baseurl}}/images/ml4_49_1.svg)


--to be continued
