# Blood-cell image generation

[Português](README.md) | [English](README.en.md)

Experimental comparison of five generative architectures on **BloodMNIST**: VAE, DAE, DCGAN, CGAN and a diffusion model. Each notebook covers a baseline implementation, optimisation, final training, sample generation and quantitative evaluation.

> This is an academic research project. The synthetic images and conclusions have not been validated for diagnosis, clinical training or medical decisions.

## Dataset

BloodMNIST contains more than 17,000 RGB images of peripheral blood smears at 28 × 28 pixels, across eight cell types: neutrophils, eosinophils, basophils, lymphocytes, monocytes, immature granulocytes, erythroblasts and platelets.

The notebooks use the MedMNIST API and download the data when `download=True`; the dataset does not need to be stored in the repository.

## Models

| Notebook | Approach | Question explored |
|---|---|---|
| `VAE.ipynb` | Variational Autoencoder | quality, stability and latent-space structure |
| `DAE.ipynb` | Denoising Autoencoder | reconstruction/denoising versus generation |
| `DCGAN.ipynb` | Deep Convolutional GAN | visual quality under adversarial training |
| `CGAN.ipynb` | Conditional GAN | generation conditioned on cell class |
| `DiffusionModel.ipynb` | U-Net and diffusion process | stability, attention and computational cost |

## Evaluation and results

The final evaluation uses **10,000 real and 10,000 synthetic images**, computes Fréchet Inception Distance (FID) with InceptionV3 and repeats the measurement five times with different seeds. A lower FID indicates closer feature distributions; it does not prove clinical validity or the absence of artefacts.

| Final model | Mean FID ± standard deviation | Interpretation |
|---|---:|---|
| DCGAN | **37.83 ± 0.49** | best measured generative quality |
| VAE | 67.94 ± 0.37 | best stability–quality balance |
| Diffusion | 73.43 ± 0.11 | competitive, but slower and more demanding |
| CGAN | 146.99 ± 0.83 | class control with a quality trade-off |
| DAE — reconstruction | 9.67 ± 0.05 | excellent on the reconstruction task |
| DAE — generation | 384.32 ± 0.30 | unsuitable for generation from pure noise |

DCGAN achieved the best absolute FID despite the usual instability of adversarial training. DAE highlights an important distinction: reconstructing corrupted images and generating new samples are different goals, so its reconstruction FID is not directly comparable with the generative performance of the other models.

## Technology stack

- Python and Jupyter/Google Colab;
- PyTorch, torchvision and TensorBoard;
- MedMNIST/BloodMNIST;
- TorchMetrics and InceptionV3 for FID;
- NumPy, SciPy, Matplotlib and tqdm.

## Repository structure

```text
Generation_of_Images/
├── VAE.ipynb
├── DAE.ipynb
├── DCGAN.ipynb
├── CGAN.ipynb
├── DiffusionModel.ipynb
├── Generation_of_Images.pdf
├── requirements.txt
├── README.md
└── README.en.md
```

## Run the experiments

### Google Colab — original environment

1. Open the chosen notebook in Google Colab.
2. Enable a GPU runtime.
3. Copy the notebook to Drive.
4. Adjust `project_path`/`DRIVE_PATH` in the first cells.
5. Run the cells in order.

Several notebooks mount `/content/drive` and contain paths from the original environment; replace them before training.

### Local environment

```bash
git clone https://github.com/josepedrocunhazzz/Generation_of_Images.git
cd Generation_of_Images
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter lab
```

Locally, skip `google.colab`-specific cells, replace Drive paths with local directories and select `cuda`, `mps` or `cpu` for the available hardware. Final training runs (100–150 epochs), grid searches and generation of 10,000 images require time, storage and preferably a GPU.

## Academic context

Project presented in **José Cunha's** portfolio and developed in the University of Coimbra Master's programme in Data Science and Engineering. Full academic authorship, architectures, search spaces, curves, samples and discussion are recorded in the [report](Generation_of_Images.pdf).
