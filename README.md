# Genomes_masking

During the analysis of ChIP-Seq samples, the presence of artifacts such as pileups of multimapped reads or regions with low coverage due to assembly gaps can interfere with the identification of enrichment peaks.

Here we provide a simple procedure to identify these artifacts from input samples, chosen to represent as many different treatments and sequencing platforms as possible.

1.  Score the Read Coverage (RC) over 100-bp non-overlapping windows in each input sample. 
2.  Correct the Read Coverage for GC-content.      
           2a. Split the genome in 10 quantiles according to GC content.  
           2b. Divide the median RC of each quantile by the median RC of the genome, 
            to compute a quantile-specific and sample-specific correction factor.           
           2c. Divide the coverage of each 100-bp bin by the correction factor.
3.  calculate the Median Absolute Deviation (MAD) of the corrected coverage in each sample,
4.  Mask all the 100-bp genomic bins that are above or below the median +/- 3 MADs, respectively
5.  Pool all the samples, discard the 100-bp bins masked on only one sample and merge all the remaining contiguous bins.
