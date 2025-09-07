---
title: 'Cost of mammalian cell culture'
date: 2025-09-08
permalink: /posts/2025/09/single_ko_generation/
tags:
  - Experiment Costs
  - CRISPR
  - Knock-out
  - Bio assets
toc: true
excerpt: 'Cell culture is performed to get enough cells for future experiments'
---

## Why do you do this experiment?

Cell culture is done in most experiments to provide biological material to perturb and measure.

## Strategic Value

- Provides cells to perturb and measure.
- Provide cells for patients (stem cell transplant, CAR-T cells, [gene therapy cell product](https://www.nature.com/articles/s41392-025-02135-9))

## Cost & Scale

- Variable per run: **\\$30/flask/week** (output 5-12M cells). Range: \\$21 (cheap cell lines and medium) - \\$150 (expensive cell line and medium)
- Cost breakdown:
    + Cells aquisition: \\$0-\\$1700
    + Culture medium: \\$5-100/week
    + Plasticware: \\$6-20/week
- Capex: BSL1 or BSL2 cell culture, [multi-purpose centrifuge](https://www.eppendorf.com/en/your-centrifuge-solution/multipurpose-centrifuges/), [CO2 incubator](https://www.thermofisher.com/fr/en/home/life-science/lab-equipment/co2-incubators/models.html), water bath.

## Experimental Modules

1. Procure cells (couple weeks, 5' hands-on to order)
2. Thaw cells (1h full hands-on)
3. Grow the cells (1+ week(s), 2-6h hands-on/week)
4. (optional) Freeze the cells (2h, 1h hands-on)

## Ops & Throughput

**Turnaround**: 1 day (splitting already culture cells) - 2 weeks (need high volume of slow growing cells from liquid nitrogen storage)

**Hands-on time**: 2-6h/week per flask/dish, 6-15h/week per multi-well plate.

**Parallelizability** Medium, multiple knock-outs in multiple cell lines can be done in parallel. All steps bottleneck at about the same rate with the number of samples to handle.

**Batching** Generally 1-4 cell lines in parallel of other experiments. Up to 12 high maintenance cell lines can be maintained in parallel by a full time technician but beware of contamination risks.

**Automation readiness** Low, cell culture automata cost \\$500k-1.5M and require a full time engineer to handle. Technicians are generally cheaper and more flexible.

**Outsourceability** Yes, e.g [AcroBiosystem](https://www.acrobiosystems.com/A2746-Gene-knockout-Cell-Lines.html), [Cyagen](https://www.cyagen.com/custom-cell-line-models/knockout-cell-lines), [iXCells](https://ixcellsbiotech.com/preclinical-cro-services/genome-editing/), [Runtogen](https://www.runtogen.com/category/gene-editing-cell-lines/knockout-cell-lines/), [Abcam](https://www.abcam.com/en-us/technical-resources/product-overview/knockout-cell-lines?srsltid=AfmBOorPQ4cKD8fp18pjFR53cCc8cNlZgZy_gxwGW7-093WOpdiNtrcG).

<!--
- Data scale: reads/images/features generated]
## Data API
Raw format: [FASTQ, TIFF, etc.]
Processed format: [count matrix, gene-level scores, feature vectors]
Resolution: [cell-level, gene-level, transcript-level]

## Analysis Ecosystem
Tools / packages
Common workflows

-->

## Pitfalls & Failure Modes

- <u>Cell line contamination</u> can take many forms:
    + [bacteria and fungi](https://www.thermofisher.com/fr/fr/home/references/gibco-cell-culture-basics/biological-contamination.html) are easy to detect and deal with, they will also lead to the cells dying rapidly.
    + [Mycoplasma infection](https://www.science.org/content/blog-post/those-darn-invisible-creatures) is more subtle and should be [checked regularly](https://www.thermofisher.com/order/catalog/product/fr/en/M7006).
    + [Cross-contamination by other cell lines](https://iclac.org/databases/cross-contaminations/) is the most pernicious contamination, you want to [identify](https://www.culturecollections.org.uk/services/authenticell/search-by-name/) your cell lines when you receive them. Also check at least once a year if you grow cells in parallel as cross-contamination can also happen in your own cell culture.
- Cell line drift is another issue you can encounter. As cells get passaged they will accumulated mutations both randomly via genetic drift and deterministically via selection. Always check that your cell lines still have the key genetic alteractions you are working on via deep [RNAseq](/posts/2025/09/short-read-sequencing/) or WGS (e.g check for a specific mutation if you study mutant vs non-mutant cell line sensitivity to a drug).
- There are many different cell culture medium available:
    + For ease of use you will sometimes want to grow all your cell lines in the same medium. In order to do so start by amplifying the cell line in its recommended medium before switching one culture flask to the new medium. If the cell lines keeps growing satisfyingly in the new medium you can amplify and freeze the cell in this new medium (but always have frozen aliquots grown and frozen in the recommended medium). Switching to a richer medium (from DMEM to RPMI for example) will always be easier than the other way around so is generally prefered.
    + Medium switching can be done to make a cell line grow faster. [HepG2](https://www.atcc.org/products/hb-8065) for example are notoriously slow to grow but their recommended culture medium is the very minimal [EMEM](https://www.atcc.org/products/30-2003). Changeing the medium can also [alter chemical sensitivity](https://pmc.ncbi.nlm.nih.gov/articles/PMC6563005/).
- Fetal Calf Serum (FCS) is not a fully characterized component. [Test](https://www.culturecollections.org.uk/culture-collection-news/fbs-screening/) for any new batch of FCS that your cells grow as well and that your major phenotypes are not altered (e.g your [lentiviral vector production](https://pmc.ncbi.nlm.nih.gov/articles/PMC3798230/)). If they differ too much, order a new batch and try again.
    + The easiest way to ensure your cells are not contaminated by bacteria or fungi is to grow [without PenStrep](https://ibidi.com/content/436-prevention-of-contaminations) but this also increases contamination risks. Culturing without PenStrep might also be a good idea to [avoid unwanted gene expression changes](https://pmc.ncbi.nlm.nih.gov/articles/PMC5548911/)

## Related publications

- [Horbach2017](https://pmc.ncbi.nlm.nih.gov/articles/PMC5638414/) on massive cell contamination by HeLa cells.
- [ATCC cell culture guide](https://www.atcc.org/the-science/culturing-cells).
- [Greenfield2018](https://cshprotocols.cshlp.org/content/2018/3/pdb.prot103150.abstract), Screening for Good Batches of Fetal Bovine Serum for Myeloma and Hybridoma Growth.
- [Selenius2019](https://pmc.ncbi.nlm.nih.gov/articles/PMC6563005/), The Cell Culture Medium Affects Growth, Phenotype Expression and the Response to Selenium Cytotoxicity in A549 and HepG2 Cells.
- [Ryu2017](https://pmc.ncbi.nlm.nih.gov/articles/PMC5548911/), Use antibiotics in cell culture with caution: genome-wide identification of antibiotic-induced changes in gene expression and regulation.
- [Morgan2025](https://www.nature.com/articles/s41392-025-02135-9) presents a complex cell culture pipeline to modify hematopoietic stem cells from patients and reinject them in the patient to replace their disfonctionnal remaining hematopoietic stem cells.

## Public resources

- Cell lines repositories:
    + The [American Type Culture Collection (ATCC)](https://www.atcc.org/) is the major cell lines collection in the world with over 3,000 human and animal cell lines and over 1,000 hybridomas (to produce specific antibodies). All ATCC cell lines are authentified, and you can find specific information for cell culture such as the medium and the passaging rate.
    + The [European Collection of Authenticated Cell Cultures (ECACC)](https://www.culturecollections.org.uk/ECACC) is the major European cell lines collection. Like ATCC the cell lines are authentified.
    + The [German Collection of Microorganisms and Cell Cultures (DSMZ)](https://www.dsmz.de/collection/catalogue/human-and-animal-cell-lines/catalogue) is another collection with about 1,000 human and animal cell lines. It also provides an [extensive collection](https://www.dsmz.de/collection/catalogue) of fungi, virus and bacteria.
- [Cellosaurus](expasy.org/resources/cellosaurus) is a knowledge resources on most publicly available cell lines built and maintained by the Swiss Institute of Bioinformatics.

## Order list

Assuming cell culture in T75 flasks, a medium change requires ~2x10mL complete medium and a 90% confluent culture is 5-12M cells for adherent cells.
Number are similar for culture in multiwell plates.
Here are the cell culture cost for commonly used cell line models:

**HepG2** Medium change every 2-3 days (3x a week), split once a week.
|---------|--------|--------|
|Item|Cost|Number of medium changes|Link|
|---------|--------|--------|
|HepG2 cells|\\$550|>100|https://www.atcc.org/products/hb-8065|
|Eagle's Minimum Essential Medium (EMEM) 500mL|\\$30|25|https://www.atcc.org/products/30-2003|
|Fetal Bovine Serum (FCS) 500mL|\\$700|250|https://www.atcc.org/products/30-2020|
|PenStrep 1x|\\$40|500|https://www.thermofisher.com/order/catalog/product/A5669701|
|10mL serological pipettes|\\$100|50|https://shop.integra-biosciences.com/fr/s/product/detail/01tVj000005rTahIAE?language=fr|
|Cell Culture Treated Flasks with Filter Caps|\\$100|50|https://www.thermofisher.com/order/catalog/product/178905|
|Trypsin-EDTA (0.25%), phenol red|\\$20|50|https://www.thermofisher.com/order/catalog/product/fr/en/25200056|
|---------|--------|--------|
|Total per culture week|\\$21|3||
|---------|--------|--------|

**Primary cell line** Medium change every 2-3 days (3x a week), split once a week.
Primary cell lines are tricky because somatic cells will age and eventually [stop proliferating](https://en.wikipedia.org/wiki/Hayflick_limit).
|---------|--------|--------|
|Item|Cost|Number of medium changes|Link|
|---------|--------|--------|
|Human cardiac myocytes|\\$1800|>50|https://www.sigmaaldrich.com/FR/fr/product/sigma/c12810|
|Human dermal fibroblasts|\\$1100|>50|https://www.sigmaaldrich.com/FR/fr/product/sigma/c12300|
|Fibroblast Growth Medium 500mL|\\$250|25|https://www.sigmaaldrich.com/FR/fr/product/sigma/c23010|
|PenStrep 1x|\\$40|500|https://www.thermofisher.com/order/catalog/product/A5669701|
|10mL serological pipettes|\\$100|50|https://shop.integra-biosciences.com/fr/s/product/detail/01tVj000005rTahIAE?language=fr|
|Cell Culture Supra Treated Flasks with Filter Caps|\\$600|100|https://www.thermofisher.com/order/catalog/product/156372|
|Trypsin-EDTA (0.25%), phenol red|\\$20|50|https://www.thermofisher.com/order/catalog/product/fr/en/25200056|
|---------|--------|--------|
|Total per culture week|\\$76|3||
|---------|--------|--------|

**iPSCs** Medium change every day (7x a week), split once a week.
|---------|--------|--------|
|Item|Cost|Number of medium changes|Link|
|---------|--------|--------|
|Human Induced Pluripotent Stem (iPS) Cells|\\$1800|>100|https://www.atcc.org/products/acs-1013|
|Pluripotent Stem Cell SFM XF/FF|\\$300|25|https://www.atcc.org/products/acs-3002|
|Fetal Bovine Serum (FCS) 500mL|\\$700|250|https://www.atcc.org/products/30-2020|
|Stem Cell Dissociation Reagent|\\$100|50|https://www.atcc.org/products/acs-3010|
|ROCK inhibitor|\\$250|1000|https://www.atcc.org/products/acs-3030|
|PenStrep 1x|\\$40|500|https://www.thermofisher.com/order/catalog/product/A5669701|
|10mL serological pipettes|\\$100|50|https://shop.integra-biosciences.com/fr/s/product/detail/01tVj000005rTahIAE?language=fr|
|Cell Culture Treated Flasks with Filter Caps|\\$100|50|https://www.thermofisher.com/order/catalog/product/178905|
|---------|--------|--------|
|Total per culture week|\\133$|7||
|---------|--------|--------|

## Going further

A typical medium composition is:
- 450mL base medium (EMEM, DMEM, RPMI, etc)
- 50mL FBS (10%)
- 5mL 100x PensTrep (final concentration 10U/mL Penicilin + 10ug/mL streptomycin)

A good work practice is to aliquot FCS and full medium (with PenStrep and FCS) in 50mL aliquots right after opening/preparation.
This will limit the risk of contamination with bacteria and fungi by limiting the number of opening of each tube. Moreover if someone accidently contaminates a 50mL aliquot they are way more likely to discard it a use a new one than with higher volumes.

