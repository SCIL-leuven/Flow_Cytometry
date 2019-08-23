FlowCytometry

This project was based on the Introduction to R for flow cytometry data analysis from the Sanger Institute https://github.com/SangerCytometry/R_flowcytometry_course

Use FlowDataAnalalysis for autogating to singlets, after this, you can autogate or gate manualy

Do not select colours with *, use another colourname from the same channel

Change the channels in the order you need them

For autogating:

1D gating: .mindensity .tailgate .QuantileGate
<<<<<<< HEAD
2D gating: .boundaryGate .singletGate .flowClust.2d .quadGate.tmix
For more informatpion: https://www.bioconductor.org/packages/devel/bioc/vignettes/openCyto/inst/doc/HowToAutoGating.html
=======

2D gating: .boundaryGate .singletGate .flowClust.2d .quadGate.tmix

For more information: https://www.bioconductor.org/packages/devel/bioc/vignettes/openCyto/inst/doc/HowToAutoGating.html
>>>>>>> 865da41156b41a3475902d75fc1481568ae78aae
