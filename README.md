This project aims to extract important features from multi-omics data to train multiple machine learning models that predict cancer stage.

Data Links:
- Methylation: https://xenabrowser.net/datapages/?dataset=TCGA-BRCA.methylation450.tsv&host=https%3A%2F%2Fgdc.xenahubs.net&removeHub=https%3A%2F%2Fxena.treehouse.gi.ucsc.edu%3A443
- Gene Expression: https://xenabrowser.net/datapages/?dataset=TCGA-BRCA.star_tpm.tsv&host=https%3A%2F%2Fgdc.xenahubs.net&removeHub=https%3A%2F%2Fxena.treehouse.gi.ucsc.edu%3A443
- Clinical: https://xenabrowser.net/datapages/?dataset=TCGA-BRCA.clinical.tsv&host=https%3A%2F%2Fgdc.xenahubs.net&removeHub=https%3A%2F%2Fxena.treehouse.gi.ucsc.edu%3A443

Download the data from the links above and make sure they are in the same directory as the scripts.


Then start by _preprocessing_ the data with `import_preprocess_data.ipynb` which imports the multi-omics and phenotypical data, and outputs the preproccesed gene expression, methylation and clinical data. Clinical data is split into train and test sets already in this step to avoid data leakage.

_Exploratory analysis_ of preprocessed files can be done with `exploratory_analysis.ipynb`.

To extract _differentially methylated regions_, use the script `find_dmrs.Rmd` which takes preprocessed clinical (train set) and methylation data (quality control can be done with `methyl_qc.ipynb`) as input. The output is the differentially methylated regions table.

To find _differentially expressed genes_, use the script `diff_exp_analysis.ipynb`, which takes preprocessed gene expression data. The output is a gene list, consisting of genes that are found to be differentially expressed between two stages.

Then `machine_learning_methods.ipynb` can be used to _train and evaluate several ML models_ (Logistic Regression, SVM, Random Forest). The input files are preprocessed gene expression, methylation and clinical data (split into train and test sets). The script outputs top features of different ML models for annotation.

The pre-annotation file `pre-annotation.ipynb` takes the important feature lists that are the outputs of the ML models and splits all of them into two, genes and methylation sites. The output is then important gene and methylation site lists for each ML model. 

The _annotation_ script `annotation.Rmd` performs gene annotation for differentially methylated probes (DMPs) identified in methylation analysis and pathway enrichment analysis for the differently expressed genes using overrepresentation analysis (ORA) and gene set enrichment analysis (GSEA). It takes the output of pre-annotation and the clinical features data as input, and outputs differentially enriched pathways and important genes.
