# RecoverY

RecoverY is a tool for shortlisting enriched reads from a sequencing dataset, based on k-mer abundance. Specifically, it can be used for isolating Y-specific reads from a Y flow-sorted dataset.

### Usage  

    python recoverY.py
  	
Important parameters for the user to choose are : 


**kmer-size** : 
- the size of k used while iterating through every read 
- this must be the same as DSK's kmer-size
- usually optimal in the range [25, 31] for Illumina 150x150 bp reads


**strictness** : 
- the # of successful matches to the Ymer table required per read, before classifying a read as Y-specific 
- usually optimal in the range [20, 50] for Illumina 150x150 bp reads


### Installation 

	git clone https://github.com/makovalab-psu/RecoverY
	cd RecoverY


### Dependencies 

Numpy and Biopython

    pip install numpy
    pip install biopython
    

### Input

The following input files are required in ./data folder
    	
	
	r1.fastq : Enriched raw reads (first in pair) 
	r2.fastq : Enriched raw reads (second in pair) 
	kmers_from_reads : kmer counts from DSK for r1.fastq
	trusted_kmers : kmer counts from DSK for human Y single copy genes

The input folder and filenames can be changed by the user within the program. 


### Output 

The ./output folder contains :

 	op_r1.fastq
	op_r2.fastq

These are the Y-reads files produced by RecoverY.  


### Example

The data folder contains an example reads dataset and kmer tables. 
It can be used to test if RecoverY runs to completion. 


### Scripts 

The following scripts are included with this distribution of RecoverY
	
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
Please cite this Github repository if you use this tool in your research. Thanks !
https://github.com/makovalab-psu/RecoverY