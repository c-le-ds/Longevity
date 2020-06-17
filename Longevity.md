# Project Overview
Two sample Mendelian Randomization Analysis on GWAS of human lifespan looking at new and known exposure relationships related to longevity outcomes.
test
## A. Exposures

Exposures are obtained from the mrbase.org website from the following catalogs
  - NHGRI-EBI GWAS catalog
  - MR BASE GWAS catalog
  - Gene Expression QTLs
  - Protein Level QTLs
  - Metabolite Level OTLs
  - Methylation Level QTLs

## B. Outcomes

### I. First Outcome: cases 90

Outcome Data obtained from CPMC that was utilized in Deelen et al., 2019 (https://pubmed.ncbi.nlm.nih.gov/31413261/). Exposure - Outcome relationships will be examined in persons whose longevity is in the 90th percentile using the rest of the cohort as the control.


### II. Second outcome: cases 99

Using data from section I, compare persons in the 90th percentile with persons in the 99th percentile. 

### III. Third Outcome: UK Biobank

Compare persons in the 90th percentile from section I to those in the 90th percentile from the UK Biobank

### IV. Fourth Outcome: UK Biobank Parental Longevity
## C. Preparation of Data

### I. Load Libraries
```{r}
library(TwoSampleMR)
library(MRInstruments)
library(dplyr)
library(knitr)
```

### II.  Exposures from each catalog


#### a. GWAS Catalog

```{r}

#import gwas catalog into R
data(gwas_catalog)
gwas_catalog

#create a list of unique gwas phenotpes & make a list

gwas_unique <- gwas_catalog[!duplicated(gwas_catalog$Phenotype),] %>% pull(Phenotype)
# 3590 unique phenotypes in catalog



#make a for loop to create a database of exposure SNPs formatted for exposure data and clumped
exposures_gwas <- data.frame()
for(i in 1 : length(gwas_unique)) {
  
  gwas_phenotypes<- subset(gwas_catalog, Phenotype == gwas_unique[i])
  
  #format exposure data and clump by LD r2 <0.001 to reduce covariance
  gwas_format<- gwas_phenotypes %>% format_data() %>% clump_data()

  exposures_gwas <- rbind(exposures_gwas, gwas_format)
    
}

  
#check that 'phenotypes' is the same as the last entry in gwas_unique
gwas_unique[3590]
gwas_phenotypes$Phenotype


save(exposures_gwas, file = "~/exposures_gwas.Rdata")
```

#### b. MR Base GWAS Catalog

#### c. Gene Expression QTLs
should we search for gene and tissue together or just gene?
- below is search by gene name only.
```{r}

#import Gene Expression QTLs list
data(gtex_eqtl)
gtex_eqtl


#create a list of unique gene names

genes_unique <- gtex_eqtl[!duplicated(gtex_eqtl),] %>% pull(gene_name)

exposures_genes <- data.frame()
for(i in 1: length(gene_unique)){
  genes <- subset(gtex_eqtl, gene_name == gene_unique[i])
  
  #create a separate clumping step in case LD lookup times out
  genes_format <- format_data(genes) %>% clump_data()
  
  exposures_genes <- rbind(exposures_genes, gene_format)
}
```