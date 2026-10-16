## **System Architecture**

​	The proposed system employs a foldable ground platform with four adjustable arms to facilitate transportation, rapid deployment, and online calibration in confined environments. As illustrated in Fig. 1, each sensing unit is mounted on an arm rotating about a fixed hinge, with its rotation angle being the only unknown structural degree of freedom. The hinge locations, arm lengths, and sensor mounting extrinsic are known, while the camera pan-tilt angles are measured inputs. Thus, the proposed design relaxes the conventional fixed-anchor assumption by allowing constrained, low-DoF anchor adjustments rather than arbitrary anchor transforms.

<img src="Fig. 1.png" alt="Fig. 1" style="zoom:20%;" />

*Fig. 1. Foldable ground-aerial cooperative localization platform and its deployment configurations. (a) folded for transport, (b) unfolded with arms closed, and (c) unfolded with arms expanded.*

## **Experiment video**

<video src="video.mp4"></video>


## **Principal algorithm settings**

​	We have clarified the roles and selection criteria of the parameters across the three algorithmic modules: dynamic anchor calibration, reliability-aware measurement evaluation, and sliding-window estimation. A consolidated table has been added to summarize the key hyperparameters, their definitions, and the values used in our experiments, facilitating implementation and reproduction. We have provided reference values for key, straightforward parameters in our design. 

> **Table P. Principal algorithm settings.** 
> 
> | Module | Setting | Value |
>|---|---|---|
> | Calibration | Release prior $p_{\mathrm{rel}}$; edge-count penalty $\lambda_{\mathrm{size}}$ | $0.02;\ 1$ |
> | Calibration | $\sqrt{q_\theta^{\mathrm{st}}},\sqrt{q_\theta^{\mathrm{mv}}},\sigma_{\theta,\mathrm{ref}}$ | $1.2^\circ,\ 12^\circ,\ 0.5^\circ$ |
> | Calibration | AA / UA / camera residual standard deviations | $0.05/0.35/0.08\ \mathrm m$ |
> | Calibration | Huber thresholds: AA / camera / temporal | $3.0/2.5/1.5^\circ$ |
> | Calibration | Window epochs / lag epochs / solve rate | $4/1/10\ \mathrm{Hz}$ |
> | Reliability | Soft-gate thresholds $c_0,c_1$ | $1.96,\ 3.0$ |
> | Reliability | Student-$t$ $\nu$; scale-update strength $\eta_0$; SD-ratio bounds $[r_{\min},r_{\max}]$ | $4;\ 20;\ [0.1,10]$ |
> | Reliability | Reference expiry: UWB / camera; confirmation count | $0.32/0.30\ \mathrm s;\ 3$ |
> | Localization | Degree $n_p$; nominal horizon $T_w$; update rate | $3$ (current hardware configuration), $4$ (phase simulation); $0.32\ \mathrm s$; $25\ \mathrm{Hz}$ |
> | Localization | Initial boundary scales $\sigma_{p,0},\sigma_{v,0}$ | $0.10\ \mathrm m,\ 0.10\ \mathrm{m\,s^{-1}}$ |
> | Localization | Initial acceleration / UWB / camera $xy,z$ scales | $0.10\ \mathrm{m\,s^{-2}};\ 0.10\ \mathrm m;\ 0.10,0.10\ \mathrm m$ |
> | Localization | Effective-sample floor ratio $\gamma_N$ | $0.25$ |