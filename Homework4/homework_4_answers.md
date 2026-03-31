Set up correct directory structure~={red}(1point)=~
Correct file path for loading in alpha diversity data ~={red}(1 point)=~
Correct file path for loading in beta diversity data ~={red}(1 point)=~
Correct file path for loading in tabulate_results.tsv (taxabarplot) ~={red}(1 point)=~
Alpha diversity plot, Beta diversity plot, Taxabar plot  ~={red}(1 point)=~
Filter table for ANCOM-BC2 (~={red}1 point=~)
Filtering out low abundance/low prevalence ASVs (~={red}1 point=~)
Running ANCOM-BC2 (~={red}3 points, 1 point per correct chunk=~)
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
- still will do the filtering line ancomb-bc 
```
# Filter samples
qiime feature-table filter-samples \
  --i-table dada2_table.qza \
  --p-min-frequency 2000 \
  --o-filtered-table table_2k.qza
```

```
# Filter out low abundance/low prevalence ASVs
qiime feature-table filter-features \  
  --i-table table_2k.qza \  
  --p-min-frequency 50 \  
  --p-min-samples 4 \  
  --o-filtered-table table_2k_abund.qza
```





## Homework questions: (~={red}5 POINTS=~)
1. Describe one way to get data from your qiime2 outputs into a format that can be used for R. 
   ~={red}Unzip qza's and get the text file with the data, pull csv's from qiime2 view, or use the export command from qiime2=~
2. Taxabarplot question
3. When generating the filtered table for ANCOM-BC2, what value did you choose for `--p-min-frequency`? Which core metrics parameter should this match, and why do these values need to be the same? (Report your core metrics value here: ___) ~={red}this should match their core metrics sampling depth, and they are the same becuase we need to filter out any samples with fewer features than we rarefied at=~
4. Why do we filter out samples with low frequency and filter out low abundance ASVs? ~={red}Filtering can provide better resolution and limit false discovery rate which increases statistical power=~ 
5. Are there any differentially abundant features between body site (e.g., skin and fecal)?

