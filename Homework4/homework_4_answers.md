Set up correct directory structure~={red}(1point)=~
Correct file path for loading in alpha diversity data ~={red}(1 point)=~
Correct file path for loading in beta diversity data ~={red}(1 point)=~
Correct file path for loading in tabulate_results.tsv (taxabarplot) ~={red}(1 point)=~
Alpha diversity plot, Beta diversity plot, Taxabar plot  ~={red}(1 point)=~
Filter table for ANCOM-BC2~={red} (~={red}1 point=~)=~
Filtering out low abundance/low prevalence ASVs ~={red}(~={red}1 point=~)=~
Collapse to species level ~={red}(~={red}1 point=~)=~
Running ANCOM-BC2 ~={red}(~={red}1 point=~)=~
Visualize ANCOM-BC outputs ~={red}(1 point)=~
Questions ~={red} (5 points)=~ 

~={red}15 points total=~
------------------------------------------------------------------

Due: 

**For complete credit for this assignment, you must answer all questions and include all commands in your obsidian upload.** 

------------------------------------------------------------------
**Learning Objectives**
1. Practice recording commands and editing code to match your analysis.
2. Create publication ready figures for alpha and beta diversity.
3. Understand how to run ANCOM-BC2 and how to interpret the results. 
--------------------------------------------------

#### Cow Body Site - making figures in R





#### Cow Body Site - ANCOM-BC2
**Start and interactive session and activate Qiime2**
```
ainteractive --ntasks=4 --time=04:00:00
```

```
module purge
module load module load qiime2/2024.10_amplicon
```

**Filter Samples ~={red}(1 point)=~** 
- Navigate into the decomp tutorial and make a new ancombc2 directory for the ANCOM-BC2 analysis
- Navigate into the ancombc2 directory
- Choose the min frequency for sample filtering:
```
qiime feature-table filter-samples \
--i-table ../dada2/cow_table_dada2_filtered300.qza \
--p-min-frequency 5000 \
--o-filtered-table table_5k.qza
```

**Filter out low abundance and low prevalence ASVs ~={red}(1 point)=~**
```
qiime feature-table filter-features \
--i-table table_5k.qza \
--p-min-frequency 50 \
--p-min-samples 20 \
--o-filtered-table table_5k_abund.qza
```

**Collapse features to species level ~={red}(1 point)=~**
```
qiime taxa collapse \
--i-table table_5k_abund.qza \
--i-taxonomy ../taxonomy/taxonomy_gg2.qza \
--p-level 7 \
--o-collapsed-table table_5k_abund_L7.qza
```
-made it up to this point - ancombc2 is not avaliable for qiime2 2024.10

**Run ANCOM-BC ~={red}(1 point)=~**
```
qiime composition ancombc \
--i-table table_5k_abund_L7.qza \
--m-metadata-file ../metadata/cow_metadata.txt \
--p-formula 'body_site' \
--p-reference-levels 'BodySite::fecal'
--o-differentials ancombc_bodysite.qza  


```

**Visualize ANCOM-BC results ~={red}(1 point)=~**
```
qiime composition tabulate \
--i-data ancombc_bodysite.qza \
--o-visualization ancombc_bodysite.qzv  
  
qiime composition da-barplot \
--i-data ancombc_bodysite.qza \
--p-significance-threshold 0.05 \
--o-visualization da_barplot_bodysite.qzv
```

- All the ancombc ran so will probably just stick with ancombc since we don't want to have to switch qiime versions


on thrusday I can test the new qiime2 version but probably won't have them do it in the HW







**Run ANCOM-BC2 ~={red}(1 point)=~**
```
qiime composition ancombc2 \
--i-table table_5k_abund_L7.qza \
--m-metadata-file ../metadata/cow_metadata.txt \
--p-fixed-effects-formula body_site \
--o-ancombc2-output ancombc2-results-bodysite.qza
```


**Visualize the ANCOM-BC2 results ~={red}(1 point)=~**
- Generate a barplot to visualized the differentially abundant features. 
```
# Visualize ANCOM-BC2 results
bash
qiime composition ancombc2-visualizer \
--i-data ancombc2-results-bodysite.qza \
--i-taxonomy ../taxanomy/taxonomy_gg2.qza \
--o-visualization ancombc2-barplot-bodysite.qzv
```

## Homework questions: (~={red}5 POINTS=~)
1. Describe one way to get data from your qiime2 outputs into a format that can be used for R. 
   ~={red}Unzip qza's and get the text file with the data, pull csv's from qiime2 view, or use the export command from qiime2=~
2. Taxabarplot question
3. When generating the filtered table for ANCOM-BC2, what value did you choose for `--p-min-frequency`? Which core metrics parameter should this match, and why do these values need to be the same? (Report your core metrics value here: ___) ~={red}this should match their core metrics sampling depth, and they are the same becuase we need to filter out any samples with fewer features than we rarefied at=~
4. Why do we filter out samples with low frequency and filter out low abundance ASVs? ~={red}Filtering can provide better resolution and limit false discovery rate which increases statistical power=~ 
5. Are there any differentially abundant features between body site (e.g., skin and fecal)?

