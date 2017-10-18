# RecoverY

RecoverY is a tool for shortlisting enriched reads from a sequencing dataset, based on k-mer abundance. Specifically, it can be used for isolating Y-specific reads from a Y flow-sorted dataset.

## Usage 

To run RecoverY with default parameters, 

	cd RecoverY
	python recoverY.py

### Input

The following input files are required in ./data folder. 
Note that currently RecoverY expects the folder to be named "data".
    		
	r1.fastq : Enriched raw reads (first in pair) 
	r2.fastq : Enriched raw reads (second in pair) 
	kmers_from_reads : kmer counts from DSK for r1.fastq
	trusted_kmers : kmer counts from DSK for human Y single copy genes

The input folder and file names can be changed by the user within the program. 


### Output 

The ./output folder contains :

 	op_r1.fastq
	op_r2.fastq

These are the Y-reads files produced by RecoverY.  


### Parameters
Important parameters for the user to choose are : 

- kmer-size (default: 25): k-mer size used for classifying reads. This must be the same as DSK's k-mer-size. We recommend a value between 25 and 31 for Illumina 150x150 bp reads
- strictness (default: 20) : the # of k-mers a read must match to the Ymer table in order to be classified as coming from the Y. We recommend a value between 20 and 50 for Illumina 150x150 bp reads.
- num\_threads (default: 2): The number of threads that RecoverY should use


### Generating k-mer counts with DSK

The ./dependency folder contains DSK binaries and a script that helps generate k-mer counts required for RecoverY. There are separate binaries for Linux 64 bit and Mac OSX. Usage is as follows (example shown below is for a Linux system) :

    cd dependency
    ./run_dsk_Linux.sh <FASTQ_file> <kmer_size>

In this case, FASTQ_file is r1.fastq and kmer_size is recommended to be 25.  
The kmer_counts table will be generated in :

    ./dependency/dsk_output/kmer_counts_from_dsk


### Generating k-mer plots 

Make sure that Matplotlib and Seaborn packages are installed as described below, and then un-comment the following line from recoverY.py prior to execution:

	print "Generating kmer plot"
	plot_kmers.plot_kmers()



## Installation 

### Download

	git clone https://github.com/makovalab-psu/RecoverY
	

### Dependencies 

RecoverY requires the numpy and biopython python packages in order to run.
Additionally, if you would like to generate certain visual plots, RecoverY needs the matplotlib and seaborn packages for python.
However, matplotlib and seaborn are not required for RecoverY to run. These packages can be installed as follows:

    pip install numpy
    pip install biopython
    pip install matplotlib
    pip install seaborn

RecoverY also uses the k-mer counter DSK. The latest DSK binaries (v2.2.0 for Linux 64 bit and v2.2.0 for Mac OSX) are provided in the dependency folder. Thus, if you are using either of these operating systems, DSK need not be installed, and you may use the binaries as provided.  

For other operating systems, or if alternate versions or functionality of DSK is desired, see https://gatb.inria.fr/software/dsk/.


## Example

The data folder contains an example reads dataset and kmer tables. 
It can be used to test if RecoverY runs to completion. 

Before running recoverY.py, please navigate to the data folder and un-compress the tar.xz file : 

	cd data/
	tar xf kmers_from_reads.tar.xz

Subsequently, RecoverY can be run as : 

	cd ../
	python recoverY.py
	
**Results :**

The data/r1.fastq and data/r2.fastq were generated from hg38 using wg-sim.
Thus, each FASTQ record header has the chromosome of origin for a given read. 

Using grep and wc commands, one can check if RecoverY has correctly retrieved most of the Y-reads. 

	grep "@chrY" data/r1.fastq | wc -l
	grep "@chrY" output/op_r1.fastq | wc -l



### Trusted kmers 

The trusted_kmers file consists of all the single copy k-mers obtained by k-merizing known single copy regions on the Y. This file is obtained by running DSK on X-degenerate gene sequences from human Y, and from these, extracting only k-mers with a count of 1. These trusted k-mers are used as a proxy to determine the abundance threshold for Y-mers. 

The trusted_kmers file consists of (kmer_sequence,count) pairs separated by a whitespace, as generated by DSK. Below is an example of the lexicographically smallest 5 trusted k-mers :
	
	AAAAAAAAAAAAAAAAGAAAAACAA 1
	AAAAAAAAAAAAAAACAAGCTGAAT 1
	AAAAAAAAAAAAAAAGAAAAACAAA 1
	AAAAAAAAAAAAAACAAGCTGAATG 1
	AAAAAAAAAAAAAAGAAAAACAAAA 1
	


### Scripts 

The following scripts are included with this distribution of RecoverY, and are automatically run by recovery.py as part of the pipeline. Users may consider them separately for custom needs if required. 

	
**kmers.py** 
	
	a set of general purpose functions to work with kmers

**kmerPaint.py**
	
	input : trusted_kmers and reads_from_kmers 
	output : Ymer_table with new abundance threshold

**classify_as_Y_chr.py**
	
	input : all raw reads (first in pair) and Ymer table
	output : Y-specific reads according to RecoverY algorithm (first in pair)

**find_mates.py** 

	input : all raw reads (second in pair) and Y-specific reads accoding to RecoverY algorithm (first in pair)
	output : Y-specific reads according to RecoverY algorithm (second in pair)
	
	

### License
This program is released under the MIT License. Please see LICENSE.md for details


### Citation
If you use RecoverY in your research, please cite 

[RecoverY : K-mer based read classification for Y-chromosome specific sequencing and assembly](https://doi.org/10.1101/148114), 

Samarth Rangavittal, Robert S. Harris, Monika Cechova, Marta Tomaszkiewicz, Rayan Chikhi, Kateryna Makova, Paul Medvedev

bioRxiv 2017.
