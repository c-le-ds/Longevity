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
- below is search by gene and tissue together
```{r}

#import Gene Expression QTLs list
data(gtex_eqtl)
gtex_eqtl


#create a list of unique gene names

genes_unique <- gtex_eqtl[!duplicated(gtex_eqtl$gene_name),] %>% pull(gene_name)

<<<<<<< HEAD
exposures_genes <- data.frame()
for(i in 1: length(gene_unique)){
  genes <- subset(gtex_eqtl, gene_name == gene_unique[i])
=======
# 32432 unique gene names
```




try as a function

```{r}
genes_clump <- function(genes){
  gene <- subset(gtex_eqtl, gene_name == genes) %>% format_data() %>% clump_data()
}

gen <- genes_unique[1:10]

test <- lapply(gen, genes_clump)
```

try local clumping
reference downloaded at : http://fileserve.mrcieu.ac.uk/ld/1kg.v3.tgz
```{r}


#format for ld_clumping

gene_dat <-subset(gtex_eqtl, gene_name == "RP4-669L17.10") %>% 
  format_data() %>%
  rename(rsid = SNP,
         pval = pval.exposure,
         id = id.exposure) %>%
  ld_clump(plink_bin = genetics.binaRies::get_plink_binary(), bfile = "C:/Users/me/Desktop/MPH/Internship/CPMC/Data/LD_reference/EUR", #no need to include extensions
           clump_kb = 1000, clump_r2 = .001, 
           clump_p = 1, pop = "EUR") 
 

#same results
```



#### d. Protein Level QTLs

Instead of retrieving via API, do ld clumping locally using the same reference population "EUR" from 1000 genomes

```{r}
#import Protein Level QTLs into R
data(proteomic_qtls)
proteomic_qtls

#create a list of unique protein analyte & make a list

proteins_unique <- proteomic_qtls[!duplicated(proteomic_qtls$analyte),] %>% pull(analyte)


exposures_protein <- data.frame()
for(i in 1 : length(proteins_unique)) {
>>>>>>> Exposures
  
  proteins_phenotypes<- subset(proteomic_qtls, analyte == proteins_unique[i])
  
  #format exposure data and clump by LD r2 <0.001 to reduce covariance
  proteins_format<- proteins_phenotypes %>% format_data() %>%
    rename(rsid = SNP,
         pval = pval.exposure,
         id = id.exposure)
  
  proteins_clump <- ld_clump(proteins_format, 
                             plink_bin = genetics.binaRies::get_plink_binary(), 
                             bfile = "C:/Users/me/Desktop/MPH/Internship/CPMC/Data/LD_reference/EUR",
                             #no need to include extensions
                             clump_kb = 1000, clump_r2 = .001, clump_p = 1, pop = "EUR")

  exposures_protein <- rbind(exposures_protein, proteins_clump)
    
}

exposures_protein <- exposures_protein %>% rename (SNP = rsid, id.exposure = id)

```



For Loops
```{r}

#import Protein Level QTLs into R
data(proteomic_qtls)
proteomic_qtls

#create a list of unique protein analyte & make a list

proteins_unique <- proteomic_qtls[!duplicated(proteomic_qtls$analyte),] %>% pull(analyte)
#  unique phenotypes in catalog



#make a for loop to create a database of exposure SNPs formatted for exposure data and clumped
exposures_proteins_API <- data.frame()
for(i in 1 : length(proteins_unique)) {
  
  proteins_phenotypes_API<- subset(proteomic_qtls, analyte == proteins_unique[i])
  
  #format exposure data and clump by LD r2 <0.001 to reduce covariance
  proteins_format_API<- proteins_phenotypes_API %>% format_data() %>% clump_data()

  exposures_proteins_API <- rbind(exposures_proteins_API, proteins_format_API)
    
}


#check that 'phenotypes' is the same as the last entry in gwas_unique
proteins_unique[]
proteins_phenotypes$analyte


saveRDS(exposures_proteins_API, file = "C:/Users/me/Desktop/MPH/Internship/CPMC/exposures_proteins.rds")
```

Both dataframes are the same. 

```{r}
identical(exposures_protein$rsid, exposures_proteins_API$rsid)
```


#### e. Metabolite Level OTLs


```{r}

#import Metabolite Level QTLs into R
data(metab_qtls)
metab_qtls

#create a list of unique metabolomic phenotype & make a list

metabolites_unique <- metab_qtls[!duplicated(metab_qtls$phenotype),] %>% pull(phenotype)
# 121 unique phenotypes in catalog



#make a for loop to create a database of exposure SNPs formatted for exposure data and clumped
exposures_metabolite <- data.frame()
for(i in 1 : length(metabolites_unique)) {
  
  metabolites_phenotypes<- subset(metab_qtls, phenotype == metabolites_unique[i]) %>% 
    format_data() %>% 
    rename(rsid = SNP,
         pval = pval.exposure,
         id = id.exposure) %>%
    ld_clump( plink_bin = genetics.binaRies::get_plink_binary(), 
              bfile = "C:/Users/me/Desktop/MPH/Internship/CPMC/Data/LD_reference/EUR",
              clump_kb = 1000, clump_r2 = .001, clump_p = 1, pop = "EUR")
    

  exposures_metabolite <- rbind(exposures_metabolite, metabolites_phenotypes)
    
}

exposures_metabolite <- exposures_metabolite %>% rename (SNP = rsid, id.exposure = id)  
  
#check that 'phenotypes' is the same as the last entry in metabolites_unique
metabolites_unique[]
metabolites_phenotypes$phenotype


saveRDS(exposures_metabolite, file = "C:/Users/me/Desktop/MPH/Internship/CPMC/exposures_metabolites.rds")
```

#### f. Methylation Level QTLs
by cpg site and age



```{r}

#import Methylation Level QTLs into R
data(aries_mqtl)
aries_mqtl

#create a list of unique Methylation  cpg & make a list

methylation_unique <- aries_mqtl[!duplicated(aries_mqtl$cpg),]%>% pull(cpg)
age <- c("Birth", "Adolescence", "Childhood", "Middle age", "Pregnancy")
#  33256 unique cpg sites in catalog
# 5 'ages'

#make a for loop to create a database of exposure SNPs formatted for exposure data and clumped


exposures_methylation <- data.frame()
for (j in 1:length(age)){
  for(i in 1 : 1) {
  
    methylation_phenotypes<- subset(aries_mqtl, cpg == methylation_unique[i] & age == age[j])%>% 
      format_data() %>% 
      rename(rsid = SNP,
         pval = pval.exposure,
         id = id.exposure) %>%
    ld_clump( plink_bin = genetics.binaRies::get_plink_binary(), 
              bfile = "C:/Users/me/Desktop/MPH/Internship/CPMC/Data/LD_reference/EUR",
              clump_kb = 1000, clump_r2 = .001, clump_p = 1, pop = "EUR")
    
    
  exposures_methylation <- rbind(exposures_methylation, methylation_phenotypes)
     

    
    
  }


}

exposures_methylation <- exposures_methylation %>% rename (SNP = rsid, id.exposure = id)   
#check that 'phenotypes' is the same as the last entry in metabolites_unique
methylation_unique[]
methylation_phenotypes$cpg


saveRDS(exposures_methylation, file = "C:/Users/me/Desktop/MPH/Internship/CPMC/exposures_methylation.rds")
```