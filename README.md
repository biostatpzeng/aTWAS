 
HMAT: Harmonic Mean p-value Aggregated TWAS
========================================================================================================
## Background
**HMAT** is a method which aggregates multiple expression prediction models improves the power of transcriptome wide association studies.
Transcriptome-wide association study (TWAS) is an important integrative method for identifying genes that are causally associated with phenotypes. TWAS is often carried out in two stages. In the first stage, TWAS constructs a gene expression prediction model for each gene in turn using its cis-SNPs as predictors in the gene expression study. In the second stage, TWAS performs an association analysis to identify genes whose predicted expression level is associated with the phenotype in the genome-wide association study. Different TWAS methods rely on different models for gene expression prediction and each model makes a distinct modeling assumption that is suitable for a particular genetic architecture underlying gene expression. However, genetic architectures underlying gene expression may vary across genes throughout the transcriptome. Consequently, different TWAS methods are beneficial in detecting genes with distinct genetic architectures underlying gene expression.

Here, we develop a new method, HMAT, that can aggregate TWAS association evidence obtained across multiple gene expression prediction models. Because each expression prediction model is suited to capture a particular genetic architecture underlying gene expression, aggregating TWAS association across multiple prediction models as in HMAT ensures accurate gene expression prediction and subsequent powerful TWAS analysis across the transcriptome. A key feature of HMAT is its ability to accommodate correlations among test statistics that are output from multiple prediction models while producing calibrated p values for TWAS applications.

**[HMAT](https://github.com/biostatpzeng/HMAT/blob/main/HMAT_function.R)** is implemented in R statistical environment.

## Example
For GWAS with individual genotyps and phenotype
```ruby
source("HMAT_function.R")
y <- read.table("y.txt",sep=""),head=F)[,1]
G2 <- read.table("snp_gwas.txt",head=F)
weight <- matrix(runif(m*7),m,7)

# Here, we assume, for simplicity, that these simulated weights are estimated from seven various gene expression
# prediction models. Then, actually, there are seven various TWAS analyses. For each TWAS, we can obtain its p value
# to evaluate the significance of the gene. Finally, we combine these p values into a single one using HMAT.

HMAT_individual(y,G2,weight,outcome="B")

$p_HMAT
[1] 0.9125274

$p_TWAS
[1] 0.9185220 0.8028170 0.7293027 0.9295604 0.7424362 0.9007160 0.9363431
```

For GWAS with only summary statistics
```ruby
source("HMAT_function.R")
y <- read.table("y.txt",sep=""),head=F)[,1]
G2 <- read.table("snp_gwas.txt",head=F)
weight <- matrix(runif(m*7),m,7)

# Here, we assume, for simplicity, that these simulated weights are estimated from seven various gene expression
# prediction models. Then, actually, there are seven various TWAS analyses. For each TWAS, we can obtain its p value
# to evaluate the significance of the gene. Finally, we combine these p values into a single one using HMAT.

Z = rep(dim(G2)[2])
for (j in 1:dim(G2)[2]){Z[j] <- summary(glm(y~G2[,j],family = gaussian))$coef[2,3]}
G <- G2
HMAT_summary(Z,G,weight,0.9)

$p_HMAT
[1] 0.8695053

$p_TWAS
[1] 0.7421750 0.6300880 0.5776437 0.7748156 0.5727138 0.7264019 0.7925278
```

## Cite
[Ping Zeng](https://github.com/biostatpzeng) and Xiang Zhou (2021). Aggregating multiple expression prediction models improves the power of transcriptome wide association studies. Human Molecular Genetics.
## Contact
We are very grateful to any questions, comments, or bugs reports; and please contact [Ping Zeng](https://github.com/biostatpzeng) via zpstat@xzhmu.edu.cn.

## Update
2020-10-26  HMAT version 1.0.
