# Metagenomik uygulaması

Bu deneme kapsamında `metaphlan3` aracı kapsamında insan mikrobiyota örneklerini inceleyeceğiz. Önce araçları nasıl kullanacağımıza bakalım. İlk olarak `metaphlan3` aracını kullancağız.

Aşağıdaki satırları yazarak kullanacağımız araçların yolunu belirleyelim:

```bash
source ~/../egitim/.bashrc
shopt -sq expand_aliases
```

`Metaphlan3` programının kullanım kılavuzunu görüntüleyelim:

```bash
metaphlan -h
```

Veri tabanına bakalım:

```
ls /truba/home/egitim/Emrah/Databases/mpa_v30_CHOCOPhlAn_201901/
```

Fasta dosyasına bakalım:

```
less /truba/home/egitim/Emrah/Databases/mpa_v30_CHOCOPhlAn_201901/mpa_v30_CHOCOPhlAn_201901.fna
```

Bir biyoinformatik proje dosyası oluşturalım. Kendi projenizin ismini belirleyin. Ben `Metagenomik` ismini kullancağım:

```
mkdir Metagenomik
cd Metagenomik
```

Şimdi de bu klasör içinde verilerimizi depolayacağımız ve sonuçları depolayacağımız klasörleri oluşturalım:

```
mkdir data
mkdir results

```

Bu kısımda kullancağımız dosyalar bu klasörde:

```
/truba/home/egitim/Emrah/HandsOn_Metaphlan3
```

Şimdi de, kullanacağımız slurm dosyasını kopyalayalım ve içine bakalım:


```
cp /truba/home/egitim/Emrah/HandsOn_Metaphlan3/metaphlan2.sh .

cat metaphlan2.sh
```

Şimdi de kullancağımız dosyaları kopyalayalım:

```
cp /truba/home/egitim/Emrah/HandsOn_Metaphlan3/data/*.gz data/

ls data/
```


Şimdi sadece bir tane dosyayı metaphlan3 ile inceleyelim:


```
sbatch metaphlan2.sh
```

Diğer dosyaları da tek tek profilleyelim:

```
cp /truba/home/egitim/Emrah/HandsOn_Metaphlan3/metaphlan_all.sh .
```

Şimdi farklı profilleri birleştirelim

```
merge_metaphlan_tables.py results/*profile.txt > merged_abundance_table.txt
```

Acaba oldu mu?

```

less -S merged_abundance_table.txt
```

Sadece tür profillerini alsak?

```
grep -E "s__|clade" merged_abundance_table.txt | sed 's/^.*s__//g'\ | cut -f1,3-8 | sed -e 's/clade_name/body_site/g' > merged_abundance_table_species.txt
```

Heatmap oluşturma:

```
 hclust2.py -i merged_abundance_table_species.txt -o abundance_heatmap_species.png --f_dist_f braycurtis --s_dist_f braycurtis --cell_aspect_ratio 0.5 -l --flabel_size 10 --slabel_size 10 --max_flabel_len 100 --max_slabel_len 100 --dpi 300
```