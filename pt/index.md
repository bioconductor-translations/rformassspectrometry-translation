# R for Mass Spectrometry
{:.no_toc}

<p class="author-name">Laurent Gatto, Sebastian Gibb, Johannes Rainer</p>

## Contents
{:.no_toc}

-   [Preamble](#preamble)
    -   [Targeted audience and assumed background](#targeted-audience-and-assumed-background)
    -   [Setup](#setup)
    -   [Acknowledgments](#acknowledgments)
    -   [License](#license)
{:toc}

## Preamble

The aim of the [R for Mass Spectrometry](https://www.rformassspectrometry.org/) initiative is to provide efficient, thoroughly documented, tested and flexible R software for the analysis and interpretation of high throughput mass spectrometry assays, including proteomics and metabolomics experiments. The project formalises the longtime collaborative development efforts of its core members under the RforMassSpectrometry organisation to facilitate dissemination and accessibility of their work.

<img src="https://github.com/rformassspectrometry/stickers/raw/master/sticker/RforMassSpectrometry.png" alt="The *R for Mass Spectrometry* intiative sticker, designed by Johannes Rainer." width="50%" />

<p class="caption">The *R for Mass Spectrometry* intiative sticker,
designed by Johannes Rainer.</p>

This material introduces participants to the analysis and exploration of mass spectrometry (MS) based proteomics data using R and Bioconductor. The course will cover all levels of MS data, from raw data to identification and quantitation data, up to the statistical interpretation of a typical shotgun MS experiment and will focus on hands-on tutorials. At the end of this course, the participants will be able to manipulate MS data in R and use existing packages for their exploratory and statistical proteomics data analysis.

### Targeted audience and assumed background

The course material is targeted to either proteomics practitioners or data analysts/bioinformaticians that would like to learn how to use R and Bioconductor to analyse proteomics data. Familiarity with MS or proteomics in general is desirable, but not essential as we will walk through and describe a typical MS data as part of learning about the tools. *A beginner’s guide to mass spectrometry–based proteomics* ([Sinha and Mann 2020](#ref-Sinha:2020)) is an approachable introduction to sample preparation, mass spectrometry and data analysis.

A working knowledge of R (R syntax, commonly used functions, basic data structures such as data frames, vectors, matrices, … and their manipulation) is required. Familiarity with other Bioconductor omics data classes and the tidyverse syntax is useful, but not necessary.

### Setup

This material uses the latest version of the R for Mass Spectrometry package and their dependencies. It might thus be possible that even the latest Bioconductor stable version isn’t recent enough.

To install the latest versions of all the package, please use R version 4.2 and execute:

    if (!requireNamespace("BiocManager", quietly = TRUE))
        install.packages("BiocManager")
    
    BiocManager::install("msdata")
    BiocManager::install("mzR")
    BiocManager::install("rhdf5")
    BiocManager::install("rpx")
    BiocManager::install("MsCoreUtils")
    BiocManager::install("QFeatures")
    BiocManager::install("Spectra")
    BiocManager::install("ProtGenerics")
    BiocManager::install("PSMatch")
    BiocManager::install("pheatmap")
    BiocManager::install("RforMassSpectrometry/SpectraVis")

To compile and render the teaching material, you will also need the (slighly modified) [Modern Statistics for Model Biology (msmb) HTML Book Style](https://www-huber.embl.de/users/msmith/msmbstyle/) by Mike Smith:

    BiocManager::install("lgatto/msmbstyle")

### Acknowledgments

Thank you to [Charlotte Soneson](https://github.com/csoneson) for fixing many typos in a previous version of this book.

### License

<a rel="license"
href="http://creativecommons.org/licenses/by-sa/4.0/"><img
alt="Creative Commons Licence" style="border-width:0"
src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This material is licensed under a <a rel="license"
href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons
Attribution-ShareAlike 4.0 International License</a>. You are free to **share** (copy and redistribute the material in any medium or format) and **adapt** (remix, transform, and build upon the material) for any purpose, even commercially, as long as you give appropriate credit and distribute your contributions under the same license as the original.

Sinha, Ankit, and Matthias Mann. 2020. “<span class="nocase">A beginner’s guide to mass spectrometry–based proteomics</span>.” *The Biochemist*, September. <https://doi.org/10.1042/BIO20200057>.
