---
layout: page
title: Aberration Separation
description: Revealing the preference of deep-learning model on optical aberrations
img: assets/img/aberration_separation.jpg
# redirect: https://unsplash.com
importance: 4
category: work
related_publications: ZHOU2024108220, Chen_2022_TPAMI
---

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/publication_preview/aberration separation.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Abstract: 
The joint design of the optical system and the downstream algorithm is a challenging and promising task. Due to the demand for balancing the global optima of imaging systems and the computational cost of physical simulation, existing methods cannot achieve efficient joint design of complex systems such as smartphones and drones. In this work, starting from the perspective of the optical design, we characterize the optics with separated aberrations. Additionally, to bridge the hardware and software without gradients, an image simulation system is presented to reproduce the genuine imaging procedure of lenses with large field-of-views. As for aberration correction, we propose a network to perceive and correct the spatially varying aberrations and validate its superiority over state-of-the-art methods. Comprehensive experiments reveal that the preference for correcting separated aberrations in joint design is as follows: longitudinal chromatic aberration, lateral chromatic aberration, spherical aberration, field curvature, and coma, with astigmatism coming last. Drawing from the preference, a 10% reduction in the total track length of the consumer-level mobile phone lens module is accomplished. Moreover, this procedure spares more space for manufacturing deviations, realizing extreme-quality enhancement of computational photography. The optimization paradigm provides innovative insight into the practical joint design of sophisticated optical systems and post-processing algorithms.

## Background:  
With the popularity of mobile photography (e.g., smartphones, action cameras, drones, etc.), the optics of cameras tend to be more miniaturized and lightweight for imaging. The engaged methods can be divided into several categories: 1. from the perspective of image processing and 2. from the consideration of the whole system with end-to-end optimization. While they all have some imitations in implementation. So **what if we start from the perspective of the optical design?**

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 60%;">
            {% include figure.html path="assets/img/background_olen.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="caption">
    Comparison of the imaging system’s design methods: (a) Fixed lens design remedied by post-processing algorithms, (b) End-to-end method, and (c) Ours (Step-by-step joint design).
</div>

## Method:
Our aim is to investigate the preference for correcting separated aberrations in joint optic-image design.
<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/main_olen.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of our joint optic-image design method.
</div>

Compound lens design has various degrees of freedom, and traversing all the potential designs is time-consuming. Following the vector quantization in auto-regressive, we note that mapping intricate data (e.g., lens configuration) to a highdimensional representation (e.g., aberrations coefficients) could realize a more comprehensive characterization. Fortunately, imaging quality is mostly corrupted by the degradation of primary aberrations, which happens to be quantified by Seidel coefficients. Therefore, considering the trade-off between accuracy and concision, Seidel is a better option in the view of aberration decomposition. Here we show the design results of our aberration separation strategy

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 60%;">
            {% include figure.html path="assets/img/separation_olen.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>

And we analysis the distribution of degradation across all designs. This results shows that our strategy could obtain adquately sampled aberration/degration space.

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 60%;">
            {% include figure.html path="assets/img/distribution_analysis_olen.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="caption">
    Distribution analysis of degradation across all designs. From the perspectives of the MTF difference between tangential and sagittal directions (the upper half sector in (a)) and the PSF energy dispersion size (the upper half sector in (b)), (a) and (b) outline the differences in deterioration produced by various types of designs.
</div>

By the design strategy of aberration separation, we work with the same restoration pipeline used in [Imaging Simulation](https://tangeego.github.io/projects/1_project/). In this way, we could analyse which aberrations are easier for correction, and which aberrations are more difficult to restore.

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 60%;">
            {% include figure.html path="assets/img/easytohard_olen.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="caption">
    The line charts of (a) PSNR and (b) SSIM evaluation before vs. after correction.
</div>

From the results above, we can easily find out that the deep learning model has a preference to different optical aberrations. The sequence is given as: **CL > CT > SPHA > FCUR > COMA > ASTI** (larger means easier to correct).

After getting this prior, we start to use this as a guidance for photography object lens design. And we realize the optimization of **10% height decrease** in the main camera of mobile phone (from **6.1mm** to **5.5mm**).

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 60%;">
            {% include figure.html path="assets/img/thickness_olen.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="caption">
    lower TTL lens, but could have evenly matched imaging quality after the same deep-learning reconstruction.
</div>

**We do the same analysis procedures on two different objective lens configuration, and the conclusions are the same!**

## Our Main Observation: 
What optical aberrations do you need to control first in optical designing?

Follow me and say **ASTI > COMA > FCUR > SPHA > CT > CL** (loudly)

Let this **MANTRA** makes your **LENS DESIGN BETTER**!!!