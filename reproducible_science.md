# Tekrarlanabilir bilim

Conda kurmak için önce şu bağlantıdaki dosyayı indiriniz:

```
wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh
```



sonra çalıştırınız:

```
bash Miniconda3-latest-Linux-x86_64.sh
```


Yeni bir çalışma çevresi oluşturalım:

```
conda create -n hizalama

conda activate hizalama

conda install -c bioconda snakemake bwa samtools bcftools
```