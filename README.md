# angular-spectrum-capstone

Got it — here’s the entire README.md as one single paste-ready Markdown block, no code fences, no extra formatting required. Just copy and paste this directly into the GitHub file editor or any Markdown file:

⸻

Angular Spectrum Propagation Toolkit

A high-speed Python tool for simulating free-space diffraction behind metasurfaces using the angular spectrum method. Designed to post-process 1D field slices exported from COMSOL, this code reconstructs full 2D intensity and phase maps orders of magnitude faster than FEM continuation.

⸻



⸻

🔬 Motivation

Full-wave solvers like COMSOL capture near-field effects with high accuracy — but extending the field solution downstream (e.g. 50–100 μm) becomes computationally expensive. This project solves that bottleneck.

By importing just a 1D complex field slice E(x, z=0) from COMSOL and applying a DFT-based propagation algorithm, we can:
- Visualize focusing and diffraction in under 1 second
- Extract spatial frequencies, periodicities, and resolution limits
- Reproduce Talbot carpets from periodic input structures

⸻

📈 Highlights
- 🚀 Fast: Propagates 2000 planes in <0.7s on a laptop
- 🧽 Edge Conditioning: Trim-and-extend to suppress Gibbs artifacts
- 🧠 Spectral Analysis: Peak detection yields resolution estimates, NA, and periodicities
- 🌀 Talbot Imaging: Reconstructs self-images of periodic gratings
- 📊 COMSOL Validation: RMS intensity error <3%, phase error <5 mrad

⸻

📦 Requirements

pip install numpy matplotlib scipy pandas

⸻

⚙️ How It Works

The field is expanded in plane waves via a Discrete Fourier Transform:

E(x, z) = (1 / 2π) ∫  Ê(kx) · exp[i(kx·x + kz·z)] dkx

Propagation multiplies each mode by a kernel:

H(kx; z) = exp[i √(k² − kx²) · z]

Then the field is reconstructed via inverse DFT.

⸻

🚀 How to Use

from angular_spectrum import propagate_field_1d
import numpy as np

Load 1D field slice (complex format)

E_init = np.loadtxt(“comsol_slice.csv”, delimiter=”,”, dtype=complex)

Parameters

- Lx = 120e-6            # width of field
- wavelength = 4.05e-6   # mid-IR
- eps_r = 1.0            # refractive index squared
- z_vals = np.linspace(0, 55e-6, 2000)

Run propagation

E_z, A_kx, kx = propagate_field_1d(E_init, Lx, z_vals, wavelength, eps_r, pad_factor=2, trim_edges=10)

⸻

🧪 Validation Against COMSOL

The reconstructed field agrees with COMSOL reference solutions within:
- <3% RMS intensity error
- <5 mrad RMS phase error

With edge conditioning, spectral resolution improves and ringing is reduced.


⸻

🌀 Talbot Imaging

The toolkit reproduces Talbot self-imaging via spectral phase accumulation.
For a grating of period a, the Talbot length is:

z_T = 2a² / λ

At z = n · z_T, the field self-images.


⸻

📊 Spectral Analysis

We extract resolution metrics by analyzing the angular spectrum:
- Mean peak spacing → spatial period
- Max |kx| → Numerical Aperture
- Peak phases → modal contributions


⸻

📜 Paper Reference

John Davis, Fast DFT-Based Angular-Spectrum Post-Processing of COMSOL Slices for Mid-IR Fresnel-Lens Metasurfaces, May 2025.
See angular_spectrum_v5.pdf in this repo for full details.

⸻

🧠 Acknowledgments

Thanks to:
- Sreeja Purkait for COMSOL lens data
- Dr. Viktor Podolskiy for insightful discussions

⸻

🔓 License

MIT License — free to use, modify, and distribute with attribution.
