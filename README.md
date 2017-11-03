# BAGEL
## Bayesian Analysis of Gene Essentiality

BAGEL is a Bayesian classifier for pooled library genetic perturbation screens, using either CRISPR-Cas9 or shRNA libraries. It uses training sets of known essential and nonessential genes to estimate what the fold change distribution of an essential or nonessential gene should look like. Then, for each uncharacterized gene, it takes all observations of reagents targeting that gene (guide RNA, for CRISPR-Cas9 screens, or short hairpin RNA) and makes a probabilistic statement about whether those observations were more likely drawn from the essential or nonessential training set. A log2 Bayes Factor for each gene is reported.
Usage

```
BAGEL.py -i [fold change file] -o [output file] -e [reference
     essentials] -n [reference nonessentials] -c [columns to test]

  from the Bayesian Analysis of Gene EssentiaLity (BAGEL) suite
  Version 0.91

  required options:
     -i  [fold change file]         Tab-delmited file of reagents and 
                 fold changes.  See below for format.
     -o  [output file]              Output filename
     -e  [reference essentials]     File with list of training set of 
                essential genes
     -n  [reference nonessentials]  File with list of training set of
                nonessential genes
     -c  [columns to test]          comma-delimited list of columns in
                input file to include in analyisis

  other options:
     --numiter=N                    Number of bootstrap iterations 
                (default 1000)
     -h, --help                     Show this help text
```
BAGEL.py calculates a log2 Bayes Factor for each gene; positive BFs indicate confidence that the gene is essential. The output file has the following format:
```
GENE    BF      STD     NumObs
A1BG    -1.751  0.593   391
A1CF    -7.959  0.675   395
A2M     -7.948  0.679   391
A2ML1   -14.297 0.719   359
```
GENE is specified in the input file. BF is the mean of the log2 Bayes Factors calculated across NumObs bootstrap iterations, and STD is the standard deviation of those BFs.

The input file must follow this format:
```
SEQID   GENE    T05A    T05B    T13A    T13B    T21A    T21B
Seq1    A1BG    1.006   -0.040  -0.024  0.761   0.346   0.395
Seq2    A1BG    0.163   0.009   -0.111  -0.866  -1.377  -1.912
Seq3    A1BG    0.031   -0.822  -0.475  -0.565  -0.687  -0.416
Seq4    A1CF    -0.298  0.005   -0.170  0.002   -0.072  -0.080
Seq5    A1CF    -0.678  -0.109  -0.722  -0.481  -0.327  -0.750
Seq6    A1CF    0.727   0.516   0.230   0.993   0.656   0.724
Seq7    A2M     -0.789  1.166   1.574   1.034   1.658   1.529
Seq8    A2M     -0.045  -0.406  -0.608  0.505   -0.425  -0.361
Seq9    A2M     -0.272  0.246   -0.427  -0.498  -0.652  -0.538
```

The format is:
The file must be tab delimited text file
There must be a header line
The first column is the reagent ID; it is not used by BAGEL
The second column must be the gene symbol targeted by the reagent in column 1
Subsequent columns must contain a log2 fold change.

For the above file, to calculate Bayes Factors for the T21 timepoint, use the following command line:
```
BAGEL.py -i foldchange_file -o output_file.bf -e essentials_training_set -n nonessentials_training_set -c 5,6
```
Columns 5 and 6 correspond to the T21A and T21B replicates above. Note that column numbering starts with 1 at the column after gene name.
Training sets of essential and nonessential genes, for experiments with human cell lines, can be downloaded from the Files tab above.
