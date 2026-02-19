**ANEQ Class Assignment #2: Practice Qiime2 Workflow**

Due:

**For complete credit for this assignment, you must answer all questions and include all commands in your obsidian upload.**

------------------------------------------------------------------
**Learning Objectives**
1.    Practice independently running a partial Qiime2 workflow on new data using both the command line and slurm to submit jobs.
2.    Practice recording commands and editing code to match your analysis.
3.    Perform statistical analyses and interpret alpha & beta diversity.
--------------------------------------------------

**Cow Site Data Workflow**
1.    Taxonomy
	* Using GreenGenes, assign taxonomy to your representative sequences:

First get the Greengenes2 database:

```
wget \ ### add the reference here###
```

```
qiime feature-classifier classify-sklearn \
--i-reads <YourRepresentativeSequencesFile.qza> \
--i-classifier gg-13-8-99-515-806-nb-classifier.qza \
--o-classification taxonomy.qza
```
           
```
#visualize the taxonomy of your unfiltered table:

qiime metadata tabulate \
--m-input-file taxonomy.qza \
--o-visualization taxonomy.qzv
```

```
#visualize the taxonomy in taxa bar plots:

qiime taxa barplot \
--i-table <YourDenoisedTable.qza> \
--i-taxonomy taxonomy.qza \
--m-metadata-file metadata.txt \
--o-visualization taxa_barplot_unfiltered_16S.qzv
```

```
#filter mitochondria and chloroplast, fill in the blank (--p-exclude) to exclude these DNA

qiime taxa filter-table \
--i-table <YourDenoisedTable.qza> \
--i-taxonomy taxonomy.qza \
--p-exclude <WhatToExclude> \
--o-filtered-table dada2_table-no-mito-no-chloro_16S.qza
```


```
qiime taxa barplot \
--i-table dada2_table-no-mito-no-chloro_16S.qza \
--i-taxonomy taxonomy.qza \
--m-metadata-file metadata.txt \
--o-visualization dada2_table-no-mito-no-chloro_16S.qzv
```

2.    Final taxa bar plot
	* Questions: what’s the most abundant ASV in X sample.

3.    Phylogenetic tree
	* Create another job script to run the phylogenetic tree building.

```
#!/bin/bash
#SBATCH --job-name=denoise
#SBATCH --nodes=1
#SBATCH --ntasks=47
#SBATCH --partition=amilan
#SBATCH --time=02:00:00
#SBATCH --mail-type=ALL
#SBATCH --mail-user=<YourEmail>@colostate.edu
#SBATCH --output=slurm-%j.out
#SBATCH --qos=normal

#Activate qiime
module purge
module load  ###qiime2-2023.5###

#Get gg2 reference backbone
wget \ ###put in gg2 backbone###

#Command
qiime fragment-insertion sepp \
--i-representative-sequences rep-seqs-dada2.qza \
--i-reference-database sepp-refs-gg-13-8.qza \
--o-tree tree_16S.qza \
--o-placements tree_placements_16S.qza
```

```
#submit the job
sbatch <YourJobName.sh>
```