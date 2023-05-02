# Genomes_masking

During the analysis of ChIP-Seq samples the presence of artifacts such as pileups of multimapped reads or regions with low coverage due to assembly gaps can bias the identification of enrichment peaks.

Here we provide a simple procedure to identify these artifacts from input samples, and generate an assembly-specific file to mask them.

1) score the Read Coverage (RC) in each input sample. The i representing different treatment and sequencing platforms as non-overlapping windows of 100bp and corrected for GC content (31); 

2) split the genome of each sample in 10 quantiles according to GC content; 
3) compute a correction factor for each sample by dividing the median coverage of each quantile by the median coverage of the genome; 
4) use this quantile- specific correction factor to divide the coverage of each 100-bp bin; 
5) calculate the median absolute deviation of the corrected coverage values in each sample, and mask all the 100-bp genomic bins that are above or below the median +/- 3 MADs, respectively; 6) pool all the samples, filter the 100-bp bins masked on only one sample and merged all the contiguous bins masked in the most relevant samples. 



1- Generate a bed file with 100bp windows genome wide

2- Calculate GC content per window: I remembered I did it very home made at that time. I used FastaFromBed to retreive the sequence of each window and export as a table. Then I made a loop across the table and, for each line, I removed all A and T using something like sed 's/A//g' | sed 's/T//g', and the use wc to cunt the number of remaining nucleotides (i.e Gs and Cs).

2- map Col-0 reads on Col-CEN and extract coverage per window (for instance number of reads per window) by coverageBed or intersectBed -c

3- To correct read depth based on GC content, you should make a boxplot of coverage per windows (y) vs GC content (x). This should looks like a sad smile (intermediate GC contents are typically sequenced more than low or high GC content). Then, for each category of GC contents (for instance windows with 20 to 30% GC content), you divide the median value of read depth of the categary, by the median of all windows. This will be your correcting factor for windows with 20 to 30% GC content. You do the same for all the categories. You apply this corrections factors (basically you have to divide the read depth of the window for its corresponding correction factor) to all the windows.

4-You calculate the median and the Median absolute deviation for the corrected read-depth values.

5-All 100bp windows that are above or below median +/- 3MAD are outlyers.
