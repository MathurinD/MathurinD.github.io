---
title: 'Cost of long read RNA sequencing'
date: 2025-09-13
permalink: /posts/2025/09/long-read-sequencing/
tags:
  - Experiment Costs
  - RNAseq
  - Long-read
  - Data assets
toc: true
excerpt: 'RNA sequencing is performed to quantify the relative abundance of various RNA in a sample.'
---

## Why do you do this experiment?

Long-read RNA sequencing enables the identification and quantification of RNA expressed in a cell or a sample (the transcriptome) at the isoform resolution.
<!--
Long-read DNA sequencing enables the identification of the genomic sequence for complex regions such as highly repetitive regions, the resolution of complex chromosomal structural variations, and the quantification of DNA methylation. It can also be used for bacterial identification from metagenomes.
-->

**Input** 300ng polyA+ RNA or 1ug total RNA (~300k cells)

**Output** Fastq file (20-100M PE reads) -> Gene expression

## Strategic Value

<!--
By comparing multiple samples, we know the effect of perturbations (drug, disease, [knock-out](/2025-09-02-single-ko.md), etc) on the transcriptome of the cell. This can be used to understand gene regulation, how a drug works, or which processes a disease affects.

RNAseq provides the sequence of all expressed genes, meaning variants (e.g. SNPs, gene fusions) can be called but coverage will be biased towards highly expressed genes.
In the context of cancer and with deep enough RNAseq, sub-clonal exonic mutations can be detected for most genes.
-->

## Cost & Scale

- Variable per run: **\\$600/sample** \\$315 (cDNA) - \\$1160 (direct RNA)
- Cost breakdown:
    + RNA extraction: \\$56
    + Long-read library preparation: \\$50 - \\$150
    + Sequencing (5M reads, 60Gb): \\$
- Capex: Thermocycler (\\$10-20k), TapeStation (\\$6-30k), ONT PromethION sequencer (\\$50-500k) or PacBio sequencer (\\$250k-600k)

## Experimental Modules

1. RNA extraction (2h30, 40' hands-on)
2. Sequencing library preparation (2h15, 30' hands-on)
3. Sequencing run (48-72h depending on the sequencer)

## Ops & Throughput

**Turnaround**: 3+ days (day 1 extraction, day 2 library prep, day 3 or later sequencing 48-72h)

**Hands-on time**: 4h

**Parallelizability**: High. All steps can be done in parallel for as many samples as needed.

**Bottlenecks**: availability of sequencer (4-16 samples on Revio, 24 on ONT P24) Tapestation (16 lanes) and thermocycler (96 wells).

**Batching**: 1 to 16 samples per technician.

**Automation readiness**: Full, with commercial solutions available.

**Outsourceability**: Yes.

**Data scale**: 20-100M reads/sample, ~60Gb/sample

## Data API
Raw format: FASTQ (via [POD5](https://github.com/nanoporetech/pod5-file-format) for ONT)
Processed format: count matrix
Resolution: transcript-level expression, single nucleotide variants

## Analysis Ecosystem

0. (ONT) 
    - [dorado](https://github.com/nanoporetech/dorado): Official base caller by ONT
    - [remora](https://github.com/nanoporetech/remora)
1. QC and cleaning
    - [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/): Quality control of the run
    - [cutadapt](https://cutadapt.readthedocs.io/en/stable/): Trimming of sequencing adapters from the reads
2. Alignement:
    - [minimap2](https://github.com/lh3/minimap2)
    - [LRA](https://github.com/ChaissonLab/LRA)
3. Gene expression quantification:
    - [htseq-count](https://htseq.readthedocs.io/en/release_0.11.1/count.html): Gene-read overlap counts
    - [salmon](https://combine-lab.github.io/salmon/): Quantification taking into account bias in the sequencing method
4. Differential expression
    - [DELongSeq](https://pmc.ncbi.nlm.nih.gov/articles/PMC9985341/) for isoform differential expression.
    - [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html) or [PyDESeq2](https://pydeseq2.readthedocs.io/en/stable/)
    - [edgeR](https://bioconductor.org/packages/release/bioc/html/edgeR.html) or [edgePy](https://edgepy.readthedocs.io/en/latest/index.html)
    - [Sleuth](https://pachterlab.github.io/sleuth_walkthroughs/trapnell/analysis.html)
    <!-- - [glmgampoi](https://bioconductor.org/packages/release/bioc/html/glmGamPoi.html) -->
5. Variant calling
    - [Sniffles2](https://github.com/fritzsedlazeck/Sniffles)
    - [PAV](https://github.com/EichlerLab/pav)
    - [svim](https://github.com/eldariont/svim)
    - [pbsv](https://github.com/PacificBiosciences/pbsv)

## Public datasets

- [Genotype-Tissue Expression (GTEx)](https://gtexportal.org/home/downloads/adult-gtex/long_read_data): RNAseq from all major organs from a subset of individuals.
- [Gene Expression Omnibus (GEO)](https://www.ncbi.nlm.nih.gov/geo/): Repository of sequencing data from publications
- [European Nucleotide Archive (ENA)](https://www.ebi.ac.uk/ena/browser/home): Repository of sequencing data from publications

## Pitfalls & Failure Modes

- Long read RNA sequencing methods relying on cDNA use polyA primers to generate the cDNA so will be exclusively composed of mRNA and lncRNA. If you are interested in other long RNAs (because if you are interested in short ones you should go for cheaper per read [short read sequencing](/posts/2025/09/short-read-sequencing)) use [polyA tailing](https://www.neb.com/en/protocols/2014/08/13/poly-a-tailing-of-rna-using-e-coli-poly-a-polymerase-neb-m0276), eventually after [ribo-depletion](https://www.neb.com/en/products/e6310-nebnext-rrna-depletion-kit-human-mouse-rat).

## Related publications

- [Helal2024](https://www.nature.com/articles/s41598-024-56604-2): Benchmark of long-read aligners
- [Sakamoto2019](https://www.nature.com/articles/s10038-019-0658-5): overview of the benefits of long-read sequencing for cancer genomics.
- [Ebbert2019](https://link.springer.com/article/10.1186/s13059-019-1707-2): uncovering the "dark" genome with long-read sequencing.
- [Glinos2022](https://pmc.ncbi.nlm.nih.gov/articles/PMC10337767/) "Transcriptome variation in human tissues revealed by long-read sequencing"
- [ONT transcriptome pipeline](https://github.com/nanoporetech/pipeline-transcriptome-de)
- [Wang2024](https://www.nature.com/articles/s41467-024-51639-5): Customizing ONT base-calling to improve detection of modifications

## Order list

**Oxford nanopore** starting from extracted RNA.

|Item|Cost|Number of experiments|Link|
|---------|--------|--------|
|PromethION Flow Cell Packs|\\$4000|4-16|https://store.nanoporetech.com/eu/promethion-flow-cell-packs-r10-4-1-m-version-2025.html|
|(multiplexing) cDNA-PCR Barcoding Kit V14|750|144|https://store.nanoporetech.com/eu/cdna-pcr-barcoding-kit-v14.html|
|(direct RNA) Direct RNA Sequencing Kit|\\$600|6|https://store.nanoporetech.com/eu/direct-rna-sequencing-kit-004.html|
|Induro® Reverse Transcriptase and 5x Induro® RT Reaction Buffer (NEB, M0681)|\\$200|20|https://www.neb.com/en-us/products/m0681-induro-reverse-transcriptase|
|RNAse inhibitor||\\$600|100|https://www.neb.com/en/products/m0314-rnase-inhibitor-murine|
|dNTP mix|\\$300|600|https://www.neb.com/en/products/n0447-deoxynucleotide-dntp-solution-mix|
|NEBNext® Quick Ligation Module|\\$400|20|https://www.neb.com/en/products/e6056-nebnext-quick-ligation-module?srsltid=AfmBOorXl-1Gi1lRYSdY_Jho1SkcAJHKD2uDSeUBcift4YTJwUje9Aac|
|RNAClean XP RNA and cDNA Cleanup Reagent, 40 mL|\\$1200|400|https://www.beckman.fr/reagents/genomic/cleanup-and-size-selection/rna-and-cdna/a63987|
|Qubit™ RNA High Sensitivity (HS)|\\$500|500|https://www.thermofisher.com/order/catalog/product/Q32855|
|Qubit™ Assay Tubes|\\$100|500|https://www.thermofisher.com/order/catalog/product/Q32856|
|---------|--------|--------|
|Total per xp|\\$315 (cDNA with multiplexing) - \\$1160 (direct RNA)|1||
|---------|--------|--------|

<!--
nanopore = 250:1000 + 100 + 10 + 1 + 20 + 30 + 1
|Random Primer Mix|||https://www.neb.com/en/products/s1330-random-primer-mix|
https://www.pacb.com/revio/
https://gcore.ucsd.edu/isoseq-pricing
for 20x WGS: |Revio SPRQ sequencing plate|\\$4000|4-8|https://www.pacb.com/products-and-services/consumables/hifi-sequencing-kits/|
pacbio = 250 + 140 + 200 + 1 + 0.2
-->

|Item|Cost|Number of experiments|Link|
|---------|--------|--------|
|Revio SPRQ sequencing plate|\\$4000|4|https://www.pacb.com/products-and-services/consumables/hifi-sequencing-kits/|
|Kinnex full-length RNA kit|\\$700|1-4|https://www.pacb.com/products-and-services/consumables/application-kits/|
|Iso-Seq express 2.0 kit|\\$2400|12|https://www.pacb.com/products-and-services/consumables/application-kits/|
|Qubit™ RNA High Sensitivity (HS)|\\$500|500|https://www.thermofisher.com/order/catalog/product/Q32855|
|Qubit™ Assay Tubes|\\$100|500|https://www.thermofisher.com/order/catalog/product/Q32856|
|---------|--------|--------|
|Total per xp|\\$600 (with multiplexing) - \\$1900|1||
|---------|--------|--------|

## Protocol variations

- 10X genomics used to provided so-called [linked reads sequencing](https://www.10xgenomics.com/products/linked-reads) where long reads were isolated in droplets, fragmented, and the fragments barcoded with the same barcode.


{% include costs_disclaimer.html %}
