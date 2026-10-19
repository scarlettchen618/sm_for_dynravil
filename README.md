## **System Architecture**

​	The proposed system employs a foldable ground platform with four adjustable arms to facilitate transportation, rapid deployment, and online calibration in confined environments. As illustrated in Fig. 1, each sensing unit is mounted on an arm rotating about a fixed hinge, with its rotation angle being the only unknown structural degree of freedom. The hinge locations, arm lengths, and sensor mounting extrinsic are known, while the camera pan-tilt angles are measured inputs. Thus, the proposed design relaxes the conventional fixed-anchor assumption by allowing constrained, low-DoF anchor adjustments rather than arbitrary anchor transforms.

<img src="Fig. 1.png" alt="Fig. 1" style="zoom:20%;" />

*Fig. 1. Foldable ground-aerial cooperative localization platform and its deployment configurations. (a) folded for transport, (b) unfolded with arms closed, and (c) unfolded with arms expanded.*

## **Experiment video**

<video src="video.mp4"></video>


## **Principal algorithm settings**

​	Table P summarizes the principal system and estimation parameters of the current implementation, distinguishing physical configuration from the settings that control estimation. It reports the sensor configuration and sampling rates, estimation windows and trajectory degree, residual and boundary uncertainty scales, structural regularization parameters, and robust-loss parameters. These parameters have distinct roles. Residual scales determine the relative influence of heterogeneous observations; the retained and released angular scales balance geometry retention against responsiveness; and the window length and polynomial degree specify the local trajectory model. 

**Table P. Principal system and estimation parameters.**

| Category       | Parameter                                                    | Setting                                                      |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| System         | Anchors / cameras / processed UAV-tag streams                | $4/4/(1\text{ or }2)$                                        |
| System         | Nominal dimensions: $W_p,L_p,l_i$                            | $0.38/0.59/0.80\ \mathrm m$                                  |
| System         | IMU / UWB (AA and UA) / camera rates                         | $100/50/30\ \mathrm{Hz}$                                     |
| Calibration    | Window / update rate                                         | $4$ epochs; nominal span $0.30\ \mathrm s$ / $10\ \mathrm{Hz}$ |
| Calibration    | AA graph                                                     | $4$ anchors; complete graph $K_4$ with $6$ edges when all links are available |
| Localization   | Trajectory degree $n_p$ / horizon $T_w$ / update rate        | $3/0.32\ \mathrm s/25\ \mathrm{Hz}$                          |
| Noise scales   | Calibration residual SDs: AA / UA / CAM                      | $0.05/0.35/0.08\ \mathrm m$                                  |
| Noise scales   | Initial localization SDs: IMU / UWB / CAM $(xy,z)$           | $0.10\ \mathrm{m\,s^{-2}}/0.10\ \mathrm m/(0.10,0.10)\ \mathrm m$ |
| Noise scales   | Initial boundary SDs: position / velocity                    | $0.10\ \mathrm m/0.10\ \mathrm{m\,s^{-1}}$                   |
| Regularization | $\sqrt{q_\theta^{\rm st}},\sqrt{q_\theta^{\rm mv}},\sigma_{\theta,\rm ref}$ | $1.2^\circ/12^\circ/0.5^\circ$                               |
| Regularization | Release prior $p_m$ / edge-count coefficient $\lambda_{\rm size}$ | $0.02/1$                                                     |
| Robust losses  | Huber transitions: AA / CAM / angular increment              | $3.0/2.5/1.5^\circ$                                          |
| Robust losses  | Student-$t$ degrees of freedom $\nu_\mu$                     | $4$                                                          |
| Scale memory   | UWB: $\gamma_{\mathrm U_i,k}$                                | $\max(1,N_{\mathrm U_i,k})$                                  |
| Scale memory   | CAM: $\gamma_{\mathrm C_i,xy,k},\gamma_{\mathrm C_i,z,k}$    | $\max(1,2N_{\mathrm C_i,k}),\ \max(1,N_{\mathrm C_i,k})$     |
| Scale memory   | IMU: $\gamma_{\mathrm I,k}$                                  | $\max\!\left(1,3\sum_j\Delta t_j/(0.04\ \mathrm s)\right)$   |
| References     | Observation-reference timeout $T_{\rm ref}$: UWB / CAM       | $0.32/0.30\ \mathrm s$                                       |

*Notes.* SD denotes standard deviation. $N$ counts window events before residual screening, and $\Delta t_j$ denotes the temporal support assigned to inertial event $j$. 

## Ground-Truth Availability and Loss Intervals

​	To clarify ground-truth availability, Table L summarizes NOKOV loss intervals and coverage over the plotted timelines in S1 and S2, with interval endpoints rounded to two decimal places. Estimated trajectories during reference loss cannot establish positioning accuracy. Thus, the reported RMSEs of $0.071,\mathrm m$ and $0.077,\mathrm m$ for S1 and S2 support the sub-$0.08,\mathrm m$ accuracy claim only at timestamps with valid ground truth.

**Table L. NOKOV ground-truth loss intervals and coverage in S1 and S2.** 

| Trial | Plotted interval ($\mathrm{s}$) | Approximate reference-message loss intervals ($\mathrm{s}$)  | Approximate reference-message coverage |
| ----- | ------------------------------- | ------------------------------------------------------------ | -------------------------------------- |
| S1    | $0.00$–$113.00$                 | $37.44$–$41.71$; $41.78$–$42.18$; $50.09$–$55.22$; $78.44$–$82.20$ | $\approx 88.00\%$                      |
| S2    | $0.00$–$80.96$                  | $56.77$–$58.54$; $59.49$–$62.59$; $65.66$–$66.13$; $67.53$–$77.55$; $78.46$–$80.96$ | $\approx 77.93\%$                      |

