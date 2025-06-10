# 03 PyPop analysis
### Tutorial: http://pypop.org/

## install PyPop
```bash
pip install pypop-genomics
```

## Run PyPop
```bash
pypop -c config.ini {input}.pop
```

## config.ini for PyPop
```bash
[General]
debug=0

[ParseGenotypeFile]
untypedAllele=****
alleleDesignator=*
validSampleFields=*A_1
 *A_2
 *B_1
 *B_2
 *C_1
 *C_2
 *DQA1_1
 *DQA1_2
 *DQB1_1
 *DQB1_2
 *DRB1_1
 *DRB1_2
 *DPA1_1
 *DPA1_2
 *DPB1_1
 *DPB1_2

[HardyWeinberg]
lumpBelow=5

[HardyWeinbergGuoThompson]
dememorizationSteps=2000
samplingNum=1000
samplingSize=1000


[HomozygosityEWSlatkinExact]
numReplicates=10000

[Emhaplofreq]
lociToEstHaplo=b:c,drb1:dqa1:dqb1,dpa1:dpb1
allPairwiseLD=1
allPairwiseLDWithPermu=1000
;;numPermuInitCond=5
```
