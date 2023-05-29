# CCMpred implementaion on one TM protein (1aig chain L)
<span style="color: red;">**Whta is CCMpred?**</span>

CCMpred (Conditional Coupling Model) is a bioinformatics tool used for predicting residue-residue contacts in protein sequences. It is based on the statistical analysis of large multiple sequence alignments (MSAs) and employs a machine learning approach to infer the likelihood of residue pairs being in contact based on their co-evolution patterns.

CCMpred uses a probabilistic model to capture the co-evolutionary signals in the MSAs, which are indicative of residues that tend to co-occur in the three-dimensional (3D) structure of proteins.

<span style="color: red;">**The input and output of CCMpred**</span>
#### input: 
- Multiple sequence alignment of the target protein or sequence (here 1aig) in .aln format
#### the output:
- An output file represent a matrix of shape L* L where L is the length of the query sequence (here 281 residues).
- The output file contains **the predicted co-evolution scores for residue pairs** in a protein sequence.
- The scores represent the predicted strength of co-evolution between each pair of residues.
-  contacts between residues are typically defined based on a contact threshold applied to the predicted co-evolution scores.
-  a common approach is to choose a contact threshold that corresponds to the top L predicted contacts, where L is typically determined based on the expected number of true contacts in the protein of interest.
-  "L" refers to the number of top predicted contacts to consider. It is typically determined based on the expected number of true contacts in the protein of interest.
-  For example, if the protein of interest is known to have approximately 100 true contacts, then L can be set to 100 to consider the top 100 predicted contacts. 


## Running Steps:

### Installation

	git clone --recursive https://github.com/soedinglab/CCMpred.git

### Compilation
- first, we need to change the CCMpred_my_implemenation/lib/libconjugrad/include/conjugrad.h file by adding `include <cmath>`
- With the sourcecode ready, simply run cmake with the default settings and libraries should be auto-detected:

	cmake .
	make

You should find the compiled version of CCMpred at `bin/ccmpred`. To check if the CUDA libraries were detected, you can run `ldd bin/ccmpred` to see if CUDA was linked with the program, or simply run a prediction and check the program's output.
	
### Creating a Multiple Sequence Alignment file Using HHblits
- First we need to download a database to conduct the MSA search on. (here I use Uniclust30 from https://gwdu111.gwdg.de/~compbiol/uniclust/2022_02/)
- Second, we use HHblits to create MSA so we can run CCmpred on those alignments to get the co-evolution scores
- HHblits can be downloaded from (https://github.com/soedinglab/hh-suite)
- we run the search using the following command:
	`hhblits -i <input-file> -o <result-file> -n 1 -d <database-basename>`
- where, i= input, -o output we should use -oa3m insted of -o to output a .a3m format, d=database

### Converting the .a3m format to .aln format
- CCmpred accept MSA of .aln format, so we need to change the format of the a3m file to .aln 
- we use the following code to do that
	`egrep -v "^>" example.a3m | sed 's/[a-z]//g' | sort -u > example.aln`





## References
- The original implementation of CCMpred (https://github.com/soedinglab/CCMpred)
- The original implementation of HHblits (https://github.com/soedinglab/hh-suite)
