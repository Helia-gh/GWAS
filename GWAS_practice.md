\# Genome Wide Association Study practice in UNIX  
\# By Helia Ghazinejad

\[\~\]$ cd GWASrawdata  
\[GWASrawdata\]$ R

\> \#Overview of raw dataset  
\> simped \<- read.table("GWAS.ped", header \= F, stringsAsFactors \= F)   
   
\> nrow(simped)   
\[1\] 248

\> ncol(simped)  
\[1\] 12854

\> simped\[1:5, 1:10\]  
    V1      V2 V3 V4 V5 V6 V7 V8 V9 V10  
1 1328 NA06989  0  0  2 \-9  G  G  0   0  
2 1377 NA11891  0  0  1 \-9  A  G  0   0  
3 1349 NA11843  0  0  1 \-9  G  G  0   0  
4 1330 NA12341  0  0  2 \-9  G  G  0   0  
5 1444 NA12739  0  0  1 \-9  G  G  0   0

\> simmap \<- read.table("GWAS.map", header \= F, stringsAsFactors \= F)

\> nrow(simmap) \#Number of SNPs  
\[1\] 6424

\> ncol(simmap)  
\[1\] 4

\> simmap\[1:5,\]  
  V1        V2 V3      V4  
1  1 rs9439458  0 1441243  
2  1  rs262662  0 2074893  
3  1 rs2843403  0 2518957  
4  1 rs4648462  0 3155127  
5  1 rs1128474  0 3535472

\> simpheno \<- read.table("pheno.txt", header \= T, sep \= "\\t")

\> dim(simpheno)  
\[1\] 253   3

\> head(simpheno)  
   FID     IID Aff  
1 1328 NA06989   1  
2 1377 NA11891   2  
3 1349 NA11843   1  
4 1330 NA12341   2  
5 1444 NA12739   1  
6 1328 NA06984   2

\> table(simpheno$Aff)

  1   2  
162  91  
\> \#There are 162 controls and 91 cases in our dataset

\> q()  
Save workspace image? \[y/n/c\]: y

\[GWASrawdata\]$ module load plink/1.9b\_4.1-x86\_64

\[GWASrawdata\]$ plink \--file GWAS \--pheno pheno.txt \--geno 0.05 \--recode \--out GWAS\_qc1 \#Exclude SNPs with low genotype calls of more than 5%  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to GWAS\_qc1.log.  
Options in effect:  
  \--file GWAS  
  \--geno 0.05  
  \--out GWAS\_qc1  
  \--pheno pheno.txt  
  \--recode

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (6424 variants, 248 people).  
\--file: GWAS\_qc1-temporary.bed \+ GWAS\_qc1-temporary.bim \+  
GWAS\_qc1-temporary.fam written.  
6424 variants loaded from .bim file.  
248 people (125 males, 123 females) loaded from .fam.  
248 phenotype values present after \--pheno.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 248 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see GWAS\_qc1.hh ); many commands  
treat these as missing.  
Total genotyping rate is 0.99641.  
2 variants removed due to missing genotype data (--geno).  
6422 variants and 248 people pass filters and QC.  
Among remaining phenotypes, 89 are cases and 159 are controls.  
\--recode ped to GWAS\_qc1.ped \+ GWAS\_qc1.map ... done.

\[GWASrawdata\]$ plink \--file GWAS\_qc1 \--pheno pheno.txt \--mind 0.10 \--recode \--out GWAS\_qc2 \#Exclude individuals missing 10% of genotype  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to GWAS\_qc2.log.  
Options in effect:  
  \--file GWAS\_qc1  
  \--mind 0.10  
  \--out GWAS\_qc2  
  \--pheno pheno.txt  
  \--recode

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (6422 variants, 248 people).  
\--file: GWAS\_qc2-temporary.bed \+ GWAS\_qc2-temporary.bim \+  
GWAS\_qc2-temporary.fam written.  
6422 variants loaded from .bim file.  
248 people (125 males, 123 females) loaded from .fam.  
248 phenotype values present after \--pheno.  
1 person removed due to missing genotype data (--mind).  
ID written to GWAS\_qc2.irem .  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 247 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see GWAS\_qc2.hh ); many commands  
treat these as missing.  
Total genotyping rate in remaining samples is 0.99703.  
6422 variants and 247 people pass filters and QC.  
Among remaining phenotypes, 88 are cases and 159 are controls.  
\--recode ped to GWAS\_qc2.ped \+ GWAS\_qc2.map ... done.

\[GWASrawdata\]$ plink \--file GWAS\_qc2 \--pheno pheno.txt \--check-sex \#Check for sex discrepancies  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to plink.log.  
Options in effect:  
  \--check-sex  
  \--file GWAS\_qc2  
  \--pheno pheno.txt

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (6422 variants, 247 people).  
\--file: plink-temporary.bed \+ plink-temporary.bim \+ plink-temporary.fam  
written.  
6422 variants loaded from .bim file.  
247 people (125 males, 122 females) loaded from .fam.  
247 phenotype values present after \--pheno.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 247 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see plink.hh ); many commands treat  
these as missing.  
Total genotyping rate is 0.99703.  
6422 variants and 247 people pass filters and QC.  
Among remaining phenotypes, 88 are cases and 159 are controls.  
\--check-sex: 194 Xchr and 0 Ychr variant(s) scanned, 5 problems detected.  
Report written to plink.sexcheck .

\[GWASrawdata\]$ grep "PROBLEM" plink.sexcheck| awk '{print$1,$2}'\> sex\_discrepancy.txt \#Create file for those with unmatching sex versus genotype

\[GWASrawdata\]$ plink \--file GWAS\_qc2 \--pheno pheno.txt \--remove sex\_discrepancy.txt \--recode \--out GWAS\_qc3 \#Remove sex discrepancy data  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to GWAS\_qc3.log.  
Options in effect:  
  \--file GWAS\_qc2  
  \--out GWAS\_qc3  
  \--pheno pheno.txt  
  \--recode  
  \--remove sex\_discrepancy.txt

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (6422 variants, 247 people).  
\--file: GWAS\_qc3-temporary.bed \+ GWAS\_qc3-temporary.bim \+  
GWAS\_qc3-temporary.fam written.  
6422 variants loaded from .bim file.  
247 people (125 males, 122 females) loaded from .fam.  
247 phenotype values present after \--pheno.  
\--remove: 242 people remaining.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 242 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see GWAS\_qc3.hh ); many commands  
treat these as missing.  
Total genotyping rate in remaining samples is 0.997124.  
6422 variants and 242 people pass filters and QC.  
Among remaining phenotypes, 88 are cases and 154 are controls.  
\--recode ped to GWAS\_qc3.ped \+ GWAS\_qc3.map ... done.

\[GWASrawdata\]$ plink \--file GWAS\_qc3 \--pheno pheno.txt \--het \#Check heterozygosity rate  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to plink.log.  
Options in effect:  
  \--file GWAS\_qc3  
  \--het  
  \--pheno pheno.txt

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (6422 variants, 242 people).  
\--file: plink-temporary.bed \+ plink-temporary.bim \+ plink-temporary.fam  
written.  
6422 variants loaded from .bim file.  
242 people (125 males, 117 females) loaded from .fam.  
242 phenotype values present after \--pheno.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 242 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see plink.hh ); many commands treat  
these as missing.  
Total genotyping rate is 0.997124.  
6422 variants and 242 people pass filters and QC.  
Among remaining phenotypes, 88 are cases and 154 are controls.  
\--het: 6228 variants scanned, report written to plink.het .

\[GWASrawdata\]$ R

\> Dataset \<- read.table("plink.het", header=TRUE, sep="", na.strings="NA", dec=".", strip.white=TRUE)

\> mean(Dataset$F)  
\[1\] 0.00931633  
\> sd(Dataset$F)  
\[1\] 0.02110704

\> \#Plot histogram of heterozygosity rate distribution  
\> jpeg("hist.jpeg", height=1000, width=1000)  
\> hist(scale(Dataset$F), xlim=c(-4,4), xlab="Heterozygosity Rate")  
\> dev.off()  
null device  
          1  
\> \#Create file for those who deviate more than 3 sd from heterozygosity rate mean  
\> Dataset$HET\_RATE \= (Dataset$"N.NM." \- Dataset$"O.HOM.")/Dataset$"N.NM."  
\> het\_fail \= subset(Dataset, (Dataset$HET\_RATE \< mean(Dataset$HET\_RATE)-3\*sd(Dataset$HET\_RATE)) | (Dataset$HET\_RATE \> mean(Dataset$HET\_RATE)+3\*sd(Dataset$HET\_RATE)));  
\> het\_fail$HET\_DST \= (het\_fail$HET\_RATE-mean(Dataset$HET\_RATE))/sd(Dataset$HET\_RATE);  
\> write.table(het\_fail, "fail-het-qc.txt", row.names=FALSE)  
\> q()  
Save workspace image? \[y/n/c\]: y

\[GWASrawdata\]$ sed 's/"// g' fail-het-qc.txt | awk '{print$1, $2}'\> het\_fail\_ind.txt \#Adapt the file from R to be compatible for plink

\[GWASrawdata\]$ plink \--file GWAS\_qc3 \--pheno pheno.txt \--remove het\_fail\_ind.txt \--recode \--out GWAS\_qc4 \#Remove heterozygosity rate outliers  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to GWAS\_qc4.log.  
Options in effect:  
  \--file GWAS\_qc3  
  \--out GWAS\_qc4  
  \--pheno pheno.txt  
  \--recode  
  \--remove het\_fail\_ind.txt

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (6422 variants, 242 people).  
\--file: GWAS\_qc4-temporary.bed \+ GWAS\_qc4-temporary.bim \+  
GWAS\_qc4-temporary.fam written.  
6422 variants loaded from .bim file.  
242 people (125 males, 117 females) loaded from .fam.  
242 phenotype values present after \--pheno.  
\--remove: 242 people remaining.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 242 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see GWAS\_qc4.hh ); many commands  
treat these as missing.  
Total genotyping rate is 0.997124.  
6422 variants and 242 people pass filters and QC.  
Among remaining phenotypes, 88 are cases and 154 are controls.  
\--recode ped to GWAS\_qc4.ped \+ GWAS\_qc4.map ... done.

\[GWASrawdata\]$ plink \--file GWAS\_qc4 \--pheno pheno.txt \--genome \--min 0.2 \--out GWAS\_qc5 \#Check for cryptic relatedness with inbreeding coefficient of 0.2  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to GWAS\_qc5.log.  
Options in effect:  
  \--file GWAS\_qc4  
  \--genome  
  \--min 0.2  
  \--out GWAS\_qc5  
  \--pheno pheno.txt

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (6422 variants, 242 people).  
\--file: GWAS\_qc5-temporary.bed \+ GWAS\_qc5-temporary.bim \+  
GWAS\_qc5-temporary.fam written.  
6422 variants loaded from .bim file.  
242 people (125 males, 117 females) loaded from .fam.  
242 phenotype values present after \--pheno.  
Using up to 23 threads (change this with \--threads).  
Before main variant filters, 242 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see GWAS\_qc5.hh ); many commands  
treat these as missing.  
Total genotyping rate is 0.997124.  
6422 variants and 242 people pass filters and QC.  
Among remaining phenotypes, 88 are cases and 154 are controls.  
Excluding 194 variants on non-autosomes from IBD calculation.  
IBD calculations complete.  
Finished writing GWAS\_qc5.genome .

\[GWASrawdata\]$ R

\> \#Create a table for duplicates, PI\_Hat more than 0.4, in our dataset  
\> dups \= read.table("GWAS\_qc5.genome", header \= T)  
\> problem\_pairs \= dups\[which(dups$PI\_HAT \> 0.4),\]  
\> problem\_pairs  
   FID1    IID1 FID2    IID2 RT EZ     Z0     Z1     Z2 PI\_HAT PHE      DST PPC  
1  1444 NA12739 1444 NA12749 OT  0 0.0026 0.9802 0.0172 0.5073  \-1 0.839312   1  
2  1444 NA12739 1444 NA12748 OT  0 0.0026 0.9948 0.0026 0.5000  \-1 0.836930   1  
3 13291 NA25001 1344 NA12057 UN NA 0.0000 0.0025 0.9975 0.9988   1 0.999597   1  
4  M041 NA25000 M033 NA19774 UN NA 0.0000 0.0000 1.0000 1.0000  \-1 1.000000   1  
  RATIO  
1   870  
2   867  
3    NA  
4    NA  
\> q()  
Save workspace image? \[y/n/c\]: y

\[GWASrawdata\]$ plink \--file GWAS\_qc4 \--pheno pheno.txt \--remove GWAS\_qc5.genome \--recode \--out GWAS\_qc5 \#Remove duplicates  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to GWAS\_qc5.log.  
Options in effect:  
  \--file GWAS\_qc4  
  \--out GWAS\_qc5  
  \--pheno pheno.txt  
  \--recode  
  \--remove GWAS\_qc5.genome

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (6422 variants, 242 people).  
\--file: GWAS\_qc5-temporary.bed \+ GWAS\_qc5-temporary.bim \+  
GWAS\_qc5-temporary.fam written.  
6422 variants loaded from .bim file.  
242 people (125 males, 117 females) loaded from .fam.  
242 phenotype values present after \--pheno.  
\--remove: 239 people remaining.  
Warning: At least 1 duplicate ID in \--remove file.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 239 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see GWAS\_qc5.hh ); many commands  
treat these as missing.  
Total genotyping rate in remaining samples is 0.997195.  
6422 variants and 239 people pass filters and QC.  
Among remaining phenotypes, 87 are cases and 152 are controls.  
\--recode ped to GWAS\_qc5.ped \+ GWAS\_qc5.map ... done.

\[GWASrawdata\]$ plink \--file GWAS\_qc5 \--pheno pheno.txt \--maf 0.05 \--recode \--out MAF\_greater\_5 \#Create dataset with Minor Allele Frequency threshold of 0.05  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to MAF\_greater\_5.log.  
Options in effect:  
  \--file GWAS\_qc5  
  \--maf 0.05  
  \--out MAF\_greater\_5  
  \--pheno pheno.txt  
  \--recode

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (6422 variants, 239 people).  
\--file: MAF\_greater\_5-temporary.bed \+ MAF\_greater\_5-temporary.bim \+  
MAF\_greater\_5-temporary.fam written.  
6422 variants loaded from .bim file.  
239 people (123 males, 116 females) loaded from .fam.  
239 phenotype values present after \--pheno.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 239 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see MAF\_greater\_5.hh ); many  
commands treat these as missing.  
Total genotyping rate is 0.997195.  
547 variants removed due to minor allele threshold(s)  
(--maf/--max-maf/--mac/--max-mac).  
5875 variants and 239 people pass filters and QC.  
Among remaining phenotypes, 87 are cases and 152 are controls.  
\--recode ped to MAF\_greater\_5.ped \+ MAF\_greater\_5.map ... done.

\[GWASrawdata\]$ plink \--file GWAS\_qc5 \--pheno pheno.txt \--exclude MAF\_greater\_5.map \--recode \--out MAF\_less\_5 \#Dataset for low MAF SNPs  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to MAF\_less\_5.log.  
Options in effect:  
  \--exclude MAF\_greater\_5.map  
  \--file GWAS\_qc5  
  \--out MAF\_less\_5  
  \--pheno pheno.txt  
  \--recode

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (6422 variants, 239 people).  
\--file: MAF\_less\_5-temporary.bed \+ MAF\_less\_5-temporary.bim \+  
MAF\_less\_5-temporary.fam written.  
6422 variants loaded from .bim file.  
239 people (123 males, 116 females) loaded from .fam.  
239 phenotype values present after \--pheno.  
\--exclude: 547 variants remaining.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 239 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Total genotyping rate is 0.997032.  
547 variants and 239 people pass filters and QC.  
Among remaining phenotypes, 87 are cases and 152 are controls.  
\--recode ped to MAF\_less\_5.ped \+ MAF\_less\_5.map ... done.

\[GWASrawdata\]$ plink \--file MAF\_greater\_5 \--pheno pheno.txt \--hwe 10e-07 \--recode \--out GWAS\_clean \#Apply the Hardy-Weinberg Principal  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to GWAS\_clean.log.  
Options in effect:  
  \--file MAF\_greater\_5  
  \--hwe 10e-07  
  \--out GWAS\_clean  
  \--pheno pheno.txt  
  \--recode

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (5875 variants, 239 people).  
\--file: GWAS\_clean-temporary.bed \+ GWAS\_clean-temporary.bim \+  
GWAS\_clean-temporary.fam written.  
5875 variants loaded from .bim file.  
239 people (123 males, 116 females) loaded from .fam.  
239 phenotype values present after \--pheno.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 239 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see GWAS\_clean.hh ); many commands  
treat these as missing.  
Total genotyping rate is 0.99721.  
Warning: \--hwe observation counts vary by more than 10%, due to the X  
chromosome.  You may want to use a less stringent \--hwe p-value threshold for X  
chromosome variants.  
\--hwe: 2 variants removed due to Hardy-Weinberg exact test.  
5873 variants and 239 people pass filters and QC.  
Among remaining phenotypes, 87 are cases and 152 are controls.  
\--recode ped to GWAS\_clean.ped \+ GWAS\_clean.map ... done.

\[GWASrawdata\]$ plink \--file GWAS\_clean \--pheno pheno.txt \--assoc \--out assoc\_results \#Association test between cases and controls for the SNPs  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to assoc\_results.log.  
Options in effect:  
  \--assoc  
  \--file GWAS\_clean  
  \--out assoc\_results  
  \--pheno pheno.txt

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (5873 variants, 239 people).  
\--file: assoc\_results-temporary.bed \+ assoc\_results-temporary.bim \+  
assoc\_results-temporary.fam written.  
5873 variants loaded from .bim file.  
239 people (123 males, 116 females) loaded from .fam.  
239 phenotype values present after \--pheno.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 239 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see assoc\_results.hh ); many  
commands treat these as missing.  
Total genotyping rate is 0.997214.  
5873 variants and 239 people pass filters and QC.  
Among remaining phenotypes, 87 are cases and 152 are controls.  
Writing C/C \--assoc report to assoc\_results.assoc ... done.

\[GWASrawdata\]$ plink \--file GWAS\_clean \--pheno pheno.txt \--assoc \--adjust \--out adjusted\_assoc\_results \#Adjust for multiple testing to get corrected p value  
PLINK v1.90b4.6 64-bit (15 Aug 2017\)           www.cog-genomics.org/plink/1.9/  
(C) 2005-2017 Shaun Purcell, Christopher Chang   GNU General Public License v3  
Logging to adjusted\_assoc\_results.log.  
Options in effect:  
  \--adjust  
  \--assoc  
  \--file GWAS\_clean  
  \--out adjusted\_assoc\_results  
  \--pheno pheno.txt

257404 MB RAM detected; reserving 128702 MB for main workspace.  
.ped scan complete (for binary autoconversion).  
Performing single-pass .bed write (5873 variants, 239 people).  
\--file: adjusted\_assoc\_results-temporary.bed \+  
adjusted\_assoc\_results-temporary.bim \+ adjusted\_assoc\_results-temporary.fam  
written.  
5873 variants loaded from .bim file.  
239 people (123 males, 116 females) loaded from .fam.  
239 phenotype values present after \--pheno.  
Using 1 thread (no multithreaded calculations invoked).  
Before main variant filters, 239 founders and 0 nonfounders present.  
Calculating allele frequencies... done.  
Warning: 6 het. haploid genotypes present (see adjusted\_assoc\_results.hh );  
many commands treat these as missing.  
Total genotyping rate is 0.997214.  
5873 variants and 239 people pass filters and QC.  
Among remaining phenotypes, 87 are cases and 152 are controls.  
Writing C/C \--assoc report to adjusted\_assoc\_results.assoc ...  
done.  
\--adjust: Genomic inflation est. lambda (based on median chisq) \= 1.1321.  
\--adjust values (5873 variants) written to  
adjusted\_assoc\_results.assoc.adjusted .

\[GWASrawdata\]$ R

\> \#Read results of the association test after adjusting for multiple testing   
\> res1 \<- read.table("adjusted\_assoc\_results.assoc", header \= T, stringsAsFactors \= F)  
\> res1\[1:5,\]  
  CHR        SNP      BP A1    F\_A    F\_U A2    CHISQ      P     OR  
1   1  rs2843403 2518957  T 0.3046 0.3553  C 1.272000 0.2594 0.7949  
2   1  rs1490413 4267183  A 0.4138 0.4474  G 0.507500 0.4762 0.8720  
3   1  rs1878052 4452662  A 0.4885 0.4605  G 0.347700 0.5554 1.1190  
4   1  rs2071999 4673126  C 0.2907 0.3212  A 0.476300 0.4901 0.8661  
5   1 rs10915297 4910002  C 0.4425 0.4459  T 0.005182 0.9426 0.9863  
\> res1 \<- res1\[order(res1$P),\]  
\> res1\[1:5,\]  
     CHR        SNP        BP A1     F\_A     F\_U A2  CHISQ         P      OR  
2888   8  rs4571722  60326734  T 0.03448 0.49670  C 106.90 4.695e-25 0.03619  
1665   4 rs10008252 179853616  G 0.14370 0.48030  A  54.56 1.505e-13 0.18160  
3976  12  rs2881297  27298922  T 0.15520 0.36510  C  23.76 1.094e-06 0.31940  
1242   3   rs798658 150318957  G 0.34480 0.17110  A  18.63 1.590e-05 2.55100  
5367  19  rs7254875  61010979  T 0.17240 0.05592  C  16.94 3.861e-05 3.51700  
\> head(res1)  
     CHR        SNP        BP A1     F\_A     F\_U A2  CHISQ         P      OR  
2888   8  rs4571722  60326734  T 0.03448 0.49670  C 106.90 4.695e-25 0.03619  
1665   4 rs10008252 179853616  G 0.14370 0.48030  A  54.56 1.505e-13 0.18160  
3976  12  rs2881297  27298922  T 0.15520 0.36510  C  23.76 1.094e-06 0.31940  
1242   3   rs798658 150318957  G 0.34480 0.17110  A  18.63 1.590e-05 2.55100  
5367  19  rs7254875  61010979  T 0.17240 0.05592  C  16.94 3.861e-05 3.51700  
4914  16 rs10459871  79650568  G 0.20110 0.07566  C  16.35 5.278e-05 3.07600

\> install.packages("qqman", repos \= "http://cran.utstat.utoronto.ca/")  
\> library("qqman")

\> \#Plot Manhattan and QQ plots for the adjusted association results  
\> forplot \<- res1\[, c("SNP", "CHR", "BP", "P")\]  
\> pdf("GWAS\_Manhattan.pdf")  
\> manhattan(forplot,chr="CHR",bp="BP",p="P",snp="SNP", main \= "Manhattan Plot")  
\> dev.off()  
null device  
          1  
\> pdf("GWAS\_QQplot.pdf")  
\> qq(res1$P, main="Q-Q Plot of GWAS p-values")  
\> dev.off()  
null device  
          1

