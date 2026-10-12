# Research on Integrated Radar Communication Based on FMCW and OFDM with Multi-Object Tracking Technology [Attached MATLAB Code]

This paper, relying on multiple MATLAB simulation codes, conducts a systematic study on the integration of radar communication and ISAC (Integrated Synthetic Aperture Channel) waveform design, MIMO array angle estimation, and multi-object tracking (MOT) core technologies for millimeter wave radar signal processing.
The full text thoroughly reviews the theoretical framework of radar perception: Firstly, it derives the frequency modulation continuous wave (FMCW) radar differential frequency signal model and the principle of two-dimensional fast Fourier transform (2D-FFT) distance-Doppler processing, introducing the two-dimensional unit average constant false alarm rate (CA-CFAR) detection algorithm; Secondly, for OFDM signals, it deduces the element-level division by element channel estimation algorithm, constructing an ISAC integrated perception mathematical model; Subsequently, it analyzes the uniform linear array (ULA) MIMO radar distance-Doppler-angle three-dimensional processing mechanism; Finally, it completes the derivation of the Kalman filter algorithm based on the constant velocity (CV) model, combined with the nearest neighbor data association to achieve stable management of multi-object track trajectories.
The research combines rigorous theoretical deduction and simulation verification, covering the entire process of radar perception, integrated communication and perception, angle measurement, and target tracking comprehensively, providing theoretical support and code references for the design and engineering implementation of intelligent radar systems.
In the context of rapid deployment in cutting-edge scenarios such as autonomous driving, intelligent transportation, and low-altitude perception, traditional single-function radars cannot meet system requirements. Radar equipment needs not only high-precision ranging, speed measurement, and angle measurement capabilities but also to deeply integrate with wireless communication systems to achieve integrated communication and perception (ISAC), hardware coexistence, signal reuse, and functional compatibility, becoming the core development trend in the industry.
Among them, FMCW radar, with its simple hardware architecture, low power consumption, excellent anti-interference performance, and stability in ranging and speed measurement, has become the mainstream technology solution for civilian millimeter wave radar. Meanwhile, OFDM, as the core standard waveform of 5G NR, possesses high spectral efficiency, resistance to multipath fading, and is easy to implement digitally. It can superimpose communication transmission and radar perception functions on the same signal carrier, making it the core research direction for ISAC technology at present.
The radar system obtains target point cloud data through signal processing, but the original detection results are plagued by noise, false alarms, and missed detections. It is essential to rely on filtering and tracking algorithms to optimize target states and stabilize trajectories. Based on this, this paper focuses on theoretical derivation + simulation implementation as its core, thoroughly analyzes the full chain technology principles of FMCW radar signal processing, OFDM integrated sensing, MIMO angle estimation, and Kalman multi-target tracking. This provides a complete technical system for related algorithm design and simulation implementation.
2. The principle of FMCW radar signal processing: FMCW radar transmits linear frequency modulated continuous wave signals, utilizing the time delay of the echo and Doppler shift to calculate target parameters. The core process includes signal Dechirp processing, distance-Doppler two-dimensional FFT processing, and constant false alarm target detection.
2.1 Linear Frequency Modulation Signal and Dechirp Processing: The expression of the single Chirp linear frequency modulated pulse signal transmitted by FMCW radar is:
In which: represents the carrier frequency, represents the modulation slope, represents the signal bandwidth, represents the duration of the single Chirp.
When the radar electromagnetic wave illuminates a moving target with a distance of and relative velocity of, the echo signal will produce a time delay and Doppler shift. The target distance can be approximately constant within the single Chirp period, and the signal delay satisfies:
Where represents the speed of light, corresponds to the radar receiving the echo signal as:
Represents the target reflection coefficient. At the receiving end, through Dechirp demodulation processing, the received signal is conjugated with the transmitted local oscillator signal to complete signal downconversion:
Expanding the formula and ignoring higher order minor components, the final pure difference frequency signal is obtained:
From the formula, we can conclude that the frequency of the difference frequency signal is linearly proportional to the target distance; the initial phase of the signal contains high-precision distance information, providing a theoretical basis for subsequent range measurement.
2.2 Range-Doppler two-dimensional processing: A radar continuously transmits multiple Chirp signals within a coherent processing interval (CPI), solving the target distance and velocity simultaneously through fast and slow time dimension dual FFT processing.
For the 1st slow time Chirp, the real-time distance of moving targets can be represented as follows:

Where \(f_d\) is the Doppler frequency of the target and corresponds to the target's velocity.

The Fast Time FFT (FFT): For a single Chirp differenced signal, perform a Fast Time FFT on the distance dimension, combined with the formula, it is possible to extract the absolute distance information of the target.

The Slow Time FFT (FFT): Process all the distance dimension results from the Chirp for the Slow Time FFT, solving for the target's radial motion speed.

Through two-dimensional FFT processing, the time-domain radar signal can be converted into a 2D power spectrum of range-Doppler (RD), achieving two-dimensional matching of target parameters.

2.3 In actual radar scenarios in 2D CA-CFAR detection, there are a large number of Gaussian noise and clutter interference. It is necessary to adaptively set the detection threshold through constant false alarm detection algorithms to ensure low false alarm rate while effectively extracting the target. This paper adopts the 2D Cellular Automata-based Constant False Alarm Rate (2D CA-CFAR).

The core mechanism of the algorithm: Centered around the current detection cell (CUT), set protection cells to avoid target signal diffusion interference, and count the average power of noise by surrounding training cells. The noise power estimation formula is:

Where \(N\) is the total number of training cells, and \(S\) is the collection of training cells.

According to the preset false alarm probability, adaptively calculate the threshold factor:

The final detection criterion: If the power of the detection cell is high, then determine that this position has an effective target, completing the target detection.

3. OFDM-ISAC communication perception integration principle OFDM integrated technology core is signal multiplexing: The same OFDM waveform not only carries communication data transmission functions but also realizes radar target perception through signal decoding, without occupying additional spectrum resources, significantly improving spectrum utilization efficiency, which is the core technology of 5G/6G integrated communication and perception.

3.1 Radar echo signal model: Assuming that an OFDM signal contains subcarriers, with a subcarrier interval of \( \Delta f \), and each frame transmits one OFDM symbol, the frequency domain transmitting signal matrix is \( [ \mathbf{c}_i ] = [ c_{i,j} ] \), where \( i \) is the subcarrier index and \( j \) is the symbol index.
In the case of multi-objective scenarios, the distances, velocities, and reflection coefficients for each target are respectively,
,   
After superimposing Gaussian noise, the frequency domain echo signal model is:
In the formula: represents the target echo delay, represents the Doppler frequency, represents the OFDM symbol duration with cyclic prefix, and represents the frequency domain Gaussian noise.
3.2 Element-level Division and Two-dimensional Perception Processing This paper adopts QPSK constant modulation, which can meet the requirement of extracting pure radar channel response through element-level division and removing communication modulation information, avoiding interference from communication data to perception.
From the channel response formula, it can be seen that: target delay information is modulated in the subcarrier dimension, and Doppler velocity information is modulated in the symbol dimension. Both are decoupled from each other and can be separately calculated through two-dimensional Fourier transform.
Distance Estimation: Perform an IFFT along the subcarrier dimension of the channel response to obtain a distance profile. The peak position corresponds to the target echo delay, which is then used to calculate the target distance.
Speed Estimation: Based on the processing results, perform an FFT along the symbol dimension to obtain a Doppler profile. The peak position corresponds to the target's Doppler frequency, thereby achieving velocity calculation.
This scheme does not require the design of a dedicated perception waveform, fully compatible with 5G communication standards, and truly realizes the integrated reuse of communication and perception.
4. MIMO radar and angle estimation principle Traditional single-antenna radar can only calculate target distance and velocity, but cannot obtain the information of direction. This paper introduces Uniform Line Array (ULA) MIMO radar, which realizes high precision measurement of target angle through spatial phase difference, constructing three-dimensional target information of distance-Doppler-angle (RDA).
4.1 Array receiving signal model: The array contains a number of elements, with the spacing between the elements (to avoid spatial domain aliasing). When the target azimuth angle is, there exists a fixed spatial phase difference between different elements. The phase shift of the ith element is:
In combination with the FMCW differential frequency signal model, the received signal of each element can be represented as:
The final radar receive data forms a cube of three-dimensional antenna data in fast time × slow time, which provides the data foundation for three-dimensional parameter solution.
4.2 3D-FFT Three-dimensional processing: Conducts three-dimensional Fourier transform on the 3D data cube, simultaneously solving for the target's three-dimensional parameters:
1. Fast time FFT: Solve for the target's distance information;
2. Slow time FFT: Solve for the target's motion velocity information;
3. Antenna dimension FFT: Calculate the target's azimuth angle through phase difference in the aerial domain (can be zero padded to improve angle resolution).
After the three-dimensional transformation, obtain the RDA three-dimensional power spectrum. Firstly, filter candidate targets on the range-Doppler plane through 2D CA-CFAR, then perform peak search in the angle dimension for each candidate target to acquire high-precision azimuth angles. Finally, use a grid-based clustering algorithm to merge neighboring spurious detection points, outputting unique target point clouds.
5. Multi-target Tracking and Kalman Filtering: The raw radar point clouds from multi-target tracking have random noise, instantaneous missed detection, and false alarm interference, making it impossible to directly output stable trajectories. This paper adopts the constant velocity (CV) Kalman filter model, combined with the nearest neighbor data association algorithm, to achieve smooth state transitions, trajectory association, and continuous tracking of multiple targets.
5.1 State Transition and Prediction Equation Construction: A four-dimensional state vector describes the target's motion state: X-axis position, X-axis velocity, Y-axis position, Y-axis velocity.
Discrete system state transition prediction equation:
Where the state transition matrix is:
The radar frame interval, and the process noise are respectively.
Prediction covariance update equation:
The process noise covariance matrix is determined by the target acceleration variance, used to describe the target's motion disturbance.
5.2 Measurement Equation and Update Equation: Radar observation outputs are target planar coordinates, observation vector, corresponding measurement matrix:
Calculate Kalman gain to balance the weight between predicted values and observed values:
The observation noise covariance matrix is corresponding to the radar detection error.
Complete the update of target state and covariance matrix:
Through prediction-update iteration, can effectively smooth out point cloud noise and correct target motion states.
5.3 Data Association and Trajectory Management: For multi-target crossing and adjacent scenes, employ the greedy nearest neighbor data association algorithm to achieve precise pairing of observation points and trajectories:
1. Calculate the Euclidean distances between all predicted trajectories and the current frame observation points:;
2. Set a distance threshold, only matching valid candidate points, selecting the minimum distance to complete association;
3. Initialize new trajectories for unmatched observation points, and mark confirmed trajectories that are not matched as missed detections.
Trajectory management adopts M/N logic criteria: if a newly established temporary trajectory hits M times within a sliding window, it becomes an official effective trajectory; if an official trajectory fails to detect consecutively, it is automatically deleted to avoid false alarms and ensure tracking stability.
6. Conclusion This paper is based on MATLAB simulation, and has completed the theoretical derivation and principle analysis of the entire link in FMCW radar signal processing, OFDM integrated communication and sense, MIMO angle estimation, and Kalman multi-objective tracking.
The study clarifies the core mechanisms of FMCW radar Dechirp processing, two-dimensional FFT parameter calculation, and CA-CFAR target detection; validates the feasibility of OFDM waveforms through element-level division to strip communication information and achieve low-cost integrated communication and sense; elucidates the three-dimensional FFT measurement of angles in MIMO arrays and point cloud clustering optimization methods; establishes a multi-objective trajectory management system based on Kalman filtering + nearest neighbor association.
The full text's theoretical derivations and simulation codes are highly compatible, covering the core technical links of radar perception and tracking completely, providing solid theoretical basis and technical references for the design of millimeter wave integrated communication and sense radar systems, algorithm optimization, and engineering implementation.

## Images

![Research on Integrated Radar Communication Based on FMCW and OFDM with Multi-Target Tracking Technology [Attached MATLAB Code]](cover.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
