---
layout: post
title: Machine Learning Activity 1
---


```python
%load_ext oct2py.ipython
```

## Data Plot


```python
%%octave -f svg

x = load('ml1x.dat');
y = load('ml1y.dat');

disp("  Age(yrs)   Height(m)")
[x y]

m = length(x);

figure, plot(x,y,'gh', 'markerfacecolor', 'auto');
xlabel('Age (yrs.)');
ylabel('Height (m)');
```


    Age(yrs)   Height(m)
    ans =
    
      2.06587  0.77919
      2.36841  0.91597
      2.53999  0.90538
      2.54208  0.90566
      2.54908  0.93899
      2.78669  0.96685
      2.91168  0.96437
      3.03563  0.91446
      3.11467  0.93934
      3.15824  0.96075
      3.32759  0.89837
      3.37932  0.91210
      3.41220  0.94238
      3.42158  0.96625
      3.53157  1.05265
      3.63930  1.01438
      3.67325  0.95969
      3.92565  0.96854
      4.04986  1.07661
      4.24833  1.14550
      4.34401  1.03406
      4.38265  1.00700
      4.42306  0.96684
      4.61024  1.08959
      4.68812  1.06345
      4.97773  1.12372
      5.03600  1.03234
      5.06845  1.08745
      5.41615  1.07030
      5.43956  1.16065
      5.45632  1.07780
      5.56985  1.10698
      5.60157  1.09719
      5.68776  1.16486
      5.72156  1.14118
      5.85389  1.08442
      6.19780  1.12525
      6.35109  1.11683
      6.47970  1.19708
      6.73838  1.20695
      6.86377  1.12510
      7.02234  1.12357
      7.07824  1.21328
      7.15142  1.25227
      7.46640  1.24971
      7.59739  1.17997
      7.74407  1.18973
      7.77297  1.30299
      7.82645  1.26011
      7.93064  1.25623



![_config.yml]({{ site.baseurl }}/images/output_2_1.svg)


## Hypothesis Set


```python
%%octave -f svg

x = load('ml1x.dat');
y = load('ml1y.dat');

m = length(x);
MAX_ITERATION = 1500;
alpha = 0.07;


%Plot the data
figure, plot(x,y,'gh', 'markerfacecolor', 'auto');
xlabel('Age (yrs.)');
ylabel('Height (m)');
hold on;

% For convenience of notation, add x_0 = 1, x_1 = <age in yrs. data>
x = [ones(m, 1), x];

% The size of vector theta is equal to the  number of x columns
printf("Initial theta: \n");
theta = zeros(columns(x),1)  % initialize theta as zero column vector

plot(x(:,2), x*theta, 'rs-');


for ii = 1:MAX_ITERATION
           % Dimensions:      2x50 (50x2 * 2x1  - 50x1) = 2x1 
           gradient = (1/m).* x' * ((x * theta) - y);
           theta = theta - alpha .* gradient;
           if (ii == 1)
              printf("The result after one iteration is : \n");
              theta
              plot(x(:,2), x*theta, 'bd-');
           endif
            if (ii == 500)
              printf("The result after 500 iteration is : \n");
              theta
              plot(x(:,2), x*theta, 'r:');
           endif
endfor

 printf("The result after %d iterations is : \n", MAX_ITERATION);
 theta
 plot(x(:,2), x*theta, 'm-');
legend ("Data","Initial theta","First iteration theta","500th iteration theta","1500th iteration theta");
```


    Initial theta: 
    theta =
    
            0
            0
    
    The result after one iteration is : 
    theta =
    
      0.07453
      0.38002
    
    The result after 500 iteration is : 
    theta =
    
      0.73178
      0.06722
    
    The result after 1500 iterations is : 
    theta =
    
      0.75015
      0.06388



![_config.yml]({{ site.baseurl }}/images/output_4_1.svg)


## Cost Function


```python
%%octave -f svg

x = load('ml1x.dat');
y = load('ml1y.dat');

m = length(x);


% For convenience of notation, add x_0 = 1, x_1 = <age in yrs. data>
  x = [ones(m, 1), x];


% Calculate J matrix

% Grid over which we will calculate J
theta0_vals = linspace(-3, 3, 50);
theta1_vals = linspace(-1, 1, 50);

% initialize J_vals to a matrix of 0's
J_vals = zeros(length(theta0_vals), length(theta1_vals));

for i = 1:length(theta0_vals)
	  for j = 1:length(theta1_vals)
	  t = [theta0_vals(i); theta1_vals(j)];
                        %  50x2 * 2x50 -50x1  50x2 * 2x50 - 50x1
	  J_vals(i,j) = (1/(2*m)) .* (x * t - y)' * (x * t - y);
    endfor
endfor

J_vals = J_vals';


figure, surf(theta0_vals, theta1_vals, J_vals)
xlabel('\Theta_0');
ylabel('\Theta_1');


figure, contour(theta0_vals, theta1_vals, J_vals, logspace(-1.5,1.5,12))
xlabel('\theta_0');
ylabel('\theta_1');
```


    



![_config.yml]({{ site.baseurl }}/images/output_6_1.svg)



![_config.yml]({{ site.baseurl }}/images/output_6_2.svg)



## Prediction


```python
%%octave -f svg

x = load('ml1x.dat');
y = load('ml1y.dat');


figure, plot(x,y,'gh', 'markerfacecolor', 'auto');
xlabel('Age (yrs.)');
ylabel('Height (m)');


m = length(x);
MAX_ITERATION = 1500;
alpha = 0.07;

% For convenience of notation, add x_0 = 1, x_1 = <age in yrs. data>
x = [ones(m, 1), x];

% The size of vector theta is equal to the  number of x columns
theta = zeros(columns(x),1);  % initialize theta as zero column vector

for ii = 1:MAX_ITERATION
           % Dimensions:      2x50 (50x2 * 2x1  - 50x1) = 2x1 
           gradient = (1/m).* x' * ((x * theta) - y);
           theta = theta - alpha .* gradient;
endfor

 printf("The result after %d iterations is : \n", MAX_ITERATION);
 theta

 hold on;
 plot(x(:,2), x*theta, 'm-');

 % Prediction

 % age = 3.5
 printf("Predicted height of a 3.5 year-old boy is ");
 height1 = [1, 3.5]*theta
 plot(x, height1*ones(size(y)),'r:');
 plot(3.5*ones(size(y)), y,'r:')

 % age = 7
 printf("Predicted height of a 7 year-old boy is ");
 height2 = [1, 7]*theta
 plot(x, height2*ones(size(y)),'b:');
 plot(7*ones(size(y)), y,'b:')

 grid on
```


    The result after 1500 iterations is : 
    theta =
    
      0.75015
      0.06388
    
    Predicted height of a 3.5 year-old boy is height1 =  0.97374
    Predicted height of a 7 year-old boy is height2 =  1.1973



![_config.yml]({{ site.baseurl }}/images/mlprediction1.svg)



Based on Andrew Ng's Machine Learning Course

-mkc
