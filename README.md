# Inspecting-Variants-in-IGV

Protocol derived from [Matt Eldridge, Cancer Research UK Cambridge Institute](https://bioinformatics-core-shared-training.github.io/intro-to-IGV/InspectingVariantsInIGV.html)

#### STEP1: extract reads from bam file
view the header of the bam file, check the format of the chr, e.g. chr10, Chr10 or 10?
`samtools view -H your.bam`

to extract alll reads in chr10 between 18000-45500 bp.
`samtools view -h -b input.bam "chr11:120,833,548-120,833,865" > output.bam`

index the bam files
`samtools index output.bam`

or index multiple bam files at one place
```
#!/usr/bin/env bash

# index_all.sh

for i in "$@"
do
   echo "Indexing: "${i}
   samtools index ${i}
done

./index_all.sh /dir/*.bam
```

#### STEP2: load the bam file into IGV
File -> Open .bam fifle -> select chr and type in the regions of interest, e.g. chr11:120833100-120834100 -> Go -> double click the main visual panel to zoom in the read alignments, do this a few times until aligned reads are loaded and displayed.

Possible variants are highlighted in the coverage track where the allele fraction is above a configurable threshold. These are the coloured stacked bars within what is a mostly grey coverage plot, where the coloured portion of each bar represents the fraction of reads with different alleles at that position.


#### STEP3: load dbSNP anno track to see if the variants are known SNPs
File -> Load from Server -> Available Datasets > Annotations > Variation and Repeats > dbSNP

The dbSNP track is at the bottom of the IGV window. Black bars represent known SNPs.


#### STEP4: choose which SNVs to color
We can adjust the allele fraction threshold **above** which the bar in the coverage track will be coloured by allele read count.
Choosing View > Preferences dialog -> Alignments tab -> Change the Coverage allele-fraction threshold to e.g. 0.1 and click OK.

> Zoom out again and observe the uneven coverage across the region. In some parts of the region the coverage drops to zero. It will be much more difficult to reliably identify variants in a low coverage region.


#### STEP5: Examining read alignments
Refer to Matt's tutorial.
