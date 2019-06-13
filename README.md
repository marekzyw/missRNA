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
#### Main input - required

missRNA requires input file in BAM, SAM or 6-column BED format.

Sample input file is provided in "sample" folder. *sample.bam* consists of 11742 Glu tRNA reads.

#### Reference file

For the calculation of product sequence, missRNA requires additional reference file in FASTA format. Sample reference file is provided in "sample" folder. *reference.fa* contains reference sequences for Glu tRNAs.

### Output file

missRNA generates 4-5 output files, depending on used options. 

#### Main output file *\<prefix\>*

Main output file contains informations about processing products found, using a following format:

    rna_tRNA_Glu_TTC_5	rna_tRNA_Glu_TTC_5_1_33_+	.	1	33	+798	1003	1384	44.38	55.78	76.97	72.47	TCCCACATGGTCTAGCGGTTAGGATTCCTGGTT
    rna_tRNA_Glu_TTC_5	rna_tRNA_Glu_TTC_5_1_53_+	.	1	53	+68	390	1335	3.78	21.69	74.25	29.21	TCCCACATGGTCTAGCGGTTAGGATTCCTGGTTTTCACCCAGGCGGCCCGGGT
    rna_tRNA_Glu_TTC_5	rna_tRNA_Glu_TTC_5_1_28_+	.	1	28	+17	71.74	74	0.95	3.99	4.12	100.0	TCCCACATGGTCTAGCGGTTAGGATTCC
    rna_tRNA_Glu_TTC_5	rna_tRNA_Glu_TTC_5_1_26_+	.	1	26	+14	67.26	509	0.78	3.74	28.31	13.75	TCCCACATGGTCTAGCGGTTAGGATT


Sample main output file *sample_result* is provided in "sample" folder.

The file structure is as follows (coordinates are in 1-based format):
1. transcript name, e.g. *rna_tRNA_Glu_TTC_5*
2. product name (transcript name_start_stop_strand), e.g. *rna_tRNA_Glu_TTC_5_1_33_+*
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

    rna_tRNA_Glu_TTC_5_1_33_+ -> 396726-1 399886-1 404030-1 407067-1 412412-1 416574-1 806-561 5395-48 6156-41 7815-30 15718-12 17439-11 17731-10 17847-10 18760-10 20261-9 39240-4 44115-3 58145-3 69585-2 71854-2 87996-2 89280-2 92576-2 173598-1 179067-1 185192-1 205785-1 220197-1 238630-1 239949-1 249761-1 266332-1 271182-1 285850-1 294812-1 296235-1 296371-1 309225-1 315538-1 318154-1 324274-1 324358-1 348575-1 349064-1 353219-1 356012-1 358214-1 369915-1 372909-1 374884-1 375261-1 378015-1 385435-1 2725-118 16557-11 21754-8 33506-5 50793-3 52665-3 80253-2 82142-2 109255-1 150698-1 212422-1 223807-1 244728-1 248222-1 249581-1 271856-1 281389-1 286699-1 332880-1 349284-1 349520-1 379967-1 390888-1 419741-1 11790-18 56180-3 70272-2 174053-1 175281-1 360168-1 380579-1 265161-1 296272-1 299539-1 30788-5 117621-1 260233-1
    rna_tRNA_Glu_TTC_5_1_53_+ -> 430317-1 5248-50 44483-3 53019-3 132312-1 133217-1 141461-1 142529-1 171559-1 223526-1 236957-1 246253-1 275767-1 310556-1 357519-1 329745-1 358639-1 388813-1 142441-1 229236-1 308190-1
    rna_tRNA_Glu_TTC_5_1_28_+ -> 13510-15 189046-1 233527-1 31244-5 18452-10 64859-2 74264-2 136213-1 378844-1 381562-1
    rna_tRNA_Glu_TTC_5_1_26_+ -> 18756-10 223568-1 253964-1 294850-1 351832-1 39415-4 51759-3 61041-2 94148-2 31244-5

Sample rlist output file *sample_result.rlist* is provided in "sample" folder.

The file structure is as follows (coordinates are in 1-based format and match those in the main file):
1. product name from main output file (transcript name_start_stop_strand), e.g. *ENST00000362645_1_23_+*
2. *->* sign which separates product_name and read list
3. read list. Uniq reads names, that creates the product, are separated with one whitespace.

#### Precursors output file *\<prefix\>.precursors*

Precursors output file contains informations about products precursors transcripts, using a following format:

    rna_tRNA_Glu_TTC_5	.	.	.	+	4	1738.0	897	51.61
    rna_tRNA_Glu_CTC_2	.	.	.	+	11	7503	3412	45.48
    rna_tRNA_Glu_CTC_1	.	.	.	+	12	7603	3494	45.96
    rna_tRNA_Glu_CTC_7	.	.	.	+	12	7603	3494	45.96

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

    rna_tRNA_Glu_TTC_5	0	33	rna_tRNA_Glu_TTC_5_1_33_+	1003	+
    rna_tRNA_Glu_TTC_5	0	53	rna_tRNA_Glu_TTC_5_1_53_+	390	+
    rna_tRNA_Glu_TTC_5	0	28	rna_tRNA_Glu_TTC_5_1_28_+	72.0	+
    rna_tRNA_Glu_TTC_5	0	26	rna_tRNA_Glu_TTC_5_1_26_+	67.0	+

Sample BED output file *sample_result.bed* is provided in "sample" folder.

The file structure is as follows (coordinates are in 0-based format, product coords are in 1-based format and match those in rest of files:
1. transcript name
2. product start
3. product stop
4. product name from main output file (transcript name_start_stop_strand), e.g. *rna_tRNA_Glu_TTC_5_1_33_+*
5. number of reads within the product
6. strand sign

#### Optional fasta output file *\<prefix\>.fasta*

Fasta output file is optional (when *-ec* or *-r* option is used) and contains informations about produkt sequence, either consensus or parsed from reference, using a following format:

    >rna_tRNA_Glu_TTC_5_1_33_+
    TCCCACATGGTCTAGCGGTTAGGATTCCTGGTT
    >rna_tRNA_Glu_TTC_5_1_53_+
    TCCCACATGGTCTAGCGGTTAGGATTCCTGGTTTTCACCCAGGCGGCCCGGGT
    >rna_tRNA_Glu_TTC_5_1_28_+
    TCCCACATGGTCTAGCGGTTAGGATTCC
    >rna_tRNA_Glu_TTC_5_1_26_+
    TCCCACATGGTCTAGCGGTTAGGATT

Sample fasta output file *sample_result.fasta* is provided in "sample" folder.

#### Commandline output

Additionally after the analysis missRNA will print basic statistics consisting of:
1. Number of all reads in analysis
2. Nuber of reads within identified stable RNAs
3. Percent of reads within identified stable RNAs

Example:

    Number of all reads in analysis:  11742
    Number of reads within identified stable RNAs:  9653
    Percent of reads within identified stable RNAs:  82.21%


#### Command used to generate sample output files

```../missRNA -i sample.bam -o sample_result -r Homo_sapiens.GRCh38.transcriptome.primary_assembly.fa -c```


### Contribute

If you notice any errors and mistakes, or would like to suggest some new features, please use Github's issue tracking system to report it. You are also welcome to send a pull request with your corrections and suggestions.

### Licence

This project is licensed under the terms of the GNU General Public License v3.0 license.






    


