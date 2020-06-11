# Project Overview
Two sample Mendelian Randomization Analysis on GWAS of human lifespan looking at new and known exposure relationships related to longevity outcomes.

## A. Exposures

Exposures are obtained from the mrbase.org website from the following catalogs
  - NHGRI-EBI GWAS catalog
  - MR BASE GWAS catalog
  - Gene Expression QTLs
  - Protein Level QTLs
  - Metabolite Level OTLs
  - Methylation Level QTLs

## B. Outcomes

Outcome Data obtained from CPMC that was utilized in Deelen et al., 2019 (https://pubmed.ncbi.nlm.nih.gov/31413261/)
and from UK Biobank.

UK Biobank studies
1. Pilling et al. PMID: 29227965. 
Human longevity: 25 genetic loci associated in 389,166 UK biobank participants.

This paper used many longevity definitions based on parental lifespan. We want to use the case-control longevity definition defined as both parents in top 10%. 

https://www.ebi.ac.uk/gwas/studies/GCST006698

Upon clicking on the FTP link, you go here:
ftp://ftp.ebi.ac.uk/pub/databases/gwas/summary_statistics/PillingLC_29227965_GCST006698

Check out the harmonized version as well:
ftp://ftp.ebi.ac.uk/pub/databases/gwas/summary_statistics/PillingLC_29227965_GCST006698/harmonised/

2. Timmers et al. PMID: 30642433.
Genomics of 1 million parent lifespans implicates novel pathways and common diseases and distinguishes survival chances.

https://www.ebi.ac.uk/gwas/studies/GCST009890

Upon clicking on the FTP link, you go here:
ftp://ftp.ebi.ac.uk/pub/databases/gwas/summary_statistics/TimmersPR_30642433_GCST009890

3. Deelen et al. PMID: 31413261. 
A meta-analysis of genome-wide association studies identifies multiple longevity genes. 
Cases 90. Cases achieved the 90th percentile of survival age.

https://www.ebi.ac.uk/gwas/studies/GCST008598

ftp://ftp.ebi.ac.uk/pub/databases/gwas/summary_statistics/DeelenJ_31413261_GCST008598


4. Deelen et al. PMID: 31413261.
A meta-analysis of genome-wide association studies identifies multiple longevity genes.
Cases 99. Cases achieved the 99th percentile of survival age.

https://www.ebi.ac.uk/gwas/studies/GCST008599

ftp://ftp.ebi.ac.uk/pub/databases/gwas/summary_statistics/DeelenJ_31413261_GCST008599


## C. Analysis
TwoSampleMR package in R was used for analysis.
(mrbase.org)

