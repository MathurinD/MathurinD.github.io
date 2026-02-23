---
permalink: sequencing_xp_costs/
title: "Sequencing Costs"
excerpt: "Sequencing cost breakdown"
author_profile: true
redirect_from: 
  - /sequencing_xp_costs.html
---

With the ongoing [Virtual Cell Challenge](https://virtualcellchallenge.org/) and it's attractive \\$100k first prize, it would be good for computational biologist who spend thousands of dollars in compute to get a good idea of the cost of generating the data they use to train their models.
Here are different scenarios for experiments relying on sequencing. We will separate the library generation cost from the sequencing costs, as those evolve fast and highly depend on the sequencers available. We have a separate table for sequencing costs so refer to them for you exact possibilities, for this table we will consider \\$3/Gb which is achievable by most state-of-the-art sequencers as of August 2025.
Time represents the time to prepare the samples for sequencing, usually add 24h-48h for the sequencing run. If you have to generate the samples, count one to three weeks for cell culture.

<!-- Cost, time (hands-on and total) -->
{% include csv_table.html csv_file=site.data.sequencing_scenarios name="sequencing_scenarios" %}

This page references manufacturer prices (in 2025 euros, see [genohub](https://genohub.com/high-throughput-sequencers/) for dollar costs). If you have access to a sequencing core facility (university or company) that has the appropriate device you can get those prices or cheaper depending on what was negotiated with the supplier (or note [NCSU](https://research.ncsu.edu/gsl/pricing/#nextgen), [Feinberg School of Medicine](https://www.cgm.northwestern.edu/cores/nuseq/pricing.html#price-link1) or [NCI](https://crtp.ccr.cancer.gov/sf/pricing/)). If you have to go through a third party (Eurofins, CeGaT, etc) plan to pay about double the amount but they should have the latest sequencers available.<br/>
Kits from each manufacturers exist in different sizes, bigger is always cheaper per read/Gb but if you cannot fill it the flowcells are more expensive.
Prices are given normalised per read and per Gb for different usages:
    - For RNAseq, the cost per read is what is of interest but be aware of your read size, 2x50bp loses more reads to multiple alignement and reduces exon junction coverage to call alternative transcript. As of 2025 I recommend at least 150bp and going for 300bp when available. Note that read refers in the table to read pairs, double that number if you are interested in the total number of reads for bulk RNA sequencing (but not single cell as one read in each pair contains the cell barcode). To compare long vs short read prices my rule of thumb is that one 10k long read is worth 100 2x100bp short reads ; with more coverage for the short read to call variants but much more structural information with the long read.
    - For genome sequencing (whole genome or whole exome) you want the lowest cost per Gb (for reference a human genome is ~3Gb, so 30x coverage is 120Gb). Long-reads have extra value for repetitive regions and rearrangements but the advantage is not so overwhelming that I would apply a multiplier.

For single cell perturbation screening aficionados like me, sequencing 1M single cells at 50k reads/cell (50B reads total) is best done with 2 Novaseq X flowcell 300 cycles for a bit under 33k€.
For the full price of the experiment, add the biological sample preparation cost (cell culture or patient conservation) and the library preparation cost (DNA/RNA extraction and barcoding).

{% include csv_table.html csv_file=site.data.singlecell_costs name="singlecell_costs" %}

{% include csv_table.html csv_file=site.data.sequencing_costs name="sequencing_costs" %}
