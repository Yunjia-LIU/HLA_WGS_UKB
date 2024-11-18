# 01-2 Run Kourami using AoU WGS data
## In All of Us Research Wrokbench

## Preprocessing for Kourami
### Tutorial: https://github.com/Kingsford-Group/kourami

## Download CRAM files and Run Kourami
```python
import os
from datetime import datetime
start = datetime.now()
```

```python
bucket = os.getenv("WORKSPACE_BUCKET")
bucket
```

```python
%%bash
HOME=`pwd`
CRAM_FILE=$HOME/cram_id.list
DB=$HOME/kourami/db
```

```python
%%bash
cat ${CRAM_FILE} | while read LINE
do
sampleid="$(cut -d',' -f1 <<<${LINE})"
cramfile="$(cut -d',' -f2 <<<${LINE})"
cramfile_index="$(cut -d',' -f3 <<<${LINE})"

## Download CRAM file
gsutil -m -u $GOOGLE_PROJECT cp ${cramfile} ${HOME}
gsutil -m -u $GOOGLE_PROJECT cp ${cramfile_index} ${HOME}
echo "Finish Downloading"

## Read Extraction and input bam generation from GRCh38 bam
${HOME}/kourami/scripts/alignAndExtract_hs38DH.sh ${sampleid} ${sampleid}.cram

## Run Kourami
java -jar ${HOME}/kourami/Kourami.jar -o ${sampleid} -d ${DB} -a ${sampleid}_on_KouramiPanel.bam

done
echo "Script Finished"
```