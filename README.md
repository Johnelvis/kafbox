﻿Kernel Adaptive Filtering Toolbox
=================================

A Matlab benchmarking toolbox for kernel adaptive filtering.

The Kernel Adaptive Filtering Toolbox contains implementations of many kernel adaptive filtering algorithms and tools to compare their performance.

Author: Steven Van Vaerenbergh  
Contact: steven at gtas dot dicom dot unican dot es  
Contributors: Miguel Lazaro-Gredilla, Sohan Seth  
Development web: https://github.com/steven2358/kafbox  
Major versions are released on https://sourceforge.net/projects/kafbox  

Directories included in the toolbox
-----------------------------------

`data/` - data sets

`demo/` - demos and test files

`lib/` - algorithm libraries and utilities

Setup
-----

Run install.m

Octave / Matlab pre-2008a
-------------------------
This toolbox uses the `classdef` command which is not supported in Matlab pre-2008a and not yet in Octave. The older 0.x versions of this toolbox do not use `classdef` and can therefore be used with all versions of Matlab and Octave. http://sourceforge.net/projects/kafbox/files/

Example: time-series prediction
-------------------------------
Usage demo from `demos/demo_prediction.m`
```matlab

%% PARAMETERS
datafile = 'lorenz.dat'; % Lorenz attractor time-series data
L = 6; % length of time-embedding
horizon = 1; % 1-step ahead prediction

% make a kernel adaptive filter object of class aldkrls with options: 
% ALD threshold 1E-4, Gaussian kernel, and kernel width 32
kaf = aldkrls(struct('nu',1E-4,'kerneltype','gauss','kernelpar',32));

%% PREPARE DATA
data = load(datafile); % 1-dimensional time-series
data = data(:);
N = length(data)-horizon; % number of data to use (all data)
X = zeros(N,L);
for i = 1:L,
    X(i:N,i) = data(1:N-i+1); % time embedding of length L
end
Y = data(1+horizon:N+horizon); % vector with desired outputs

%% RUN ALGORITHM
Y_est = zeros(N,1);
for i=1:N,
    if ~mod(i,floor(N/10)), fprintf('.'); end % progress indicator, 10 dots
    Y_est(i) = kaf.evaluate(X(i,:)); % predict the next output
    kaf = kaf.train(X(i,:),Y(i)); % train with one input-output pair
end
fprintf('\n');
SE = (Y-Y_est).^2; % test error

%% OUTPUT
fprintf('MSE after first 1000 samples: %.2fdB\n\n',10*log10(mean(SE(1001:end))));

```
Result:
    MSE after first 1000 samples: -40.17dB

Included algorithms
-------------------
- Approximate Linear Dependency Kernel Recursive Least-Squares (ALD-KRLS), as proposed in Y. Engel, S. Mannor, and R. Meir. "The kernel recursive least- squares algorithm", IEEE Transactions on Signal Processing, volume 52, no. 8, pages 2275-2285, 2004.
- Sliding-Window Kernel Recursive Least-Squares (SW-KRLS), as proposed in S. Van Vaerenbergh, J. Via, and I. Santamaria. "A sliding-window kernel RLS algorithm and its application to nonlinear channel identification", 2006 IEEE International Conference on Acoustics, Speech, and Signal Processing (ICASSP), Toulouse, France, 2006.
- Naive Online Regularized Risk Minimization Algorithm (NORMA), as proposed in J. Kivinen, A. Smola and C. Williamson. "Online Learning with Kernels", IEEE Transactions on Signal Processing, volume 52, no. 8, pages 2165-2176, 2004.
- Kernel Least-Mean-Square (KLMS), as proposed in W. Liu, P.P. Pokharel, and J.C. Principe, "The Kernel Least-Mean-Square Algorithm," IEEE Transactions on Signal Processing, vol.56, no.2, pp.543-554, Feb. 2008.
- Fixed-Budget Kernel Recursive Least-Squares (FB-KRLS), as proposed in S. Van Vaerenbergh, I. Santamaria, W. Liu and J. C. Principe, "Fixed- Budget Kernel Recursive Least-Squares", 2010 IEEE International Conference on Acoustics, Speech, and Signal Processing (ICASSP 2010), Dallas, Texas, U.S.A., March 2010.
- Kernel Recursive Least-Squares Tracker (KRLS-T), as proposed in S. Van Vaerenbergh, M. Lazaro-Gredilla, and I. Santamaria, "Kernel Recursive Least- Squares Tracker for Time-Varying Regression," Neural Networks and Learning Systems, IEEE Transactions on , vol.23, no.8, pp.1313- 1326, Aug. 2012.
- Quantized Kernel Least Mean Squares (QKLMS), as proposed in Chen B., Zhao S., Zhu P., Principe J.C. "Quantized Kernel Least Mean Square Algorithm," IEEE Transactions on Neural Networks and Learning Systems, vol.23, no.1, Jan. 2012, pages 22-32.
- Random Fourier Fourier Feature Kernel Least Mean Squares (RFF-KLMS), as proposed in Abhishek Singh, Narendra Ahuja and Pierre Moulin, "Online Learning With Kernels: Overcoming The Growing Sum Problem", 2012 IEEE International Workshop on Machine Learning For Signal Processing.
- Extended Kernel Recursive Least Squares (EX-KRLS), as proposed in W. Liu and I. Park and Y. Wang and J.C. Principe, "Extended kernel recursive least squares algorithm", IEEE Transactions on Signal Processing, volume 57, number 10, pp. 3801-3814, oct. 2009.
- Gaussian-Process based estimation of the parameters of KRLS-T, as proposed in Steven Van Vaerenbergh, Ignacio Santamaria, and Miguel Lazaro-Gredilla, "Estimation of the forgetting factor in kernel recursive least squares," 2012 IEEE International Workshop on Machine Learning for Signal Processing (MLSP), 2012.
- Kernel Affine Projection algorithm with Coherence Criterion, as proposed in C. Richard, J.C.M. Bermudez, P. Honeine, "Online Prediction of Time Series Data With Kernels," IEEE Transactions on Signal Processing, vol.57, no.3, pp.1058,1067, March 2009.
- Kernel Normalized Least-Mean-Square algorithm with Coherence Criterion, as proposed in C. Richard, J.C.M. Bermudez, P. Honeine, "Online Prediction of Time Series Data With Kernels," IEEE Transactions on Signal Processing, vol.57, no.3, pp.1058,1067, March 2009.