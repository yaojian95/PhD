
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
- ***downgrade of the mask??***
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
-  Try PYMC3 (too complicated) and emcee(works)

# 0803
- Try to find out why dynesty is so slow with all the frequencies 

# 0804
- for pixel 400, [0,2,3,4] - 20s
- [1,2,3,4] - 5s
- [0,1,2,3,4] - 1m27s
for pixel 3520 [0,1,2,3,4] 4m44s
[0,2,3,4] 0m37s
[1,2,3,4] 0m12s
- dynesty will stuck on some pixel
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU0MTM2NzIyMSwtMTk1NjYyOTg5OF19
-->