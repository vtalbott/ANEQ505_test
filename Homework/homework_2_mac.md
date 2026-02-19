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
cd /scratch/alpine/$USER/cow/taxonomy`
```

```
wget --no-check-certificate https://ftp.microbio.me/greengenes_release/2024.09/2024.09.backbone.v4.nb.qza
```

```
qiime feature-classifier classify-sklearn \
--i-reads ../dada2/<YourRepresentativeSequencesFile.qza> \
--i-classifier gg-13-8-99-515-806-nb-classifier.qza \
--o-classification taxonomy_gg2.qza
```
           
```
#visualize the taxonomy of your unfiltered table:

qiime metadata tabulate \
--m-input-file taxonomy_gg2.qza \
--o-visualization taxonomy_gg2.qzv
```

```
#visualize the taxonomy in taxa bar plots:

qiime taxa barplot \
--i-table ../dada2/<YourDenoisedTable.qza> \
--i-taxonomy taxonomy_gg2.qza \
--m-metadata-file ../metadata/cow_metadata.txt \
--o-visualization ../taxaplots/taxa_barplot_unfiltered_gg2.qzv
```

```
#filter mitochondria, chloroplast, and sp004296775, fill in the blank (--p-exclude) to exclude these DNA

qiime taxa filter-table \
--i-table ../dada2/<YourDenoisedTable.qza> \
--i-taxonomy taxonomy.qza \
--p-exclude <WhatToExclude> \
--o-filtered-table ../dada2/dada2_table_nomitochloro_gg2.qza
```


```
qiime taxa barplot \
--i-table ../dada2/dada2_table_nomitochloro_gg2.qza \
--i-taxonomy taxonomy.qza \
--m-metadata-file ../metadata/cow_metadata.txt \
--o-visualization ../taxaplot/taxa_barplot_nomitochloro_gg2.qzv
```

2.    Final taxa bar plot
	* Questions: what’s the most abundant ASV in X sample.

3.    Phylogenetic tree
	* In your **slurm** directory create another job script to run the phylogenetic tree building.
	
```
nano <YourJobName.sh>
```

```
#!/bin/bash
#SBATCH --job-name=sepp_tree
#SBATCH --nodes=1
#SBATCH --ntasks=47
#SBATCH --partition=amilan
#SBATCH --time=04:00:00
#SBATCH --mail-type=ALL
#SBATCH --mail-user=<YourEmail>@colostate.edu
#SBATCH --output=slurm-%j.out
#SBATCH --qos=normal

#Activate qiime
module purge
module load qiime2/2024.10_amplicon

#Get reference
wget --no-check-certificate \ -P ../tree \ https://ftp.microbio.me/greengenes_release/2022.10/2022.10.backbone.sepp-reference.qza


#Command
qiime fragment-insertion sepp \
--i-representative-sequences ../dada2/<YourRepresentativeSequencesFile.qza> \
--i-reference-database ../tree/2022.10.backbone.sepp-reference.qza \
--o-tree ../tree/tree_gg2.qza \
--o-placements ../tree/tree_placements_gg2.qza
```

```
#submit the job
sbatch <YourJobName.sh>
```