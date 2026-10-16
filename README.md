## **System Architecture**

​	The proposed system employs a foldable ground platform with four adjustable arms to facilitate transportation, rapid deployment, and online calibration in confined environments. As illustrated in Fig. 1, each sensing unit is mounted on an arm rotating about a fixed hinge, with its rotation angle being the only unknown structural degree of freedom. The hinge locations, arm lengths, and sensor mounting extrinsic are known, while the camera pan-tilt angles are measured inputs. Thus, the proposed design relaxes the conventional fixed-anchor assumption by allowing constrained, low-DoF anchor adjustments rather than arbitrary anchor transforms.

<img src="Fig. 1.png" alt="Fig. 1" style="zoom:20%;" />

*Fig. 1. Foldable ground-aerial cooperative localization platform and its deployment configurations. (a) folded for transport, (b) unfolded with arms closed, and (c) unfolded with arms expanded.*

## **Experiment video**

<video src="video.mp4"></video>


## **Principal algorithm settings**

​	Table P summarizes the principal system and estimation parameters of the current implementation, distinguishing physical configuration from the settings that control estimation. It reports the sensor configuration and sampling rates, estimation windows and trajectory degree, residual and boundary uncertainty scales, structural regularization parameters, and robust-loss parameters. These parameters have distinct roles. Residual scales determine the relative influence of heterogeneous observations; the retained and released angular scales balance geometry retention against responsiveness; and the window length and polynomial degree specify the local trajectory model. 

> **Table P. Principal system and estimation parameters of the current implementation.** 
>
> | Category             | Parameter                                                    | Value                                                        |
> | -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
> | System configuration | Configured anchors / cameras / processed UAV-tag streams     | $4/4/1 \mathrm {or}2$                                        |
> | System configuration | Nominal base width / length / arm length: $W_p,L_p,l_i$      | $0.38/0.59/0.80\ \mathrm m$                                  |
> | System configuration | Nominal IMU / UWB / camera sampling rates                    | $100/50/30\ \mathrm{Hz}$                                     |
> | Temporal model       | Calibration window / update rate                             | $4$ epochs / $10\ \mathrm{Hz}$                               |
> | Temporal model       | Trajectory degree $n_p$ / nominal horizon $T_w$ / update rate | $3$ / $0.32\ \mathrm s$ / $25\ \mathrm{Hz}$                  |
> | Statistical model    | Calibration residual SDs: AA / UA / visual position          | $0.05/0.35/0.08\ \mathrm m$                                  |
> | Statistical model    | Initial localization residual SDs: acceleration / range / visual $xy,z$ | $0.10\ \mathrm{m\,s^{-2}}$ / $0.10\ \mathrm m$ / $(0.10,0.10)\ \mathrm m$ |
> | Statistical model    | Initial boundary SDs: position / velocity                    | $0.10\ \mathrm m$ / $0.10\ \mathrm{m\,s^{-1}}$               |
> | Statistical model    | Angular SDs: $\sqrt{q_\theta^{\rm st}},\sqrt{q_\theta^{\rm mv}},\sigma_{\theta,\rm ref}$ | $1.2^\circ/12^\circ/0.5^\circ$                               |
> | Statistical model    | Structural-release prior $p_m$ / edge-count coefficient $\lambda_{\rm size}$ | $0.02/1$                                                     |
> | Statistical model    | Huber transitions: AA / visual position / angular increment  | $3.0/2.5/1.5^\circ$                                          |
> | Statistical model    | Student-$t$ degrees of freedom $\nu$                         | $4$                                                          |
>
> *SD denotes standard deviation; AA/UA denote anchor-anchor/UAV-anchor ranges. AA and visual Huber transitions use normalized residuals; angular SDs are converted to radians. Dimensions are nominal; loaded fitted kinematics and sensor extrinsics determine effective geometry. The phase-sweep simulation uses one camera-UWB unit, $n_p=4$, a 25 Hz camera stream, and visual horizontal/vertical noise SDs of $0.08/0.10$ m.*