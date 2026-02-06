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