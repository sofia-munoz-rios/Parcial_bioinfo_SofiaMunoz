# Parcial_bioinfo_SofiaMunoz
# Parcial Bioinformática

## Nombre

Sofía Muñoz Rios

---
# Punto 3. Gráficas en R

## Código utilizado

```R
library(ggplot2)

ggplot(datos,
aes(x=fecha,y=casos))+
geom_line()

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



