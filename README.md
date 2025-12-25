🧬 Conditional Diffusion for Brightfield → Fluorescence Translation

This repository implements a conditional diffusion (DDPM) model to translate
Brightfield (BF) microscopy images into corresponding fluorescence images
(Red or Green channels).

✨ This enables virtual fluorescence microscopy, reducing:

Physical staining requirements

Long acquisition times

Phototoxic effects

🔬 Problem Statement

Fluorescence microscopy provides rich biological insights, but it is:

Expensive

Time-consuming

Potentially harmful to live samples

🎯 Goal

Learn a conditional generative model that predicts fluorescence images from:

A Brightfield image

A specified fluorescence channel (Red or Green)

💡 Key Idea

Instead of directly predicting fluorescence images, the model learns to:

Iteratively denoise fluorescence images
while conditioning on structural and modality information

The conditioning includes:

Brightfield image structure

Desired fluorescence channel (Red / Green)

This is achieved using a conditional denoising diffusion probabilistic model (DDPM).

🧠 Model Overview
🔹 Input to the Network (6 Channels)

Noisy fluorescence image — 1 channel

Brightfield RGB image — 3 channels

Condition map (Red / Green one-hot, spatially broadcast) — 2 channels

Total input shape:

[6, 256, 256]

🔹 Backbone Network

Conditional UNet

Predicts the noise added at each diffusion timestep

🔁 Diffusion Process (Plain Explanation)
➕ Forward Diffusion

Noise is gradually added to a clean fluorescence image

After many steps, the image becomes pure Gaussian noise

➖ Reverse Diffusion

Starting from random noise, the model:

Predicts the noise component

Removes noise step-by-step

Generates a clean fluorescence image conditioned on:

Brightfield image

Fluorescence channel (Red or Green)

📦 Dataset

Source:
🔗 Kaggle – Brightfield vs Fluorescent Staining Dataset

📊 Dataset Split

Training: 159 samples

Validation: 42 samples

Testing: 51 samples (held-out environment)

🧹 Preprocessing

Resize images to 256 × 256

Normalize pixel values to [-1, 1]

One fluorescence channel selected per training iteration

⚙️ Training Details

Optimizer: AdamW

Epochs: 100

EMA (Exponential Moving Average) applied to model weights

EMA weights used during inference for stable and smooth outputs

🎯 Loss Functions
1️⃣ Denoising Loss

L1 loss between predicted and true noise

Encourages accurate diffusion step prediction

2️⃣ Perceptual Loss

VGG16 feature-based loss

Preserves biologically meaningful structures

🔢 Total Loss

Total = Denoising loss + 0.01 × Perceptual loss

🚀 Inference

During inference:

Start from random Gaussian noise

Run the reverse diffusion process

Condition on:

Brightfield image

Selected fluorescence channel (Red or Green)

✨ The model can generate multiple valid fluorescence samples from the same Brightfield input.

📈 Results

Stable convergence over 100 epochs

Fluorescence structures align well with Brightfield morphology

Successful generation of both Red and Green channels from a single BF image

📌 Example predictions and loss curves are included in the notebook.
