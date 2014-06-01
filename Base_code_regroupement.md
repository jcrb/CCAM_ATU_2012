Fichier "Base dans code regroupement"
========================================================


```r
path <- "~/Documents/Resural/FEDORU/CCAM_ATU_2012_Data"
file <- "base_dans_code_regroupement.csv"
path.data <- paste(path, file, sep = "/")
d.base <- read.csv2(path.data, header = TRUE, fileEncoding = "UTF-8")
libelle <- c("REGION", "FINESS", "ETABLISSEMENT", "ANNEE", "TYPEDOS", "ACTE","CCAM","LIBELLE.ACTE","NB.ACTES","TARIF","MT.FACTURE","NB.ATU","FREQ.1000.ATU")
names(d.base) <- libelle

d.base$FREQ.1000.ATU <- as.numeric(d.base$FREQ.1000.ATU)
d.base$REGION <- as.factor(d.base$REGION)
d.base$TARIF <- as.numeric(gsub(",", ".", d.base$TARIF))
```

```
## Warning: NAs introduits lors de la conversion automatique
```

```r
d.base$MT.FACTURE <- as.numeric(gsub(",", ".", d.base$MT.FACTURE))
```

```
## Warning: NAs introduits lors de la conversion automatique
```

```r
str(d.base)
```

```
## 'data.frame':	94593 obs. of  13 variables:
##  $ REGION       : Factor w/ 27 levels "11","12","21",..: 19 19 19 19 19 19 19 19 19 19 ...
##  $ FINESS       : Factor w/ 450 levels "100000017","100006279",..: 3 3 3 3 3 3 3 3 3 3 ...
##  $ ETABLISSEMENT: Factor w/ 897 levels "ALPHA SANTE",..: 213 213 213 213 213 213 213 213 213 213 ...
##  $ ANNEE        : int  2012 2012 2012 2012 2012 2012 2012 2012 2012 2012 ...
##  $ TYPEDOS      : logi  NA NA NA NA NA NA ...
##  $ ACTE         : Factor w/ 10 levels "0","ACO","ADA",..: 7 6 6 6 6 6 4 6 7 6 ...
##  $ CCAM         : Factor w/ 1997 levels "AAFA001","AAGB001",..: 357 1267 1828 1844 1468 1426 1691 1227 1495 1302 ...
##  $ LIBELLE.ACTE : Factor w/ 1979 levels "","Ablation de broche d'ostéosynthèse enfouie, par voie transcutanée sans guidage",..: 569 1418 1456 1406 1413 1465 1270 1410 314 1441 ...
##  $ NB.ACTES     : int  1327 1017 957 810 630 600 459 364 333 331 ...
##  $ TARIF        : num  13.5 19.9 21.3 19.9 19.9 ...
##  $ MT.FACTURE   : num  17941 20289 20365 16160 12568 ...
##  $ NB.ATU       : int  13769 13769 13769 13769 13769 13769 13769 13769 13769 13769 ...
##  $ FREQ.1000.ATU: num  1109 925 883 775 639 ...
```

```r
total.facture <- sum(d.base$MT.FACTURE, na.rm=TRUE) # 224 254 885 €
```

