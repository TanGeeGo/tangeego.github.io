---
layout: page
title: Degradation Transfer
description: if the degradation on one image can transfer to another picture?
# img: assets/img/degradation_transfer.png
importance: 2
category: work
related_publications: Chen_2021_ICCV, Lin_2022_OE
---

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/publication_preview/degradation calibration.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Abstract: 
To meet the space limitation of optical elements, free-form surfaces or high-order aspherical lenses are adopted in mobile cameras to compress volume. However, the application of free-form surfaces also introduces the problem of image quality mutation. Existing model-based deconvolution methods are inefficient in dealing with the degradation that shows a wide range of spatial variants over regions. And the deep learning techniques in low-level and physics-based vision suffer from a lack of accurate data. To address this issue, we develop a degradation framework to estimate the spatially variant point spread functions (PSFs) of mobile cameras. When input extreme-quality digital images, the proposed framework generates degraded images sharing a common domain with real-world photographs. Supplied with the synthetic image pairs, we design a Field-Of-View shared kernel prediction network (FOV-KPN) to perform spatial-adaptive reconstruction on real degraded photos. Extensive experiments demonstrate that the proposed approach achieves extreme-quality computational imaging and outperforms the state-of-the-art methods. Furthermore, we illustrate that our technique can be integrated into existing postprocessing systems, resulting in significantly improved visual quality.

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 50%;">
            {% include figure.html path="assets/img/teaserfigure_iccv.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="caption">
    The reconstructions of a real-world image captured by HUAWEI HONOR 20.
</div>

## Background:  
The image quality always change greatly in the edge of FoV, which is generally caused by the manufacturing error. Directly measuring these deviations is really difficult after the lens is mounted, so we consider transfer the degradation of one image to another uncorrupted photo. Below, you will see the image mutation in the edge of mobile phone camera.

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 60%;">
            {% include figure.html path="assets/img/background_iccv.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="caption">
    Image quality mutation of the mobile camera. One can see that in the magnified region (right), the two sides separated by the dotted line have a significant difference in blurring.
</div>

## Method: 
Our aim is to transfer the degradation of one image to another. To achieve this,

- we built the first deep-learning-based calibration to densely represent the optical degradation of deviated camera.

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/method_iccv.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of the degradation framework. In stage I, backward transfer constructs the data pairs for the training procedure of degradation transfer. And in stage II, each degradation transfer is tailored to one FOV, aiming at estimating the spatially variant PSFs of different FOVs and generating realistic imaging results.
</div>

The degradation transfer used for representation is controlled by many constraints:

$$\mathcal{L} = \alpha \mathcal{L}_{fidelity} + \beta \mathcal{L}_{sum2one} + \gamma \mathcal{L}_{boundary} + \delta \mathcal{L}_{sym}$$

here alpha, beta, gamma, delta are the weight coefficients of different loss functions. 

$$\mathcal{L}_{fidelity}=||x^{d}-y||_{1}$$ 

$$\mathcal{L}_{sum2one}=|1-\Sigma k_{i,j}|$$ 

$$\mathcal{L}_{boundary}=\Sigma |k_{i,j} \cdot m_{i,j}|$$ 

$$\mathcal{L}_{sym} = \Sigma_{i,j}(k(i,j)-\frac{1}{2}(k(i,j)+k_{sym}(-i, -j)))^{2}$$

in this way, we force each PSF to be similar to its central symmetry counterpart. The unprocess and process pipeline is the same as the [Imaging Simulation](https://tangeego.github.io/projects/1_project/)

After training the degradation transfer model, we could use it to copy the behavior of degradation and to influence the clean image. The evaluations on natural photographs are shown as follows

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/transfer_iccv.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Comparison of real photographs and simulation results. (a) is the warpped digital images, (b) is the simulation result of our framework when input (a), and (c) is the real photograph.
</div>

We construct a large dataset for each camera, and propose two deep learning models for correction. One model is blind and another is non-blind.

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 80%;">
            {% include figure.html path="assets/img/network1_iccv.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="caption">
    The UNet-based network architecture is shown on the top, and the layer configurations illustrate with different colored blocks (bottom left). The FOV Block, Deformable ResBlock, and KPN Block are detailed in the bottom right corner.
</div>

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        <div style="max-width: 80%;">
            {% include figure.html path="assets/img/network2_iccv.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
<div class="caption">
    This model work like a deep weiner filter, which aims to perform frequency self-adaptive restoration on the input data. It engage with the predetermined PSFs to deconvolution and predict the targeted 1/SNR map.
</div>

the key to perform deconvolution in latent space could be formulated as,

$$I_{latent}(x,y)=\Sigma_{i}^{m} \mathcal{F}^{-1}(\eta_{i} \cdot \frac{conj(\mathcal{F}(k_{i}))}{|\mathcal{F}(k_{i})|^{2}+\Sigma_{j}^{n} \phi_{ij} \cdot |\mathcal{F}(p_{j})|^{2}} \cdot \mathcal{F}(I_{degraded}(x,y)))$$

## Experiments: 
We show some visualization here, for detailed comparisons and illustrations, please refer to our paper.
<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/comparisons_iccv.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row">
    <div class="col-sm d-flex justify-content-center mt-3 mt-md-0">
        {% include figure.html path="assets/img/isp_iccv.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Real image restoration. The results of different methods magnify in the right and the positions highlight on the left.
</div>

## Our Main Observation: 
- The degradation of one image could be **transferred**. In this way, we could realize more accurate data-pair generation. As mentioned before, we constructed a degradation framework for estimating the spatially variant PSF of a specific camera, including but not limited to the device that shows image quality mutation. The proposed framework generates authentic imaging results that resemble real-world photographs, where a lot of shooting, registration, and color correction are needless.
- The imaging quality of low-end mobile terminal has the potential to surpass high-end DSLR.

## Limitations:
- The representation only work for one specific camera. If you want to transplant the transfer to another device, retraining is needed.
- Overfitting will introduce some wried texture in some high resolution natural images.
