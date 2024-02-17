---
layout: page
title: Tolerance Modeling
description: how to predict the manufacturing error in mass production?
img: assets/img/tolerance_calibration.png
# redirect: https://unsplash.com
importance: 3
category: work
related_publications: Chen_2022_TPAMI, CHEN202364
---

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/publication_preview/tolerance.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Abstract: 
Correcting the optical aberrations and the manufacturing deviations of cameras is a challenging task. Due to the limitation on volume and the demand for mass production, existing mobile terminals cannot rectify optical degradation. In this work, we systematically construct the perturbed lens system model to illustrate the relationship between the deviated system parameters and the spatial frequency response (SFR) measured from photographs. To further address this issue, an optimization framework is proposed based on this model to build proxy cameras from the machining samples’ SFRs. Engaging with the proxy cameras, we synthetic data pairs, which encode the optical aberrations and the random manufacturing biases, for training the learning-based algorithms. In correcting aberration, although promising results have been shown recently with convolutional neural networks, they are hard to generalize to stochastic machining biases. Therefore, we propose a dilated Omni-dimensional dynamic convolution (DOConv) and implement it in post-processing to account for the manufacturing degradation. Extensive experiments which evaluate multiple samples of two representative devices demonstrate that the proposed optimization framework accurately constructs the proxy camera. And the dynamic processing model is well-adapted to manufacturing deviations of different cameras, realizing perfect computational photography. The evaluation shows that the proposed method bridges the gap between optical design, system machining, and postprocessing pipeline, shedding light on the joint of image signal reception (lens and sensor) and image signal processing (ISP).

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/teaserfigure_tpami.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Manufacturing biases adaptation and comparisons. (a) centrosymmetric PSF calculated by the proxy camera of different machining samples (Best viewed with zoom). (b) magnified comparisons of the photographs taken in the same scene. We show the measured degradation of each Phone (converted to sRGB for visualization) and our restoration output from the same ISP pipeline. And the results of high-end DSLR cameras are shown for reference (captured under same aperture for comparisons).
</div>

## Background: 
In the machining and assembly procedure of imaging systems, deflection and manufacturing bias affect the shape and positions of lenses. Even subtle shape or position variations will introduce additional aberrations, which significantly degrade the optical performance of cameras. To be more specific, the deflection will lead to the point spread function (PSF) differenceof the symmetrical field-of-view (FoV), and the manufacturing deviation will cause the overall decrease in SFR. Hence, analyzing the biases between the ideal and manufacturing is a critical issue in the optomechanical design of the imaging system, and it is essential for improving processing quality and controlling the cost.

## Method:
We built the **physical-based camera perturbation model** to predict the deviation of systems, aiming at constructing proxy cameras whose imaging results are close to reality.

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/method_tpami.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of the perturbed optical system model. (a) We simulate the imaging results of the ideal edge by the camera’s parameters (top) and acquire the measured edge by photographing with real devices (bottom). (b) The procedure from edge profile to SFR goes through projection, differential, and DFT. In this way, we obtain the SFRA of ideal design and the measured edge. (c) Set the measured SFRAs as targets to optimize the system parameters and predict the proxy camera by damped least-squares iteration.
</div>

The procedure of the turbulance prediction is just like the procedure of optical system optimization. Different from the traditional optimization used in Zemax or CodeV, this framework consider the sampling of sensor plane and the simulated with a comprehensive imaging pipeline.

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/comparisons_wz_zemax_tpami.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Qualitative evaluation on PSF and imaging simulation. We visualize the centrosymmetric PSFs (10 x resampling for detailed comparisons) of the proxy cameras constructed from one machining sample of the Phone (the measured PSFs are shown in the sensor resolution). For the proxy camera, the resolutions of PSFs are 50, 50, 80, 80, and 120 of FoV 0.1, 0.3, 0.5, 0.7, and 0.9, respectively. And in measurement, the resolutions of PSFs are 5, 8, and 12 for FoV 0.1, 0.5, and 0.9. We present the imaging simulation results and their SSIM compared with actual measurements.
</div>

the proposed perturbation model outperforms optical design program (*e.g.*, CODE V) and other SOTA algorithms. The proposed **dynamic post-processing pipeline** shed light on the joint of image signal reception (lens and sensor) and image signal processing (ISP).

For training the restoring network, we use proxy cameras to generate the data pairs that characterize the mapping of optical degradation. To be specific, we sample many real device to get the range of tolerance, and construct a large dataset for modeling the cameras in mass production. Then, we propose two deep learning models for correction. One model is blind and another is non-blind. The correction results show that **simulating mass production** could realize minimal computational cost and fast adapting to the data acquisition of new devices with tolerance.

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 80%;">
            {% include figure.html path="assets/img/network1_tpami.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="caption">
    Overview of the dynamic postprocessing pipeline. (a) an optical degradation correction module is embedded into the ISP pipeline of mobile terminal. (b) we propose a dynamic postprocessing model based on dilated Omni-dimensional dynamic convolution, aiming at self-adaptively tackling the stochastic manufacturing deviation. All the layer configurations are marked with different colored blocks.
</div>

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 80%;">
            {% include figure.html path="assets/img/publication_preview/prior_quantization.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="caption">
    The detail of the proposed prior quantization model. The layer configurations are illustrated with different colored blocks. It engage with many pre-determined optical prior and quantify them into different processing mode.
</div>

## Experiments: 
We show some visualization here, for detailed comparisons and illustrations, please refer to our paper.
<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/comparisons1_tpami.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Adaptation for manufacturing deviation. (a) we show the magnified patch to illustrate the image quality mutation of Phone. The actual checkers’ restoration of different machining samples are present for comparison. And we evaluate the average SFR (MTF) enhancements on the machining samples of test set. (b) the natural photograph restoration when applied on different machining samples of Phone.
</div>

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/comparisons2_tpami.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Real image restoration comparison, where the magnified position is highlighted in red. We show multiple samples of Honor 20 and iPhone 12 to evaluate the generalization of the proposed method. The MTF enhancements of different FoVs are plotted.
</div>

We also present an interesting ablation here, evaluating how the **different optical prior** influence the restoration.

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/comparisons3_tpami.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Impact of each pixel-level prior on the image restoration. We show the influences on restored images that are carried out by the mismatch of pixel-level priors, where (a) is the SFR Cube, (b) is the FoV, (c) is the Spectral Cube, and (d) is the Noise Variance.
</div>

The benifits to the **down-stream application**:
<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 60%;">
            {% include figure.html path="assets/img/comparisons4_tpami.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 60%;">    
            {% include figure.html path="assets/img/comparisons5_tpami.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>

## Our Main Observation: 
- With the proxy cameras from multiple machining samples, we synthesized the data pairs with complex degenerate distributions, aiming at encoding the optical aberrations and the random bias introduced during processing. 
- It is convenient to deploy the proposed model in the mass production of arbitrary imaging devices. And this work has been applied to some mobile terminals to realize significantly improved imaging.