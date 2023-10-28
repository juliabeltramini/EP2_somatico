# EP2_somatico
Trabalho EP2 Somático

# Workflow:
1. Instalar os arquivos (sratoolkit, parallel-fastq-dump e vdb-config);
2. Baixar referências (PoN, Gnomad;
4. Instalar BWA;
5. Descompactar o arquivo;
6. BWA index para o arquivo;
7. Instalar samtools;
8. Fazer o alinhamento (samtools: fixmate, sort e index);
9. Instalar bedtools;
10. BAMtoBED,merge e sort;
11. Cobertura média;
12. Filtragem por reads >=20;
13. Instalar GATK (MuTect2 com Pon);
14. Descompactar GATK;
15. Gerar arquivo .dict;

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


