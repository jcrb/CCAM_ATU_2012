CCAM_Analyse
========================================================

Initialisation.
---------------

Pour des raisons de confidentialité, les données sources ne sont pas accessibles dans ce répertoire mais restent privées.
Chemin d'accès local:

```r
path <- "~/Documents/Resural/FEDORU/CCAM_ATU_2012_Data"

# file <- 'table_ccam.csv' d <- read.csv2(path.data, header = TRUE, skip =
# 1, fileEncoding = 'UTF-8')
```

Analyse du fichier ATU_2012
---------------------------


```
## 'data.frame':	450 obs. of  4 variables:
##  $ FINESS       : Factor w/ 450 levels "100000017","100006279",..: 3 4 5 54 55 56 57 58 59 103 ...
##  $ ETABLISSEMENT: Factor w/ 450 levels "ALPHA SANTE ",..: 107 19 37 381 378 382 45 374 387 391 ...
##  $ NBRE.D.ATU   : int  13769 22477 10385 36855 27050 33894 13304 21591 10716 23251 ...
##  $ REGION       : int  82 82 82 22 22 22 22 22 22 83 ...
```

```
##    Région nb de SU
## 1      11       44
## 2      12        8
## 3      21       12
## 4      22       16
## 5      23       12
## 6      24       21
## 7      25       20
## 8      26       19
## 9      31       27
## 10     41       21
## 11     42       11
## 12     43        9
## 13     52       19
## 14     53       21
## 15     54       17
## 16     72       20
## 17     73       26
## 18     74        8
## 19     82       43
## 20     83       13
## 21     91       12
## 22     93       35
## 23     94        2
## 24   9701        4
## 25   9702        3
## 26   9703        3
## 27   9704        4
```

```
##  [1] HOPITAUX UNIVERSITAIRES DE STRASBOURG          
##  [2] GROUPE HOSPITALIER SAINT-VINCENT DE STRASBOURG 
##  [3] CENTRE HOSPITALIER DE HAGUENAU                 
##  [4] CENTRE HOSPITALIER DE SAVERNE                  
##  [5] CENTRE HOSPITALIER  WISSEMBOURG                
##  [6] CENTRE HOSPITALIER DE SELESTAT                 
##  [7] CTRE HOSPIT. ST-MORAND  ALTKIRCH               
##  [8] CENTRE HOSPITALIER DE THANN                    
##  [9] CENTRE HOSPITALIER DE MULHOUSE                 
## [10] CENTRE HOSPITALIER DE COLMAR                   
## [11] CENTRE HOSPITALIER DE GUEBWILLER               
## 11 Levels: CENTRE HOSPITALIER DE COLMAR  ...
```



Analyse du fichier base_ccam_2012
-------------------------------------


```
## [1] "FINESS"            "Libellé"           "Code.CCAM"        
## [4] "Effectif.avec.ATU" "code.acte"
```

```
## 'data.frame':	94143 obs. of  5 variables:
##  $ FINESS           : Factor w/ 447 levels "010008407","010780054",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ Libellé          : Factor w/ 447 levels "ALPHA SANTE",..: 107 107 107 107 107 107 107 107 107 107 ...
##  $ Code.CCAM        : Factor w/ 1997 levels "AAFA0010 EXÉRÈSE T. INTRAPARENCH CERVELET CRANIOT.",..: 357 1268 1828 1844 1469 1427 1691 1228 1495 1303 ...
##  $ Effectif.avec.ATU: num  1327 1017 957 810 630 ...
##  $ code.acte        : Factor w/ 1996 levels "AAFA001","AAGB001",..: 357 1267 1827 1843 1468 1426 1690 1227 1494 1302 ...
```

```
## DEQP003 ZBQK002 MDQK001 NGQK001 NDQK001 ZCQK002 NFQK001 MGQK003 MAQK003 
## 1104744  792252  486803  351125  310184  250392  204699  201511  192485 
## QZJA002 
##  182411
```

En 2012, __8.2274 &times; 10<sup>6</sup>__ actes ont été réalisés au service des urgences de __447__ établissements de santé.

Merging d et d.atu
-------------------

On peut merger les dataframe __d__ et __d.atu__ sur la colonne __FINESS__ pour récupérer l'information région que ne possède pas d. Cependant on constate qu'un certain nombre d'établissements disparaissent avec un merging simple:

```r
merge1 <- merge(d, d.atu, by.x = "FINESS", by.y = "FINESS")
merge1 <- merge1[, -6]  # supprime la colonne ETABLISSEMENT redondante avec Libellé
actes.region <- tapply(merge1$Effectif.avec.ATU, merge1$REGION, sum)  # région 94 (Corse)?
actes.region
```

```
##      11      12      21      22      23      24      25      26      31 
## 1341687   97609  107911  233557  241087  346843  262406  204879  547207 
##      41      42      43      52      53      54      72      73      74 
##  299870  189523  158651  396582  416252  272153  325620  314201   85119 
##      82      83      91      93      94    9701    9702    9703    9704 
##  772976  111161  241983  558341      NA   38273   30174   20053   72036
```

```r
sum(actes.region, na.rm = TRUE)  # à comparer à n.atu
```

```
## [1] 7686154
```

```r

boxplot(merge1$Effectif.avec.ATU ~ merge1$REGION, outline = FALSE)  # interprétation ?
```

![plot of chunk merge1](figure/merge1.png) 

Il n'y a que 86762 lignes contre 94143 attendues.

Si on impose toutes les lignes de d:

```r
merge2 <- merge(d, d.atu, by.x = "FINESS", by.y = "FINESS", all.x = TRUE)
merge2 <- merge2[, -6]  # supprime la colonne ETABLISSEMENT redondante avec Libellé
```

Croisement actes / région
-------------------------


```r
a <- tapply(merge1$Effectif.avec.ATU, list(merge1$code.acte, merge1$REGION), 
    sum)
str(a)
```

```
##  num [1:1996, 1:27] NA 1 1 NA NA NA 50 23 NA 413 ...
##  - attr(*, "dimnames")=List of 2
##   ..$ : chr [1:1996] "AAFA001" "AAGB001" "AAJA001" "AAJA004" ...
##   ..$ : chr [1:27] "11" "12" "21" "22" ...
```

```r
a["DEQP003", ]
```

```
##     11     12     21     22     23     24     25     26     31     41 
## 195642  17538  17742  34311  33832  46364  32889  32414  79471  40268 
##     42     43     52     53     54     72     73     74     82     83 
##  27821  25671  49733  48340  35288  56253  40730   8801  90903   9787 
##     91     93     94   9701   9702   9703   9704 
##  33142  66691     NA   5559   4660   3206   9521
```

```r
sum(a["DEQP003", ], na.rm = TRUE)
```

```
## [1] 1046577
```

On obtient une matrice de 27 colonnes et 1996 lignes. A standardiser sur 10.000 ATU

Base dans code regroupement
---------------------------


```r
file <- "base_dans_code_regroupement.csv"
path.data <- paste(path, file, sep = "/")
d.base <- read.csv2(path.data, header = TRUE, fileEncoding = "UTF-8")

d.base$fréquence.pour.10000.ATU <- as.numeric(d.base$fréquence.pour.10000.ATU)
d.base$REGION <- as.factor(d.base$REGION)
d.base$tarif <- as.numeric(gsub(",", ".", d.base$tarif))
```

```
## Warning: NAs introduits lors de la conversion automatique
```

```r
d.base$X.MtFact..... <- as.numeric(gsub(",", ".", d.base$X.MtFact.....))
```

```
## Warning: NAs introduits lors de la conversion automatique
```

```r

str(d.base)
```

```
## 'data.frame':	94593 obs. of  13 variables:
##  $ REGION                  : Factor w/ 27 levels "11","12","21",..: 19 19 19 19 19 19 19 19 19 19 ...
##  $ FINESS.                 : Factor w/ 450 levels "100000017","100006279",..: 3 3 3 3 3 3 3 3 3 3 ...
##  $ ETABLISSEMENT           : Factor w/ 897 levels "ALPHA SANTE",..: 213 213 213 213 213 213 213 213 213 213 ...
##  $ ANNEE                   : int  2012 2012 2012 2012 2012 2012 2012 2012 2012 2012 ...
##  $ X.Typedos.              : logi  NA NA NA NA NA NA ...
##  $ X.Acte.....             : Factor w/ 10 levels "0","ACO","ADA",..: 7 6 6 6 6 6 4 6 7 6 ...
##  $ CCAM                    : Factor w/ 1997 levels "AAFA001","AAGB001",..: 357 1267 1828 1844 1468 1426 1691 1227 1495 1302 ...
##  $ libellé                 : Factor w/ 1979 levels "","Ablation de broche d'ostéosynthèse enfouie, par voie transcutanée sans guidage",..: 569 1418 1456 1406 1413 1465 1270 1410 314 1441 ...
##  $ X.NbActes....           : int  1327 1017 957 810 630 600 459 364 333 331 ...
##  $ tarif                   : num  13.5 19.9 21.3 19.9 19.9 ...
##  $ X.MtFact.....           : num  17941 20289 20365 16160 12568 ...
##  $ nombre.d.ATU            : int  13769 13769 13769 13769 13769 13769 13769 13769 13769 13769 ...
##  $ fréquence.pour.10000.ATU: num  1109 925 883 775 639 ...
```

```r

total.facture <- sum(d.base$X.MtFact....., na.rm = TRUE)  # 224 254 885 €
```


#### Code ECG

Ce fichier comporte une colonne intitulée __fréquence pour 10000 ATU__

```r

ecg <- d.base[d.base$CCAM == "DEQP003", c("fréquence.pour.10000.ATU", "REGION", 
    "nombre.d.ATU")]
nrow(ecg)
```

```
[1] 444
```

```r
summary(ecg)
```

```
 fréquence.pour.10000.ATU     REGION     nombre.d.ATU   
 Min.   :   4             11     : 44   Min.   :   750  
 1st Qu.: 226             82     : 43   1st Qu.: 11361  
 Median : 442             93     : 35   Median : 18062  
 Mean   : 558             31     : 27   Mean   : 25821  
 3rd Qu.: 958             73     : 26   3rd Qu.: 31350  
 Max.   :1132             53     : 21   Max.   :746755  
                          (Other):248                   
```

```r
summary(ecg$fréquence.pour.10000.ATU)
```

```
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
      4     226     442     558     958    1130 
```

```r
n.atu <- sum(ecg$nombre.d.ATU)
n.atu
```

```
[1] 11464364
```


Pour chaque région on calcule un boxplot. On range les boxplot par médiane croissante. On utilise pour cela la méthode __reorder__ qui admet trois variables: la fréquence pour 10000 ATU partitionée par REGION et une fonction, la médiane.

```r
bymedian <- with(ecg, reorder(REGION, fréquence.pour.10000.ATU, median))
boxplot(fréquence.pour.10000.ATU ~ bymedian, data = ecg, ylab = "nombre d'ECG", 
    xlab = "Régions", main = "Nombre d'ECG (DEQP003) pour 10.000 ATU par région", 
    col = "lightgray", las = 2)
m <- mean(ecg$fréquence.pour.10000.ATU)
abline(h = m, col = "red", lty = 2)
```

![plot of chunk ecg_médiane](figure/ecg_médiane.png) 


