---
permalink: /
title: "Sequencing Costs"
excerpt: "Sequencing cost breakdown"
author_profile: true
redirect_from: 
  - /sequencing_costs/
  - /sequencing_costs.html
---

This page references manufacturer prices. If you have access to a sequencing core facility (university or company) that has the appropriate device you can get those prices or cheaper (depending on what was negotiated with the supplier). If you have to go through a third party (Eurofins, CeGaT, etc) plan to pay about double the amount but they should have the latest sequencers available.<br/>
Kits from each manufacturers exist in different sizes, bigger is always cheaper per read/Gb but if you cannot fill it the flowcells are more expensive.
Prices are given normalised per read and per Gb for different usages. For RNAseq, the cost per read is what is of interest but be aware of your read size, 2x50bp loses more reads to multiple alignement and reduces exon junction coverage to call alternative transcript. As of 2025 I recommend at least 300bp when available. Note that read refers in the table to read pairs, double that number if you are interested in the total number of reads for bulk RNA sequencing (but not single cell as one read in each pair contains the cell barcode). For genome sequencing (whole genome or whole exome) you want the lowest cost per Gb (for reference a human genome is ~3Gb, so 30x coverage is 120Gb).<br/>

For single cell perturbation screening aficionados like me, sequencing 1M single cells at 50k reads/cell (50B reads total) is best done with 2 Novaseq X flowcell 300 cycles for a bit under 33k€.
For the full price of the experiment, add the biological sample preparation cost (cell culture or patient conservation) and the library preparation cost (DNA/RNA extraction and barcoding).

<!-- https://jekyllrb.com/tutorials/csv-to-table/ -->
<table>
  {% for row in site.data.sequencing_costs %}
    {% if forloop.first %}
    <thead>
    <tr>
      {% for pair in row %}
        <th>{{ pair[0] }}</th>
      {% endfor %}
    </tr>
    </thead>
    <tbody>
    {% endif %}

    {% tablerow pair in row %}
      {{ pair[1] }}
    {% endtablerow %}
  {% endfor %}
   </tbody>
</table>

