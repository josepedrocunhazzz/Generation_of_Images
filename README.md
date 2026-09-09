# Geração de imagens de células sanguíneas

[Português](README.md) | [English](README.en.md)

Comparação experimental de cinco arquiteturas generativas aplicadas ao **BloodMNIST**: VAE, DAE, DCGAN, CGAN e modelo de difusão. Cada notebook percorre implementação base, otimização, treino final, geração de amostras e avaliação quantitativa.

> Projeto académico de investigação. As imagens sintéticas e conclusões não foram validadas para diagnóstico, treino clínico ou decisão médica.

## Dataset

O BloodMNIST contém mais de 17 000 imagens RGB de esfregaços de sangue periférico, com resolução de 28 × 28 píxeis e oito tipos celulares: neutrófilos, eosinófilos, basófilos, linfócitos, monócitos, granulócitos imaturos, eritroblastos e plaquetas.

Os notebooks usam a API do MedMNIST e descarregam os dados quando `download=True`; o dataset não precisa de ser guardado no repositório.

## Modelos estudados

| Notebook | Abordagem | Questão explorada |
|---|---|---|
| `VAE.ipynb` | Variational Autoencoder | qualidade, estabilidade e estrutura do espaço latente |
| `DAE.ipynb` | Denoising Autoencoder | diferença entre reconstrução/remoção de ruído e geração |
| `DCGAN.ipynb` | Deep Convolutional GAN | qualidade visual com treino adversarial |
| `CGAN.ipynb` | Conditional GAN | geração condicionada pela classe celular |
| `DiffusionModel.ipynb` | U-Net e processo de difusão | estabilidade, atenção e custo computacional |

## Avaliação e resultados

A avaliação final usa **10 000 imagens reais e 10 000 sintéticas**, calcula Fréchet Inception Distance (FID) com InceptionV3 e repete o processo cinco vezes com sementes diferentes. Um FID inferior indica maior proximidade entre as distribuições de features; não prova validade clínica nem ausência de artefactos.

| Modelo final | FID médio ± desvio-padrão | Leitura |
|---|---:|---|
| DCGAN | **37,83 ± 0,49** | melhor qualidade generativa medida |
| VAE | 67,94 ± 0,37 | melhor compromisso entre estabilidade e qualidade |
| Difusão | 73,43 ± 0,11 | competitivo, mas mais lento e exigente |
| CGAN | 146,99 ± 0,83 | controlo por classe com perda de qualidade |
| DAE — reconstrução | 9,67 ± 0,05 | excelente na tarefa de reconstrução |
| DAE — geração | 384,32 ± 0,30 | inadequado para gerar a partir de ruído puro |

O DCGAN produziu o melhor FID absoluto, apesar da instabilidade típica do treino adversarial. O DAE ilustra uma distinção importante: reconstruir imagens corrompidas e gerar amostras novas são objetivos diferentes, pelo que o seu FID de reconstrução não é diretamente comparável ao desempenho generativo dos restantes modelos.

## Tecnologias

- Python e Jupyter/Google Colab;
- PyTorch, torchvision e TensorBoard;
- MedMNIST/BloodMNIST;
- TorchMetrics e InceptionV3 para FID;
- NumPy, SciPy, Matplotlib e tqdm.

## Estrutura

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

## Executar

### Google Colab — ambiente original

1. Abrir o notebook pretendido no Google Colab.
2. Ativar um runtime com GPU.
3. Copiar o notebook para o Drive.
4. Ajustar `project_path`/`DRIVE_PATH` nas primeiras células.
5. Executar as células por ordem.

Vários notebooks montam `/content/drive` e contêm caminhos do ambiente original; estes caminhos têm de ser substituídos antes do treino.

### Ambiente local

```bash
git clone https://github.com/josepedrocunhazzz/Generation_of_Images.git
cd Generation_of_Images
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter lab
```

Num ambiente local, ignorar as células específicas de `google.colab`, substituir os caminhos do Drive por diretórios locais e selecionar `cuda`, `mps` ou `cpu` conforme o hardware. Os treinos finais (100–150 épocas), a grid search e a geração de 10 000 imagens requerem tempo, armazenamento e, idealmente, uma GPU.

## Contexto académico

Projeto apresentado no portefólio de **José Cunha**, desenvolvido no Mestrado em Engenharia e Ciência de Dados da Universidade de Coimbra. A autoria académica completa, arquiteturas, espaços de pesquisa, curvas, amostras e discussão encontram-se no [relatório](Generation_of_Images.pdf).
