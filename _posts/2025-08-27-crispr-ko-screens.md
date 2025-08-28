---
title: 'Cost of a CRISPR dropout screen'
date: 2025-08-27
permalink: /posts/2025/09/crispr_ko_screens/
tags:
  - Experiment Costs
  - CRISPR
  - Dropout Screen
toc: true
---

# TL;DR

**Total cost** ~\\$1000 for most use cases, range \\$400-10,000 (\\$0-1700 to procure the cells + \\$200-3100 for the cell culture + \\$100-1200 for sequencing)

**Question answered** What is the impact of every gene/promoter/sequence family (alone or in combination) on my phenotype of interest ?

**Protocol** [Yang2023](https://star-protocols.cell.com/protocols/3177)

# Full story

Today we will dive into the cost of dropout screen experiments.
I will start with a little history and explanation of the protocol, you can also just cut short to the [cost breakdown](#cost-table).
I will also use "knock-out" (short "KO") for every gene that is affected by your library. In the context of CRISPR screens this is often called "guide", to get an overview of other genetic perturbations that are usable in screen see [here](#other_genetic_screens)

<!--
The idea of dropout screen starts with the advent of sequencing and the discovery/engineering of shRNA.
A dropout screen is a specific
-->

## Rational

Dropout screen were designed when researchers realised that it was possible to treat cells in a pooled fation with several perturbations that could then be deconvoluted.
Dropout screens always rely on sequencing, the workhorse of modern high-throughput screening.
The idea is a quite simple one: if you can sequence your perturbation in a quantitative manner (say once per cell), then you can enrich for a phenotype of interest (such as growth rate) by sequencing everything.

Comes in shRNAs, an engineered variant of the naturally occuring siRNA which can easily expressed from plasmids that be transfected or transduced into cells.
Add a selection process, via antibiotics and resistance genes, and a bit of statistical magic, that if you transfect cells with less than one plasmid per cell then most cells with a plasmid will have been transfected only once, and there you have it a single DNA copy of your perturbation in each cell in your culture vessel.
Now you can filter for you phenotype of interest.
A dropout screen is the simplest form of selection screening and simply consist in letting the cells grow. Detrimental KOs will get lost, and advantageous KOs will get enriched.

A quick breakdown is:

1. Introduce a CRISPR guide RNA (sgRNA) in each cell to remove a single gene
2. Let the cells grow for a bit
3. Count the number of cells with each sgRNA

The world of dropout screens is a world of statistics. You will be using thousands of perturbations each with a chance of entry into a cell drawn from a poisson distribution. You **will** have outliers because you are sampling **a lot** of distributions (one for each knock-out). So the recommendation is to maintain an average of 400 cells per knock-out to be on the safe side. You can do less if your cell culture system is limited but it's at your own statistical risks. Sequencing costs use to be a limit as well but it should not be the case as of 2025 (hasn't been since at least 2012 when the first MiSeq came out).

## Experimental explanation

Now that you know the process you can design your library. Don't try to reinvent the wheel, if you want to knock-out genes in model organisms there are many high performing CRISPR libraries that you can order (such as the [Brunello](https://www.addgene.org/pooled-library/broadgpp-human-knockout-brunello/) for humans). If you want to design a custom panel use existing plasmid constructs such as [lentiCRISPv2](https://www.addgene.org/52961/), order oligos with the correct overhang and clone your sequences in there with Gibson assembly. Never forget that you need non-targeting knock-outs in your library, they are necessary to compute the true effect of your effective knock-outs. As a rule of thumb, use about 10% of your library for those controls, with a max of about 1000 (which is the number of controls in the Brunello library) where you are in very safe statistical territory.

## Protocol overview
{: #protocol}

### Cells and sgRNA library: \\$700-\\$5100 (delivery time + 1-2 week to have a healthy cell culture)

Our basic scenario will be: you want to screen the whole human genome for how each coding gene affects your knock-out of interest. This is abusively referred to as genome-wide screening while coding sequences represent ~2% of the human DNA and you will only be targeting parts of those sequencing.
As of 2025 our technology of choice will be CRISPR, and since we are in humans we will use the Brunello library which can be [ordered from addgene](https://www.addgene.org/pooled-library/broadgpp-human-knockout-brunello/) as a lentiviral prep for \\$3400. Unless you know what you are doing or you plan on doing CROPseq/Perturb-seq, you want the lentiCRISPR v2 (Plasmid #52961) backbone. The plasmid expresses Cas9 so you save a step in the protocol and work closer to your cells of origin.
<!-- You will want to order the lentiGuide-Puro variant if you plan on doing CROPseq, we will cover Perturb-seq in another post but know that you will need -->

### Cell culture: \\$200-\\$3100 (5-40h of technician + 24h to 21 days of cell culture)

The next step is to put your virus on your cells and you will aim for a multiplicity of infection (short MOI) between 0.1 and 0.3, which means that you will incubate with a ratio of 1 to 3 plasmids for every 10 cells. This is trade-off between having mostly one plasmid per cell and your cells surviving post-selection (most cells do not like being alone in a sea of medium)/not requiring billions of cells. For the Brunello library (76,441 distinct sgRNAs) this means you need ~90m cells (75k x 400 x 0.3). This represents about five 15cm dishes, ten T75 or three T225 for medium-sized cells ([other formats are possible](https://www.thermofisher.com/fr/en/home/references/gibco-cell-culture-basics/cell-culture-protocols/cell-culture-useful-numbers.html)). Give or take a factor two in each direction to account for cell size variability and density tolerance, and you will need 50-300mL of medium for each passage.<br/>
For adherent cells plate the cells at 70% confluency 24h before adding the virus. For non-adherent cells I recommend reverse transfection where you put the virus first then the cells and spin at 800g for 1h which will get the virus in even those pesky B-cells.<br/>
Note that you will need an S2 for this kind of work. Third generation lentivirus are really safe but you still don't want to gene therapy yourself and remove tumor suppressors in your stem cells. If you don't then you can go with the pooled plasmid library and use less efficient transfection with lipofectamin or cell-stressing electroporation (and cry if you work with the B-cell lineage).

The cost of cell culture varies between cell types so adapt to yours, but for a screen with the rather expensive [human induced pluripotent stem cells](https://www.atcc.org/cell-products/primary-cells/stem-cells/human-induced-pluripotent-stem-cells#t=productTab&numberOfResults=24) count ~250mL per medium change which should be done every day. Over a classical dropout-screen experiment of 21 days that's 5-6L of medium, taking into account the extra volume necessary during passaging. During passaging pay special care to always maintain your representation, you need to reseed at least 30m cells (75k x 400).

### Sequencing: \\$150-\\$1200 (6h of technician + 6h of sequencing)

At the end of your 21 days (or other selection process such as GFP gating), it's time to lyse the cells and extract your precious DNA strands. There a multiple kits that do both, such as [Monarch](https://www.neb.com/en/products/t3010-monarch-spin-gdna-extraction-kit) or [Qiagen](https://www.qiagen.com/us/products/discovery-and-translational-research/dna-rna-purification/dna-purification/genomic-dna/dneasy-blood-and-tissue-kit). Be aware that most standard kits are for a few million cells so you will consume a lot of doses a whole human genome screen or use a [bulk kit](https://www.qiagen.com/us/products/discovery-and-translational-research/dna-rna-purification/dna-purification/genomic-dna/blood-and-cell-culture-dna-kits). The representation rule still applies, you will lyse ~30m cells.

You now have about 120ug of genomic DNA but are only interested with a tiny fragment: the targeting sequence. There are two things left to do to be able to sequence: 1) isolate the targeting sequence to save on sequencing and compute cost and 2) add adapters to the DNA so that the sequencer can work with the fragments. Luckily we can be smart and do both in one step with PCR (polymerase chain reaction). We will order oligos complementary to the flanking sequence from our construct that will also contain the [Illumina adapter sequences](https://support-docs.illumina.com/SHARE/AdapterSequences/Content/SHARE/AdapterSeq/TruSeq/UDIndexes.htm) (or whichever adapter sequences your favorite sequencer uses). If you want to multiplex, order multiple i7 sequences (and i5 if you want to properly dual index). A more flexible approach if you want to do a lot of dropout screens is to only have the Read1 and Read2 sequences on your PCR primers, and perform a second PCR with Index1 and Index2 adapters that you can order from Illumina. At any oligo provider such as [IDT](https://eu.idtdna.com/pages/products/custom-dna-rna/dna-oligos/custom-dna-oligos), [ThermoFischer](https://www.thermofisher.com/fr/en/home/life-science/oligonucleotides-primers-probes-genes/custom-dna-oligos.html), [Twist](https://www.twistbioscience.com/products/oligopools) or [Metabion](https://www.metabion.com/knowledge-hub/products/dna-oligos-single-tube) you can order such oligos for \\$50-100. If you want to be fancy you can order the first primers with UMI, but that will cost you a few thousand and is only worth it if you need a very high precision (which you don't, that's one reason you target each gene with several guides).
<!--TODO provide the example sequences with lentiCRISPRv2 -->
PCR reagents are cheap, any major biology company has kits. Pick a [high volume 2x kit](https://www.thermofisher.com/order/catalog/product/K1082?SID=srch-srp-K1082) an run those PCRs. You will need to run several in parallel because of the [limit on input DNA](https://documents.thermofisher.com/TFS-Assets/LSG/manuals/MAN0012702_DreamTaq_K1071_UG.pdf). You should run about 100ug of DNA for a 400x coverage.

With the sequencing library in tube, you can go to your favorite sequencing team and order 30m reads (1 read per cell) which will cost you between \\$30 (with NovaseqX 25B kit) and \\$1000 (with an underused NextSeq 2000 P2 kit). Amplicon libraries tend to behave differently than more complex libraries on sequencers so I would actually recommend going with the more expensive option. Interestingly enough you could also [sequence the amplicons on a nanopore promethion](https://nanoporetech.com/document/rapid-sequencing-v14-amplicon-sequencing-sqk-rbk114-24-or-sqk) for about \\$1000 (but you still need the amplified fragments, there is too much genomic DNA extracted).

And there you have it, a CRISPR dropout screen of iPSCs with the Brunello library will cost you \\$5100 to procure the cells and library (this is a capital cost if you repeat the screen multiple time and/or use the cells for other purposes), ~\\$3100 for the cell culture, and \\$1200 for the sequencing (up to 10x less if multiplexing). Grand total \\$8400. You are now the proud owner of a fastq file containg 30m sequences that you will now have to map, normalize and quantify.

## Cost table
{: #cost-table}
*(Note: prices change so I will round them to the nearest hundred)*

## iPSC scenario

|Item|Cost|Number of experiments|Link|
|---------|--------|
|Pooled lentiviral library|\\$3400|>10|https://www.addgene.org/pooled-library/broadgpp-human-knockout-brunello/|
|Human induced pluripotent stem cells|\\$1700|1000s|https://www.atcc.org/cell-products/primary-cells/stem-cells/human-induced-pluripotent-stem-cells#t=productTab&numberOfResults=24
|iPSC medium 500mLx10|10x\\$300|1|https://www.atcc.org/products/acs-3002|
|Stem cell dissociation reagent|\\$100|5|https://www.atcc.org/products/acs-3010|
|Monarch® Spin gDNA Extraction Kit|\\$450|5|https://www.neb.com/en/products/t3010-monarch-spin-gdna-extraction-kit|
|PCR primer oligos with sequencer adapters and indices|\\$200|1000s||
|PCR Master Mix 2x|\\$400|10|https://www.thermofisher.com/order/catalog/product/K1082?SID=srch-srp-K1082|
|NextSeq™ 1000/2000 P2 XLEAP-SBS™ Reagent Kit (100 Cycles)|\\$1100|0.5|https://emea.illumina.com/products/by-type/sequencing-kits/cluster-gen-sequencing-reagents/nextseq-1000-2000-reagents.html#tabs-b15481120d-item-473efe9d42-order|
|---------|--------|
|Total|\\$8400|1||
|---------|--------|

I chose on purpose a rather extreme case to show you that selection screens are really not expensive. For most cell lines medium only needs to be changed every 2-3 days so the cost can be divided accordingly, and medium is cheaper (e.g [RPMI](https://www.thermofisher.com/order/catalog/product/fr/en/11875093) which reduces the cost even further. If you look for a fast phenotype like the activity of a pathway you might not even need to change your culture medium. In such cases the cell culture cost could be as low as \\$50 for a perturbation of all human genes.
A custom library on the other hand will cost you more that the Brunello from addgene. 30bp oligos cost about \\$30 from most provider so for a 2000 genes library that would be \\$6000. Addgene can afford the small cost because they generated a large batch that they sell off with a comfortable margin. For smaller panels, use [arrayed screening](https://www.thermofisher.com/fr/en/home/life-science/genome-editing/crispr-libraries.html).
<!-- Note that addgene also provides the plasmid DNA library with the virus pool so you can always make more if needed, but beware of balancing -->

## Cheapest scenario

|Item|Cost|Number of experiments|Link|
|---------|--------|
|Amortized pooled lentiviral library|\\$70|>10|https://www.addgene.org/pooled-library/broadgpp-human-knockout-brunello/|
|Amortized cell line|\\$0|1000s|https://www.atcc.org/cell-products/primary-cells/stem-cells/human-induced-pluripotent-stem-cells#t=productTab&numberOfResults=24
|Cell culture medium 500mL|\\$200|1|https://www.atcc.org/products/acs-3002|
|Monarch® Spin gDNA Extraction Kit|\\$450|5|https://www.neb.com/en/products/t3010-monarch-spin-gdna-extraction-kit|
|Amortized PCR primer oligos with sequencer adapters and indices|\\$20|1000s||
|PCR Master Mix 2x|\\$400|10|https://www.thermofisher.com/order/catalog/product/K1082?SID=srch-srp-K1082|
|NovaSeqX 25B sequencing (30m reads)|\\$30|1|https://emea.illumina.com/products/by-type/sequencing-kits/cluster-gen-sequencing-reagents/nextseq-1000-2000-reagents.html#tabs-b15481120d-item-473efe9d42-order|
|---------|--------|
|Total|\\$400|1||
|---------|--------|

Overall count between \\$400 and \\$10,000 for a dropout screen, with most setup leaning towards the \\$1000 mark.

## Other genetic screen
{: #other_genetic_screens}

In this post we focused on CRISPR knock-out screens where Cas9 is used to induce double-strand break in the target gene that will eventually be repared incorrectly, which inactivates the gene.
However many more constructs exist that can be used in those screens:
- "dead" Cas9, which cannot cut DNA, can be used to direct any kind of protein fused with it to specific genomic locations:
    - CRISPRa fuses a transcriptional activator such as VP64 or [VPR](https://pmc.ncbi.nlm.nih.gov/articles/PMC4393883/) (VP64-p65-Rta) to activate the target gene. CRISPRa can be finicky because the promoter must be targeted without blocking the binding of the RNA polymerase elongation complex. For more details see [this addgene post](https://blog.addgene.org/crispr-activators-dcas9-vp64-sam-suntag-vpr).
    - [CRISPRi](https://www.nature.com/articles/nprot.2013.132) fuses a transcriptional repressor such as the [KRAB domain](https://pmc.ncbi.nlm.nih.gov/articles/PMC328446/) to inactivate the target gene without introducing DNA breaks. It is a robust system.
- [arrayed screens](https://www.nature.com/articles/s41551-024-01278-4) use the processing capability of specific Cas proteins such as Cas12 and Cas13 to target multiple genomic locations (or RNA locations for Cas13) with each construct. This can be used either to inactivate several genes in a combinatorial screen, or to ensure a high inactivation efficiency by targeting the same gene at multiple locations. While very powerful, cloning such constructs is more tricky. Luckily, you can also find published libraries (e.g [AnYin2024](https://pmc.ncbi.nlm.nih.gov/articles/PMC11754104/)).
- "dual gRNA" libraries are similar to arrayed screen in the sense that each construct expresses multiple gRNAs, but each sgRNA pair targets closeby regions of the same target gene which induces a large deletion. They address one major challenge of Cas9 knock-outs that about a third of the indels induced by DNA-repair error will be in frame and can yield a truncated but functional protein. Those can be ordered (e.g at [vectorbuilder](https://en.vectorbuilder.com/products-services/product/dual-grna-crispr-libraries.html)).
- "small hairpin" RNA (shRNA) use siRNA-mimicking constructs instead of gRNA, which presents the advantage of not having to express Cas9 in the target cell. They are however less efficient that CRISPR constructs.
- [TALENs](https://pmc.ncbi.nlm.nih.gov/articles/PMC5536959/) (Transcription Activator-Like Effector Nucleases) were all the trend before the discovery of CRISPR. They consist of rather complex engineered proteins with tandem-repeat DNA-binding motifs with base specificity. TALENs construct are bulky, making them hard to transfect, and less efficient than CRISPR. You will likely never use it but at least you know it exists.

[^1]: stem cells for example tend to silence Cas9, this can be aleviated by using an inducible construct for transient Cas expression.

## Etc

When analysing CRISPR knock-out data, you will have to account for the fact that you introduce double strand breaks in the cell's DNA. This will have differential effects based on things like copy number or relative position to the centromeres. See [Vinceti2024](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-024-03336-1) for an overview.
