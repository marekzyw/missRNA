# missRNA - Method for Identification of Small Stable RNAs

## Installation

missRNA requires Python 3.5 or higher and following Python3 library:
+ pysam

Also, following software should be installed on your system:
+ SAMtools

On Debian-based systems (e.g. Ubuntu) you can simply follow the instructions:

 1. Make sure you have Python 3.5 or higher on your system:
 
	 ```python3 --version```
	 
 3. Install python dependencies:
 
	 ```pip3 install pysam```
	 
 4. Install SAMtools:
 
 	 ```sudo apt-get install samtools```

 5. Download latest version of missRNA by clonning this repository:
 
	 ```git clone https://github.com/marekzyw/missRNA.git```
	 
	 or download a .zip package and unpack it:
   
	 ```wget https://github.com/marekzyw/missRNA/archive/master.zip```
	 
	 ```unzip master.zip```
   
  &nbsp;
  
  ## Quick start

The main file of missRNA program is *missRNA*. To run missRNA on provided example file with default parameters and using your system's default python3 interpreter, enter the *sample* directory and type:

```../missRNA -i RNASTRAND.fasta```
	 
To show full list of available  options type:

```missRNA -h```

&nbsp;

## Detailed usage
### Options
#### Required

-i, --input 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Input file in BED, SAM or BAM format.
#### Optional

-h, --help  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;show help message and exit

-o, --output  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Output files prefic. Default: missRNA_<random_string>

-g, --genomic

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Genomic data analysis

-r, --reference

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Reference sequences for consensus sequence generation

-c, --collapsed_reads

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Collapsed reads

-w, --window SMALL-BIG

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Window sizes. The small window determines the expression of the product and the large window has its background. Default: 1-2

-m, --minimum

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Minimal number of reads in product


-e, --enrichment

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Enrichment percentage. Ratio of product's expression level to the background level


-f, --offset

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Offset when mapping


-s, --strand

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Strand specific analysis (-1 for minus strand, 0 for both strands and 1 for plus strand). Default: 0


-fo, --forbid_overlapping

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Forbif small window overlapping


-wg, --weight

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Distance weight when determining expression


-ec, --expretimental_consensus

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Calculate sequence consensus based on BAM/SAM file


-t, --threads

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Number of threads. Default: 1

### Input

missRNA requires input file in BAM, SAM or 6-column BED format.

Sample input file is provided in "sample" folder. *10.collapsed.bam* consists of WHAT?? Transcripts from Ensembl ans some tRNA??

### Output file

missRNA generates 4-5 output files, depending on used options. 

#### Main output file *<prefix>*

Main output file contains informations about processing products found, using a following format:

    ENST00000408189	ENST00000408189_1_75_+	.	1	75	+	3438	5047	6064	48.13	70.66	84.89	83.23	TTGCAGTGATGACTTGCGAATCAAATCTGTCAATCCCCTGAGTGCAATCACTGATGTCTCCATGTCTCTGAGCAA
    ENST00000408189	ENST00000408189_33_75_+	.	33	75	+	579	884	985	8.11	12.38	13.79	89.75	ATCCCCTGAGTGCAATCACTGATGTCTCCATGTCTCTGAGCAA
    ENST00000362645	ENST00000362645_1_23_+	.	1	23	+	13	15	15	40.62	46.88	46.88	100.0	GGCTGGTCCGATGGTAGTGGGTT

Sample output file *sample_result* is provided in "sample" folder.

The file structure is as follows:
1. transcript name, e.g. *ENST00000408189*
2. precursor name (transcript name_start_stop_strand), e.g. *ENST00000408189_1_75_+*
3. chromosome name, "." for transcriptomic data
4. product start position
5. product stop position
6. strand sign
7. number of reads in product
8. number of reads in expression window
9. number of reads in background window
10. ratio of product reads number and total reads number per transcript [%]
11. ratio of expression window reads number and total reads number per transcript [%]
12. ratio of background window reads number and total reads number per transcript [%]
13. enrichment value [%]
14. consensus or reference sequence if option *-ec* or *-r* used

    


