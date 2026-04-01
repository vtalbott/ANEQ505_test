Set up correct directory structure~={red}(1point)=~
Correct file path for loading in metadata~={red}(1 point)=~
Correct file path for loading in alpha diversity data ~={red}(1 point)=~
Correct file path for loading in beta diversity data ~={red}(1 point)=~
Correct file path for loading in tabulate_results.tsv (taxabarplot) ~={red}(1 point)=~
Filter table for ANCOM-BC2~={red} (~={red}1 point=~)=~
Filtering out low abundance/low prevalence ASVs ~={red}(~={red}1 point=~)=~
Collapse to species level ~={red}(~={red}1 point=~)=~
Running ANCOM-BC2 ~={red}(~={red}1 point=~)=~
Visualize ANCOM-BC outputs ~={red}(1 point)=~
Questions ~={red} (5 points)=~ 

~={red}15 points total=~
------------------------------------------------------------------

Due: 

**For complete credit for this assignment, you must answer all questions and include all commands in your Obsidian upload.** 

------------------------------------------------------------------
**Learning Objectives**
1. Practice recording commands and editing code to match your analysis.
2. Create publication-ready figures for alpha and beta diversity.
3. Understand how to run ANCOM-BC2 and how to interpret the results. 
--------------------------------------------------

#### Cow Body Site - making figures in R

**Set up the cow R analysis file structure**
- Make a cow_r directory, and inside the cow_r directory, make the following directories 
cow_r  
├── 01_notes  
├── 02_data  
├── 03_metadata  
├── 04_code  
└── 05_figures

- Inside the 04_code directory, make the following directories
04_code  
├── alpha_div 
├── beta_div 
├── taxonomy

- Download the cow_metadata.txt, shannon.tsv, unweighted_unifrac.txt, tabulated_results.tsv, and cow_HW4_r.Rmd files from Canvas and put them in the correct directories. 

**What directory should the cow_HW4_r.Rmd file go in? ~={red}(1 point)=~**
~={red}The 04_code directory=~

#### Statistical analysis and figure generation in R 

- Now that we have set up the correct file structure and put our files in the correct directories, we can start our cow R analysis. 
- Open the cow_HW4_r.Rmd file and start working through the analysis 

**Note that if you open the markdown file in your Downloads, the working directory will not be correct. Make sure to only open the markdown file after you have put it in the correct working directory.**

**Read in metadata**
- Fill in the file path you used in the R Markdown to load the metadata. 
```
metadata <- read_tsv("../03_metadata/cow_metadata.txt")
```

**Read in alpha diversity data**
- Fill in the file path you used in the R Markdown to load the shannon data
```
shannon <- read_tsv("alpha_div/shannon.tsv")
```

**Read in beta diversity data**
- Fill in the file path you used in the R Markdown to load the unweighted unifrac data
```
uw_unifrac <- read_tsv("beta_div/unweighted_unifrac.txt")
```

**Load in tabulated results
- Fill in the file path you used in the R Markdown to load the tabulated_results.tsv
```
tabulated_results <- read_tsv("taxonomy/tabulated_results.tsv")
```

#### Cow Body Site - ANCOM-BC2 in Qiime2
**Start an interactive session and activate Qiime2**
```
ainteractive --ntasks=4 --time=04:00:00
```

- **ANCOMBC2 is only available in the 2026 versions of qiime2, so we need to activate the latest version. Make sure to activate qiime2026.2**
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
-made it up to this point - ancombc2 is not available for qiime2 2024.10


**Run ANCOM-BC2 ~={red}(1 point)=~**
```
qiime composition ancombc2 \
--i-table table_5k_abund_L7.qza \
--m-metadata-file ../metadata/cow_metadata.txt \
--p-fixed-effects-formula body_site \
--o-ancombc2-output ancombc2-results-bodysite.qza
```


**Visualize the ANCOM-BC2 results ~={red}(1 point)=~**
- Generate a barplot to visualize the differentially abundant features. 
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
2. Which body site appeared most distinct in the taxa bar plot, meaning it was not similar to at least one of the other body sites? Explain why that site looks different. ~={red}Fecal. Nasal and oral should be similar and skin and udder should be similar =~
3. When generating the filtered table for ANCOM-BC2, what value did you choose for `--p-min-frequency`? Which core metrics parameter should this match, and why do these values need to be the same? (Report your core metrics value here: ___) ~={red}this should match their core metrics sampling depth, and they are the same becuase we need to filter out any samples with fewer features than we rarefied at=~
4. Why do we filter out samples with low frequency and filter out low abundance ASVs? ~={red}Filtering can provide better resolution and limit false discovery rate which increases statistical power=~ 
5. Are there any differentially abundant features between body site (e.g., skin and fecal)?

