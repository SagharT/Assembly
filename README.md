Long read de novo assembly  
# Set up

    -Set up a new directory structure  
    -The dataset is a HiFi Pacbio reads from the yeast Saccharomyces cerevisiae.
The data can be downloaded from NCBI Sequence Read Archive (SRA)

# Software
FastQC (Version 0.11.9)  
Download fastQC using conda  
```bash
conda install -c bioconda fastqc 
```
 
Hifiasm (Version 0.18.7)  
Install Hifiasm    
```bash
conda install -c bioconda hifiasm 
```
Quast (Version 5.2.0)   
Download using conda  
Make environment for installing quast   
```bash
conda create --name quast_env  
conda activate quast_env  
conda install -c bioconda busco 
``` 
Busco (Version 5.4.4)  
```bash
conda create --name busco  
conda activate busco
conda install -c conda-forge -c bioconda busco=5.4.4
``` 
MultiQC (Version 5.4.4)  
```bash
conda create -n multiqc
conda activate multiqc
conda install -c conda-forge -c bioconda busco=5.4.4
``` 


------

# <span style="color:blue"> Check quality of the data, FASTQC </span>
### Check the quality by fastqc  
```bash
fastqc data/SRR13577846.fastq.gz
xdg-open SRR13577846_fastqc.html 
``` 

# <span style="color:blue"> Assembly

## Run hifiasm
```bash
hifiasm -s stats.txt -o output -t 10 data/SRR13577846.fastq.gz
#change .gfa to .fa (fasta file)
awk '/^S/{print ">"$2;print $3}'  output.bp.p_ctg.gfa > output.bp.p_ctg.fa
```

# <span style="color:blue"> Quality assesment using Quast </span>
QUAST stands for QUality Assessment Tool. QUAST can evaluate assemblies using reference genomes, as well as without reference genomes. QUAST produces detailed reports, tables and plots which show the different aspects of assemblies.
## Finding the refrence 
https://www.ncbi.nlm.nih.gov/genome/?term=Saccharomyces%20cerevisiae%5B0rganism%5D&cmd=DetailsSearch
## Run Quast
```bash
quast 2_assembly/output.bp.p_ctg.fa -r Refrence/GCF_000146045.2_R64_genomic.fna
```
## Find information by looking at report
```bash
cd results_2023_02_23_09_11_14/
cat report.txt
```

Assembly statistics:  
Contigs &rarr; 36  
Largest contig	&rarr; 1505909  
Total length	&rarr;12502635  
N50	&rarr;805283  
NG50&rarr;	805283  
Misassemblies &rarr;  111  

# <span style="color:blue"> Quality assesment using Busco </span>
BUSCO (Benchmarking Universal Single-Copy Orthologs) provides measures for quantitative assessment of genome assembly, gene set, and transcriptome completeness based on evolutionarily informed expectations of gene content from near-universal single-copy orthologs.
## Run Busco
```bash
busco -i 2_assembly/ output.bp.p_ctg.fa --auto-lineage -o Busco -m genome
```
## Find information by looking at report 
```bash
cat short_summary.specific.saccharomycetes_odb10.Busco.txt
```
Complete &rarr;2129  
Fragmented &rarr;2  
Missing	&rarr;6  
Total BUSCOs&rarr;2137  

# <span style="color:blue"> MultiQC Report </span>
MultiQC searches a given directory for analysis logs and compiles a HTML report. It's a general use tool, perfect for summarising the output from numerous bioinformatics tools.
## Run MultiQC
```bash
cd Assembly
multiqc ./
```
## Find information by looking at report 
```bash
cat multiqc_general_stats.txt
```
Sample	&rarr;SRR13577846  
FastQC_percent_duplicates &rarr;0.853946558668  
FastQC_percent_gc &rarr;38.0    
FastQC_avg_sequence_length	&rarr;9391.45693257  
FastQC_total_sequences	&rarr;117525.0  
FastQC_percent_fails &rarr;20.0  
QUAST_Total_length	&rarr;12502635.0  
QUAST_N50 &rarr;805283.0
						




