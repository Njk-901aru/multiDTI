# Deep learning-based integration of multi-interactome data for protein-compound interaction prediction

## Requirements
* python anaconda3(installed rdkit, chainer)

  * pubchempy (for converting PunChem ID into smiles)

  * node2vec, networkx(for applying node2vec algorithm)

  * pyensembl (for converting ensembl protein ID into amino acid sequences)
  ```bash
  pyensembl install --release 93 --species human
  ```

## Usage
1. Make input file for integrate model
Hard dataset already processed is contained in [dataset/hard_dataset](https://github.com/Njk-901aru/multiDTI/blob/master/dataset/hard_dataset) except for 'protein.npy'(protein onehot).
Easy dataset and middle dataset is saved as input files (csv format) that haven't preprocessed yet in [dataset/easy_dataset](https://github.com/Njk-901aru/multiDTI/blob/master/dataset/easy_dataset) and [dataset/middle_dataset](https://github.com/Njk-901aru/multiDTI/blob/master/dataset/middle_dataset). 

1.1 run [preprocessed_dataset.py](https://github.com/Njk-901aru/multiDTI/blob/master/dataset/preprocessed_dataset.py)

```bash
python preprocessed_dataset.py --input <INPUT_FILE>
```

`<INPUT_FILE>` input file (only csv format is available)


2. Apply node2vec to multi-interactome data

node2vec model of chemical-chemical interaction data are already saved as [data_multi/modelcc.pickle](https://github.com/Njk-901aru/multiDTI/blob/master/data_multi).
node2vec model of protein-protein interaction data are already saved as [data_multi/modelpp.pickle](https://github.com/Njk-901aru/multiDTI/blob/master/data_multi).

construct graph network connecting compounds(proteins) interacting each other and apply the graph to node2vec here.

2.1 run [omics_compound.py](https://github.com/Njk-901aru/multiDTI/blob/master/data_multi/omics_compound.py)

```bash
python omics_compound.py
```
chemical_chemical_interaction.csv (multi-interactome data) is setteing as default input file.

2.2 run [omics_protein.py](https://github.com/Njk-901aru/multiDTI/blob/master/data_multi/omics_protein.py)

```bash
python omics_protein.py
```
protein_protein_interaction.csv (multi-interactome data) is setteing as default input file.


3. Learn and predict by integrate model


## Example
You can demonstrate our program with test data.

```bash
cd /path/to/multiDTI/data_multi
python omics_compound.py
python omics_protein.py
```

- 5 fold Cross-validation for hard dataset
  - PreProcessed train dataset
  ```bash
  cd /path/to/multiDTI/dataset
  python preprocessed_dataset.py --input <FILEPATH> --data <DATA>
  ```
  `<FILEPATH>` './hard_dataset'
  
  `<DATA>` './train'
  
  - PreProcessed test dataset
  ```bash
  python preprocessed_dataset.py --input <FILEPATH> --data <DATA>
  ```
  `<FILEPATH>` './hard_dataset'
  
  `<DATA>` './test'
  
  - Learn
  ```bash
  cd /path/to/multiDT
  python training.py --input <FILEPATH> --output <DIRECTORY>
  ```
  - Predict
  ```bash
  python evaluation.py --input <FILEPATH> --output <DIRECTORY> --place <PLACE_DIRECTORY>
  ```


## Reference
* Watanabe, N., Sakakibara, Y. 
  * Deep learning-based integration of multi-interactome data for protein-compound interaction prediction

