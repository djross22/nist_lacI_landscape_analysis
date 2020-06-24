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