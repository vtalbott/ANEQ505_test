<mark style="background: #FFB8EBA6;">1 pt for subdirectories 
1 pt for launching interactive session and launching qiime
1 pt for demux
4 pts for denoise</mark> - me 
Describe output files 3pts val
last 5 question khalid


1.  Log into Alpine using OnDemand and create a new directory for this new analysis in your _scratch_ directory. Hint: it should look something like: /scratch/alpine/$USER/cow/
2. Move into your new directory using OnDemand
3. Create the following sub-directories using OnDemand: 
	1. slurm
	2. taxonomy
	3. tree
	4. taxaplots
	5. dada2
	6. demux
	7. metadata
	8. core_metrics
~={red} 1 pt=~
4. Download the cow_barcodes.txt and cow_metadata.txt files from Canvas and upload them both to your metadata folder within your new cow directory. So, your filepath should look something like this: /scratch/alpine/$USER/cow/metadata

5. On OnDemand, go to your cow directory and open a new terminal

6. Copy the raw sequencing files from this public folder to **your new folder** using the terminal. Do **not** change the names of these files and folders. Hint: make sure you are in your new cow folder before you run this code (this will copy over the whole folder):  

```
cp -r /pl/active/courses/2024_summer/maw_2024/raw_reads .
```


5.    Launch an interactive session and load qiime2 within your cow directory. 

```
#launch an interactive session: 
ainteractive --ntasks=6 --time=02:00:00

#insert your code here to activate qiime. Hint: there should be 2 things you add here


```
~={red}1 pt=~

6.    Import the sequences/reads into a Qiime2-readable format (.qza). Note this might take 10-20 mins

```
qiime tools import \
--type EMPPairedEndSequences \
--input-path raw_reads \
--output-path cow_reads.qza
```


7.    Demultiplex the reads by submitting a job. Note this may take ~30 mins

a.    Go into your slurm directory using OnDemand. Create a new file named **demux.sh** so you can submit a job that will demultiplex your sequences quicker. Fill in the lines that need editing (denoted by capital letters or hashes) to this demultiplexing command and add that to your new script. 


```
#!/bin/bash
#SBATCH --job-name=demux
#SBATCH --nodes=1
#SBATCH --ntasks=12
#SBATCH --partition=amilan
#SBATCH --time=02:00:00
#SBATCH --mail-type=ALL
#SBATCH --output=slurm-%j.out
#SBATCH --qos=normal
#SBATCH --mail-user=vtalbott@colostate.edu

#Load Qiime2
module purge
module load qiime2/2024.10_amplicon

#change the following line if your file path looks different
cd /scratch/alpine/$USER/cow_test/demux

#Below is the command you will run to demultiplex the samples.

qiime demux emp-paired \
--m-barcodes-file ../metadata/cow_barcodes.txt \
--m-barcodes-column barcode \
--p-rev-comp-mapping-barcodes \
--p-rev-comp-barcodes \
--i-seqs ../cow_reads.qza \ #added the ../ to correct dir
--o-per-sample-sequences ../demux/demux_cow.qza \ # see note above
--o-error-correction-details ../demux/cow_demux_error.qza # see note above

#visualize the read quality
qiime demux summarize \
--i-data ../demux/demux_cow.qza \ #changed directory path
--o-visualization ../demux/demux_cow.qzv # see note above
```
~={red}1 pt=~

 Run the script in your slurm directory as a job using: 
 ```
 sbatch demux.sh
 ```

8.    Denoise. 

Fill in the blank to denoise your samples based on what you think should be trimmed (from the front of the reads) or truncated (from the ends of the reads) based on the demux_cow.qzv file. 
- Are we running from demux or from dada2?
- 
```
#cd demux
cd dada2

qiime dada2 denoise-paired \
--i-demultiplexed-seqs ../demux/demux_cow.qza \
--p-trim-left-f 0 \
--p-trim-left-r 0 \
--p-trunc-len-f 250 \
--p-trunc-len-r 250 \
--p-n-threads 6 \
--o-representative-sequences cow_seqs_dada2.qza \
--o-denoising-stats cow_dada2_stats.qza \
--o-table cow_table_dada2.qza

#Visualize the denoising results:
qiime metadata tabulate \
--m-input-file cow_dada2_stats.qza \
--o-visualization cow_dada2_stats.qzv

qiime feature-table summarize \
--i-table cow_table_dada2.qza \
--m-sample-metadata-file ../metadata/cow_metadata.txt \
--o-visualization cow_table_dada2.qzv

qiime feature-table tabulate-seqs \
--i-data cow_seqs_dada2.qza \
--o-visualization cow_seqs_dada2.qzv
```
~={red}4 pts =~
	
Briefly **describe** the key information from each denoising output file:
1. Representative Sequences
2. Denoising Stats
3. Denoised Table
~={red}3 pts=~
**Answer the following questions:**  
1. What is the mean reads per sample?
2. How long are the reads?
3. What is the maximum length of all your sequences?
4. Which sample (not including extraction controls starting with EC) lost the highest % of reads?
5. Why did you chose to trim or truncate where you did?
~={red}5 pts=~


```
qiime feature-table filter-samples \
--i-table dada2/table_type_days_nomitochloro.qza \
--m-metadata-file metadata/metadata.txt \
--o-filtered-table dada2/table_type_days_nomitochloro_filtered.qza
```

```

```