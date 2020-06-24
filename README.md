# nist_lacI_landscape_analysis
Starting point and instructions for software used for analysis of the NIST lacI landscape dataset

The custom software used for analysis of the NIST lacI landscape dataset includes the following 
software packages, available on GitHub as indicated:
1. bartender-1.1, barcode clustering algorithm
		Described in: Zhao, L., Liu, Z., Levy, S. F. & Wu, S. Bartender: a fast and accurate clustering algorithm to count barcode reads. Bioinformatics 34, 739–747 (2018).
		GitHub repository: https://github.com/LaoZZZZZ/bartender-1.1
		
2. NISTBartender, Windows GUI interface for dual barcode parsing and automating calls to bartender-1.1
		GitHub repository: https://github.com/djross22/NISTBartender
		
3. engineering-bio-lacI-landscape, bioinformatic pipeline for processing logn-read PacBio sequencing data
		GitHub repository: https://github.com/nate-d-olson/engineering-bio-lacI-landscape
		
4. gsf_ims_fitness, Python package and Jupyter notebooks used for analysis of sequencing data after bartender-1.1, NISTBartender, and engineering-bio-lacI-landscape
		GitHub repository: https://github.com/djross22/gsf_ims_fitness
		
5. dnn_model_rep_name, Python code to set up and train the deep neural network model
		GitHub repository: 
		
		
Detailed instructions for installing and running each software component are included with each components repository.

Once each component is installed, follow these steps to analyze a dataset:
1. Run the NISTBartender Windows GUI program.
	A. Load the file "example.xml".
	B. Use the File menu to select the input and output data directories.
		i. Make sure the filenames for the Forward and Reverse fastq files are correct (files should be g-zipped).
		ii. The Forward and Reverse fastq files fields can each contain multiple filenames. They should be in matching order and separated by commas.
	C. Click the "Analyze Sequences" button.
	D. Click the "Analyze Multi-Tags" button.
	E. Click the "Auto Regex" button.
	F. The Forward Read Sequence and Reverse Read Sequence fields should now show the amplicon sequence for barcode sequencing,
		with color codes to indicate various parts of the sequence. 
		Z's (yellow) indicate the UMI tags used for PCR jackpotting correction.
		X's (light purple) indicate the sample multiplexing tags.
		Light blue is used to highlight flanking sequences used for locating the multiplexing tags and the barcodes.
		Green is used to highlight the barcode sequences, with '*' used to indicate random bases in the barcodes.
	G. Click the "Parse" button. This will start the parsing algorithm. On a Windows PC with mid-level performance configuration, this takes about a minute for the example data, or approximately 2 hours for four lanes of HiSeq data.
		i. Test Parsing and Clustering initially with the "Max Sequences to Parse" field set to a relatively low value (10,000 to 1,000,000); then set to zero, to run the full set of input sequences.
	H. Click the "Cluster" button to run the bartender-1.1 clustering algorithm on the local PC, in the Windows Subsystem for Linux Ubuntu environment (see setup instructions in the file named "Ubuntu and Bartender setup on Windows 10 or AWS computer.docx")
		i. The command line call needed to run the clustering on AWS or another Linux system is displayed at the bottom of the Windows GUI; after initial testing (with Max Sequences to Parse set to low value), copy the bartender command line string, move the parser output files to the AWS/Linus system and paste the bartender command string into the command prompt to run the clustering algorithm for the full scale data.
		ii. On a high-end AWS machine, with four lanes of HiSeq data, the clustering algorithm takes about 6 hours to run.
	I. Move clustering output files back to Windows machine and Click "Merge Lengths" to run the algorithm to merge barcodes of different lengths (from sequencing in-del errors); the algorithm uses a Levenshtein distance criteria to merge barcode clusters of different lengths to account for in-del read errors.
		i. This step takes 20-30 minutes on a typical Windows PC for a 4-lane HiSeq dataset.
	J. Click the "Sort" button. This sorts the double barcode reads by sample and saves to a new file for each sample; then corrects for PCR de-jackpotting and creates an output file with the read counts for each double barcode for every sample ("xxx.sorted_counts.csv").
		i. This takes ~1 minute for the example data or ~3 hours for a HiSeq dataset.
	K. Click the "Trim Sorted BCs" button. This runs saves barcodes to new file with only double barcodes above the Sorted Barcode Cutoff Frequency; also re-orders the output from most abundant to least abundant barcodes and marks possible chimeric barcodes; saves result with new filename: "xxx.trimmed_sorted_counts.csv"
		i. This takes ~1 minute for the example data or ~6 minutes for a HiSeq dataset.
		
