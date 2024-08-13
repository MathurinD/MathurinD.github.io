---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Work experience
======
* 2021 - present    Bioinformatician in the group <a href="https://www.molgen.mpg.de/166100/Gene-Regulation-and-Systems-Biology-of-Cancer">Gene Regulation and System Biology of Cancer</a>, Max Plank Institute for Molecular Genetics, Berlin.
    * Design and analysis of a CROPseq dataset in paediatric cancer cell lines (B-cell precursor Acute Lymphocytic Leukemia, Ewing Sarcoma, Hepatoblastoma)
    * Design and analysis of a dropout screen with drug treatments in paediatric cancer cell lines
    * Maintenance and development of sequencing pipelines
    * Mechanistic modelling of paediatric cancers
* 2015 - 2021       PhD Reverse engineering signalling networks in neuroblastoma in the group of <a href='http://sys-bio.net'>Computationnal Modelling in Medicine</a>, Humboldt-Universität zu Berlin and Charité Universitätsmedizin, Berlin.
    * Development of the R/C++ package <a href="https://github.com/molsysbio/STASNet">STASNet</a>
    * Perturbation of a panel of Neuroblastoma cell lines with targeted therapies
    * Modelling of Neuroblastoma and Colo-Rectal Cancer cell lines signalling

Education
======
* 2012 - 2015 M.S in Molecular and Cellular Biology with a specialisation in Bioinformatics and Modelling, École Normale Supérieure and UPMC, Paris.
* 2010 - 2012 Classes préparatoires aux Grandes Ecoles BCPST (Biologie, Chimie, Physique, Sciences de la Terre), Lycée Hoches, Versailles.
  
Skills
======
* Tissue culture
* Sequencing on Illumina sequencers (library preparation and sequencer operation)
* Single cell sequencing (library preparation and raw data processing)
    * <a href="https://www.10xgenomics.com/products/single-cell-gene-expression">10X</a>
    * <a href="https://www.parsebiosciences.com/products/evercode-whole-transcriptome">Evercode</a>
* Programming
    * R/tidyverse
    * python
    * C/C++

Publications
======
  <ul>{% for post in site.publications %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
<!--
Talks
======
  <ul>{% for post in site.talks %}
    {% include archive-single-talk-cv.html %}
  {% endfor %}</ul>
  
Teaching
======
  <ul>{% for post in site.teaching %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
Service and leadership
======
* Currently signed in to 43 different slack teams
-->
