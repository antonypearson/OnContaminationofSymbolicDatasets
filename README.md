# OnContaminationofSymbolicDatasets
Processed data for well-covered CpG triplets

WGBS data from 121 experimental replicates representing 77 unique biological samples, publicly available from ENCODE. These replicates include clinical tissue samples, cell lines, and primary cells. We selected replicates using single-end reads which were not flagged by ENCODE as having low coverage or insufficient read length. The list of the sample identifiers is found in ENCODE_IDs.xlsx.

Each replicate is associated with a BAM file generated by mapping reads to GRCh38 using Bismark. We used the MethPipe methylation software suite to convert BAM files into MethPipe format and generate epiread files, an efficient format reporting the genomic index and methylation status of each CpG contained in a read. 

For each replicate, we used the epireads file generated to extract data from ``well-covered'' triplets---i.e. those where all three CpGs are jointly covered by at least 100 reads, discarding reads which report a CpG with ambiguous methylation status. For each autosome in each sample we estimated the exchangeable weight of each well-covered triplet and corrected for estimator bias using a sample mean of N=1000 full bootstrap resamples. Although each estimate of a triplet's exchangeable weight $\hat\lambda$ lies in $[0,1]$, the bias-adjusted estimate $(\hat\lambda-\bar{\hat\lambda^*})$ may be larger than $1$ or smaller than $0$. Therefore we truncate these estimates to $[0,1]$. The Numpy files here contain processed triplets corresponding to each BAM file ID. Each row corresponds to a well-covered triplet, with columns corresponding to 1) chromosome number, 2) index of the triplet on the chromosome, 3) an estimate of the total variation distance to the class of exchangeable distributions, 4) an estimate of the exchangeable weight of the triplet (bias-corrected), 5) a bootstrap estimate of the standard deviation of $\hat\lambda$, 6-13) the counts of each of the 8 possible triplet configurations (ordered lexicographically, i.e. `000', `001', etc.), and 14-21) an estimate of the largest exchangeable component.
