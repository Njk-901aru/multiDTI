= Deep learning-based integration of multi-interactome data for protein-compound interaction prediction

== Requirements
* python anaconda3(installed rdkit, chainer)

* pyensembl (for converting ensembl protein ID into amino acid sequences)
* usage : pyensembl install --release 93 --species human (install all human reference data from Ensembl release93)

* pubchempy (for converting PunChem ID into smiles)

* node2vec, networkx(for applying node2vec algorithm)


== Usage
[1] Make input file for CNN model

hard dataset already processed is contained in "./dataset/hard_dataset".
easy dataset and middle dataset is saved as input files (csv format) that haven't preprocessed yet in "./dataset/easy_dataset" and "./dataset/middle_dataset". 

[1.1] run preprocessed_dataset.py

Usage : python preprocessed_dataset.py

<INPUT_FILE> input file (only csv format is available)
<OUTPUT_FILE> interaction.npy(label), proIDs.txt(ensembl protein ID), chemIDs.npy(PubChem ID), ecfp format, protein.npy(protein onehot)

to read the input file you want to process, you can change relative path in "data = pd.read_csv(<INPUT_FILE>)".

if you want to use your original data, you have to get ensembl protein ID(ENSP) and PubChem ID and save them to csv file using the style below:
[PubChemID \t ENSP \t label(active_or_inactive)]

for example:
#     chemical              protein    label
# 1    9851499       ENSP00000215781       0
# 2       3440       ENSP00000361186       0
# 3    5280363       ENSP00000309818       1
...

[2] Apply node2vec to multi-interactome data

node2vec model of chemical-chemical interaction data are already saved as "./data_multi/modelcc.pickle".
node2vec model of protein-protein interaction data are already saved as "./data_multi/modelpp.pickle".

construct graph network connecting compounds(proteins) interacting each other and apply the graph to node2vec here.

[2.1] run omics_compound.py

Usage : python omics_compound.py

<INPUT_FILE> chemical_chemical_interaction.csv (multi-interactome data)
<OUTPUT_FILE> modelcc.pickle

[2.1] run omics_protein.py

Usage : python omics_protein.py

<INPUT_FILE> protein_protein_interaction.csv (multi-interactome data)
<OUTPUT_FILE> modelpp.pickle

[2] Learn and predict by CNN model


== Example
You can demonstrate our program with test data.

% cd /path/to/multiDTI/data_multi
% python omics_compound.py
% python omics_protein.py

- 5 fold Cross-validation
-> PreProcessed train dataset
% cd /path/to/multiDTI/dataset
% python preprocessed_dataset.py
<INPUT_FILE> "./dataset/hard_dataset(easy_dataset,middle_dataset)/cv_(0~4)/train.csv"
-> PreProcessed test dataset
% python preprocessed_dataset.py
<INPUT_FILE> "./dataset/hard_dataset(easy_dataset,middle_dataset)/cv_(0~4)/test.csv"
-> Learn
% cd /path/to/multiDT
% python training.py
-> Predict
% python evaluation.py


== Reference
* Watanabe, N., Sakakibara, Y. 
: Deep learning integration of multi-interactome data for protein-compound interaction prediction