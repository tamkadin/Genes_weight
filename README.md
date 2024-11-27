# Genes_weight
Tính toán trọng số đỉnh (gene) theo chú thích gen, biểu hiện gen và bậc

## Python Libraries
Imported libraries and their version:

- numpy, version 1.19.1
- networkx, version 2.4
- sklearn, version 0.23.1 



## Data Preprocessing


### Computing the Ontology Graph

BRW requires in the Ontology Graph to bias the teleporting probability e the transition matrix. It is a multilayer bipartite graph G(V,E) where V consists of two types of nodes: genes and ontologies. There is an edge between a gene and an ontology if that gene is involved in that ontology. Each layer represents a dataset (GO, KEGG, and Reactome)

To compute the ontology graph, run the following commands:

 ```
 cd data_preprocessing
 ```
 
 ```
 python3 compute_ontology_graph.py -go <path to .gaf file> -r <path to Reactome file> -k <path to KEGG file> -o <output file path>  
 ```
The files used in the manuscript can be downloaded at: https://drive.google.com/file/d/12oDaaEs1vso82UXsRe2AWeoGqNccZuLM/view?usp=sharing
An update version of the .gaf file can be downloaded from Gene Onotology Consortium at the following link: http://current.geneontology.org/products/pages/downloads.html

The Reactome file can be downloaded at https://reactome.org/download-data (Uniprot to all pathways)
The KEGG file has been downloaded using KEGG rest api.

The ontology graph will have the following structre:
```
 < ensembl_id > \t < annotation_id > \t < dataset_name >
```

### Computing disease specific ontology

To compute the set of statistically significant disease annotations run the followings commands 

 ```
 cd data_preprocessing
 ```
 
```
python3 compute_disease_specific_ontologies.py -s <path to seed set> -a <path to ontology network> -o <output file path>
```

### Computing the Tumor-Control Table TCGA

To compute Tumor and Control Table for each Tumor taken in consideration in the article, run the following commands:

 ```
 cd data_preprocessing
 ```
 
 ```
 python3 TCGA_analyzer.py -gdc <path to gdc sheet file> -m <path to manifest file> -rna_dir <path rna dir downloaded using cdc-client> -o <output dir path>  
```
The gdc sheet and manifest files that we have used in the manuscript can be found in the data_set/TCGA directory. The rna_seq file downloaded using cdc-client with the gdc sheet and manifest files is to heavy to be uploaded here. Thus, It can be found at the following link: https://drive.google.com/file/d/1f2V6fji8dPshiH6ohV81K0Cxv9q5D4ew/view?usp=sharing


### Computing the Co-expression network and Differentially Expressed Genes

Once Tumor and control Table have been created following the steps described above, we can generate the co-expression network and the DE genes using the following commands:
 ```
 cd data_preprocessing
 ```
 
  ```
 python3 compute_co_expression_and_de_genes.py -T <path to TCGA Tumor Table> -C <path to TCGA Control Table> -de <de output file path> -co < co-expression network output file path>  
```

## Install and run Run_comput_weight.py


To install the framework download the zip file and decompress it. Then, Iin order to run the algorithm, from terminal go in the BRW directory and type "python Run_comput_weight.py" followed by option:

 -p < protein_interaction_network_file_path > (required)

 -c < co_expression_network_file_path > (optional)

 -s < seed_set_file_path > (required) 

 -de < differentially_expressed_genes_file_path > (optional, default: None) 

 -do < disease_ontologies_file_path > (required)
 
 -a < gene_ontology_assotiation_dataset_file_path> (required)
 
 -o < output_file_path > (required)

### Example (fix the Path)

 ```
python Run_comput_weight.py -p "HIPPIE.tsv" -c "TCGA-BRCA__co_expression__t_70%.tsv" -s "TCGA-BRCA_seed.txt" -de "TCGA-BRCA_de_genes.tsv" -a "ontology_network.tsv" -do "TCGA-BRCA_disease_ontologies.txt" -o "test.txt"
 ```


