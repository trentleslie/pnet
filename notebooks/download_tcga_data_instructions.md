# Instructions for Downloading TCGA Data

The SKCM_purity.ipynb notebook expects TCGA SKCM (Skin Cutaneous Melanoma) data that is not included in this repository. Here's how to obtain it:

## Option 1: Download from cBioPortal

1. Visit [cBioPortal](https://www.cbioportal.org/)
2. Search for "Skin Cutaneous Melanoma (TCGA, PanCancer Atlas)"
3. Download the following files:
   - `data_mrna_seq_v2_rsem_zscores_ref_all_samples.txt` - RNA expression z-scores
   - `data_cna.txt` - Copy number alterations
   - `data_clinical_sample.txt` - Clinical data

4. Create the expected directory structure:
   ```bash
   mkdir -p /mnt/disks/pancan/data/skcm_tcga_pan_can_atlas_2018
   ```

5. Place the downloaded files in that directory

## Option 2: Use GDC Data Portal

1. Visit [GDC Data Portal](https://portal.gdc.cancer.gov/)
2. Filter for:
   - Project: TCGA-SKCM
   - Data Category: Transcriptome Profiling, Copy Number Variation
3. Download and process the data into the expected format

## Option 3: Use the Test Data (Recommended for Testing)

If you just want to test P-Net functionality without downloading large datasets:

1. Use the provided test data notebook: `SKCM_purity_test_data.ipynb`
2. This uses small sample data from `data/test_data/` directory
3. It demonstrates all P-Net features with a smaller dataset

## Expected Data Format

The `util.load_tcga_dataset()` function expects:

1. **RNA data** (`data_mrna_seq_v2_rsem_zscores_ref_all_samples.txt`):
   - Tab-separated file
   - Columns: Hugo_Symbol, Entrez_Gene_Id, followed by sample IDs
   - Values: Z-scores of RNA expression

2. **CNA data** (`data_cna.txt`):
   - Tab-separated file  
   - Columns: Hugo_Symbol, Entrez_Gene_Id, followed by sample IDs
   - Values: Copy number alterations (-2, -1, 0, 1, 2)

3. **Clinical data** (for tumor purity):
   - The notebook expects purity values from `TCGA_mastercalls.abs_tables_JSedit.fixed.txt`
   - This can be downloaded from the [PanCanAtlas publications page](https://gdc.cancer.gov/about-data/publications/pancanatlas)

## Alternative: Create Synthetic Data

For development/testing, you can create synthetic data that matches the expected format:

```python
import pandas as pd
import numpy as np

# Create synthetic RNA data
n_genes = 1000
n_samples = 100
genes = [f'GENE_{i}' for i in range(n_genes)]
samples = [f'TCGA-XX-{i:04d}-01' for i in range(n_samples)]

rna_data = pd.DataFrame(
    np.random.randn(n_genes, n_samples),
    index=genes,
    columns=samples
)
rna_data.index.name = 'Hugo_Symbol'
rna_data.insert(0, 'Entrez_Gene_Id', range(1, n_genes + 1))

# Save in expected format
rna_data.to_csv('data_mrna_seq_v2_rsem_zscores_ref_all_samples.txt', sep='\t')
```

## Note on Cancer Gene Lists

The notebook also references cancer gene lists that should be placed in:
- `../../pnet_database/genes/cancer_genes_demo.txt`

You can use well-known cancer gene sets like:
- COSMIC Cancer Gene Census
- OncoKB Cancer Gene List
- MSK-IMPACT gene panel