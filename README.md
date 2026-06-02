# Parcial_bioinfo_SofiaMunoz
# Parcial Bioinformática

## Nombre

Sofía Muñoz Rios

---
# Punto 3. Gráficas en R

## Código utilizado

```R
library(ggplot2)
```
# Punto 1: Descarga de Referencias y Limpieza 

```wget https://raw.githubusercontent.com/paula-torres/Bioinfo_UR/refs/heads/main/files/muestra_paciente.fasta```

# Se descargaron las secuencias del NCBI según los códigos de acceso
# Se pasaron al cluster con el siguiente código:
```scp -r -i bio.pt.pem -P 53841 /mnt/c/Users/User/Desktop/parcial/sequences* bio.pt@loginpub-hpc.urosario.edu.co:```

Se concatenaron las secuencias de la siguiente forma:
``` cat sequence* > concat.fasta```
Y se modificó el nombre de las secuencias con el siguiente código:

```sed -E '/^>/s/^>([^ ]+).*([A-Z][a-z]+) ([a-z]+).*/>\1_\2_\3/' concat.fasta > concat2.fasta```
Con este código se toma las líneas que empiecen con “>”, luego se agrupa en el primer paréntesis el código inicial, con el “.*” se le dice que vaya hasta la siguiente indicación, luego en el siguiente paréntesis se agrupa el nombre del género al ser mayúsculas y minúsculas, luego se toma en otro grupo el nombre del género y por último se selecciona hasta el final de la línea. Al final se toman los tres grupos, quedando de ese modo: >código_género_especie

Se generó la base de datos tipo blastn de la siguiente forma:
makeblastdb -in concat2.fasta -dbtype nucl -parse_seqids -out parasito -title "datos tenia"
Para el BLAST se hizo lo siguiente:
blastn -query muestra_paciente.fasta -task megablast -db parasito -outfmt 7 -word_size 7 -out blast_parasito -num_threads 1
Al revisar el blast:
# Fields: query acc.ver, subject acc.ver, % identity, alignment length, mismatches, gap opens, q. start, q. end, s. start, s. end, evalue, bit score
# 32 hits found
```muestra_paciente        EU747661.1_Taenia_solium        99.725  363     1       0       1       363     1       363    0.0      665
muestra_paciente        S69013.1_Taenia_solium  99.174  363     3       0       1       363     4       366     0.0    660
muestra_paciente        KX511891.1_Taenia_multiceps     89.356  357     38      0       1       357     25      381    5.71e-130        449
muestra_paciente        KX511891.1_Taenia_multiceps     100.000 10      0       0       127     136     48      57     1.9      19.6
muestra_paciente        KX511891.1_Taenia_multiceps     87.500  16      2       0       98      113     50      65     1.9      19.6
muestra_paciente        KX511891.1_Taenia_multiceps     100.000 9       0       0       298     306     281     273    6.9      17.7
muestra_paciente        KX511891.1_Taenia_multiceps     92.308  13      0       1       180     191     293     305    6.9      17.7
muestra_paciente        KX511892.1_Taenia_multiceps     89.356  357     38      0       1       357     25      381    5.71e-130        449
muestra_paciente        KX511892.1_Taenia_multiceps     100.000 10      0       0       127     136     48      57     1.9      19.6
muestra_paciente        KX511892.1_Taenia_multiceps     87.500  16      2       0       98      113     50      65     1.9      19.6
muestra_paciente        KX511892.1_Taenia_multiceps     100.000 9       0       0       298     306     281     273    6.9      17.7
muestra_paciente        KX511892.1_Taenia_multiceps     92.308  13      0       1       180     191     293     305    6.9      17.7
muestra_paciente        KU324547.1_Taenia_lynciscapreoli        85.994  357     50      0       1       357     13     369      5.87e-110       383
muestra_paciente        KU324547.1_Taenia_lynciscapreoli        92.308  13      0       1       124     136     34     45       6.9     17.7
muestra_paciente        KU324547.1_Taenia_lynciscapreoli        100.000 9       0       0       267     275     74     82       6.9     17.7
muestra_paciente        KU324546.1_Taenia_lynciscapreoli        85.434  357     52      0       1       357     13     369      1.27e-106       372
muestra_paciente        KU324546.1_Taenia_lynciscapreoli        92.308  13      0       1       124     136     34     45       6.9     17.7
muestra_paciente        KU324546.1_Taenia_lynciscapreoli        100.000 9       0       0       267     275     74     82       6.9     17.7
muestra_paciente        KP663662.1_Taenia_saginata      87.342  316     40      0       26      341     1       316    7.65e-104        363
muestra_paciente        KP663662.1_Taenia_saginata      88.235  17      2       0       98      114     1       17     0.53     21.4
muestra_paciente        KR095314.1_Taenia_omissa        84.831  356     54      0       1       356     25      380    9.90e-103        359
muestra_paciente        KR095313.1_Taenia_omissa        84.270  356     56      0       1       356     22      377    2.14e-99 348
muestra_paciente        KJ910295.1_Iran_cytochrome      87.329  292     37      0       66      357     1       292    1.67e-95 335
muestra_paciente        KM538116.1_Taenia_crassiceps    94.118  17      0       1       84      99      360     376    0.041    25.1
muestra_paciente        KM538116.1_Taenia_crassiceps    88.235  17      1       1       228     244     357     372    1.9      19.6
muestra_paciente        KM538116.1_Taenia_crassiceps    100.000 9       0       0       10      18      7       15     6.9      17.7
muestra_paciente        KM538116.1_Taenia_crassiceps    100.000 9       0       0       279     287     205     197    6.9      17.7
muestra_paciente        KM538116.1_Taenia_crassiceps    91.667  12      1       0       21      32      355     366    6.9      17.7
muestra_paciente        KM538116.1_Taenia_crassiceps    100.000 9       0       0       17      25      479     471    6.9      17.7
muestra_paciente        KM538116.1_Taenia_crassiceps    100.000 9       0       0       324     332     498     506    6.9      17.7
muestra_paciente        KM538116.1_Taenia_crassiceps    100.000 9       0       0       215     223     518     526    6.9      17.7
muestra_paciente        KM538116.1_Taenia_crassiceps    100.000 9       0       0       53      61      574     582    6.9      17.7```
# BLAST processed 1 queries

Se observa que laSe observa que la mejor coinciencia es _Taenia solium_, con una identidad del 99.725%.

Concatenación de las secuencias
Se cargó:
```module load muscle/3.8.31 ```
Se concatenaron las secuencias:
```cat concat2.fasta muestra_paciente.fasta > alineamiento.fasta```
Y se realizó el alineamiento con muscle:
```muscle -in alineamiento.fasta -out muscle.fasta```
## Para el árbol de máxima verosimilitud copié el FASconCAT a la carpeta del parcial:
```cp FASconCAT-G_v1.05.1.pl Sofia_mr/Parcial/```

Cargué iqtree:
```module load iqtree/1.6.12``` 




