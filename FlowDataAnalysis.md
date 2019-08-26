Automatic Gating To Singlets
================
Hanne Grosemans

# Load packages

``` r
#source("http://www.bioconductor.org/biocLite.R")
#biocLite("BiocUpgrade")
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   0.8.3     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(openCyto)
```

    ## Loading required package: flowWorkspace

    ## Loading required package: flowCore

    ## 
    ## Attaching package: 'flowCore'

    ## The following object is masked from 'package:tibble':
    ## 
    ##     view

    ## Loading required package: ncdfFlow

    ## Loading required package: RcppArmadillo

    ## Loading required package: BH

    ## Registered S3 method overwritten by 'R.oo':
    ##   method        from       
    ##   throw.default R.methodsS3

``` r
library(flowViz)
```

    ## Loading required package: lattice

    ## 
    ## Attaching package: 'lattice'

    ## The following objects are masked from 'package:ncdfFlow':
    ## 
    ##     densityplot, histogram, xyplot

``` r
library(flowUtils)
library(flowDensity)
```

    ## Warning: replacing previous import 'flowCore::plot' by 'graphics::plot'
    ## when loading 'flowDensity'

``` r
library(ggplot2)
library(dplyr)
library(ggcyto)
library(flowWorkspace)
```

\#Load fcs file

``` r
fcsFile <- list.files(path = "FCS/", pattern = ".fcs")
fs <- read.flowSet(fcsFile, path = "FCS/")
fcsFile <- read.FCS("FCS/WT.fcs")
```

\#Explore data

``` r
fcsFile
```

    ## flowFrame object 'WT.fcs'
    ## with 50000 cells and 34 observables:
    ##                      name                       desc  range   minRange
    ## $P1                  Time                       <NA> 262144    0.00000
    ## $P2                 FSC-A                      FSC-A 262144    0.00000
    ## $P3                 FSC-H                      FSC-H 262144    0.00000
    ## $P4                 FSC-W                      FSC-W 262144    0.00000
    ## $P5       YellowGreen_D-A            YellowGreen_D-A 262144  -38.06389
    ## $P6       YellowGreen_D-H            YellowGreen_D-H 262144    0.00000
    ## $P7       YellowGreen_D-W            YellowGreen_D-W 262144    0.00000
    ## $P8       PE-CF594 (YG)-A CD142 (F3) PE-CF594 (YG)-A 262144 -111.00000
    ## $P9       PE-CF594 (YG)-H CD142 (F3) PE-CF594 (YG)-H 262144    0.00000
    ## $P10      PE-CF594 (YG)-W CD142 (F3) PE-CF594 (YG)-W 262144    0.00000
    ## $P11      YellowGreen_B-A            YellowGreen_B-A 262144 -111.00000
    ## $P12      YellowGreen_B-H            YellowGreen_B-H 262144    0.00000
    ## $P13      YellowGreen_B-W            YellowGreen_B-W 262144    0.00000
    ## $P14      YellowGreen_A-A            YellowGreen_A-A 262144  -42.73840
    ## $P15      YellowGreen_A-H            YellowGreen_A-H 262144    0.00000
    ## $P16      YellowGreen_A-W            YellowGreen_A-W 262144    0.00000
    ## $P17             SB436*-A              SCA1 SB436*-A 262144 -106.91323
    ## $P18             SB436*-H              SCA1 SB436*-H 262144    0.00000
    ## $P19             SB436*-W              SCA1 SB436*-W 262144    0.00000
    ## $P20           Violet_A-A                 Violet_A-A 262144 -111.00000
    ## $P21           Violet_A-H                 Violet_A-H 262144    0.00000
    ## $P22           Violet_A-W                 Violet_A-W 262144    0.00000
    ## $P23                SSC-A                      SSC-A 262144    0.00000
    ## $P24                SSC-H                      SSC-H 262144    0.00000
    ## $P25                SSC-W                      SSC-W 262144    0.00000
    ## $P26               FITC-A           LIN Sytox FITC-A 262144 -111.00000
    ## $P27               FITC-H           LIN Sytox FITC-H 262144    0.00000
    ## $P28               FITC-W           LIN Sytox FITC-W 262144    0.00000
    ## $P29             Blue_A-A                   Blue_A-A 262144 -111.00000
    ## $P30             Blue_A-H                   Blue_A-H 262144    0.00000
    ## $P31             Blue_A-W                   Blue_A-W 262144    0.00000
    ## $P32            IndexSort                       <NA> 262144    0.00000
    ## $P33            DeltaTime                       <NA> 262144    0.00000
    ## $P34 RegionClassification                       <NA> 262144    0.00000
    ##      maxRange
    ## $P1    262143
    ## $P2    262143
    ## $P3    262143
    ## $P4    262143
    ## $P5    262143
    ## $P6    262143
    ## $P7    262143
    ## $P8    262143
    ## $P9    262143
    ## $P10   262143
    ## $P11   262143
    ## $P12   262143
    ## $P13   262143
    ## $P14   262143
    ## $P15   262143
    ## $P16   262143
    ## $P17   262143
    ## $P18   262143
    ## $P19   262143
    ## $P20   262143
    ## $P21   262143
    ## $P22   262143
    ## $P23   262143
    ## $P24   262143
    ## $P25   262143
    ## $P26   262143
    ## $P27   262143
    ## $P28   262143
    ## $P29   262143
    ## $P30   262143
    ## $P31   262143
    ## $P32   262143
    ## $P33   262143
    ## $P34   262143
    ## 650 keywords are stored in the 'description' slot

``` r
exprs(fcsFile)[1:10,]
```

    ##        Time     FSC-A  FSC-H    FSC-W YellowGreen_D-A YellowGreen_D-H
    ##  [1,] 65560  73248.23  66388 72308.18        52.08743              68
    ##  [2,] 65563  81118.24  73605 72225.61        83.47344             101
    ##  [3,] 65566  93843.24  81072 75859.87       206.34636             144
    ##  [4,] 65567 156827.69 106980 96072.72      1891.17432            1152
    ##  [5,] 65571  91272.77  79860 74901.74       286.48087             292
    ##  [6,] 65572  22784.01  29657 50348.08        54.75858             121
    ##  [7,] 65573  45077.27  51700 57140.89        40.73504              68
    ##  [8,] 65574  38232.46  48609 51546.06        16.69469              37
    ##  [9,] 65575 174569.39 128721 88878.89        96.16141              97
    ## [10,] 65575  60061.82  63078 62402.29       182.30600             198
    ##       YellowGreen_D-W PE-CF594 (YG)-A PE-CF594 (YG)-H PE-CF594 (YG)-W
    ##  [1,]        50200.03       124.20848             288        28264.33
    ##  [2,]        54163.52       453.42773             529        56173.61
    ##  [3,]        93910.52      1065.12109             884        78963.55
    ##  [4,]       107586.80      9799.78223            6128       104803.94
    ##  [5,]        64297.30      1405.02502             976        94343.98
    ##  [6,]        29658.33       347.91733             509        44795.89
    ##  [7,]        39258.99       267.11502             464        37727.70
    ##  [8,]        29570.35        70.78548             181        25629.82
    ##  [9,]        64969.42       538.90454             549        64330.88
    ## [10,]        60341.45      1123.21863            1170        62915.60
    ##       YellowGreen_B-A YellowGreen_B-H YellowGreen_B-W YellowGreen_A-A
    ##  [1,]      -14.691326              38           0.000       30.050440
    ##  [2,]       56.761940              64       58124.227       94.825836
    ##  [3,]       79.466721             120       43399.426       30.050440
    ##  [4,]      473.461365             306      101401.195      212.356445
    ##  [5,]      114.191673             148       50565.305       61.436455
    ##  [6,]       22.704777              59       25220.004       18.030264
    ##  [7,]       11.352388              31       23999.682       12.020176
    ##  [8,]        6.677876              45        9725.361        4.674513
    ##  [9,]       38.731678              58       43764.125       12.020176
    ## [10,]       72.121056             124       38117.141        8.013451
    ##       YellowGreen_A-H YellowGreen_A-W    SB436*-A SB436*-H  SB436*-W
    ##  [1,]              66       29839.178    47.07373       50  61700.48
    ##  [2,]             112       55486.660   145.21051      132  72094.82
    ##  [3,]              76       25912.971  5415.87305     2940 120726.06
    ##  [4,]             210       66271.391 18330.83203     8150 147402.38
    ##  [5,]              64       62910.930   182.70992      160  74837.98
    ##  [6,]              47       25141.094  2530.81152     2591  64013.61
    ##  [7,]              47       16760.730  3493.03076     2459  93094.45
    ##  [8,]              45        6807.753    11.96790       41  19129.96
    ##  [9,]              46       17125.092   160.36984      126  83412.69
    ## [10,]              40       13129.237  1377.10620     1080  83564.84
    ##       Violet_A-A Violet_A-H Violet_A-W      SSC-A  SSC-H    SSC-W
    ##  [1,]   230.5815        182   83029.62  17580.359  15158 76009.13
    ##  [2,]  1443.3286       1012   93468.36 236985.156 210447 73800.34
    ##  [3,]  5213.2168       2808  121671.43  42554.961  34266 81389.19
    ##  [4,] 15389.9199       6886  146470.20  84185.953  57786 95476.59
    ##  [5,]  2344.1123       1780   86305.48 119029.742 110182 70798.61
    ##  [6,]  2245.1777       2363   62268.29  14038.583  17942 51278.15
    ##  [7,]  3035.8569       2235   89019.21  19821.965  20998 61865.53
    ##  [8,]   411.6957        537   50243.74   8782.381  11199 51394.06
    ##  [9,]  1876.5665       1418   86729.66  62098.676  55731 73023.98
    ## [10,]  1568.5925       1388   74062.89  26110.691  25612 66812.05
    ##            FITC-A FITC-H    FITC-W  Blue_A-A Blue_A-H Blue_A-W
    ##  [1,]    50.53441     64  51747.23  64.88368      176 24160.32
    ##  [2,]   522.81274    473  72437.75 429.85440      553 50942.02
    ##  [3,] 10396.98633   5804 117397.81 709.35333      524 88717.91
    ##  [4,]   786.71466    552  93402.41 569.60388      540 69128.81
    ##  [5,]   535.91425    528  66518.33 521.56500      544 62833.24
    ##  [6,]   114.79421    148  50832.12  20.58809       64 21082.21
    ##  [7,]  2468.07544   2208  73255.34 187.16447      288 42590.32
    ##  [8,]  4163.78564   4701  58046.77 235.20335      457 33729.29
    ##  [9,]   588.94421    421  91679.45 358.73190      709 33159.17
    ## [10,]   121.03303    138  57478.41 227.71678      282 52920.73
    ##          IndexSort DeltaTime RegionClassification
    ##  [1,] 3.433181e-43     14860         3.433181e-43
    ##  [2,] 3.447194e-43     34917         3.447194e-43
    ##  [3,] 3.461207e-43     22024         3.461207e-43
    ##  [4,] 3.475220e-43     16769         3.475220e-43
    ##  [5,] 3.489233e-43     39784         3.489233e-43
    ##  [6,] 3.503246e-43     10731         3.503246e-43
    ##  [7,] 3.517259e-43     11606         3.517259e-43
    ##  [8,] 3.531272e-43      4878         3.531272e-43
    ##  [9,] 3.545285e-43      9499         3.545285e-43
    ## [10,] 3.559298e-43      4788         3.559298e-43

``` r
summary(fcsFile)
```

    ##              Time     FSC-A     FSC-H     FSC-W YellowGreen_D-A
    ## Min.     65560.00  10491.68  16385.00  41275.07       -38.06389
    ## 1st Qu.  91217.75  40716.46  47699.75  54529.40        34.72495
    ## Median  105479.00  64927.12  64394.00  63797.59        93.49026
    ## Mean    105433.80  80032.53  65176.82  75278.46       399.52412
    ## 3rd Qu. 130450.00 100935.00  83249.75  80094.26       219.03432
    ## Max.    134614.00 262143.00 157182.00 262143.00     40034.53125
    ##         YellowGreen_D-H YellowGreen_D-W PE-CF594 (YG)-A PE-CF594 (YG)-H
    ## Min.             0.0000            0.00       -303.1755           0.000
    ## 1st Qu.         61.0000        36300.48        189.6517         308.000
    ## Median         109.0000        53865.33        545.5825         592.000
    ## Mean           326.0931        56931.04       2103.5276        1691.087
    ## 3rd Qu.        199.0000        70335.20       1236.9095        1097.000
    ## Max.         18252.0000       262143.00     202922.6094       93018.000
    ##         PE-CF594 (YG)-W YellowGreen_B-A YellowGreen_B-H YellowGreen_B-W
    ## Min.               0.00      -298.50104          0.0000            0.00
    ## 1st Qu.        39793.74        20.03363         51.0000        26258.48
    ## Median         56681.50        58.09752         79.0000        47265.26
    ## Mean           59809.87       144.25967        130.0085        50372.73
    ## 3rd Qu.        72547.70       123.54070        124.0000        66074.11
    ## Max.          262143.00     30012.37500       3866.0000       262143.00
    ##         YellowGreen_A-A YellowGreen_A-H YellowGreen_A-W     SB436*-A
    ## Min.          -42.73840          0.0000            0.00   -106.91323
    ## 1st Qu.        14.69133         56.0000        15977.38     50.26517
    ## Median         43.40619         88.0000        31764.29    161.96556
    ## Mean           93.89599        117.6865        35545.20   2552.95226
    ## 3rd Qu.        94.15804        136.0000        48243.92   1572.78137
    ## Max.        33482.86719       3474.0000       262143.00 262143.00000
    ##           SB436*-H  SB436*-W  Violet_A-A Violet_A-H Violet_A-W
    ## Min.         0.000      0.00   -374.9942      1.000       0.00
    ## 1st Qu.     57.000  55487.38    828.9764    718.000   63868.26
    ## Median     121.000  77066.57   1794.3870   1394.000   78687.79
    ## Mean      1491.243  82917.51   4221.4875   2619.989   90851.86
    ## 3rd Qu.   1099.250 102165.34   4329.1880   2941.000  102654.50
    ## Max.    258896.000 262143.00 262143.0000 258256.000  262143.00
    ##                SSC-A     SSC-H     SSC-W      FITC-A    FITC-H    FITC-W
    ## Min.        18.09257      6.00  38656.43  -1033.7717      0.00      0.00
    ## 1st Qu.  18163.06445  20342.00  55613.73    378.6961    346.00  54595.46
    ## Median   44083.78320  42840.00  64319.55   2775.3372   2502.50  61923.80
    ## Mean     73505.91199  66893.04  67827.44  25105.1255  25834.88  68732.60
    ## 3rd Qu.  87668.61719  74835.50  72495.57  23922.2710  23355.50  73858.70
    ## Max.    262143.00000 257711.00 262143.00 262143.0000 258685.00 262143.00
    ##            Blue_A-A   Blue_A-H  Blue_A-W    IndexSort  DeltaTime
    ## Min.      -113.5464      0.000      0.00 3.138909e-43     34.000
    ## 1st Qu.    297.5915    404.000  43712.00 3.236999e-43   1318.000
    ## Median     586.4487    670.000  52687.47 3.349103e-43   3693.000
    ## Mean      4623.6101   4641.691  56405.74 3.355948e-43   6137.882
    ## 3rd Qu.   2738.2163   2515.250  63267.57 3.461207e-43   8164.250
    ## Max.    262143.0000 258047.000 262143.00 3.573311e-43 262144.000
    ##         RegionClassification
    ## Min.            0.000000e+00
    ## 1st Qu.         3.265025e-43
    ## Median          3.405155e-43
    ## Mean            5.732300e+00
    ## 3rd Qu.         3.545285e-43
    ## Max.            6.200000e+01

``` r
str(keyword(fcsFile))
```

    ## List of 650
    ##  $ FCSversion                   : chr "3.1"
    ##  $ $PAR                         : chr "34"
    ##  $ $DATATYPE                    : chr "F"
    ##  $ $MODE                        : chr "L"
    ##  $ $BYTEORD                     : chr "1,2,3,4"
    ##  $ $TOT                         : chr "50000"
    ##  $ $BEGINDATA                   : chr "               12411"
    ##  $ $ENDDATA                     : chr "             6812410"
    ##  $ $BEGINSTEXT                  : chr "0"
    ##  $ $ENDSTEXT                    : chr "0"
    ##  $ $BEGINANALYSIS               : chr "0"
    ##  $ $ENDANALYSIS                 : chr "0"
    ##  $ $NEXTDATA                    : chr "0"
    ##  $ $TIMESTEP                    : chr "0.01"
    ##  $ $DATE                        : chr "16-JUL-2018"
    ##  $ $BTIM                        : chr "14:23:38.72"
    ##  $ $ETIM                        : chr "14:24:48.23"
    ##  $ EXPORT TIME                  : chr "16-JUL-2018 16:32:39.32"
    ##  $ $P1N                         : chr "Time"
    ##  $ $P1B                         : chr "32"
    ##  $ $P1E                         : chr "0,0"
    ##  $ $P1R                         : chr "262144"
    ##  $ P1KIND                       : chr "Time"
    ##  $ $P2N                         : chr "FSC-A"
    ##  $ $P2B                         : chr "32"
    ##  $ $P2E                         : chr "0,0"
    ##  $ $P2R                         : chr "262144"
    ##  $ $P2V                         : chr "95"
    ##  $ $P2S                         : chr "FSC-A"
    ##  $ P2MEAS                       : chr "A"
    ##  $ P2THRESHOLD                  : chr "16358"
    ##  $ P2MS                         : chr "1000"
    ##  $ P2KIND                       : chr "SCATTER"
    ##  $ P2TARGETVALUE                : chr "0.0790596211908806"
    ##  $ P2FLUOR                      : chr "FSC"
    ##  $ P2DET                        : chr "FSC"
    ##  $ $P2L                         : chr "488"
    ##  $ FL2ABD                       : chr "99945"
    ##  $ FL2SLOPE                     : chr "0.00386796775273979"
    ##  $ FL2INTERCEPT                 : chr "3.53559565544128"
    ##  $ $P3N                         : chr "FSC-H"
    ##  $ $P3B                         : chr "32"
    ##  $ $P3E                         : chr "0,0"
    ##  $ $P3R                         : chr "262144"
    ##  $ $P3V                         : chr "95"
    ##  $ $P3S                         : chr "FSC-H"
    ##  $ P3MEAS                       : chr "H"
    ##  $ P3THRESHOLD                  : chr "16358"
    ##  $ P3MS                         : chr "1000"
    ##  $ P3KIND                       : chr "SCATTER"
    ##  $ P3TARGETVALUE                : chr "0.0790596211908806"
    ##  $ P3FLUOR                      : chr "FSC"
    ##  $ P3DET                        : chr "FSC"
    ##  $ $P3L                         : chr "488"
    ##  $ FL3ABD                       : chr "99945"
    ##  $ FL3SLOPE                     : chr "0.00386796775273979"
    ##  $ FL3INTERCEPT                 : chr "3.53559565544128"
    ##  $ $P4N                         : chr "FSC-W"
    ##  $ $P4B                         : chr "32"
    ##  $ $P4E                         : chr "0,0"
    ##  $ $P4R                         : chr "262144"
    ##  $ $P4V                         : chr "95"
    ##  $ $P4S                         : chr "FSC-W"
    ##  $ P4MEAS                       : chr "W"
    ##  $ P4THRESHOLD                  : chr "16358"
    ##  $ P4KIND                       : chr "SCATTER"
    ##  $ P4TARGETVALUE                : chr "0.0790596211908806"
    ##  $ P4FLUOR                      : chr "FSC"
    ##  $ P4DET                        : chr "FSC"
    ##  $ $P4L                         : chr "488"
    ##  $ FL4ABD                       : chr "99945"
    ##  $ FL4SLOPE                     : chr "0.00386796775273979"
    ##  $ FL4INTERCEPT                 : chr "3.53559565544128"
    ##  $ $P5N                         : chr "YellowGreen_D-A"
    ##  $ $P5B                         : chr "32"
    ##  $ $P5E                         : chr "0,0"
    ##  $ $P5R                         : chr "262144"
    ##  $ $P5V                         : chr "450"
    ##  $ $P5S                         : chr "YellowGreen_D-A"
    ##  $ P5MEAS                       : chr "A"
    ##  $ P5MS                         : chr "1000"
    ##  $ P5KIND                       : chr "COLOR"
    ##  $ P5TARGETVALUE                : chr "1.19"
    ##  $ $P5F                         : chr "BP/582/15/BP/582/15"
    ##  $ P5FLUOR                      : chr "YellowGreen_D"
    ##  $ P5DET                        : chr "PE (YG)"
    ##  $ $P5L                         : chr "561"
    ##  $ FL5ABD                       : chr "15018"
    ##  $ FL5SLOPE                     : chr "8.72256278991699"
    ##  $ FL5INTERCEPT                 : chr "-18.934476852417"
    ##  $ $P6N                         : chr "YellowGreen_D-H"
    ##  $ $P6B                         : chr "32"
    ##  $ $P6E                         : chr "0,0"
    ##  $ $P6R                         : chr "262144"
    ##  $ $P6V                         : chr "450"
    ##  $ $P6S                         : chr "YellowGreen_D-H"
    ##  $ P6MEAS                       : chr "H"
    ##  $ P6MS                         : chr "1000"
    ##  $ P6KIND                       : chr "COLOR"
    ##   [list output truncated]

``` r
summary(fcsFile[,c(2, 8, 17, 23, 26)])
```

    ##             FSC-A PE-CF594 (YG)-A     SB436*-A        SSC-A      FITC-A
    ## Min.     10491.68       -303.1755   -106.91323     18.09257  -1033.7717
    ## 1st Qu.  40716.46        189.6517     50.26517  18163.06445    378.6961
    ## Median   64927.12        545.5825    161.96556  44083.78320   2775.3372
    ## Mean     80032.53       2103.5276   2552.95226  73505.91199  25105.1255
    ## 3rd Qu. 100935.00       1236.9095   1572.78137  87668.61719  23922.2710
    ## Max.    262143.00     202922.6094 262143.00000 262143.00000 262143.0000

\#Compensation Check $SPILLOVER for correct file by changing the x in
spillover(fcsFile)\[\[x\]\] in the console (only \#Single file)

``` r
#Single file
fcsFile_comp <- compensate(fcsFile, spillover(fcsFile)[[3]])
fcsFile_comp
```

    ## flowFrame object 'WT.fcs'
    ## with 50000 cells and 34 observables:
    ##                      name                       desc  range   minRange
    ## $P1                  Time                       <NA> 262144    0.00000
    ## $P2                 FSC-A                      FSC-A 262144    0.00000
    ## $P3                 FSC-H                      FSC-H 262144    0.00000
    ## $P4                 FSC-W                      FSC-W 262144    0.00000
    ## $P5       YellowGreen_D-A            YellowGreen_D-A 262144  -38.06389
    ## $P6       YellowGreen_D-H            YellowGreen_D-H 262144    0.00000
    ## $P7       YellowGreen_D-W            YellowGreen_D-W 262144    0.00000
    ## $P8       PE-CF594 (YG)-A CD142 (F3) PE-CF594 (YG)-A 262144 -111.00000
    ## $P9       PE-CF594 (YG)-H CD142 (F3) PE-CF594 (YG)-H 262144    0.00000
    ## $P10      PE-CF594 (YG)-W CD142 (F3) PE-CF594 (YG)-W 262144    0.00000
    ## $P11      YellowGreen_B-A            YellowGreen_B-A 262144 -111.00000
    ## $P12      YellowGreen_B-H            YellowGreen_B-H 262144    0.00000
    ## $P13      YellowGreen_B-W            YellowGreen_B-W 262144    0.00000
    ## $P14      YellowGreen_A-A            YellowGreen_A-A 262144  -42.73840
    ## $P15      YellowGreen_A-H            YellowGreen_A-H 262144    0.00000
    ## $P16      YellowGreen_A-W            YellowGreen_A-W 262144    0.00000
    ## $P17             SB436*-A              SCA1 SB436*-A 262144 -106.91323
    ## $P18             SB436*-H              SCA1 SB436*-H 262144    0.00000
    ## $P19             SB436*-W              SCA1 SB436*-W 262144    0.00000
    ## $P20           Violet_A-A                 Violet_A-A 262144 -111.00000
    ## $P21           Violet_A-H                 Violet_A-H 262144    0.00000
    ## $P22           Violet_A-W                 Violet_A-W 262144    0.00000
    ## $P23                SSC-A                      SSC-A 262144    0.00000
    ## $P24                SSC-H                      SSC-H 262144    0.00000
    ## $P25                SSC-W                      SSC-W 262144    0.00000
    ## $P26               FITC-A           LIN Sytox FITC-A 262144 -111.00000
    ## $P27               FITC-H           LIN Sytox FITC-H 262144    0.00000
    ## $P28               FITC-W           LIN Sytox FITC-W 262144    0.00000
    ## $P29             Blue_A-A                   Blue_A-A 262144 -111.00000
    ## $P30             Blue_A-H                   Blue_A-H 262144    0.00000
    ## $P31             Blue_A-W                   Blue_A-W 262144    0.00000
    ## $P32            IndexSort                       <NA> 262144    0.00000
    ## $P33            DeltaTime                       <NA> 262144    0.00000
    ## $P34 RegionClassification                       <NA> 262144    0.00000
    ##      maxRange
    ## $P1    262143
    ## $P2    262143
    ## $P3    262143
    ## $P4    262143
    ## $P5    262143
    ## $P6    262143
    ## $P7    262143
    ## $P8    262143
    ## $P9    262143
    ## $P10   262143
    ## $P11   262143
    ## $P12   262143
    ## $P13   262143
    ## $P14   262143
    ## $P15   262143
    ## $P16   262143
    ## $P17   262143
    ## $P18   262143
    ## $P19   262143
    ## $P20   262143
    ## $P21   262143
    ## $P22   262143
    ## $P23   262143
    ## $P24   262143
    ## $P25   262143
    ## $P26   262143
    ## $P27   262143
    ## $P28   262143
    ## $P29   262143
    ## $P30   262143
    ## $P31   262143
    ## $P32   262143
    ## $P33   262143
    ## $P34   262143
    ## 650 keywords are stored in the 'description' slot

``` r
#All files
comp <- fsApply(fs, function(x)spillover(x)[[3]], simplify = FALSE)
fs_comp <- compensate(fs, comp)
```

\#Autoplot

``` r
for(i in 1:length(fs)){
  print(autoplot(fs[i], x = "FSC-A", y = "SSC-A", bins = 256))
}
```

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-5-4.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-5-5.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-5-6.png)<!-- -->

\#Transform Check the correct columns (DO NOT select FSC or
SSC)

``` r
tf <- estimateLogicle(fs_comp[[1]], channels = colnames(fs_comp[, c(5:16, 20:22, 26:31)]))
fs_trans <- transform(fs, tf)
```

\#\#Automatic Gating till singlets \#Gating nonDebris

``` r
gs <- GatingSet(fs_trans)
```

    ## ......done!

``` r
thisData <- getData(gs)
nonDebrisGate <- fsApply(thisData, function(fr)openCyto:::.flowClust.2d(fr, channels = c("FSC-H", "SSC-H")))
```

    ## 'K' argument is missing! Using default setting: K = 2
    ## The prior specification has no effect when usePrior=no
    ## Using the serial version of flowClust
    ## 'K' argument is missing! Using default setting: K = 2
    ## The prior specification has no effect when usePrior=no
    ## Using the serial version of flowClust
    ## 'K' argument is missing! Using default setting: K = 2
    ## The prior specification has no effect when usePrior=no
    ## Using the serial version of flowClust
    ## 'K' argument is missing! Using default setting: K = 2
    ## The prior specification has no effect when usePrior=no
    ## Using the serial version of flowClust
    ## 'K' argument is missing! Using default setting: K = 2
    ## The prior specification has no effect when usePrior=no
    ## Using the serial version of flowClust
    ## 'K' argument is missing! Using default setting: K = 2
    ## The prior specification has no effect when usePrior=no
    ## Using the serial version of flowClust

``` r
add(gs, nonDebrisGate, parent = "root", name = "nonDebris")
```

    ## [1] 2

``` r
recompute(gs)
```

    ## ......done!

``` r
for(i in 1:length(gs)){
  print(autoplot(gs[i], x = "FSC-H", y = "SSC-H", "nonDebris", bins = 256))
}
```

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.
    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-7-2.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-7-3.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-7-4.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-7-5.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-7-6.png)<!-- -->

\#Gating singlets

``` r
thisData <- getData(gs, "nonDebris")
singletGate <- fsApply(thisData, function(fr)openCyto:::.singletGate(fr, channels = c("FSC-H", "FSC-W")))
```

    ## Warning in rlm.default(x, y, weights, method = method, wt.method =
    ## wt.method, : 'rlm' failed to converge in 5 steps

    ## Warning in singletGate(fr, area = channels[1], height = channels[2], ...):
    ## The IRLS algorithm employed in 'rlm' did not converge.

    ## Warning in rlm.default(x, y, weights, method = method, wt.method =
    ## wt.method, : 'rlm' failed to converge in 5 steps

    ## Warning in singletGate(fr, area = channels[1], height = channels[2], ...):
    ## The IRLS algorithm employed in 'rlm' did not converge.

    ## Warning in rlm.default(x, y, weights, method = method, wt.method =
    ## wt.method, : 'rlm' failed to converge in 5 steps

    ## Warning in singletGate(fr, area = channels[1], height = channels[2], ...):
    ## The IRLS algorithm employed in 'rlm' did not converge.

    ## Warning in rlm.default(x, y, weights, method = method, wt.method =
    ## wt.method, : 'rlm' failed to converge in 5 steps

    ## Warning in singletGate(fr, area = channels[1], height = channels[2], ...):
    ## The IRLS algorithm employed in 'rlm' did not converge.

    ## Warning in rlm.default(x, y, weights, method = method, wt.method =
    ## wt.method, : 'rlm' failed to converge in 5 steps

    ## Warning in singletGate(fr, area = channels[1], height = channels[2], ...):
    ## The IRLS algorithm employed in 'rlm' did not converge.

    ## Warning in rlm.default(x, y, weights, method = method, wt.method =
    ## wt.method, : 'rlm' failed to converge in 5 steps

    ## Warning in singletGate(fr, area = channels[1], height = channels[2], ...):
    ## The IRLS algorithm employed in 'rlm' did not converge.

``` r
add(gs, singletGate, parent = "nonDebris", name = "singlets")
```

    ## [1] 3

``` r
recompute(gs)
```

    ## ......done!

``` r
for(i in 1:length(gs)){
  print(autoplot(gs[i], x = "FSC-H", y = "FSC-W", "singlets", bins = 256))
}
```

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.
    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-8-3.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-8-4.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-8-5.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-8-6.png)<!-- -->

\#FITC

``` r
thisData <- getData(gs, "singlets")
LinGate <- fsApply(thisData, function(fr)openCyto:::.flowClust.2d(fr, channels = c("FSC-A","FITC-A"), target = c(1.5e05, 1.5) ))
```

    ## 'K' argument is missing! Using default setting: K = 2

    ## The prior specification has no effect when usePrior=no

    ## Using the serial version of flowClust

    ## 'K' argument is missing! Using default setting: K = 2

    ## The prior specification has no effect when usePrior=no

    ## Using the serial version of flowClust

    ## 'K' argument is missing! Using default setting: K = 2

    ## The prior specification has no effect when usePrior=no

    ## Using the serial version of flowClust

    ## 'K' argument is missing! Using default setting: K = 2

    ## The prior specification has no effect when usePrior=no

    ## Using the serial version of flowClust

    ## 'K' argument is missing! Using default setting: K = 2

    ## The prior specification has no effect when usePrior=no

    ## Using the serial version of flowClust

    ## 'K' argument is missing! Using default setting: K = 2

    ## The prior specification has no effect when usePrior=no

    ## Using the serial version of flowClust

``` r
add(gs, LinGate, parent = "singlets", name = "Lin")
```

    ## [1] 4

``` r
recompute(gs)
```

    ## .

    ## .....done!

``` r
for(i in 1:length(gs)){
  print(autoplot(gs[i], x = "FSC-A", y = "FITC-A", "Lin", bins = 256))
}
```

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.
    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-9-2.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-9-3.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-9-4.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-9-5.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-9-6.png)<!-- -->

\#Gate SB436

``` r
thisData <- getData(gs, "Lin")
Sca1 <- fsApply(thisData, function(fr)openCyto:::.mindensity(fr, channels = "Violet_A-A", min = 2.5 ))
add(gs, Sca1, parent = "Lin", name = "Sca1")
```

    ## [1] 5

``` r
getNodes(gs)
```

    ## [1] "root"                         "/nonDebris"                  
    ## [3] "/nonDebris/singlets"          "/nonDebris/singlets/Lin"     
    ## [5] "/nonDebris/singlets/Lin/Sca1"

``` r
recompute(gs)
```

    ## ......done!

``` r
getStats(gs, "Sca1", "percent")
```

    ##                sample  pop      percent
    ## 1:   FITC-Control.fcs Sca1 0.0004464286
    ## 2:     PE-Control.fcs Sca1 0.0059497576
    ## 3: SB436*-Control.fcs Sca1 0.2312717770
    ## 4:           SGCB.fcs Sca1 0.3083387201
    ## 5:      Unstained.fcs Sca1 0.0085955711
    ## 6:             WT.fcs Sca1 0.2688707093

``` r
for(i in 1:length(gs)){
  print(autoplot(gs[i], x ="Violet_A-A" , "Sca1", bins = 256))
}
```

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.
    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-10-2.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-10-3.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-10-4.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-10-5.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-10-6.png)<!-- -->

\#Gate PE

``` r
thisData <- getData(gs, "Sca1")
F3 <- fsApply(thisData, function(fr)openCyto:::.quantileGate(fr, channels = "PE-CF594 (YG)-A"))
add(gs, F3, parent = "Sca1", name = "F3")
```

    ## [1] 6

``` r
getNodes(gs)
```

    ## [1] "root"                            "/nonDebris"                     
    ## [3] "/nonDebris/singlets"             "/nonDebris/singlets/Lin"        
    ## [5] "/nonDebris/singlets/Lin/Sca1"    "/nonDebris/singlets/Lin/Sca1/F3"

``` r
recompute(gs)
```

    ## ......done!

``` r
getStats(gs, "F3", "percent")
```

    ##                sample pop     percent
    ## 1:   FITC-Control.fcs  F3 1.000000000
    ## 2:     PE-Control.fcs  F3 0.037037037
    ## 3: SB436*-Control.fcs  F3 0.001883239
    ## 4:           SGCB.fcs  F3 0.001048218
    ## 5:      Unstained.fcs  F3 0.008474576
    ## 6:             WT.fcs  F3 0.001254705

``` r
for(i in 1:length(gs)){
  print(autoplot(gs[i], y = "FSC-A", x ="PE-CF594 (YG)-A", "F3", bins = 256))
}
```

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

    ## Warning: Computation failed in `stat_binhex()`:
    ## missing value where TRUE/FALSE needed

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-11-2.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-11-3.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-11-4.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-11-5.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-11-6.png)<!-- -->

\#Get numbers

``` r
getStats(gs)
```

    ##                 sample                             pop count
    ##  1:   FITC-Control.fcs                            root 10000
    ##  2:   FITC-Control.fcs                      /nonDebris  6580
    ##  3:   FITC-Control.fcs             /nonDebris/singlets  6005
    ##  4:   FITC-Control.fcs         /nonDebris/singlets/Lin  2240
    ##  5:   FITC-Control.fcs    /nonDebris/singlets/Lin/Sca1     1
    ##  6:   FITC-Control.fcs /nonDebris/singlets/Lin/Sca1/F3     1
    ##  7:     PE-Control.fcs                            root 10000
    ##  8:     PE-Control.fcs                      /nonDebris  6508
    ##  9:     PE-Control.fcs             /nonDebris/singlets  5959
    ## 10:     PE-Control.fcs         /nonDebris/singlets/Lin  4538
    ## 11:     PE-Control.fcs    /nonDebris/singlets/Lin/Sca1    27
    ## 12:     PE-Control.fcs /nonDebris/singlets/Lin/Sca1/F3     1
    ## 13: SB436*-Control.fcs                            root 10000
    ## 14: SB436*-Control.fcs                      /nonDebris  6442
    ## 15: SB436*-Control.fcs             /nonDebris/singlets  5918
    ## 16: SB436*-Control.fcs         /nonDebris/singlets/Lin  4592
    ## 17: SB436*-Control.fcs    /nonDebris/singlets/Lin/Sca1  1062
    ## 18: SB436*-Control.fcs /nonDebris/singlets/Lin/Sca1/F3     2
    ## 19:           SGCB.fcs                            root 50000
    ## 20:           SGCB.fcs                      /nonDebris 39026
    ## 21:           SGCB.fcs             /nonDebris/singlets 35543
    ## 22:           SGCB.fcs         /nonDebris/singlets/Lin  9282
    ## 23:           SGCB.fcs    /nonDebris/singlets/Lin/Sca1  2862
    ## 24:           SGCB.fcs /nonDebris/singlets/Lin/Sca1/F3     3
    ## 25:      Unstained.fcs                            root 30000
    ## 26:      Unstained.fcs                      /nonDebris 20327
    ## 27:      Unstained.fcs             /nonDebris/singlets 18615
    ## 28:      Unstained.fcs         /nonDebris/singlets/Lin 13728
    ## 29:      Unstained.fcs    /nonDebris/singlets/Lin/Sca1   118
    ## 30:      Unstained.fcs /nonDebris/singlets/Lin/Sca1/F3     1
    ## 31:             WT.fcs                            root 50000
    ## 32:             WT.fcs                      /nonDebris 37787
    ## 33:             WT.fcs             /nonDebris/singlets 34473
    ## 34:             WT.fcs         /nonDebris/singlets/Lin 11857
    ## 35:             WT.fcs    /nonDebris/singlets/Lin/Sca1  3188
    ## 36:             WT.fcs /nonDebris/singlets/Lin/Sca1/F3     4
    ##                 sample                             pop count

\#\#Manual gating for fluorescence \#Gate
FITC

``` r
LinGate <- polygonGate("FSC-A" = c(40000, 40000, 100000, 200000, 200000), "FITC-A" = c(0, 1.4, 2.2, 2.2,0))
add(gs, LinGate, parent = "singlets", name = "Lineage negative")
```

    ## replicating filter 'defaultPolygonGate' across samples!

    ## [1] 7

``` r
getNodes(gs)
```

    ## [1] "root"                                
    ## [2] "/nonDebris"                          
    ## [3] "/nonDebris/singlets"                 
    ## [4] "/nonDebris/singlets/Lin"             
    ## [5] "/nonDebris/singlets/Lin/Sca1"        
    ## [6] "/nonDebris/singlets/Lin/Sca1/F3"     
    ## [7] "/nonDebris/singlets/Lineage negative"

``` r
recompute(gs)
```

    ## .

    ## .....done!

``` r
for(i in 1:length(gs)){
  print(autoplot(gs[i], x = 'FSC-A', y = 'FITC-A', "Lineage negative", log = "FITC-A", bins = 256))
}
```

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.
    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-13-2.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-13-3.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-13-4.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-13-5.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-13-6.png)<!-- -->

\#gate PE and SB436

``` r
F3 <- quadGate("PE-CF594 (YG)-A" = 2.4, "Violet_A-A"  = 2.6)
add(gs, F3, parent = "Lineage negative", name = c("Sca1+F3-", "Sca1+F3+", "Sca1- F3+", "Sca1-F3-"))
```

    ## replicating filter 'defaultQuadGate' across samples!

    ## [1]  8  9 10 11

``` r
getNodes(gs)
```

    ##  [1] "root"                                          
    ##  [2] "/nonDebris"                                    
    ##  [3] "/nonDebris/singlets"                           
    ##  [4] "/nonDebris/singlets/Lin"                       
    ##  [5] "/nonDebris/singlets/Lin/Sca1"                  
    ##  [6] "/nonDebris/singlets/Lin/Sca1/F3"               
    ##  [7] "/nonDebris/singlets/Lineage negative"          
    ##  [8] "/nonDebris/singlets/Lineage negative/Sca1+F3-" 
    ##  [9] "/nonDebris/singlets/Lineage negative/Sca1+F3+" 
    ## [10] "/nonDebris/singlets/Lineage negative/Sca1- F3+"
    ## [11] "/nonDebris/singlets/Lineage negative/Sca1-F3-"

``` r
recompute(gs)
```

    ## .

    ## .....done!

``` r
getStats(gs, c("Sca1+F3-", "Sca1+F3+", "Sca1- F3+", "Sca1-F3-"), "percent")
```

    ##                 sample       pop      percent
    ##  1:   FITC-Control.fcs  Sca1+F3- 0.0000000000
    ##  2:   FITC-Control.fcs  Sca1+F3+ 0.0000000000
    ##  3:   FITC-Control.fcs Sca1- F3+ 0.0000000000
    ##  4:   FITC-Control.fcs  Sca1-F3- 1.0000000000
    ##  5:     PE-Control.fcs  Sca1+F3- 0.0019024970
    ##  6:     PE-Control.fcs  Sca1+F3+ 0.0000000000
    ##  7:     PE-Control.fcs Sca1- F3+ 0.0240190250
    ##  8:     PE-Control.fcs  Sca1-F3- 0.9740784780
    ##  9: SB436*-Control.fcs  Sca1+F3- 0.2207392197
    ## 10: SB436*-Control.fcs  Sca1+F3+ 0.0002566735
    ## 11: SB436*-Control.fcs Sca1- F3+ 0.0000000000
    ## 12: SB436*-Control.fcs  Sca1-F3- 0.7790041068
    ## 13:           SGCB.fcs  Sca1+F3- 0.2664709336
    ## 14:           SGCB.fcs  Sca1+F3+ 0.0387551380
    ## 15:           SGCB.fcs Sca1- F3+ 0.0251321198
    ## 16:           SGCB.fcs  Sca1-F3- 0.6696418086
    ## 17:      Unstained.fcs  Sca1+F3- 0.0021728109
    ## 18:      Unstained.fcs  Sca1+F3+ 0.0005794162
    ## 19:      Unstained.fcs Sca1- F3+ 0.0001448541
    ## 20:      Unstained.fcs  Sca1-F3- 0.9971029188
    ## 21:             WT.fcs  Sca1+F3- 0.2514043650
    ## 22:             WT.fcs  Sca1+F3+ 0.0231144673
    ## 23:             WT.fcs Sca1- F3+ 0.0191546183
    ## 24:             WT.fcs  Sca1-F3- 0.7063265494
    ##                 sample       pop      percent

``` r
for(i in 1:length(gs)){
  print(autoplot(gs[i], x = "PE-CF594 (YG)-A", y = "Violet_A-A" , c("Sca1+F3-", "Sca1+F3+", "Sca1- F3+", "Sca1-F3-"), bins = 256))
}
```

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.
    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-14-2.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-14-3.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-14-4.png)<!-- -->

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-14-5.png)<!-- -->![](FlowDataAnalysis_files/figure-gfm/unnamed-chunk-14-6.png)<!-- -->
