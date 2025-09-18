---
title: 'Cost of gene panels sequencing'
date: 2025-09-18
permalink: /posts/2025/09/panel-sequencing/
tags:
  - Experiment Costs
  - DNA sequencing
  - Long-read
  - Data assets
toc: true
excerpt: 'Gene panels are curated sets of genes with known significance for a specific disease or collection of clinical symptoms that can help diagnose the disease.'
---

## Why do you do this experiment?

Gene panels are curated sets of genes with known significance for a specific disease or collection of clinical symptoms.

**Input** 100ng genomic DNA (~100k cells)

**Output** Fastq file (100k SE reads) -> High depth sequence of the genes in the panel

## Strategic Value

- Elucidate the cause of a genetic disease.
- Detect subclonal mutations to adapt treatment before the resistant clones cause a relapse (from biopsy or circulating tumor DNA).

## Cost & Scale

- Variable per run: **\\$58/sample** \\$61 (sequenced on large sequencer with other samples) - \\$113 (dedicated sequencing in batches of 10)
- Cost breakdown:
    + DNA extraction: \\$5
    + Panel enrichment: \\$55
    + Sequencing: \\$1-\\$53

- Capex: Thermocycler (\\$10-20k), TapeStation (\\$6-30k), [Nanodrop](https://www.thermofisher.com/fr/fr/home/industrial/spectroscopy-elemental-isotope-analysis/molecular-spectroscopy/uv-vis-spectrophotometry/instruments/nanodrop.html) (\\$15k), ONT GridION sequencer (\\$50k) or MiSeq i100 (\\$100k)

## Experimental Modules

1. DNA extraction (2h30, 40' hands-on)
2. Panel enrichment PCR (2h15, 30' hands-on)
3. Sequencing run (8h-72h depending on the sequencer, 30' hands-on)

## Ops & Throughput

**Turnaround**: 2 days (day 1 extraction, day 2 library prep + sequencing)

**Hands-on time**: 2h30

**Parallelizability**: High. All steps can be done in parallel for as many samples as needed.

**Bottlenecks**: Tapestation (16 lanes) and thermocycler (96 wells).

**Batching**: 1 to 16 samples per technician.

**Automation readiness**: Full, with commercial solutions available.

**Outsourceability**: Yes.

**Data scale**: 100k reads/sample, <1Gb/sample

## Data API
Raw format: FASTQ (via [POD5](https://github.com/nanoporetech/pod5-file-format) for ONT)
Processed format: Variant Call Format (VCF)
Resolution: gene level mutation

## Analysis Ecosystem

1. QC and cleaning
    - [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/): Quality control of the run
    - [cutadapt](https://cutadapt.readthedocs.io/en/stable/): Trimming of sequencing adapters from the reads
2. Alignement:
    - [bowtie2](https://bowtie-bio.sourceforge.net/bowtie2/index.shtml)
    - [minimap2](https://github.com/lh3/minimap2)
3. Variant calling
    - [Sniffles2](https://github.com/fritzsedlazeck/Sniffles)
    - [PAV](https://github.com/EichlerLab/pav)
    - [svim](https://github.com/eldariont/svim)
    - [pbsv](https://github.com/PacificBiosciences/pbsv)

## Public datasets

- [Genotype-Tissue Expression (GTEx)](https://gtexportal.org/home/downloads/adult-gtex/long_read_data): RNAseq from all major organs from a subset of individuals.
- [Gene Expression Omnibus (GEO)](https://www.ncbi.nlm.nih.gov/geo/): Repository of sequencing data from publications
- [European Nucleotide Archive (ENA)](https://www.ebi.ac.uk/ena/browser/home): Repository of sequencing data from publications

## Pitfalls & Failure Modes

- Panels with few genes (<20) or highly related genes will have low sequence complexity (all fragments will have similar sequences), which will lead to bad sequencing performance on sequencing-by-synthesis sequencers. To avoid this issue always sequence those amplicons with a complex library (e.g phiX or RNAseq).
- High molecular weight DNA (20-50kb) is fragile and cannot be extracted like low molecular weight DNA. Harsh mechanical manipulations like forcing through porous medium or pipetting too harshly lead to strand breakage.
TODO The recommended method is [trizol extraction](https://nanoporetech.com/document/extraction-method/rna-human-cells) which is cheap but requires good cleaning of the DNA.
- High molecular weight DNA in water is very viscuous. Don't hesitate do add more buffer to enable manipulation or start with less cells. Always pipette very slowly to [avoid breaking the strands](https://www.qiagen.com/us/applications/molecular-biology-research/hmw-dna). If your solution because less viscuous after pipetting up and down repeatedly it's likely than you broke the strands.
- Long read DNA sequencing methods relying on cDNA use polyA primers to generate the cDNA so will be exclusively composed of mDNA and lncDNA. If you are interested in other long DNAs (because if you are interested in short ones you should go for cheaper per read [short read sequencing](/posts/2025/09/short-read-sequencing)) use [polyA tailing](https://www.neb.com/en/protocols/2014/08/13/poly-a-tailing-of-rna-using-e-coli-poly-a-polymerase-neb-m0276), eventually after [ribo-depletion](https://www.neb.com/en/products/e6310-nebnext-rrna-depletion-kit-human-mouse-rat).
-

## Related publications

- Tracking of ALK mutations in the blood of lung cancer and neuroblastoma patients: [Horn2020](https://pmc.ncbi.nlm.nih.gov/articles/PMC6823161/), [Angeles2021](https://pmc.ncbi.nlm.nih.gov/articles/PMC8651695/), [Heeke2025](https://www.jtocrr.org/article/S2666-3643(25)00011-6/fulltext)

## Order list

**Short amplicon panel** (sequenced at \\$300/Gb on small short read sequencer)
Note that the cheapest single sequencing kit on the market as of September 2025 is the MiSeq i100 Series 5M Reagent Kit (300 cycles) which can accomodate 10-50 panels in parallel.
Whenever you can try to sequence panels on runs with more high throughput samples to save about \\$50 per panel. Panels barely take any reads which means they won't affect your complexity or your output significantly.

|Item|Cost|Number of experiments|Link|
|---------|--------|--------|
|Monarch Spin gDNA Extraction Kit|200|50|https://www.neb.com/en/products/t3010-monarch-spin-gdna-extraction-kit?srsltid=AfmBOooUGk_fw0xHD27m-7hWH86QLO4PjuA906RPBT6RHGOlmjuZskXH|
|PCR primers panel (2x20bp+sequencing adapters, 100 targets, 100nmol)|5000|100|https://eu.idtdna.com/pages/products/qpcr-and-pcr/custom-primers/rxnready-primer-pools|
|PCR-Core-Kit with Taq-DNA-Polymerase|400|200|https://www.sigmaaldrich.com/DE/de/product/sigma/coret|
|Genomic DNA ScreenTape Analysis|\\$450|100|https://www.agilent.com/en/product/automated-electrophoresis/tapestation-systems/tapestation-dna-screentape-reagents/genomic-dna-screentape-analysis-228261|
|Sequencing 1000x on Miseq i100 (100k reads, <0.03Gb)|\\$530|10-50|<\\$1 if done with other sample on large sequencer|
|---------|--------|--------|
|Total per xp|\\$58-\\$111|1||
|---------|--------|--------|

**Oxford nanopore** for long amplicon panels. We assume 20x multiplexing.

|Item|Cost|Number of experiments|Link|
|---------|--------|--------|
|MagAttract HMW DNA Kit|480|48|https://www.qiagen.com/us/products/discovery-and-translational-research/dna-rna-purification/dna-purification/genomic-dna/magattract-hmw-dna-kit-48|
|PCR primers panel (2x20bp+sequencing adapters, 100 targets, 100nmol)|5000|100|https://eu.idtdna.com/pages/products/qpcr-and-pcr/custom-primers/rxnready-primer-pools|
|PCR-Core-Kit with Taq-DNA-Polymerase|400|200|https://www.sigmaaldrich.com/DE/de/product/sigma/coret|
|Genomic DNA ScreenTape Analysis|\\$450|100|https://www.agilent.com/en/product/automated-electrophoresis/tapestation-systems/tapestation-dna-screentape-reagents/genomic-dna-screentape-analysis-228261|
|Qubit™ RNA High Sensitivity (HS)|\\$500|500|https://www.thermofisher.com/order/catalog/product/Q32855|
|Qubit™ Assay Tubes|\\$100|500|https://www.thermofisher.com/order/catalog/product/Q32856|
|ONT Native barcoding kit|\\$695|6|https://store.nanoporetech.com/eu/native-barcoding-kit-24-v14.html|
|MinION & GridION Flow Cell (R10.4.1)|\\$700|20|https://store.nanoporetech.com/eu/flow-cell-r10-4-1-ely.html|
|---------|--------|--------|
|Total per xp|\\$185|1||
|---------|--------|--------|
<!--
Monarch® HMW DNA Extraction Kit for Tissue|500|50|https://www.neb.com/en/products/t3060-monarch-hmw-dna-extraction-kit-for-tissue|
|ONT Ligation Sequencing Kit|600|6|https://store.nanoporetech.com/eu/ligation-sequencing-kit-v14.html|
-->

## Protocol variations

- For small panels (<10 genes), you will get a faster turnout and cheaper costs with Sanger Sequencing (e.g \\$10/sample with [Eurofins](https://eurofinsgenomics.com/en/products/dna-sequencing/sanger-sequencing/))
- Optimized panels are commercially available for many human genes involved in diseases (e.g [Ion AmpliSeq](https://www.thermofisher.com/fr/fr/home/life-science/sequencing/next-generation-sequencing/ion-torrent-next-generation-sequencing-workflow/ion-torrent-next-generation-sequencing-select-targets/ampliseq-target-selection/ion-ampliseq-on-demand-panels-targeted-sequencing.html))
- There are [two main ways to enrich a DNA sequence](https://www.illumina.com/techniques/sequencing/dna-sequencing/targeted-resequencing/targeted-panels.html). "Amplification" uses PCR to specifically amplify the sequence of interest. "Capture" fragments the DNA an captures the fragments containing the sequence of interest with biotinylated oligos and streptavidin-coated beads and sequences the enriched fraction. Amplification is limited to about 30kb with long-range PCR while capture is in theory not limited in size. Capture also provides a bit more context around the target sequence.
- [Whole Exome Sequencing](TODO) is a variation of capture-based panel sequencing with a panel consisting of >400k exonic sequences.
- [Adaptive sampling](https://a.storyblok.com/f/196663/x/adc22701be/gs_1089-en-_v3_28feb2025_digital.pdf) is an amplification-free approach available on ONT sequencing platforms where only strands with features of interest are sequenced.

{% include costs_disclaimer.html %}
