### Repeat Explorer

The following pipeline was used to identify repetitive DNA of Illumina sequencing of two sets of data: (1) whole genomic DNA (gDNA); and (2) probe of entire chromosome (obtained by flow sorting and fragmented by a DOP-PCR reaction). Both samples from the species *Holochilus sciureus* (2n = 56, NF = 56), a Neotropical rodent of tribe Oryzomyini.

**Software used:**

[RepeatExplorer (command line version)](http://repeatexplorer.org/?page_id=166#command-line-version). Novák P, Neumann P, Pech J, Steinhaisl J, Macas J (2013). RepeatExplorer: a Galaxy-based web server for genome-wide characterization of eukaryotic repetitive elements from nextgeneration sequence reads. Bioinformatics, 29(6):792-793.

**Input data:**
- Illumina sequencing data.

**Simple analysis**

- Step 1 > Filter the reads by quality:

`/repeatexplorer/utils/paired_fastq_filtering.R -a sample0B_r1.fastq -b sample0B_r2.fastq -x sample0B_r1_filtered.fasta -y sample0B_r2_filtered.fasta`
 
- Step 2 > Merge output files:

`/repeatexplorer/utils/fasta_interlacer -a sample0B_r1_filtered.fasta -b sample0B_r2_filtered.fasta -p sample0B_r1_r2_filtered_merged.fasta -x sample0B_r1_r2_filtered_nopair.fasta`

- Step 3 > Cluster the repetitive elements:

`/repeatexplorer/seqclust_cmd.py -s sample0B_r1_r2_filtered_merged.fasta -d Rodentia -m .1 -p -c 50 -r 100000 -v sample0B > report_sample0B`

**Comparative analysis**

- Step 1 > Filter the reads by quality:

`/repeatexplorer/utils/paired_fastq_filtering.R -a sample0B_r1.fastq -b sample0B_r2.fastq -x sample0B_r1_filtered.fasta -y sample0B_r2_filtered.fasta`

`/repeatexplorer/utils/paired_fastq_filtering.R -a sample1B_r1.fastq -b sample1B_r2.fastq -x sample1B_r1_filtered.fasta -y sample1B_r2_filtered.fasta`
  
- Step 2 > Merge output files:

`/repeatexplorer/utils/fasta_interlacer -a sample0B_r1_filtered.fasta -b sample0B_r2_filtered.fasta -p sample0B_r1_r2_filtered_merged.fasta -x sample0B_r1_r2_filtered_nopair.fasta`

`/repeatexplorer/utils/fasta_interlacer -a sample1B_r1_filtered.fasta -b sample1B_r2_filtered.fasta -p sample1B_r1_r2_filtered_merged.fasta -x sample1B_r1_r2_filtered_nopair.fasta`

- Step 3 > Add an identification prefix to the sample (samples must have the same number of characters in the prefix):

`sed 's/>/>sample0B/g' sample0B_r1_r2_filtered_merged.fasta > sample0B_r1_r2_filtered_merged_comparative.fasta`

`sed 's/>/>sample1B/g' sample1B_r1_r2_filtered_merged.fasta > sample1B_r1_r2_filtered_merged_comparative.fasta`

- Step 4 > Merge output files:

`cat sample0B_r1_r2_filtered_merged_comparative.fasta sample1B_r1_r2_filtered_merged_comparative.fasta > sample0B1B_r1_r2_filtered_merged_comparative.fasta`

- Step 5 > Cluster the repetitive elements:

`/repeatexplorer/seqclust_cmd.py -s sample0B1B_r1_r2_filtered_merged_comparative.fasta -d Rodentia -m .1 -p -c 50 -f 8 -r 100000 -v sample0B1B_comparative > report_sample0B1B_comparative`

**Parameters used:**
- -d database.
- -c number_of_cpus.
- -f prefix_number.
- -r number_of_memory_KB.
- -v name_of_output_foder.
