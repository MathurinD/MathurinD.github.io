---
permalink: sequencing_costs/
title: "Sequencing Costs"
excerpt: "Cost of sequencing with all available technologies"
author_profile: true
redirect_from: 
  - /sequencing_costs.html
---

This page references manufacturer prices in 2025 euros (see [genohub](https://genohub.com/high-throughput-sequencers/) for dollar costs) for products available in Europe. Other actors not available in Europe and the USA such as [GeneMind](https://en.genemind.com/products) are not included.
I can also recommend [this piece by Latch Bio](https://blog.latch.bio/p/a-primer-on-ngs-technologies-and) for a more detailed overview of the various sequencing technologies.

If you have access to a sequencing core facility (university or company) that has the appropriate device you can get those prices or cheaper depending on what was negotiated with the supplier (or not [NCSU](https://research.ncsu.edu/gsl/pricing/#nextgen), [Feinberg School of Medicine](https://www.cgm.northwestern.edu/cores/nuseq/pricing.html#price-link1) or [NCI](https://crtp.ccr.cancer.gov/sf/pricing/)). If you have to go through a third party (Eurofins, CeGaT, etc) plan to pay about double the amount but they should have the latest sequencers available.

Kits from each manufacturers exist in different sizes, bigger is always cheaper per read/Gb but if you cannot fill it the flowcells are more expensive.

Prices are given normalised per read and per Gb for different usages:

- For RNAseq, the cost per read is what is of interest but be aware of your read size, 2x50bp loses more reads to multiple alignement and reduces exon junction coverage to call alternative transcript. As of 2025 I recommend at least 150bp and going for 300bp when available. Note that read refers in the table to read pairs, double that number if you are interested in the total number of reads for bulk RNA sequencing (but not single cell as one read in each pair contains the cell barcode). To compare long vs short read prices my rule of thumb is that one 10k long read is worth 100 2x100bp short reads ; with more coverage for the short read to call variants but much more structural information with the long read.
- For genome sequencing (whole genome or whole exome) you want the lowest cost per Gb (for reference a human genome is ~3Gb, so 30x coverage is 120Gb). Long-reads have extra value for repetitive regions and rearrangements but the advantage is not so overwhelming that I would apply a multiplier.

For the full price of the experiment, add the biological sample preparation cost (cell culture or patient conservation) and the library preparation cost (DNA/RNA extraction and barcoding).

{% include csv_table.html csv_file=site.data.sequencing_costs name="sequencing_costs" %}
