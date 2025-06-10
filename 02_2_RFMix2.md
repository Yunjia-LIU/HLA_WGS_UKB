# 02-2 RFMix2 analysis
### Tutorial: https://github.com/slowkoni/rfmix

## extract the MHC region
```bash
plink --bfile ${geno_c6} --chr 6 --from-bp 28477797 --to-bp 33448354 --make-bed --out ${geno_mhc}
```

## Phasing using BEAGLE
### BEAGLE:https://faculty.washington.edu/browning/beagle/beagle.html
```bash
java -Xmx102400m -jar beagle.06Aug24.fce.jar \
  gt={ukb_c6_mhc}.vcf.gz \
  ref=chr6.1kg.phase3.v5a.vcf.gz \
  map=./plink_GRch37_map/plink.chr6.GRCh37.map \
  chrom=6 \
  out={ukb_c6_mhc}.beagle_phased \
  impute=false
```

## RFMix v2 analysis
```bash
./rfmix -f {ukb_c6_mhc}.beagle_phased.vcf \
	-r chr6.1kg.phase3.v5a.vcf.gz \
	-g genetic_map_chr6_combined_b37.txt \
	-m {ukb_c6_mhc}.pop \
	-o {ukb_c6_mhc}.rfmix_res --chromosome=6
```
Note: {ukb_c6_mhc}.pop: sample map file ("Sample_ID" "Subpopulation")
