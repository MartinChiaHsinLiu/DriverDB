---
title: 'FPKM to TPM'
disqus: hackmd
---

FPKM to TPM
===

## Table of Contents

[TOC]

## Loading Packages

```gherkin=1
# no package is required
```

Function
---

```gherkin=1
FPKM_to_TPM <- function(FPKM){
    FPKM[is.na(FPKM)] <- 0
    TPM <- FPKM/sum(FPKM,na.rm=TRUE) * 10^6
    return(TPM)
}
```



## 資料來源
---
- ICGC (file): /media/r740/analyzed_result/Project/DriverDBv4/preprocess_data/gene_expression/final_ICGC/FPKM_ICGC.txt
- TCGA (path): /media/r740/analyzed_result/Project/DriverDBv4/preprocess_data/gene_expression/final_TCGA/
- SCAN_B (file): /media/r740/analyzed_result/Project/DriverDBv4/preprocess_data/gene_expression/SCAN_B_rna_exp.txt
- TARGET (path): /media/r740/analyzed_result/Project/DriverDBv4/preprocess_data/gene_expression/final_TARGET/
```gherkin=1
FPKM_ICGC <- "/media/r740/analyzed_result/Project/DriverDBv4/preprocess_data/gene_expression/final_ICGC/FPKM_ICGC.txt"
FPKM_SCANB <- "/media/r740/analyzed_result/Project/DriverDBv4/preprocess_data/gene_expression/final_SCAN_B/SCAN_B_rna_exp.txt"
FPKM_TCGA.list <- list.files("/media/r740/analyzed_result/Project/DriverDBv4/preprocess_data/gene_expression/final_TCGA/",full.names=TRUE)
FPKM_TARGET.list <- list.files("/media/r740/analyzed_result/Project/DriverDBv4/preprocess_data/gene_expression/final_TARGET/",full.names=TRUE)

out.path <- "/media/r740/analyzed_result/Project/DriverDBv4/preprocess_data/gene_expression/TPM"
```

## TCGA
---
```gherkin=1
for(j in 1:length(FPKM_TCGA.list)){
print(j)
FPKM_TCGA_DATA <- read.table(FPKM_TCGA.list[j],head=TRUE, stringsAsFactor=FALSE,sep=" ",fill=TRUE)
barcode_7 <- unique(FPKM_TCGA_DATA$barcode_7)
for(i in 1:length(barcode_7)){
    tmp.data <-FPKM_TCGA_DATA[FPKM_TCGA_DATA$barcode_7==barcode_7[i],]
    tmp.data$TPM <- FPKM_to_TPM(tmp.data$fpkm)
    outname<-paste(c("TPM",gsub("TCGA_rna_exp_","",grep("txt",unlist(strsplit(FPKM_TCGA.list[j],"/")),value=TRUE))),collapse="_")
    if(i==1){
        write.table(tmp.data,file.path(out.path,outname),row.names=FALSE,quote=FALSE,sep="\t")
    } else {
        write.table(tmp.data,file.path(out.path,outname),row.names=FALSE,col.names=FALSE,quote=FALSE,sep="\t",append=TRUE)
    }
}
}

```

## TARGET

```gherkin=1

```
## ICGC

```gherkin=1
FPKM_ICGC_DATA <- read.table(FPKM_ICGC,head=TRUE, stringsAsFactor=FALSE,sep="\t")
barcode_7 <- unique(FPKM_ICGC_DATA$barcode_7)
for(i in 1:length(barcode_7)){
    tmp.data <-FPKM_ICGC_DATA[FPKM_ICGC_DATA$barcode_7==barcode_7[i],]
    tmp.data$TPM <- FPKM_to_TPM(tmp.data$fpkm)
    if(i==1){
        write.table(tmp.data,file.path(out.path,"ICGC_TPM.txt"),row.names=FALSE,quote=FALSE,sep="\t")
    } else {
        write.table(tmp.data,file.path(out.path,"ICGC_TPM.txt"),row.names=FALSE,col.names=FALSE,quote=FALSE,sep="\t",append=TRUE)
    }
}

```

## SCAN-B

```gherkin=1
FPKM_SCANB_DATA <- read.delim(FPKM_SCANB,header=TRUE, stringsAsFactor=FALSE,sep=" ",fill=TRUE,nrow=100000)
barcode_7 <- unique(FPKM_SCANB_DATA$barcode_7)
for(i in 1:length(barcode_7)){
print(i)
    tmp.data <-FPKM_SCANB_DATA[FPKM_SCANB_DATA$barcode_7==barcode_7[i],]
    tmp.data$TPM <- FPKM_to_TPM(tmp.data$fpkm)
    if(i==1){
        write.table(tmp.data,file.path(out.path,"SCANB_TPM.txt"),row.names=FALSE,quote=FALSE,sep="\t")
    } else {
        write.table(tmp.data,file.path(out.path,"SCANB_TPM.txt"),row.names=FALSE,col.names=FALSE,quote=FALSE,sep="\t",append=TRUE)
    }
}

```




###### tags: `DriverDBv4` `TCGA` `TPM` 
