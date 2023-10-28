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
8. criar arquivo fasta do cromossomo 9;
9. Fazer o alinhamento (Combinar com pipes: bwa + samtools view e sort);
10. Instalar bedtools;
11. BAMtoBED,merge e sort;
12. Cobertura média;
13. Filtragem por reads >=20;
14. Instalar GATK (MuTect2 com Pon);
15. Descompactar GATK;
16. Gerar arquivo .dict;

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
# 9 - Fazer o alinhamento (Combinar com pipes: bwa + samtools view e sort
```bash
NOME=WP312; Biblioteca=Nextera; Plataforma=illumina;
bwa mem -t 10 -M -R "@RG\tID:$NOME\tSM:$NOME\tLB:$Biblioteca\tPL:$Plataforma" chr9.fa SRR8856724_1.fastq.gz SRR8856724_2.fastq.gz | samtools view -F4 -Sbu -@2 - | samtools sort -m4G -@2 -o WP312_sorted.bam
```






