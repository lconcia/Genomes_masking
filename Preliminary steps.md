## Preliminary steps
#### 1. Download the assembly of interest
```bash
wget https://github.com/schatzlab/Col-CEN/blob/main/v1.2/Col-CEN_v1.2.fasta.gz?raw=true -O Col-CEN_v1.2.fasta.gz
```
#### 2. Uncompress the file
```bash
gzip -d Col-CEN_v1.2.fasta.gz
```
#### 3. Index the file and extract the chromosome size
```bash
samtools faidx Col-CEN_v1.2.fasta
cut -f1,2      Col-CEN_v1.2.fasta.fai > Col-CEN_v1.2.sizes.genome
```
#### 4. Split the genome in 100-bp non overlapping windows
```bash
bedtools makewindows -g Col-CEN_v1.2.sizes.genome -w 100 > Col-CEN_v1.2.100bp.bed
```
#### 5. Calculate GC% in the 100-bp bins
```bash
bedtools nuc -fi  Col-CEN_v1.2.fasta -bed Col-CEN_v1.2.100bp.bed | \
cut -f 1-3,5 | tail -n +2 > Col-CEN_v1.2.GC_value.bedgraph
```
#### 6. Count the reads mapping on each bins in each input samples
```bash
for f in *input.bam
   do
bedtools multicov -bams $f -bed Col-CEN_v1.2.100bp.bed > $f.100bp.bed
   done
```
#### 7. Concatenate the bed files
```bash
paste \
 <( cut -f 1-3 /kingdoms/mpb/workspace19/concia/H1_shared/reference_genomes.IBENS/CEN.genome_assembly/CEN.100bp.no_ChrM_ChrC.bed ) \
 <( cut -f 4 CEN.100bp.180307_NB501850_A_L1-4_ANYO-1_R1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.180903_NB501850_A_L1-4_ANYO-16_R1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.201224_I22_V300086931_L4_C2_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.201224_I22_V300086931_L4_D3_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.201224_I22_V300086931_L4_D8_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.201224_I22_V300086931_L4_G4_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_336_S2_L001_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_336_S2_L001_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_336_S2_L002_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_336_S2_L002_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_336_S2_L003_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_336_S2_L003_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_336_S2_L004_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_336_S2_L004_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_351_S9_L001_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_351_S9_L001_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_351_S9_L002_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_351_S9_L002_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_351_S9_L003_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_351_S9_L003_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_351_S9_L004_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.2017_351_S9_L004_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.210318_I100400190019_V300099928_L1_IP_WT_D_R1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.210318_I100400190019_V300099928_L1_IP_WT_D_R1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.210318_I100400190019_V300099928_L1_IP_WT_L_R1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.210318_I100400190019_V300099928_L1_IP_WT_L_R1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.211110_M029_V350030665_L02_input_1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.211110_M029_V350030665_L02_input_2_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.211110_M029_V350030665_L02_input_3_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.211110_M029_V350030665_L02_input_4_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.6_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.6_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.antiHY5-input-wt_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.antiHY5-input-wt_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.H2Bub_input_wt_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.H2Bub_input_wt_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_1A_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_1A_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_1B_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_1B_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_1_wt_D_rep1_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_1_wt_D_rep1_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_1_wt_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_1_wt_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_6_wt_D_rep2_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_6_wt_D_rep2_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_col_22C_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_col_22C_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_col_37C_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_col_37C_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_K27_WT_rep1_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_K27_WT_rep1_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_K27_WT_rep2_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_K27_WT_rep2_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_wt_L1_1.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input_wt_L1_2.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input-WT-rep2_R1_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.input-WT-rep2_R2_001.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.INWT_35217_CGATGT.sorted.filtered.nodup.bed ) \
 <( cut -f 4 CEN.100bp.SIM7_1.sorted.filtered.nodup.bed )  > \
  CEN.100bp.masking.Vidal_samples.21Feb2023.bed
```

