# 02-1 ADMIXTURE analysis
### Tutorial: https://dalexander.github.io/admixture/admixture-manual.pdf

## Pruning
```bash
plink --bfile ${geno} --hwe 0.00001 --maf 0.05 --indep-pairwise 50 10 0.1 --make-bed --out ${geno_pruned}

plink --bfile ${geno} --extract ${geno_pruned}.prune.in --make-bed --out ${geno_pruned}
```

## Unsurpervised ADMIXTURE analysis
```bash
for K in ${1..8};
do
	admixture --cv ${geno_pruned}.bed $K
done
```
### View the CV errors and the lowest one is a sensible modeling choice.


## Surpervised ADMIXTURE anlaysis
### According the reference group, prepare the .pop file
### the .pop file shoud be consistent with .fam file

```bash
## K the number of reference population groups

plink --bfile ${geno} --hwe 0.00001 --maf 0.05 --indep-pairwise 50 10 0.1 --make-bed --out ${geno_pruned}
plink --bfile ${geno} --extract ${geno_pruned}.prune.in --make-bed --out ${geno_pruned}

admixture {geno_pruned}.bed $k -j${thread_num}


```
 