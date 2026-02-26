---
title: 'What would it take to sequence 1 trillion cells ?'
date: 2026-02-26
permalink: /posts/2025/2/trillion_cells_sequencing/
tags:
  - Experiment Costs
  - Single-cell
  - Data assets
toc: true
excerpt: ''
---

Here is a funny thought exercise: what would it take to sequence 1 trillion cells ?

Let's start by visualising what 1 trillion cells represent.
For an order of magnitude [an adult human body is 28 to 36 trillion cells](https://www.pnas.org/doi/10.1073/pnas.2303077120) so this would represent sequencing ~3% of a human adult body, or about 2kg of cells.
Of course you would not sequence a significant percentage of a single individual so such a dataset would likely be from multiple individuals.
A good rule of thumb for how many cells you want per sample is between 2,000 to 10,000 as this gives you a very good coverage of the diversity of cell types and cell states in your sample (even for messy samples like cancer).
More cells for a single sample would lead to overfitting any property you try to learn to this specific sample, which is great if you want to do hyper-personalized medicine (which some companies like [One Biosciences](https://onebiosciences.fr/) are actually trying to do), but not otherwise.
If you want to learn general principle of biology to develop blockbuster drugs 2,000 to 10,000 per sample is more than enough.
Let's say you went over the top and plan to sequence 20 samples per patient at 5,000 cells per sample: that's 100,000 cells per patient so you would need to gather a cohort of 10 million people to get 1 trillion cells.
That's both a lot, and probably the amount of people the [UK genomic projects](https://community.ukbiobank.ac.uk/hc/en-gb/articles/25118589261213-500k-Whole-Genome-Sequencing-General-FAQs) will sequence by the end of the decade if they continue their initiatives.

The first challenge to overcome when wanting to do single cell sequencing is to mark the RNA or DNA that you wish to sequence for the cell of provenance.
For this purpose combinatorial barcoding is really a breakthrough technique.
It is a simple as it is elegant, relying on barcode diversity bruteforce to statistically swamp your required number of cells.
By building the barcodes progressively, it enables a very simple and fast experimental workflow that builds tremendous barcodes diversity incredibly fast.

Standard combinatorial barcoding pipelines provide the barcode pieces in 96 well plates, a convenient standard for manual molecular biology.
Each well contains a single oligo, different for each well, at high concentration (usually 2.5 to 12.5 uM final concentration).
The role of those oligos is to anneal to RNA molecules or cDNAs in a fixed cell (or to be inserted in DNA for ATAC seq protocols) to serve as primers to create a cDNA with the oligo sequence at the beginningl.
The cells from all wells, which hold those cDNA molecules, are then pooled together and split in another plate with the next barcoding oligo.
In theory the same oligos could be used, in practice this would create issues as the cDNA conversion is not 100% effective and would lead to partial barcodes so we use unique linker pairs for each iteration to ensure an oligo from plate n is bound to an oligo of plate n-1.
The combinatorial magic comes from the split-pooling step. As the cells are mixed together, there exist 96 unique barcodes in the population and statistically all of them end up represented in each well of the next plate.
As the cell comes out of the second plate, sequencing the barcode would tell you in which well of each plate the cell was, providing 96x96=9216 potential combinations.
If you were to sequence the cDNA from 9216 cells though you would not end up with exactly 9126 barcodes.
This is a sampling process and some barcodes would not be present while others would be overrepresented.
This is called a collision, you would know two cDNA molecules were in the same well but you would not know if they were from different cells from just the sequence (their are methods for doublet detection which are out of scope for this piece).
However you can calculate the probability that those collisions occur and if you were to sequence say 90 cells which is way less than the number of possible barcodes this probability would be very low.

But sequencing 90 cells is boring, we should increase the number of barcodes instead.
A good rule of thumb is that you want about 4 times as many barcodes as the number of cells you aim to sequence (I'll develop the maths one day but tonight I'm lazy).
Getting more barcodes is as easy as doing another split-pool barcoding round ; after three rounds you have ~900k barcodes available.
*(If you were wondering the Parse Bioscience WT kit does 16 samples x 96 x 96 x 8 sublibraries ~ 3.5m barcodes, while the Mega kit does 384 samples x 96 x 96 x 8 sublibraries ~ 28.3m barcodes.
They use the illumina multiplexing barcodes to get an extra factor 8.)*

You can of course continue for more rounds, the only constraint to keep in mind is that you need to sequence the barcode so each bp of barcode is one bp of the sequence of interest you are not sequencing.
This is becoming less of an issue as most modern short read sequencer in 2026 offer at least 300bp reads options, with many moving to 500bp and beyond.
And of course not an issue if you can sequence [4M bp](https://nanoporetech.com/blog/short-long-or-ultra-long-which-read-length-is-right-for-you), then your problem might be to reliably find the tip to barcode the molecule.
To put a number on the "loss", each barcoding round adds about 12bp to sequence through: 6bp for the barcode (8bp if in 384 wells plate) and 6bp for the linker.
Also keep in mind that the linker is the same for all molecules so either make sure your sequencer can handle homogenous region (for example by doing [dark cycles](https://support-docs.illumina.com/IN/NovaSeqX/Content/IN/NovaSeqX/DarkCycleSequencing.htm)) or introduce a [stagger](https://link.springer.com/article/10.1186/2049-2618-2-6) in your last barcode to increase the complexity when sequencing the linker.

Assuming only 96 well plates for barcodes, here's a table summarising how many barcodes you can reach with an estimated of the number of cells you can sequence without fearing excessive collisions:

|Barcoding rounds|#Barcodes|#Cells|
|:---:|---:|---:|
|1|96|96|
|2|9,216|2,000|
|3|884,736|200,000|
|4|85e6|2e7|
|5|8e9|2e9|
|6|7,8e11|2e11|
|7|7,5e13|1e12|

There we have it, 7 rounds for 1 trillion cells.
I hope you have a solid budget because sequencing even one read per cell is going to cost you over 100 billion dollars in 2026.
I would wait for [sequencing costs](/sequencing_costs) to drop a bit more than that.
But how much did the barcoding cost ?
There are multiple constraints there: you need to buy enough plates to get the barcodes, you need enough oligos in each well to barcode all the cells it contains, but you also need to fit the cells in the well (about 12 billion cells per well for 1 trillion total cells, taking into account the minor cells loss between barcoding rounds) with enough liquid between them for fluid and the oligos it contains to circulate.

12 billion average human cells take about 24mL, 12 billion hepatocytes would be 40mL, and your 1 trillion cells would thus take 2L.
And that's without the liquid needed in between to be able to manipulate them (without liquid think about manipulating your cell precipitate after a centrifugation).
That won't fit in the 300uL wells of a 96-wells plate ; you would actually need 80 plates just to fit the cells, probably 160 to fit them with enough liquid to manipulate them and accomodate the reaction volumes for the barcoding reagents.
It is a lot of plates but it is actually not *that* many, a robot can manage it in a couple of days ; cDNA is very stable so once the reverse transcription is done you don't have to worry about reaction time.
And if you work on [bacteria](https://academic.oup.com/ismecommun/article/5/1/ycaf134/8220722) then 12 billion cells would comfortably fit in 250uL with medium so 1 plate for each step is enough (and you could use deep well to be safe).
(incidentally enough oligos for 1 trillion cells would be 0.6 mol or 3g which cost about *$*300k per barcode or ~*$*200 million for the 7x96 barcodes, for bacteria it would be 15mg of each barcode which you can likely get for around *$*5000 per barcode/*$*3m per full round)

But I would not do that if I were you.
Because while I mentionned the *cost* of sequencing, we haven't yet calculated the number of runs.
1 trillion reads at a measly 1 read per cell would require 40 NovaseqX 25B flow cells or 84 UG200 12B flowcells which each take about 24h to run.
And you probably want several thousand reads per cell, at *$*10-20k per flowcell I let you do the expensive math.


This leads to our final table:

|Barcoding rounds|#Barcodes|#Cells|#10BFlowcells@5kreads|#Patients@100kcells|
|:---:|---:|---:|---:|---:|
|1|96|96|1|0|
|2|9,216|2,000|1|0|
|3|884,736|200,000|1|2|
|4|85e6|2e7|10|200|
|5|8e9|2e9|1,000|20,000|
|6|7,8e11|2e11|100,000|2,000,000|
|7|7,5e13|1e12|500,000|10,000,000|

So maybe is seems the sweet spot will end up around 4 barcoding rounds for 20 million cells (input 28 million to be safe): 10 flow cells is very manageable (cost around *$*150k), 200-1000 patients is a large but recruitable cohort, and your computer should survive processing 100 billion reads.
At 10 million cells/mL, those 28 million cells could be handled in 2.8mL and distributed 300uL per well of a deep 96-wells plate per round.

Because this is still over 300k cells per well each containing about 400k mRNA molecules, you want about 10x4e10 oligos in your barcoding well that is 6pmol or 30pg (20nM concentration).
This can be ordered for about *$*150 per oligo (*$*14,400/plate) for a total cost for the barcoding of *$*60,000 if you were to only do it once (as you get more than 6pmol of oligos for *$*150).


<!---
## Bonus: Main metrics for common kits
{% include csv_table.html csv_file=site.data.singlecell_costs name="singlecell_costs" %}
--->


{% include costs_disclaimer.html %}

