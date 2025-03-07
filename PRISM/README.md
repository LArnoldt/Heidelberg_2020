# PRISM

## System Requirements
*	Python 3.7
*	PyRosetta-3.7.Release (Tested on Scientific/Red Hat Linux Version.)

## Packages
*	biopython 1.78+
*	pytorch 1.5.0+
*	tensorflow 1.13+
*	numpy
*	pandas
*   requests
*	ftplip
*	goatools
*   matplotlib

## Abstract

We are presenting the neural network PRISM (**P**rotein-**R**NA-**I**nteraction **S**equence **M**odel) based on the Conditional Transformer Language Model for Controllable Generation (CTRL) by Salesforce as language model to generates sequences of novel RNA-binding Proteins (RBPs) (Keskar NS *et al.*, 2019).  PRISM generates sequences of RBPs, when a RNA-motif and protein annotations, such as the charge and taxonomy are put in.

Generative modeling of protein sequences is a unsupervised learning problem and a key to solving fundamental problems in synthetic biology, medicine, and material science. The transformer architecture as proposed by Vaswani *et al.* is the state of the art of language modelling (Vaswani A *et al.*, 2017). The layer normalisation of the CTRL model was substituted with zero (ReZero) initialisation technique as shown by Bachlechner *et al.* (Bachlener *et al.*, 2020).  We trained the neural network with sequences, annotations and ressources from UniProtKB Reviewed (Swissprot), BindingDB (Gilson MK, 2015) and Gene Ontology ((Ashburner M *et al.*, 2000); (The Gene Ontology Consoritum *et al.*, 2019)). Training was initialized with random data. Finetuning the neural network on RBPs was performed by training the network on data from the ATtRACT database (Giudice G, 2016).

Based on the annotations, such as GO-Terms, total charge, taxonomy, distribution of amino acid types distributon and amino acid distribution, and the corresponding RNA motif and the corresponding RNA motif PRISM is able to generate novel RBP-sequences. These are automatically evaluated via BLAST analysis and trRosetta (Yang J *et al.*, 2020).


More information on PRISM can be found on the [iGEM Heidelberg 2020 Wiki](https://2020.igem.org/Team:Heidelberg/Software/PRISM).

The preprocessing of the data from the mentoined databases and preparation of the sequences and annotations for the neural network can be found in the folder **“Databases”**.

The neural network PRISM can be found in the folder **“Language Model”**.

The processing of the output is described in the tool **“3DOC”**.

## Text Generation

Generating protein sequences with an input RNA-motif and input annotations as well as randomly initalized is possible after installation of the dependencies. After calling pretraining.py and finetuning.py for the initial training and finetuning or loading the final model, the user may generate RBP-sequences by accessing the file user_generator.py or the file random_generor.py

The file user_generator.py parses the RNA-motif, which should be bound by the generated RBP-sequence, as well as the annotations for the annotations_vector. An example of calling user_generator.py is given here:

```
    python user_generator.py –rna_motif “ACGUGU” –annotations “#A-004, #TOTALCHARGE-122, #POLAR-145, GO:0000001, TAX:Homo, LIGAND-Y”
```

For text generation the annotations vector is created out of the user input. At miniumum at least one annotation of each category (RNA-motif, GO-Terms, taxonomy, amino acid distribution, amino acid types and ligand-binding) must be entered. 

When choosing random_generator.py a random annotations vector is generated. random_generator.py can be called as followed:
  
```
    python random_generator.py
```
The random_generator takes the frequency of annotation types in annotation vectors in the dataset used for training and prevalence of single GO-Terms in the dataset used for training into account. Ligand-binding is per default set to true and the RNA-motif is generated except for the motif length completly random. The prevalences for the annotations are already pre-calculated. If you are using an own dataset, please remove these files so that the prevalences are recalculated. Please note, that the calculation may need to be adapted to your dataset.

The trained model receives the generated annotation vector with a masked sequence, so a single amino acid in the sequence is generated by the learned weights and parameters under consideration of the amino acids already generated. The sequence is saved by user_generator.py respectively random_generator.py as a FASTA file.

## Bibliography

[1] Ashburner, M., Ball, C., Blake, J., Botstein, D., Butler, H., Cherry, J., Davis, A., Dolinski, K., Dwight, S., Eppig, J., Harris, M., Hill, D., Issel-Tarver, L., Kasarskis, A., Lewis, S., Matese, J.C., Richardson, J., Ringwald, M., Rubin, G., & Sherlock, G. (2000). Gene Ontology: tool for the unification of biology. Nature Genetics, 25, 25-29.

[2] Bachlechner, T.C., Majumder, B.P., Mao, H.H., Cottrell, G., & McAuley, J. (2020). ReZero is All You Need: Fast Convergence at Large Depth. ArXiv, abs/2003.04887.

[3] Consortium, T.G. (2019). The Gene Ontology Resource: 20 years and still GOing strong. Nucleic Acids Research, 47, D330 - D338.

[4] Gilson, M., Liu, T., Baitaluk, M., Nicola, G., Hwang, L., & Chong, J. (2016). BindingDB in 2015: A public database for medicinal chemistry, computational chemistry and systems pharmacology. Nucleic Acids Research, 44, D1045 - D1053.

[5] Giudice, G., Sánchez-Cabo, F., Fungairino, C.T., & Pezzi, E.L. (2016). ATtRACT—a database of RNA-binding proteins and associated motifs. Database: The Journal of Biological Databases and Curation, 2016.

[6] Yang, J., Anishchenko, I., Park, H., Peng, Z., Ovchinnikov, S., & Baker, D. (2020). Improved protein structure prediction using predicted interresidue orientations. Proceedings of the National Academy of Sciences, 117, 1496 - 1503.

[7] Keskar, N., McCann, B., Varshney, L.R., Xiong, C., & Socher, R. (2019). CTRL: A Conditional Transformer Language Model for Controllable Generation. ArXiv, abs/1909.05858.

[8] Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A.N., Kaiser, L., & Polosukhin, I. (2017). Attention is All you Need. ArXiv, abs/1706.03762.
