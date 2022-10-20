<!-- [Toc] -->

# dust SS

- Generate dust small scales and measure the introduced Non-Gaussianity.

:::info
:pushpin: In real observations, we lack the information of small scales of foreground which are rather important for CMB delensing, due to the limited resolution.
:::

# 0205
- investigate how PySM2 add small scales

# 0207
- modulate small scales

ismooth = hp.smoothing(dust_IQU[0], fwhm=np.radians(5), lmax=lmax)
*left: new_mod = minmax(ismooth, 0.1, 8); ---- right: new_mod = minmax(ismooth, 0, 8)*
![](https://i.imgur.com/jbwnqlV.png =350x250)![](https://i.imgur.com/v6cJrJl.png =350x250)

small scale plots: set the same upper and lower limit

# 0211
- compare modulation direction 1 and direction 2

# 0217
use the same minmax between my maps and Peppe's maps when calculating the MFs to avoid the effect of point source 


# 0221
the relationship between non-gaussianity and Minkowski functionals 

# 0224

- calculate the TE and TB power spetra for the two sets of final maps
- set the same scale of the ell cut of small scales for I and QU to see the leakage from Intensity

# 0226 Questions for Forse

- M = M_LS + M_SS; why use $\tilde{m}_{ss} = M/M_{LS} = 1 + M_{SS}/M_{LS}$; as the target, rather than $M-M_{LS}$?
- How to restore $\tilde{m}_{ss}$ back to physical units? 
- MFs mean and std calculated from 350 different patches?
- GAN for polarization: both Q and U are targeting at $m_{ss, I}$ ? $m_{Gauss}$?

# 0228
Under typical interstellar conditions, the brightest CO rotational transition lines are the J = 1 -> 0; 2 -> 1, and 3 -> 2 transitions at 115.3, 230.6, and 345.8 GHz, respectively.

Component separation in LiteBird: NILC-Matheiu, fgbuster-Peppe, HILC-Davide

# 0303 Compare MF with small scales from Forse

- Generate IQU at Nside=1024, smooth to 12 arcmin, get the small scales at ell_cut = 110;
- Forse results are fixed at each run, plan to add some **`stochasticity???`** with the input as random noise. 

# 0307 exploring the difference for Forse 
- show the plots together with poltens for 6 patches
- smooth the maps to 5 degrees and see the difference of large scales
- calculate power spectra for the full-sky with different sky mask
- calculate power spectra for the `patch` using Namaster

# 0308 Forse
- loss function binary cross-entropy?; using the difference of MFs to choose the steps


- When extend to 3 arcmin, directly use network train in the noise-input case?

> The idea of iterating the creation of smaller scales relies on the fact that the neural network is not sensitive to the physical scale of the smallest features in an image (i.e. the resolution), but only to the ratio (R = L/r) between the angular size of the side of the patch (L) and the resolution r.

- improvement needed: 
  - MFs not overlapped for the noise-input case?
  ![](https://i.imgur.com/jA0vtFX.png)

  - structures of the network

> Further improvements could be made in the future to extend the scope of this
work. 
>- For example, the iteration of the algorithm was conducted using the network trained with noise only input, but the procedure is set up so that one can try to reach the same result with a GAN trained with large scales in the input. 
>- Also, having more computational power or longer computational time available, one could repeat the iterative procedure, but dividing the 12’ patches in 5 × 5 sub-patches, rather than 4 × 4 sub-patches, so to reach a resolution of 2.6’ instead of 3’. 
>- Furthermore, to better characterize the non-Gaussianities in the small scale maps, a useful thing would be to implement a code to compute the bispectrum or higher order correlation functions, to fully characterize the statistics of the generated small scale features. 
>- Lastly, issue that remains open is how to compensate for the angular power lost in the reprojection procedure, which was significant at high latitudes.


# 0321
- how to run forse on tensorflow-gpu
- gpu-occupation doesn't stop until shut down the kernel
- Robusteness test; domain selection and multi-clustering


# 0322
- smoothing the square with scipy function; how about smoothing the whole sky first(change the order of upsampling and smoothing)
- paper on ML on a sphere

# 0328
starts to use ForSE
- slack api for python https://api.slack.com/apps/A037Q90LGJK/event-subscriptions?
- slurm: find which gpu in running
- slurm: problem with threading: program won't stop when it finishes.

# 0401

https://www.seeedstudio.com/blog/2020/02/24/what-is-x86-architecture-and-its-difference-between-x64/

Similar to the x86, the x64 is also a family of instruction set architectures (ISA) for computer processors. However, x64 refers to a 64-bit CPU and operating system instead of the 32-bit system which the x86 stands for.

- **But why does x64 refers to a 64-bit system while x86 refers to a 32-bit system?**

That was the question I asked myself too at first. However, this is because as when the processor was first being created, it was called 8086. The 8086 was well designed and popular which can understand 16-bit machine language at first. It was later improved and expanded the size of 8086 instructions to a 32-bit machine language. As they improve the architecture, they kept 86 at the end of the model number, the 8086. This line of processors was then known as the x86 architecture.

On the other hand, x64 is the architecture name for the extension to the x86 instruction set that enables 64-bit code. When it was initially developed, it was named as x86-64. However, people thought that the name was too length where it was later shortened to the current x64.

# 0404
https://www.tensorflow.org/install/source#gpu
- [x] tensorflow=1.15.0 on Cori, cuda=10.0, keras = 2.3.1, CORRECT
- [ ] tensorflow=2.2.0 on Cori, cuda=10.1, keras = 2.3.1 , ERROR; tf.keras: NOT RIGHT
- [ ] tensorflow=2.6.0 on Perl, cuda=11.3, NOT RIGHT

# 0405 
The Adam optimizer matters!

all results are shown for epoch=10000
- optimizer = Adam(0.00001, 0.5)
![](https://i.imgur.com/xFZEDTr.png)
![](https://i.imgur.com/qukEk8O.png =1100x160)

- optimizer = Adam(0.00001)
![](https://i.imgur.com/eHKTfgq.png)
![](https://i.imgur.com/phhUU8o.png)

- optimizer = Adam(0.00001, 0.9)
![](https://i.imgur.com/BkPwrd2.png)

see Adam(0.00001, 0.2)

shifterimg pull nersc/tensorflow:ngc-20.09-tf1-v0
shifter --module none --image=nersc/tensorflow:ngc-20.09-tf1-v0 --env PYTHONUSERBASE=$HOME/.local/perlmutter/my_tf1_ngc
pip install healpy --user
create new `kernel.json`

```
{
 "argv": [
  "shifter",
   "--image=nersc/tensorflow:ngc-20.09-tf1-v0",
   "/usr/bin/python",
  "-m",
  "ipykernel_launcher",
  "-f",
  "{connection_file}"
 ],
 "env": {
        "PYTHONUSERBASE":
            "/global/homes/j/jianyao/.local/perlmutter/my_tf1_ngc"
    },
 "display_name": "tf_1.15_ngc",
 "language": "python",
 "metadata": {
  "debugger": true
 }
}
```

# 0406
- optimizer = Adam(0.00001, 0.5) as a new benchmark
- set checkpoint for job restart
- [ ] increase batch_size and ==make use of multiple GPUs to accelerate== 

# 0407  :smile:
load_model and retrain the model(a warning when load saved model: optimizer state won't restored, which may due to the `trainable` parameters flag in the network )

# 0411 :joy:
- test my version of ForSE using GAN examples on the website of tensorflow, using ==Adam(0.0002, 0.5)==
- test ==Adam(5e-6, 0.2)==
- run the original ForSE code in TF=1.15 environment

# 0412
- [ ] my version of ForSE needs improvement
- test ==Adam(1e-5, 0.2)== `not better`

# 0414 
- train on U maps 

# 0420
- generate training set for 3arcmin training.

# 0421
- write a paragraph for non-gaussianity and produce more beautiful figures
- find out the problem to produce 3 arcmin maps
- checking the smoothing 

# 0429 rewrite the preprocessing code from Marianna 

# 0502 
binning of the power spectrum 

# 0503-0506 distributed training using Horovod

`load_training_set(patches_file, part_train = 1, part_test = 0, seed=seed)`

- should each rank(process) of Horovod see the same training data(seed=None)?
- test the horovod on small dataset like 348 patches!!!

# 0509 rewrite Forse with tensorflow-GAN 
#### forget to set `part_train` = 1
- good results for 80' to 12' (348 pairs of patches)
- no good results for 12' to 3', yet:
    - somewhat reasonable results for 4 nodes (salloc)
        - start to get better results with further training (through checkpoint) 
    - nonsense results for 24 nodes (sbatch)

# 0510 continue to submit interactive jobs
- [ ] to submit consecutive jobs using sbatch 
- [ ] test if we can submit interactive job via sbatch

# 0511 should test the distributed training first, with 80' to 12'
test on U map. Seems to be a failure!!! 0521 (images look good, but MFs overlapping fraction is around 0.6-0.7)
# 0518 use the output of Marianna to test the model

# 0520
- show the small scales at the location corresponding to that of large scales
- add small scales to large scales and compute the power spectra to see the beam effect
- start with small portion of training data then extend to full data-set (**always simper things first!!!**)

# 0526
- which part of small scales at 3' corresponding to the large scales???
- test 3' 8000 patches MFs calculation

# 0530
- test: from 12' to 3' directly, without smoothing to 20'

# 0601
- test: 12' to 3' but with more pixels!!!
- Marianna'a paper about number of pixels and resolution

# 0607
- small scales defined over maps@12'/maps@80', can we redefine small scales as maps@12 with a ell_cut at ell corrsponding to 12'

# 0613 :joy:
the training data with full sky is wrong somehow... 

# 0614 :joy:
- pick other sample patches to test the training (before maps_sub_20U[:, 5:7, :,:], now maps_sub_20U[:, 7:11:2,:,:]); :thumbsup: still works!
- training **full sky patches** with jupyter, test the model trained on the full sky on 348 patches, not working... :-1:  ==Errata below(it works)==
- trying to find out why distributed is not working... 

# 0615
- use trained model for 348 patches for other 348 patches!!! :satellite_antenna: 

# 0619 
- use the model trained on ==full sky== to all other smaller patches 

# 0620 
- train on small fraction patches, then re-train on other patches with only a few epoch, and see the results.

~~:question: implementation is differnet with normalizaion equation~~ ==False Positive==
- :question: from 20' 20deg 1280px to 20' 5deg 320px, the power is decreased a lot(compared with Gaussian maps) solved

# 0628 
Forse
- strange behaviors at region where no or few structures are present in the large scales
- calculate the standard deviation for the patches without structures
- understanding the 'bump' for the power spetra at 3amin(border effect?)
- add poltens (smoothed to 3amin) power spectra with different masks 
- show the map differences
- minimum epoch training??

面对电脑， 背对幸福

# 0705

v1 2 normalization
v2 2 normalization; but 2nd normalization corrected for range [560-3000]
v3 second normalization only (v3 = v2)

- todo
- full sky MFs for 12 amin maps and 3 amin
- saliency maps of the model
- noise input 
- warp thins up together

# 1017
- where does the `NN_bs_16_nico.npy` come from?
- check the maps of the NN at 12amin(set the colorbar fixed), the MF, the power spectra 
- send the full-sky maps from this NN at 12amin to Peppe 
- use the output at 12 amin from Nico's model to train the output at 3amin again?
# 1018

- NN_bs_16_Nico is the same with Nico's
- NN_out_Q_12amin_physical_units.npy is not same with Nico's

# 1019
- show all the results(MFs :heavy_check_mark:, maps:x:, power spectra:x:) with one script
- test the same model with different noise level
- convergence of the machine learning trained model 

 