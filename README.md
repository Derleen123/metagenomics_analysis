# metagenomics_analysis
This repository offers a soil metagenomics analysis workflow, utilizing tools like FastQC, MEGAHIT, MetaSPAdes, DIAMOND, and MEGAN. Designed for user-friendly operation, it facilitates insight extraction from varied microbial communities. 

Step 1: Install FastQC
sudo apt-get install fastqc

Step 2: Run FastQC on Paired-End Reads
fastqc A1_S3_L001_R1_001.fastq.gz
fastqc A1_S3_L001_R2_001.fastq.gz

Step 3: Perform Metagenomic Assembly using MEGAHIT
wget https://github.com/voutcn/megahit/releases/download/v1.2.9/MEGAHIT-1.2.9-Linux-x86_64-static.tar.gz
tar zvxf MEGAHIT-1.2.9-Linux-x86_64-static.tar.gz
megahit -1 A1_S3_L001_R1_001.fastq.gz -2 A1_S3_L001_R2_001.fastq.gz -o MEGAHIT-OUT

Step 4: Perform Metagenomic Assembly using MetaSPAdes
wget http://cab.spbu.ru/files/release3.15.5/SPAdes-3.15.5-Linux.tar.gz
tar -xzf SPAdes-3.15.5-Linux.tar.gz
cd SPAdes-3.15.5-Linux/bin/
metaspades.py -1 A1_S3_L001_R1_001.fastq.gz -2 A1_S3_L001_R2_001.fastq.gz -o SPADES-OUT

Step 5: Install DIAMOND for Taxonomic Assignment
wget https://github.com/bbuchfink/diamond/releases/download/v0.9.36/diamond-linux64.tar.gz
tar xzf diamond-linux64.tar.gz

Step 6: Download NCBI-nr database for DIAMOND
wget https://ftp.ncbi.nlm.nih.gov/blast/db/FASTA/nr.gz
diamond makedb --in nr.gz --db nr

Step 7: Run DIAMOND on MEGAHIT output
diamond blastx -d nr.dmnd -q MEGAHIT-OUT/contigs.fa -o megahit_alignment_result.daa -f 100

Step 8: Run DIAMOND on MetaSPAdes output
diamond blastx -d nr.dmnd -q SPADES-OUT/contigs.fasta -o metaspades_alignment_result.daa -f 100

Step 9: Run MEGANIZER for DIAMOND output
daa-meganizer -i megahit_alignment_result.daa -mdb megan-map-Feb2022.db

Step 10: Run MEGANIZER for MetaSPAdes output
daa-meganizer -i metaspades_alignment_result.daa -mdb megan-map-Feb2022.db

Step 11: Install MEGAN
Follow the instructions on MEGAN website to install MEGAN_Community_unix_6_24_20.sh.

Step 12: Unzip MEGAN mapping databases
unzip megan-map-Feb2022.db.zip

Step 13: Activate MEGAN environment
Navigate to the directory where MEGAN_Community_unix_6_24_20.sh is located and run:
chmod +x MEGAN_Community_unix_6_24_20.sh
./MEGAN_Community_unix_6_24_20.sh

Step 14: Extracting reads using the read extractor tool
Activate the environment in which MEGAN is installed and run:
read-extractor -b -i megahit_alignment_result.daa -o Bacteria.fasta.gz -c Taxonomy -n Bacteria

Step 15: Interactive Analysis with MEGAN6
Launch MEGAN6 and follow the on-screen instructions for analysis.
