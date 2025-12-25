# Conditional-Diffusion-for-Brightfield-to-Fluorescence-Translation
Conditional diffusion model for translating brightfield microscopy images into red and green fluorescence channels.
Conditional Diffusion for Brightfield to Fluorescence Translation

This repository implements a conditional diffusion (DDPM) model to translate Brightfield (BF) microscopy images into corresponding fluorescence images (Red or Green channels).

The approach enables virtual fluorescence microscopy, reducing the need for physical staining, long acquisition times, and phototoxic effects.

Problem Statement

Fluorescence microscopy provides rich biological information but is expensive and time-consuming.

Goal:
Learn a conditional generative model that predicts fluorescence images from a given Brightfield image and a specified fluorescence channel (Red or Green).

Key Idea

Instead of directly predicting fluorescence images, the model learns to iteratively denoise fluorescence images conditioned on:

Brightfield image structure

Desired fluorescence channel (Red / Green)

This is achieved using a conditional denoising diffusion probabilistic model (DDPM).

Model Overview
Input to the Network (6 Channels)

Noisy fluorescence image (1 channel)

Brightfield RGB image (3 channels)

Condition map (Red / Green one-hot, broadcast spatially) (2 channels)

Total input shape:

[6, 256, 256]

Backbone

Conditional UNet

Predicts the noise added at each diffusion step

Diffusion Process (Plain Explanation)
Forward Diffusion

Noise is gradually added to a clean fluorescence image over multiple timesteps until it becomes pure Gaussian noise.

Reverse Diffusion

Starting from random noise, the model:

Predicts the noise component

Removes noise step-by-step

Generates a clean fluorescence image conditioned on:

Brightfield image

Fluorescence channel (Red or Green)

Dataset

Source:
Kaggle – Brightfield vs Fluorescent Staining Dataset

Dataset Split:

Training: 159 samples

Validation: 42 samples

Testing: 51 samples (held-out environment)

Preprocessing:

Resize images to 256 × 256

Normalize pixel values to range [-1, 1]

One fluorescence channel selected per training iteration

Training Details

Optimizer: AdamW

Training epochs: 100

Exponential Moving Average (EMA) of model weights

EMA weights used for inference to improve stability

Loss Functions
1. Denoising Loss

L1 loss between predicted noise and true noise

Encourages accurate diffusion step prediction

2. Perceptual Loss

VGG16 feature-based loss

Encourages preservation of biologically meaningful structures

Total Loss

Total loss = Denoising loss + 0.01 × Perceptual loss

Inference

At inference time:

Start from random Gaussian noise

Run the reverse diffusion process

Condition on:

Brightfield image

Selected fluorescence channel (Red or Green)

The model can generate different valid fluorescence samples from the same Brightfield input.

Results

Stable convergence over 100 epochs

Realistic fluorescence structure aligned with Brightfield morphology

Successful generation of both Red and Green fluorescence channels from a single Brightfield image

Example predictions and loss curves are included in the notebook.
