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


