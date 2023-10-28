# EP2_somatico
Trabalho EP2 Somático

#Workflow
1. Instalar os arquivos (sratoolkit, parallel-fastq-dump e vdb-config);
2. Baixar o arquivo FASTQ;
3. Baixar referências;
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

# 1
## instalar utilizando o brew install (gitpod)
brew install sratoolkit
