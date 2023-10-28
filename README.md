# EP2_somatico
Trabalho EP2 Somático

# Workflow:
1. Instalar os arquivos (sratoolkit, parallel-fastq-dump e vdb-config);
2. Baixar R1 e R2;
3. Instalar BWA;
4. Baixar cromossomo 9;
5. Descompactar o arquivo;
6. BWA index para o arquivo;
7. Instalar samtools;
8. Criar arquivo FASTA do cromossomo 9;
9. Fazer o alinhamento (Combinar com pipes: bwa + samtools view e sort);
10. Remover duplicata de PCR;
11. Instalar o bedtools;
12. BAMtoBED,merge e sort;
13. Cobertura média;
14. Filtragem por reads >=20;
15. Baixar as referências (Pon e Gnomad);
16. Instalar GATK (MuTect2 com PoN);
17. Descompactar GATK;
18. Gerar arquivo .dict;
19. Gerar interval_list do chr9;
20. Converter Bed para Interval_list;
21. GATK4 - CalculateContamination;
22. GATK4 - MuTect2 Call;
23. GATK4 - MuTect2 FilterMutectCalls;
24. 

# 1- Instalar utilizando o brew install (gitpod)
```bash
brew install sratoolkit
```

# 1.1 - Instalar parallel fastq-dump
```bash
pip install parallel-fastq-dump
```

# 1.2 -Instalar vdb-config
```bash
wget -c https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/3.0.0/sratoolkit.3.0.0-ubuntu64.tar.gz
tar -zxvf sratoolkit.3.0.0-ubuntu64.tar.gz
export PATH=$PATH://workspace/somaticoEP1/sratoolkit.3.0.0-ubuntu64/bin/
echo "Aexyo" | sratoolkit.3.0.0-ubuntu64/bin/vdb-config
```

# 2 - Baixar R1 e R2
```bash
time parallel-fastq-dump --sra-id SRR8856724 \
--threads 10 \
--outdir ./ \
--split-files \
--gzip
```
# 3 - Instalar BWA
```bash
brew install bwa
```

# 4 - Baixar cromossomo 9
```bash
wget -c https://hgdownload.soe.ucsc.edu/goldenPath/hg19/chromosomes/chr9.fa.gz
```

# 5 - Descompactar o arquivo
```bash
gunzip chr9.fa.gz
```
# 6 - BWA index para o arquivo
```bash
bwa index chr9.fa
```

# 7 - Instalar o samtools
```bash
brew install samtools
```

# 8- Criar arquivo FASTA do cromossomo 9
```bash
samtools faidx chr9.fa
```
# 9 - Fazer o alinhamento (Combinar com pipes: bwa + samtools view e sort)
```bash
NOME=WP312; Biblioteca=Nextera; Plataforma=illumina;
bwa mem -t 10 -M -R "@RG\tID:$NOME\tSM:$NOME\tLB:$Biblioteca\tPL:$Plataforma" chr9.fa SRR8856724_1.fastq.gz SRR8856724_2.fastq.gz | samtools view -F4 -Sbu -@2 - | samtools sort -m4G -@2 -o WP312_sorted.bam
```
# 10 - Remover duplicata de PCR;
```bash
samtools rmdup WP312_sorted.bam WP312_sorted_rmdup.bam
```

# 11 - Instalar o bedtools
```bash
brew install bedtools
```

# 12 - BAMtoBED
```bash
bedtools bamtobed -i WP312_sorted_rmdup.bam > WP312_sorted_rmdup.bed
```
# 12.1 merge
```bash
bedtools merge -i WP312_sorted_rmdup.bed > WP312_sorted_merged.bed
```
# 12.2 - sort
```bash
bedtools sort -i WP312_sorted_merged.bed > WP312_sorted_merged_sorted.bed
```

# 13 - Cobertura média
```bash
bedtools coverage -a WP312_sorted_merged.bed -b WP312_sorted_rmdup.bam -mean > WP312_coverageBed.bed
```

# 14 - Filtragem por reads >=20
```bash
cat WP312_coverageBed.bed | awk -F "\t" '{if($4>=20){print}}' > WP312_coverageBed20x.bed
```

# 15 - Baixar as referências (Pon e Gnomad)
```bash
wget -c https://storage.googleapis.com/gatk-best-practices/somatic-b37/Mutect2-WGS-panel-b37.vcf
```

```bash
wget -c https://storage.googleapis.com/gatk-best-practices/somatic-b37/Mutect2-WGS-panel-b37.vcf.idx
```

```bash
wget -c  https://storage.googleapis.com/gatk-best-practices/somatic-b37/af-only-gnomad.raw.sites.vcf
```

```bash
wget -c  https://storage.googleapis.com/gatk-best-practices/somatic-b37/af-only-gnomad.raw.sites.vcf.idx
```

# 14 - Cobertura média
```bash
bedtools coverage -a WP312_sorted_merged.bed -b WP312_sorted_rmdup.bam -mean > WP312_coverageBed.bed
```

# 15 - Filtragem por reads >=20
```bash
cat WP312_coverageBed.bed | awk -F "\t" '{if($4>=20){print}}' > WP312_coverageBed20x.bed
```

# 16 - Instalar GATK (MuTect2 com PoN)
```bash
wget -c https://github.com/broadinstitute/gatk/releases/download/4.2.2.0/gatk-4.2.2.0.zip
```

# 17 - Descompactar GATK
```bash
unzip gatk-4.2.2.0.zip
```

# 18 - Gerar arquivo .dict
```bash
./gatk-4.2.2.0/gatk CreateSequenceDictionary -R chr9.fa -O chr9.dict
```

# 19 - Gerar interval_list do chr9
```bash
./gatk-4.2.2.0/gatk ScatterIntervalsByNs -R chr9.fa -O chr9.interval_list -OT ACGT
```

# 20 - Converter Bed para Interval_list
```bash
./gatk-4.2.2.0/gatk BedToIntervalList -I WP312_coverageBed20x.bed \
-O WP312_coverageBed20x.interval_list -SD chr9.dict
```

21. GATK4 - CalculateContamination;
22. GATK4 - MuTect2 Call;
23. GATK4 - MuTect2 FilterMutectCalls;







