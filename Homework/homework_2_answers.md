1.    Classify taxonomy using GreenGenes

#First get the GreenGenes database:
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

First visualize unfiltered taxa plot
```
#visualize the taxonomy in taxa bar plots:

qiime taxa barplot \
--i-table ../dada2/<YourDenoisedTable.qza> \
--i-taxonomy taxonomy_gg2.qza \
--m-metadata-file ../metadata/cow_metadata.txt \
--o-visualization ../taxaplots/taxa_barplot_unfiltered_gg2.qzv
```

```
#filter mitochondria and chloroplast to generate the filtered feature table.

Add in the two commands needed to filter mitochondria and chloroplasts out of the feature table, then visualize the taxa bar plots.

qiime taxa filter-table \
--i-table ../dada2/table_cow.qza \
--i-taxonomy taxonomy.qza \
--p-exclude mitochondria,chloroplast,sp004296775 \
--o-filtered-table ../dada2/dada2_table-no-mito-no-chloro_16S.qza
```

```
qiime taxa barplot \

--i-table dada2_table-no-mito-no-chloro_16S.qza \

--i-taxonomy taxonomy.qza \

--m-metadata-file cow_metadata.txt \

--o-visualization dada2_table-no-mito-no-chloro_16S.qzv
```

Filtered Taxa Bar Plot Questions

_Question 1: Attach a picture of your taxa bar plot, organized by cow sampling location at the level 7 taxonomic level. What general trends do you notice?_  **composition looks different by body sites.**

_Question 2: What are the top 2 most abundant bacterial classes in the fecal samples? **Clostridia and Bacteroidia**_

_Question 3: What highly abundant ASV is shared between both the udder and skin samples? **k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__;s__**_

2.    Phylogenetic tree

Create a job script to run the phylogenetic tree building. Remember you must start a new terminal session, navigate to your working directory, and then submit the job. This job will take about an hour.

nano tree.sh

#paste the following filled-in code into the new job script file

#!/bin/bash

#SBATCH --job-name=tree

#SBATCH --nodes=1

#SBATCH --ntasks=8

#SBATCH --partition=amilan

#SBATCH --time=02:00:00

#SBATCH --mail-type=ALL

#SBATCH --mail-user=lindsval@colostate.edu

#Activate qiime

#Insert the two commands you need to load qiime2

Module purge

Module load qiime2

wget \

  -O "sepp-refs-gg-13-8.qza" \

  "https://data.qiime2.org/2023.5/common/sepp-refs-gg-13-8.qza"

#Command

qiime fragment-insertion sepp \

--i-representative-sequences seqs_cow.qza \

--i-reference-database sepp-refs-gg-13-8.qza \

--o-tree tree_16S.qza \

--o-placements tree_placements_16S.qza

#exit out of the file by clicking control+X and save the file.

#submit the job

sbatch tree.sh