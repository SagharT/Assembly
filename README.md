fastQC (Version 0.11.9)
#Download fastQC using conda
conda install -c bioconda fastqc
hifiasm (Version 0.18.7)
#Install Hifiasm,  
conda install -c bioconda hifiasm
Quast (Version 5.2.0)
#Download using conda
#Make environment for installing quast 
conda create --name quast_env
conda activate quast_env
conda install -c bioconda busco
Busco (gVolante Analysis ver.2.0.0)
#I tried to download BUSCO but it failed so I used this webiste to analyse my assemblly
https://gvolante.riken.jp/analysis.html



### Check quality of the data, FASTQC
#Check the quality by fastqc
fastqc data/SRR13577846.fastq.gz
xdg-open SRR13577846_fastqc.html

### Assembly
#Run hifiasm
hifiasm -s stats.txt -o output -t 10 data/SRR13577846.fastq.gz
#change .gfa to .fa (fasta file)
awk '/^S/{print ">"$2;print $3}'  output.bp.p_ctg.gfa > output.bp.p_ctg.fa

# Finding the refrence 
https://www.ncbi.nlm.nih.gov/genome/?term=Saccharomyces%20cerevisiae%5B0rganism%5D&cmd=DetailsSearch


### Quality assesment using Quast (Version 5.2.0)
#Run Quast
quast 2_assembly/output.bp.p_ctg.fa -r Refrence/GCF_000146045.2_R64_genomic.fna
#Find information by looking at report.txt
Assembly statistics:
Contigs 36
Largest contig	1505909
Total length	12502635
N50	805283
NG50	805283
Misassemblies   111

### Quality assesment using Busco (gVolante Analysis ver.2.0.0)
Sequence type:      Genome (nucleotide) 
Choose an ortholog search pipeline:     BUSCO v5
Ortholog set for BUSCO v5 (OrthoDB v10):    Other ortholog set(saccharomycetes)


