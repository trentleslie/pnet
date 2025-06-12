# Pnet
Implementation of P-Net as a flexible deep learning tool to generate insights from genetic features.

Current pytorch implementation in revision.

## Model
Pnet uses the Reactome hierarchical graph as underlying structure to reduce the number of connections in a fully connected feed forward neural network. The sparse layers connect only known pathways. This limits the number of parameters to be learnt in a meaningful way and facilitate learning via gradient descent and leads to more generalizable models. 

## Installation
1. Clone the github repository and navigate into it.
2. Create the conda environment with ```conda env create -f pnet.yml```, activate it with ```conda activate pnet```
   - Note: The environment creation may take several minutes as it downloads packages.
   - If the process appears to hang during "Installing pip dependencies", this is normal - it's installing PyTorch and other large packages.
3. Run ```pip install -e . ``` to install the package locally.
4. To check successful installation run ```python test/test_data_loading.py``` which will verify basic import and file structure.

### Troubleshooting
- If you encounter `ModuleNotFoundError: No module named 'torch'` after creating the environment, the pip dependencies may not have been installed properly. You can manually install the key dependencies:
  ```bash
  pip install torch==2.2.0 torchvision==0.17.0 torchaudio==2.2.0 lightning==2.2.0 pytorch-lightning==2.2.0
  ```
- When using the conda environment, ensure you're using the correct Python interpreter by checking: `which python`. It should point to your conda environment's Python (e.g., `/path/to/conda/envs/pnet/bin/python`).

For further functional testing see the [testing notebook](https://github.com/vanallenlab/pnet/blob/main/notebooks/testing.ipynb)

## Usage
Detailed sepcific usage examples are provided in the [notebooks](https://github.com/vanallenlab/pnet/tree/main/notebooks). Generally the network structure expects gene level data for each sample (e.g. read counts, CNA indication etc.). different data modalities can be concatenated as a dictonary and passed to the pnet_loader object. A good starting place to familiarize yourself with the usage of pnet is this [example notebook](https://github.com/vanallenlab/pnet/blob/main/notebooks/SKCM_purity.ipynb)
