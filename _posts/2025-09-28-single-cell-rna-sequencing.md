---
title: 'Cost of single cell RNA sequencing'
date: 2025-09-28
permalink: /posts/2025/09/single-cell-rna-sequencing/
tags:
  - Experiment Costs
  - Single-cell
  - RNAseq
  - Data assets
toc: true
excerpt: ''
---

## Why do you do this experiment?

Single cell RNA sequencing measures the expression of gene transcripts in individual cells.

**Input** 100-10M cells

**Output** Fastq file (100M-250B PE reads) -> Single cell gene expression

## Strategic Value

- Characterise cell type and cell state in a complex sample (developing embryo, tumor sample or simply a healthy tissue) and their individual response to perturbations.
- Measure the response of cells to many parallel perturbations in parallel with CROPseq, PerturbSeq or pooled cell culture.

<!--
By comparing multiple samples, we know the effect of perturbations (drug, disease, [knock-out](/2025-09-02-single-ko.md), etc) on the transcriptome of the cell. This can be used to understand gene regulation, how a drug works, or which processes a disease affects.

RNAseq provides the sequence of all expressed genes, meaning variants (e.g. SNPs, gene fusions) can be called but coverage will be biased towards highly expressed genes.
In the context of cancer and with deep enough RNAseq, sub-clonal exonic mutations can be detected for most genes.
-->

## Cost & Scale

- Variable per run: **\\$2700 for 20k cells**, \\$190 (96 cells) - \\$36,500 (1M cells)
- Cost breakdown:
    + Cell barcoding of RNA: \\$90 (96 cells) - \\$10k (1M cells)
    + Sequencing: \\$100 (10M, 1Gb) - \\$16,500 (25B reads, 7.5Tb)
- Capex: [Magnetic Stand 96](https://www.thermofisher.com/order/catalog/product/AM10027) (\\$800), Thermocycler (\\$10-20k), TapeStation (\\$6-30k), [Chromium Controller](https://www.10xgenomics.com/instruments/chromium-controller) (\\$20k, needed for 10x Genomics only), Illumina NovaseqX, MGI T20 or UltimaGenomics UG100 (\\$800k-1M)

## Experimental Modules

1. Cell barcoding of RNA (8h, 4h hands-on)
2. Sequencing library preparation (2h15, 30' hands-on)
3. Sequencing run (48h, 30' hands-on)

## Ops & Throughput

**Turnaround**: 4+ days (day 1 single cell RNA barcoding, day 2 library prep, day 3 or later sequencing >40h)

**Hands-on time**: 5h

**Parallelizability**: High. All steps can be done in parallel for as many samples as needed.

**Bottlenecks**: availability of sequencer (2-4 flowcells per sequencer fully occupied).

**Batching**: 1 preparation per technician, number of samples up to 96 depending on the protocol multiplexing possibilities.

**Automation readiness**: Partial. Custom solution via automation specialists for [Parse Bioscience](https://www.parsebiosciences.com/single-cell-automation/) and [Scale Bioscience](https://6586853.fs1.hubspotusercontent-na1.net/hubfs/6586853/SPT%20Labtech%20Website/09%20-%20Resources/SPT%20Labtech%20Scale%20Biosciences%20Automating%20ScaleBio%20Single%20Cell%20RNA%20Kit%20on%20firefly.pdf). Partially released [Chromium Connect](https://www.10xgenomics.com/support/instruments/chromium-connect/chromium-connect-software-release-note) by 10x Genomics. Worth mentionning is the [Cellen One X1 Neo](https://www.cellenion.com/lp/cellenone/) which can easily be adapted for SmartSeq2 automation.

**Outsourceability**: Yes, [most CROs](https://www.google.com/search?q=single+cell+sequencing+cro) offer it.

**Data scale**: 100M-25B reads/sample, 1Gb-7.5Tb/sample

## Data API

Raw format: FASTQ

Processed format: sparse single cell expression count matrix -> cell type (with [RNA velocity](https://www.nature.com/articles/s41587-020-0591-3) if relevant)

Resolution: 3'-biased polyA gene products expression for individual cells

## Analysis Ecosystem

1. QC and cleaning
    - [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/): Quality control of the run
    - [cutadapt](https://cutadapt.readthedocs.io/en/stable/): Trimming of sequencing adapters from the reads
2. Read deduplication via UMI, alignement and cell barcode attribution pipelines (most use STAR under the hood):
    - [CellRanger](https://www.10xgenomics.com/support/software/cell-ranger/latest) for 10x Genomics
    - [splitpipe](https://support.parsebiosciences.com/hc/en-us/articles/36214328983828-Guided-walkthrough-Pipeline-module-set-up) for Parse Bioscience
    - [smartseq2 pipeline](https://github.com/nf-core/smartseq2)
3. (optional) RNA velocity
    - [scvelo](https://scvelo.readthedocs.io/en/stable/) for RNA velocity
3. Count processing and cell clustering
    - [scanpy](https://scanpy.readthedocs.io/en/stable/) in python, faster and better suited for large dataset (>100k)
    - [Seurat](https://satijalab.org/seurat/) in R
5. Cell type annotation (many tools exist, including foundation models)
    - [ScType](https://sctype.app/): Marker based annotation
    - Single cell foundation models such as [scBERT](https://www.nature.com/articles/s42256-022-00534-z), [Geneformer](https://www.nature.com/articles/s41586-023-06139-9), [scGPT](https://www.nature.com/articles/s41592-024-02201-0), [CellFM](https://www.nature.com/articles/s41467-025-59926-5), or [xTrimoscFoundation](https://www.nature.com/articles/s41592-024-02305-7). 
4. Differential expression
    - [glmgampoi](https://bioconductor.org/packages/release/bioc/html/glmGamPoi.html): Fast gamma-poisson distribution fitting for single cell data.
    - [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html) or [PyDESeq2](https://pydeseq2.readthedocs.io/en/stable/)
    - [edgeR](https://bioconductor.org/packages/release/bioc/html/edgeR.html) or [edgePy](https://edgepy.readthedocs.io/en/latest/index.html)
    <!-- - [Sleuth](https://pachterlab.github.io/sleuth_walkthroughs/trapnell/analysis.html) "unique challenges will be addressed in the near future" -->

## Public datasets

- [Human Cell Atlas (HCA)](https://data.humancellatlas.org/): Aggregation of single cell sequencing data from human samples, covers >400 tissues from healthy and disease samples.
- [Single Cell Atlas (SCA)](https://www.singlecellatlas.org/): A single-cell multi-omics atlas presenting comprehensive overview sacross 125 healthy adult and fetal tissues.
- [Single Cell Expression Atlas](https://www.ebi.ac.uk/gxa/sc/home): Aggregation of single cell sequencing data across multipe organisms.
- [Tabula Sapiens](https://tabula-sapiens.sf.czbiohub.org/): A first-draft human cell atlas of over 1.1M cells from 28 organs of 24 normal human subjects
- [Broad Institute Single Cell portal](https://singlecell.broadinstitute.org/single_cell): Millions of cell from hundred of studies across multiple organisms and modalities
- [Genotype-Tissue Expression (GTEx)](https://www.gtexportal.org/home/singleCellOverviewPage): Single cell data of 8 major organs from a subset of individuals
- [Tahoe100M](https://github.com/ArcInstitute/arc-virtual-cell-atlas/tree/main/tahoe-100M): 100M cells across 50 cancer cell lines perturbed with 1,100 small-molecule single perturbations
- [scBaseCount](https://github.com/ArcInstitute/arc-virtual-cell-atlas/tree/main/scBaseCount): An AI agent-curated, uniformly processed, and continually expanding single cell data repository of human tissues by the Arc Institute
- [Gene Expression Omnibus (GEO)](https://www.ncbi.nlm.nih.gov/geo/): Repository of sequencing data from publications
- [European Nucleotide Archive (ENA)](https://www.ebi.ac.uk/ena/browser/home): Repository of sequencing data from publications

## Pitfalls & Failure Modes

- Ambient RNA contamination is a prime noise factor in single cell RNA-seq. Ambiant RNA is release by dead cells when they loose membrane integrity and can be barcoded with cells barcode. This problem is most present with encapsulation methods such as 10x or 
- Most protocols rely on polyA oligos to barcode the RNAs, leading to only mRNA and lncRNA being captured. Parse Bioscience Evercode takes an intermediate route with a [mix of polyA and random hexamers](https://www.parsebiosciences.com/blog/getting-started-with-scrna-seq-library-preparation-qc-and-sequencing/) and 10x offers [capture sequence](https://www.nature.com/articles/nprot.2014.006) on their beads. If you are interested mainly in non-polyA transcripts at the single cell resolution, there are protocols but they are usually lower throughput.
- polyA capture followed by fragmentation induces a 3' bias, limiting the resolution to the gene level. SmartSeq2 notably uses tagmentation to insert barcodes, providing reads covering the full length of the transcript. A protocol variation with 10x to perform long read sequencing strongly decreases the 3' bias for short transcripts (<10kb) but requires using long read sequencing technologies. Takara also provides a [long read variation of SmartSeq](https://www.takarabio.com/products/next-generation-sequencing/rna-seq/mrna-seq/long-read-mrna-seq).
- We have given both R and Python options, but note that the field is moving towards python, which you should chose chose unless your team is extremely unfamiliar with it and extremely familiar with R. Large single cell dataset can take a long time to process a python is faster and more memory efficient. Also don't hesitate to subsample your cells for your analysis, you don't need to compute on all the cells all the time, especially if you are interested in particular subsets or if one cell type represents a large proportion of your sample.
- UMI (Unique Molecular Identifier) are added to the transcripts during cell barcoding. They enable for the correction of PCR artifacts and to plot saturation curves (how often you sequence the same read) to estimate how much of the complexity of your sample you capture. See a more detailed discussion of UMI uses and limitations by [Jianfeng Sun](https://substack.com/inbox/post/166754641).

## Related publications

- [Picelli2014](https://www.nature.com/articles/nprot.2014.006): SmartSeq2 foundational paper
- [Rosenberg2018](https://www.science.org/doi/10.1126/science.aam8999): Single-cell profiling of the developing mouse brain and spinal cord with split-pool barcoding (Parse Bioscience foundational paper)
- [Zheng2017](https://www.nature.com/articles/ncomms14049): Massively parallel digital transcriptional profiling of single cells (10x genomics foundational paper)
- [Gaisser2024](https://www.nature.com/articles/s41596-024-01007-w): High-throughput single-cell transcriptomics of bacteria using combinatorial barcoding
- [Pan2024](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-024-03246-2): Single Cell Atlas: a single-cell multi-omics human cell encyclopedia
- [Heimberg2024](https://www.nature.com/articles/s41586-024-08411-y): A cell atlas foundation model for scalable search of similar human cells
- [Peidli2024](https://www.nature.com/articles/s41592-023-02144-y): scPerturb: harmonized single-cell perturbation data
- [Replogle2022](https://pubmed.ncbi.nlm.nih.gov/35688146/): Mapping information-rich genotype-phenotype landscapes with genome-scale Perturb-seq
- [Bergen2020](https://www.nature.com/articles/s41587-020-0591-3): Generalizing RNA velocity to transient cell states through dynamical modeling
- [Clarke2021](https://www.nature.com/articles/s41596-021-00534-0): Tutorial: guidelines for annotating single-cell transcriptomic maps using automated and manual methods
- [Ianevski2022](https://www.nature.com/articles/s41467-022-28803-w): Fully-automated and ultra-fast cell-type identification using specific marker combinations from single-cell transcriptomic data
- [Fu2024](https://academic.oup.com/bib/article/25/5/bbae392/7730135): A comparison of scRNA-seq annotation methods based on experimentally labeled immune cell subtype dataset

## Order list

**Single well barcoding**: 1 - 384 cells ([SmartSeq2](https://www.takarabio.com/products/next-generation-sequencing/rna-seq/legacy-rna-seq-kits/smart-seq-single-cell-for-scrna-seq) or [FLASHseq](https://dispendix.com/blog/flash-seq-a-faster-more-sensitive-single-cell-rna-sequencing-method-using-the-i.dot-nature-biotechnology))

|Item|Cost|Number of experiments|Link|
|---------|--------|--------|
|SMART-Seq® Single Cell Kit|\\$4400|48|https://www.takarabio.com/products/next-generation-sequencing/rna-seq/legacy-rna-seq-kits/smart-seq-single-cell-for-scrna-seq)|
|10M 2x150 reads (200k/cell) with NextSeq2000 XBS P1 or Aviti Low Output flowcell|\\$100|1|https://www.elementbiosciences.com/products/aviti/specs|
|**Total per xp**|\\$190|1|96 cells|
|**Cost per cell**|\\$2|||

**Droplet-based barcoding**: 100 - 100k cells ([10x genomics](https://www.10xgenomics.com/store/experiment-builder?assay=ThreePrime&version=V40&step=form))

|Item|Cost|Number of experiments|Link|
|---------|--------|--------|
|GEM-X Universal 3' Gene Expression v4, 16 samples|\\$24500|16|https://www.10xgenomics.com/store/experiment-builder?assay=ThreePrime&version=V40&step=form
|Chromium GEM-X Single Cell 3' Chip Kit v4, 4 chips|\\$1400|32|https://www.10xgenomics.com/store/experiment-builder?assay=ThreePrime&version=V40&step=form
|Dual Index Kit TT Set A, 96 rxn|\\$1100|96|https://www.10xgenomics.com/store/experiment-builder?assay=ThreePrime&version=V40&step=form
|500M 2x150 reads (25k/cell) on Aviti Medium Output|\\$1100|1|https://www.elementbiosciences.com/products/aviti/specs|
|**Total per xp**|\\$2700|1|20k cells|
|**Cost per cell**|\\$0.14|||

**Split-pool barcoding**: 100k - 10M cells ([Parse Bioscience](https://www.parsebiosciences.com/) or [Scale Bioscience](https://scale.bio/))

|Item|Cost|Number of experiments|Link|
|---------|--------|--------|
|Parse Bioscience Evercode WT v3|\\$10000|1|https://www.parsebiosciences.com/products/evercode-wt/|
|25B 2x150 reads (25k/cell) on NovaseqX 25B or MGI T20|\\$16500|1|https://emea.illumina.com/products/by-type/sequencing-kits/cluster-gen-sequencing-reagents/novaseq-x-series-reagent-kits.html#tabs-80eb4f32eb-item-f8cd845d52-order|
|**Total per xp**|\\$26500|1|1M cells|
|**Cost per cell**|\\$0.03|||

## Protocol variations

- CROPseq/PerturbSeq: Perturb cells with CRISPR technologies and read which guide RNA is present in each single cell, either via a polyA-sgRNA ([Datlinger2017](https://www.nature.com/articles/nmeth.4177)) or a dedicated capture sequence ([Dixit2016](http://pmc.ncbi.nlm.nih.gov/articles/PMC5181115/), [Replogle2020](https://www.nature.com/articles/s41587-020-0470-y))
<!-- compressed CROPseq https://www.nature.com/articles/s41587-023-01964-9 -->
- Single cell RNAseq with long read sequencing. This usually simply requires a longer RT step to produce a full cDNA copy of the transcript, skipping cDNA fragmentation and using a long read technology.
- Demultiplexing via SNPs with [Souporcell](https://www.nature.com/articles/s41592-020-0820-1) enables the multiplexing or an arbitrary number of samples of different genetic origin (patient or cell line).
- [Single *nuclei* RNAseq](https://www.scdiscoveries.com/blog/single-nucleus-rna-sequencing-advantages-and-drawbacks/) (snRNAseq) is a variant of scRNAseq where the cytoplasms are stripped from the cells and only the nuclei are fixed and sequenced. Nuclei have the advantage of being more robust than whole cells so it is the prefer method for degraded samples or hard to dissociate tissues that require harsh conditions. Nuclei are enriched in pre-mRNA but have less RNA than a whole cell so they are great for RNA velocity but less reads per cell can be recovered. See [this discussion](https://www.scdiscoveries.com/blog/single-nucleus-rna-sequencing-advantages-and-drawbacks/) by Single Cell Discoveries and [Ding2020](https://pmc.ncbi.nlm.nih.gov/articles/PMC7289686/) for more details.

## Bonus: Main metrics for common kits

{% include csv_table.html csv_file=site.data.singlecell_costs name="singlecell_costs" %}

{% include costs_disclaimer.html %}

