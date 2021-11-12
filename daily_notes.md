
# 20210721
Simulate foreground maps, then use likelihood analysis to constrain tbe spectral index of synchrotron, **with and without CBASS data** , to see whether including CBASS data will decreasing the error or not. 
- Use PySM to simulate SPASS 2.3GHz, CBASS 5, WMAP K 23, Ka 33, Planck 28GHz.
- PySM d0s0, same spectral index all the sky (or **first only synchrotron**)
- first full sky, without beam, homogeneous noise
- SomeonLuke's PhD thesis  Chapter 6 to know the details of likelihood for synchrotron. 

# 0726 
- use better prior for the parameters
- dust + sync
- different noise realization
- input pysm spectral index

# 0730
- use Nside = 32, 2 degress
- [ ] ***downgrade of the mask??***
- just overlapped region between C-BASS and S-PASS
-  the $\beta_s$ with SPASS, CBASS, and with SPASS + CBASS
# 0731
- statistics of beta_s? How to quantify the performance of the likelihood analysis?
- hist2d
- wide range of prior of A0 [0, 2000]
# 0801
- an error in the input: not changed to nside=32, still 64
- Setting prior of A0 to [0, 2000] results in 2.2 minutes for each loop, which is 5 times  slower.
- [0,2000]: nside_32 cbass_only : 43s for one pixel; spass_only: 2m12s; both:2.2mins
-nside_64 : cbass_only: 21s; spass_only: 46s; both: 42s

# 0802
- change from python 3.6 to python3.8, due to the installation of PYMC3
- changing uK to mK slows down the dynesty
-  Tried PYMC3 (too complicated) and emcee(works)

# 0803
- Try to find out why dynesty is so slow with all the frequencies 

# 0804
**narrow prior**
- for pixel 400, [0,2,3,4] - 20s
- [1,2,3,4] - 5s
- [0,1,2,3,4] - 1m27s
for pixel 3520 [0,1,2,3,4] 4m44s
[0,2,3,4] 0m37s
[1,2,3,4] 0m12s
- dynesty will stuck on some pixel

# 0805
- error : 弄混了 WMAP Ka and Planck 30
- -print("Multiprocessing took {0:.2f} mins".format(1.232))

# 0809 (meeting)

- shrink the prior of A0 close to the real value according the 'real value' in simulation
- nicer plots
- [ ] fgbuster to get beta_s for Q and U ( rather than P)
- errors of the likelihood analysis 
- different noise realizations  

# 0812 
Emcee: the **number of chains** really matter!

# 0813

 - [ ] error of emcee -- not symmetric? increasing chains didn't help
 - [x] dynesty has nicer results, find out why it will stuck on some pixels.  prior for high SNR pixels should be narrow- close to the 'real' value.
 
# 0814 still trying dynesty -- better results
- P1/P3 = array([3.4501884, 3.4501884, 3.4501884, ..., 3.4501886, 3.4501884,
       3.4501884], dtype=float32), power law indeed. 
  
# 0815 
 checking further for dynesty
 - mean, cov = dyfunc.mean_and_cov(samples, weights) 
 - weighted mean?
 - [ ] use the SNR at 2.3GHz to be the limit of high and low SNR (working in power law) 
- **negative values** of A0 result in error of dynesty.
	•[0-500] for low SNR

	•SPASS only and both

	    P_nu0.min() =  215, 111
	    P_nu0  – 100， P_nu0+100 for high SNR

	•CBASS only

	    P_nu0.min()  =  62
	    P_nu0 – 50， P_nu0+150 for high SNR
	    
## 0816 combine different noise realizations
make good plots
## 0817
- make good plots
 - [ ] fgbuster error for 50 realizations
## 0818-0819
fgbuster - how it works -- different patches(pixels) are independent
- [x] SNR different for different nosie realizations?
## 0820 Fgbuster code

##  0821 Faraday rotation
## 0822 use SPASS data to get RM
 - [ ] the order of downgrade, smooth, and mask
 From vnote 20190310
nside downgrade: smooth first, then downgrade
mask and beam : boost the power spectrum at small scales

# 0916
- S/N in white pixels
- compare beta_s maps of likelihood with Fgbuster
- set a fake power-law power spectrum, generate many Gaussian realizations, and using Namaster to calculate power spectrum (and uncertainty) of some sub-region, finally  the overlapped region between SPASS and CBASS. There should be some data points at certain angular scales. 

# 0921
- for beyond 1-$\sigma$ results, indicate the related SNR and error, to show that error of high SNR region is indeed small

# 0922
- check for 1 pixel at low SNR region, to see whether noise mimic the power-law behavior
- $(\beta1 - \beta2)/(\sigma)$
- power spectrum estimation
-- calculate the residuals (cl - cl_mean)/$\sigma$
-- for circular region with the same f_sky of the overlapped region
-- add noise for pure synchrotron
-- calculate the cross correlation between SPASS and CBASS with noise
-- calculate the $\rho$ in Nico's paper, which should be one
-- (signal+ noise_1*$\sqrt{2}$) $\times$ (signal + noise_2*$\sqrt{2}$) which should be noise unbiased.
-- apply the galactic cut $\pm 20 degree$  
-- compare Fgbuster and Likelihood at High SNR regime, where Fgbuster works well.

# 0928
-- for scipy curve results, find the details of the biased pixels.

# 0929
- add beam to the maps then calculate the power spetrum
- check the posterior distribution for some pixels
- add the 'cosmic variance' to the power spectrum estimation
- [ ] noise add positive effect to total_P?

# 1008
- import power spectrum estimation figures to overleaf;
- set nside = 128
- get beta_s from spass wmap planck (4 frequencies) with beam and noise, and smoothed to 5^degree;

# 1011
- noise level should be renormalized due to the beam effect? **YES 1014**
# 1014
- definition of S/N ;(signal only)/sigma
- using smoothed sigma_P in the likelihood.
- smoothing map(5 degrees), then apply the mask
 
 - [ ] I use $\sigma_Q$ rather than $\sigma_P$ to the $\sigma_P$, is this a problem?
# 1015
- get the value of likelihood of high SNR

# 1029
- using imuint to directly get the mle?
- new version of dynesty 

# 1108
rician distribution approximation when P/sigma is very high?

- Can I assign multiple cores to each MPI rank? When MPI works on multiple nodes, can I also do this?
- tutorials?
- 
Parallel Programming paradigms 
A programming model is a collection of program abstractions that provides a simplified and transparent vision of the hardware and software system in its entirety. 
Communication in a parallel computer is possible according to these patterns: 
- Shared memory: by accessing shared variables 
- Message-passing: exchanging messages 

These patterns identify two parallel programming paradigms: 

- Shared memory or global environment paradigm where processes interact exclusively working on common resources 
-  Message passing or local environment paradigm where there are no shared resources, processes handle only local information and the only way to interact is by exchange of messages (message passing)


# 1111
- cobaya; polychord
- downgrade the maps on Q and U, rather on P
- sigmaP is acually sigma_Q or sigma_U 
- (to prove sigmaP equals to sigma_Q and sigma_U)
- noise for Planck LFI from QQ and UU
- compare with pysm beta_s with fluctuations, i.e., monopole subtracted
- get the variance of rician distribution and probability on high SNR region.

# 1112 using Gaussian pdf to approximate Rician pdf when SNR is high

# 1113
- every pixel that meets a runtime error can try a second time. 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI0NzQxODU0OCwtMTk1NjYyOTg5OF19
-->