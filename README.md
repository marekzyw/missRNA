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

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Forbid small window overlapping


-wg, --weight

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Distance weight when determining expression


-ec, --expretimental_consensus

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Calculate sequence consensus based on BAM/SAM file


-t, --threads

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Number of threads. Default: 1

### Input

missRNA requires input file in BAM, SAM or 6-column BED format.

Sample input file is provided in "sample" folder. *10.collapsed.bam* consists of WHAT?? 

### Output file

missRNA generates 4-5 output files, depending on used options. 

#### Main output file *\<prefix\>*

Main output file contains informations about processing products found, using a following format:

    ENST00000408189	ENST00000408189_1_75_+	.	1	75	+	3438	5047	6064	48.13	70.66	84.89	83.23	TTGCAGTGATGACTTGCGAATCAAATCTGTCAATCCCCTGAGTGCAATCACTGATGTCTCCATGTCTCTGAGCAA
    ENST00000408189	ENST00000408189_33_75_+	.	33	75	+	579	884	985	8.11	12.38	13.79	89.75	ATCCCCTGAGTGCAATCACTGATGTCTCCATGTCTCTGAGCAA
    ENST00000362645	ENST00000362645_1_23_+	.	1	23	+	13	15	15	40.62	46.88	46.88	100.0	GGCTGGTCCGATGGTAGTGGGTT

Sample main output file *sample_result* is provided in "sample" folder.

The file structure is as follows (coordinates are in 1-based format):
1. transcript name, e.g. *ENST00000408189*
2. product name (transcript name_start_stop_strand), e.g. *ENST00000408189_1_75_+*
3. chromosome name ".", available with future genomic option
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

#### Reads list output file *\<prefix\>.rlist*

Rlist output file contains list of read names which are the part of the product, using a following format:

    ENST00000362645_1_23_+ -> 35291-4 44461-3 48000-3 75634-2 162313-1 352060-1 336201-1
    ENST00000362645_1_18_+ -> 29338-6 72436-2 84733-2 219240-1
    ENST00000362886_81_107_+ -> 6674-37 9798-22 9799-22 67798-2 247594-1 278992-1 53829-3 76496-2 17726-10 46338-3 52015-3 76047-2 396832-1 25532-7
    ENST00000362196_18_40_+ -> 7733-30 41994-4 59649-2 89923-2 245111-1 137390-1 329198-1 56013-3 65994-2

Sample rlist output file *sample_result.rlist* is provided in "sample" folder.

The file structure is as follows (coordinates are in 1-based format and match those in the main file):
1. product name from main output file (transcript name_start_stop_strand), e.g. *ENST00000362645_1_23_+*
2. *->* sign which separates product_name and read list
3. read list. Uniq reads names, that creates the product, are separated with one whitespace.

#### Precursors output file *\<prefix\>.precursors*

Precursors output file contains informations about products precursors transcripts, using a following format:

    ENST00000408189	.	.	.	+	2	6075	4017 66.12
    ENST00000362645	.	.	.	+	2	26	24	92.31
    ENST00000362886	.	.	.	+	1	116	85	73.28
    ENST00000362196	.	.	.	+	3	105.0	61	58.1


Sample precursors output file *sample_result.precursors* is provided in "sample" folder.

The file structure is as follows:
1. transcript name
2. chromosome ".", available with future genomic option
3. start position ".", available with future genomic option
4. stop position ".", available with future genomic option
5. strand sign
6. number of identified products within precursor transcript
7. number of reads mapped to precursor transcript
8. number of identified products reads within precursor transcript
9. ratio between 7. number of reads mapped to precursor transcript and 8. number of identified products reads within precursor transcript [%]

#### BED output file *\<prefix\>.bed*

BED output file contains informations about products in BED file, using a following format:

    ENST00000408189	0	75	ENST00000408189_1_75_+	5047	+
    ENST00000408189	32	75	ENST00000408189_33_75_+	884	+
    ENST00000362645	0	23	ENST00000362645_1_23_+	15	+
    ENST00000362645	0	18	ENST00000362645_1_18_+	11	+

Sample BED output file *sample_result.bed* is provided in "sample" folder.

The file structure is as follows (coordinates are in 0-based format, product coords are in 1-based format and match those in rest of files:
1. transcript name
2. product start
3. product stop
4. product name from main output file (transcript name_start_stop_strand), e.g. *ENST00000362645_1_23_+*
5. number of reads within the product
6. strand sign

#### Optional fasta output file *\<prefix\>.fasta*

Fasta output file is optional (when *-ec* or *-r* option is used) and contains informations about produkt sequence, either consensus or parsed from reference, using a following format:

    >ENST00000408189_1_75_+
    TTGCAGTGATGACTTGCGAATCAAATCTGTCAATCCCCTGAGTGCAATCACTGATGTCTCCATGTCTCTGAGCAA
    >ENST00000408189_33_75_+
    ATCCCCTGAGTGCAATCACTGATGTCTCCATGTCTCTGAGCAA
    >ENST00000362645_1_23_+
    GGCTGGTCCGATGGTAGTGGGTT
    >ENST00000362645_1_18_+
    GGCTGGTCCGATGGTAGT

Sample fasta output file *sample_result.fasta* is provided in "sample" folder.

#### Commandline output

Additionally after the analysis missRNA will print basic statistics consisting of:
1. Number of all reads in analysis
2. Nuber of reads within identified stable RNAs
3. Percent of reads within identified stable RNAs

Example:

    Number of all reads in analysis:  3992029
    Number of reads within identified stable RNAs:  3712635
    Percent of reads within identified stable RNAs:  93.0%


#### Command used to generate sample output files

```../missRNA -i 10.collapsed.bam -o sample_result -r Homo_sapiens.GRCh38.transcriptome.primary_assembly.fa -c```


### Contribute

If you notice any errors and mistakes, or would like to suggest some new features, please use Github's issue tracking system to report it. You are also welcome to send a pull request with your corrections and suggestions.

### Licence

This project is licensed under the terms of the GNU General Public License v3.0 license.






    


