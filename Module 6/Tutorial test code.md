```
`qiime feature-table filter-seqs \``--i-data cow_seqs_dada2.qza \``--m-metadata-file cow_seqs_dada2.qza \``--p-where 'length(sequence) < 300' \``--o-filtered-data cow_seqs_dada2_filtered300.qza`

`qiime feature-table tabulate-seqs \``--i-data cow_seqs_dada2_filtered300.qza \``--o-visualization cow_seqs_dada2_filtered300.qzv`

`qiime feature-table filter-features \``--i-table cow_table_dada2.qza \``--m-metadata-file cow_seqs_dada2_filtered300.qza \``--o-filtered-table cow_table_dada2_filtered300.qza`

`qiime feature-table summarize \``--i-table cow_table_dada2_filtered300.qza \``--m-sample-metadata-file ../metadata/cow_metadata.txt \``--o-visualization cow_table_dada2_filtered300.qzv`

```