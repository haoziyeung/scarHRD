scarHRD R package manual
================
true

<style>
body {
text-align: justify;
font-family: Arial}

.moderateFrame{ /* Framed text */
border: 1px solid;
border-color: #8c5400;
color: black;
background-color: #ffe2ad;
padding-top: 10px;
padding-bottom: 10px;
border-radius: 5px;
padding-left: 10px;
padding-right: 10px;
font-size: 14px;
}

</style>
<br> <br>

Introduction
============

`scarHRD` is an R package which determines the levels of homologous recombination deficiency (telomeric allelic imbalance, loss off heterozygosity, number of large-scale transitions) based on NGS (WES, WGS) data.

The first genomic scar based homologous recombination deficiency measures were produced using SNP arrays. Since this technology has been largely replaced by next generation sequencing it has become important to develop algorithms that derive the same type of genomic scar-scores from next generation sequencing (WXS, WGS) data. In order to perform this analysis, here **we introduce the `scarHRD` R package** and show that using this method the **SNP-array based and next generation sequencing based derivation of HRD scores show good correlation.**

<br> <br>

Getting started
===============

Minimum requirements
--------------------

-   Software: R
-   Operating system: Linux, OS X, Windows
-   R version: 3.4.0

Installation
------------

`scarHRD` can be installed via devtools from github:

``` r
library(devtools)
install_github('sztup/scarHRD',build_vignettes = TRUE)
```

Citation
--------

Please cite the following paper: manuscript submitted.

<br> <br>

Workflow overview
=================

A typical workflow of determining the genomic scar scores for a tumor sample has the following steps:

1.  Call allele specific copy number profile on paired normal-tumor BAM files. This step has to be executed before running scarHRD. We recommend using **Sequenza** (Favero et al. 2015) <http://www.cbs.dtu.dk/biotools/sequenza/> for copy number segmentation, Other tools (e.g. ASCAT (Van Loo et al. 2010)) may also be used in this step.

2.  Determine the scar scores with scarHRD R package

Input file examples
-------------------

The scarHRD example

Usage example
-------------

``` r
getwd()
```

Optional parameters
-------------------

`reference` -- the reference genome used, `grch38` or `grch37`

<br> <br>

Genomic scar scores
===================

Loss of Heterozygosity (HRD-LOH)
--------------------------------

The HRD-LOH score was described based on investigation in SNP-array-based copy number profiles of ovarian cancer (Popova et al. 2012). In this paper the authors showed that the samples with deficient BRCA1, BRCA2 have higher HRD-LOH scores compared to BRCA-intact samples, thus this measurement may be a reliable tool to estimate the sample's homologous recombination capacity.
<p class="moderateFrame">
The definition of a sample's HRD-LOH score is the </span> <span style="font-weight:bold">number of 15 Mb exceeding LOH regions which do not cover the whole chromosome.</span>.
</p>
In the first paper publishing HRD-LOH-score (Abkevich et al., 2012) the authors examine the correlation between HRD-LOH-score and HR deficiency calculated for different LOH region length cut-offs. In that paper the cut-off of 15 Mb approximately in the middle of the interval was arbitrarily selected for further analysis. The authors argue that the rational for this selection rather than selecting the cut-off with the lowest p-value is that the latter cut-off is more sensitive to statistical noise present in the data.
In our manuscript we also investigated **if this 15 Mb cutoff is appropriate for WXS-based HRD-LOH score**.We followed the same principles as Abkievits et al, thus while there was small difference between the p-values for the different minimum length cutoff values, we chose to use the same, 15 Mb limit as Abkevich et al. We also performed Spearman rank correlation between the SNP-array-based and WXS-based HRD-LOH scores for the different cutoff minimum LOH length cutoff (manuscript, Supplementary Figure S3C). Here the 14 Mb and 15 Mb cutoff-based WXS-HRD-LOH score had the highest correlation with the SNP-based HRD score. (0.700 and 0.695 respectively). This result reassured our choice of using the 15 Mb cutoff like in the SNP-array-based HRD-LOH score.

<div style="width:700px">
![Figure 1.A Visual representation of the HRD-LOH score on short theoretical chromosomes. Figure 1.B: Calculating HRD-LOH from a biallelic copy-number profile; LOH regions a, and c, would both increase the score by 1, while neither b, or d, would add to its value (b, does not pass the length requirement, and d covers a whole chromosome)](hrd-loh.svg) <br>

Large Scale Transitions (LST)
-----------------------------

The presence of Large Scale Transitions in connection with homologous recombination deficiency was first in studied in basal-like breast cancer (Abkevich et al. 2012). Based on SNP-array derived copy number profiles ... the number of telomeric allelic imbalances
<p class="moderateFrame">
A large scale transition is defined as a </span> <span style="font-weight:bold">chromosomal break between adjacent regions of at least 10 Mb, with a distance between them not larger than 3Mb..</span>.
</p>
![Figure 2.A: Visual representation of the LST score on short theoretical chromosomes. Figure 2.B: Calculating LST scores from a biallelic copy-number profile; events that are marked with green "marked" signs would increase the score, while events marked with red crosses would not. The grey areas represent the centromeric regions. (From left to right; Chromosome 1: the first event passes the definition of an LST, the second bounded by a shorter than 10 Mb segment from the right, the third is bounded by a segment from the left, which extends to the centromere, the fourthâ€™s gap is greater than 3 Mb. Chromosome 2: The first event is a valid LST, the second and third are not because they are bounded by centromeric segments, and the fourth is a valid LST)](lst.svg)

<br>

Number of Telomeric Allelic Imbalances
--------------------------------------

Allelic imbalance (AI) is the unequal contribution of parental allele sequences with or without changes in the overall copy number of the region. Our group have previously found, that the number telomeric AIs is indicative of defective DNA repair in ovarian cancer and triple-negative breast cancer, and that higher number of telomeric AI is associated with better response to cisplatin treatment (Birkbak et al. 2012).
<p class="moderateFrame">
The number of telomeric allelic imbalances is the </span> <span style="font-weight:bold">number AIs that extend to the telomeric end of a chromosome.</span>.
</p>
![Figure 3.A: Visual representation of the ntAI on short theoretical chromosomes. Figure 3.B: Illustration of possible telomeric allelic imbalances in an allele specific copy number profile.](ntai.svg)

<br>

References
==========

Abkevich, V., K. M. Timms, B. T. Hennessy, J. Potter, M. S. Carey, L. A. Meyer, K. Smith-McCune, et al. 2012. “Patterns of genomic loss of heterozygosity predict homologous recombination repair defects in epithelial ovarian cancer.” *Br. J. Cancer* 107 (10): 1776–82.

Birkbak, N. J., Z. C. Wang, J. Y. Kim, A. C. Eklund, Q. Li, R. Tian, C. Bowman-Colin, et al. 2012. “Telomeric allelic imbalance indicates defective DNA repair and sensitivity to DNA-damaging agents.” *Cancer Discov* 2 (4): 366–75.

Favero, F., T. Joshi, A. M. Marquard, N. J. Birkbak, M. Krzystanek, Q. Li, Z. Szallasi, and A. C. Eklund. 2015. “Sequenza: allele-specific copy number and mutation profiles from tumor sequencing data.” *Ann. Oncol.* 26 (1): 64–70.

Popova, T., E. Manie, G. Rieunier, V. Caux-Moncoutier, C. Tirapo, T. Dubois, O. Delattre, et al. 2012. “Ploidy and large-scale genomic instability consistently identify basal-like breast carcinomas with BRCA1/2 inactivation.” *Cancer Res.* 72 (21): 5454–62.

Van Loo, P., S. H. Nordgard, O. C. Lingj?rde, H. G. Russnes, I. H. Rye, W. Sun, V. J. Weigman, et al. 2010. “Allele-specific copy number analysis of tumors.” *Proc. Natl. Acad. Sci. U.S.A.* 107 (39): 16910–5.
