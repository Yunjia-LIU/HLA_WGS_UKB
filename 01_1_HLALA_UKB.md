# 01-1 Run HLA*LA using UKB WGS data
## In UK Biobank Research Analysis Platform (DNANexus)

## Build docker for HLA*LA
```bash
dx ssh job-GPZ63f8JP3bzBk55y20kvvFV ## DNAnexus Cloud Workstation
wget https://github.com/DiltheyLab/HLA-LA/archive/refs/heads/master.zip
mv master.zip HLA-LA-master.zip
wget https://github.com/mdaya/hla_la_typing/blob/master/type_hla.sh
sudo docker build -t hla_la:0.3.2 ./ ## build the docker of HLA-LA
sudo docker run -dit hla_la:0.3.2
docker ps
sudo docker exec -it e90798e63a6f /bin/bash

## homo 38 ref
wget https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/Homo_sapiens_assembly38.fasta
wget https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/Homo_sapiens_assembly38.fasta.fai 
wget https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/Homo_sapiens_assembly38.ref_cache.tar.gz
tar -zxvf Homo_sapiens_assembly38.ref_cache.tar.gz

## IMGT ref
cd /usr/local/bin/HLA-LA/graphs
wget http://www.well.ox.ac.uk/downloads/PRG_MHC_GRCh38_withIMGT.tar.gz
tar -xvzf PRG_MHC_GRCh38_withIMGT.tar.gz
rm PRG_MHC_GRCh38_withIMGT.tar.gz
## Index the graph
../bin/HLA-LA --action prepareGraph --PRG_graph_dir ../graphs/PRG_MHC_GRCh38_withIMGT

#Run the pipeline on one sample in order to create indexes for the files in graphs/PRG_MHC_GRCh38_withIMGT/extendedReferenceGenome
wget https://www.dropbox.com/s/xr99u3vqaimk4vo/NA12878.mini.cram?dl=0 
mv NA12878.mini.cram?dl=0  NA12878.mini.cram
samtools index NA12878.mini.cram
./HLA-LA.pl --BAM NA12878.mini.cram --graph PRG_MHC_GRCh38_withIMGT --sampleID NA12878 --maxThreads 7 

## delete the sample file, and save the docker
sudo docker commit
sudo docker save e90798e63a6f hla:0.3.3
docker images
sudo docker save hla:0.3.3 > hla_0.4.0.tar.gz

```

## Run HLA*LA(via Cloud Workstation)
```bash
## Pipeline for a ukb WGS sample
dx ssh ${job_id}
sudo docker load -i hla_0.4.0.tar.gz
dx download {cram_file}
sudo docker run -v /home/dnanexus:/data hla_la:0.3.3 samtools index /data/{cram_file}
sudo docker run -v /home/dnanexus:/data hla_la:0.3.3 HLA-LA.pl --BAM /data/{cram_file} --graph PRG_MHC_GRCh38_withIMGT --sampleID {sample_id} --maxThreads 30 --workingDir /data/
cp ./{sample_id}/hla/R1_bestguess_G.txt {sample_id}_R1_bestguess_G.txt
cp ./{sample_id}/hla/R1_bestguess.txt {sample_id}_R1_bestguess.txt
dx upload ./{sample_id}_R1_bestguess* --path {project_id}:/HLA_LA/
```