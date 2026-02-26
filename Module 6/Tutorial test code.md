```
qiime feature-table filter-seqs \--i-data cow_seqs_dada2.qza \--m-metadata-file cow_seqs_dada2.qza \--p-where 'length(sequence) < 300' \--o-filtered-data cow_seqs_dada2_filtered300.qza

qiime feature-table tabulate-seqs \--i-data cow_seqs_dada2_filtered300.qza \--o-visualization cow_seqs_dada2_filtered300.qzv

qiime feature-table filter-features \--i-table cow_table_dada2.qza \--m-metadata-file cow_seqs_dada2_filtered300.qza \--o-filtered-table cow_table_dada2_filtered300.qza`

qiime feature-table summarize \--i-table cow_table_dada2_filtered300.qza \--m-sample-metadata-file ../metadata/cow_metadata.txt \--o-visualization cow_table_dada2_filtered300.qzv

```

```
qiime taxa filter-table \--i-table ../dada2/table.qza \--i-taxonomy ../taxonomy/taxonomy_gg2.qza \--p-exclude mitochondria,chloroplast,sp004296775 \--p-include c__ \--o-filtered-table ../dada2/table_nomitochloro.qza  
  
#visualize:  
qiime taxa barplot \--i-table ../dada2/table_nomitochloro.qza \--i-taxonomy ../taxonomy/taxonomy_gg2.qza \--m-metadata-file ../metadata/metadata.txt \--o-visualization taxa_barplot_all_samples.qzv
```

```
qiime diversity alpha-rarefaction \--i-table ../dada2/table_nomitochloro.qza \--m-metadata-file ../metadata/metadata.txt \--p-max-depth 10000 \--o-visualization alpha_rarefaction_curves.qzv
```

```
qiime diversity core-metrics-phylogenetic \--i-table dada2/table_nomitochloro.qza \--i-phylogeny tree/tree_gg2.qza \--m-metadata-file metadata/metadata.txt \--p-sampling-depth 1500 \--output-dir core-metrics-results
```

```
qiime diversity alpha-group-significance \--i-alpha-diversity core-metrics-results/observed_features_vector.qza \--m-metadata-file metadata/metadata.txt \--o-visualization core-metrics-results/observed_features_statistics.qzv

qiime diversity alpha-group-significance \--i-alpha-diversity core-metrics-results/shannon_vector.qza \--m-metadata-file metadata/metadata.txt \--o-visualization core-metrics-results/shannon_statistics.qzv  
  
qiime diversity alpha-group-significance \--i-alpha-diversity core-metrics-results/faith_pd_vector.qza \--m-metadata-file metadata/metadata.txt \--o-visualization core-metrics-results/faiths_pd_statistics.qzv
```

```
qiime diversity alpha-correlation \--i-alpha-diversity core-metrics-results/faith_pd_vector.qza \--m-metadata-file metadata/metadata.txt \--o-visualization core-metrics-results/faith_pd_correlation_statistics.qzv
```