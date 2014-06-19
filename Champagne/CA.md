Champagne-Ardenne
========================================================

Demande d'extraction de la Champagne-Ardennes (CA)


```r
path <- "~/Documents/Resural/FEDORU/CCAM_ATU_2012_Data"
file <- "base_dans_code_regroupement.csv"
path.data <- paste(path, file, sep = "/")
d.base <- read.csv2(path.data, header = TRUE, fileEncoding = "UTF-8")

ca <- d.base[d.base$REGION == 21,]
save(ca, file="CCAM_Champ_Ardennes_2012.csv")
write.table(ca, file="CCAM_Champ_Ardennes_2012.csv", sep=";")

file <- "atu_2012.csv"
path.data <- paste(path, file, sep = "/")
d.atu <- read.csv2(path.data, header = TRUE, fileEncoding = "UTF-8")
ca.atu <- d.atu[d.atu$REGION == 21,]
write.table(ca.atu, file="ATU_Champ_Ardennes_2012.csv", sep=";")
```

