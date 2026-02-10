This workflow is for the decomp tutorial for ANEQ505 spring 2025

#### Working Directory 
```
scratch/alpine/vtalbott@colostate.edu/decomp_tutorial
```

#### Class reservation
```
sinteractive --reservation=aneq505 --time=01:00:00 --partition=amilan --nodes=1 --ntasks=2 --qos=normal

```

#### Load Qiime2
```
module purge
module load 
module load qiime2/2024.10_amplicon
```

#### Load metadata
```

```

Load in sequences 

```
qiime tools import \
--type "SampleData[PairedEndSequencesWithQuality]" \
--input-format PairedEndFastqManifestPhred33V2 \
--input-path manifest/manifest_run3.txt \
--output-path demux/demux_run3.qza
```
- this command worked 
-
```
qiime dada2 denoise-paired \
--i-demultiplexed-seqs ../demux/demux_run2.qza \
--p-trunc-len-f 150 \
--p-trunc-len-r 150 \
--p-n-threads 8 \
--o-table table_run2.qza \
--o-representative-sequences seqs_run2.qza \
--o-denoising-stats dada2_stats_run2.qza
```