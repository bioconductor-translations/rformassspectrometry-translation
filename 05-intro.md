## Contents
{:.no_toc}

-   [How does mass spectrometry work?](#how-does-mass-spectrometry-work)
-   [Accessing data](#accessing-data)
    -   [From the ProteomeXchange
        database](#from-the-proteomexchange-database)
    -   [Data packages](#data-packages)
{:toc} \# Introduction {#sec-msintro}

### How does mass spectrometry work?

Mass spectrometry (MS) is a technology that *separates* charged
molecules (ions) based on their mass to charge ratio (M/Z) It is often
coupled to chromatography (liquid LC, but can also be gas-based GC) The
time an analyte takes to elute from the chromatography column is the
*retention time*

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/chromatogram.png" alt="A chromatogram, illustrating the total amount of analytes over the retention time" width="100%" />
<p class="caption">A chromatogram, illustrating the total amount of
analytes over the retention time</p>

An mass spectrometer is composed of three components:

1  The *source*, that ionises the molecules: examples are
    Matrix-assisted laser desorption/ionisation (MALDI) or electrospray
    ionisation (ESI)
2  The *analyser*, that separates the ions: Time of flight (TOF) or
    Orbitrap
3  The *detector* that quantifies the ions

When using mass spectrometry for proteomics, the proteins are first
digested with a protease such as trypsin In mass shotgun proteomics,
the analytes assayed in the mass spectrometer are peptides

Often, ions are subjected to more than a single MS round After a first
round of separation, the peaks in the spectra, called MS1 spectra,
represent peptides At this stage, the only information we possess about
these peptides are their retention time and their mass-to-charge (we can
also infer their charge by inspecting their isotopic envelope, ie the
peaks of the individual isotopes, see below), which is not enough to
infer their identify (ie their sequence)

In MSMS (or MS2), the settings of the mass spectrometer are set
automatically to select a certain number of MS1 peaks (for example
20)[1] Once a narrow M/Z range has been selected (corresponding to one
high-intensity peak, a peptide, and some background noise), it is
fragmented (using for example collision-induced dissociation (CID),
higher energy collisional dissociation (HCD) or electron-transfer
dissociation (ETD)) The fragment ions are then themselves separated in
the analyser to produce a MS2 spectrum The unique fragment ion pattern
can then be used to infer the peptide sequence using de novo sequencing
(when the spectrum is of high enough quality) or using a search engine
such as, for example Mascot, MSGF+, …, that will match the observed,
experimental spectrum to theoretical spectra (see details below)

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/SchematicMS2.png" alt="Schematics of a mass spectrometer and two rounds of MS" width="100%" />
<p class="caption">Schematics of a mass spectrometer and two rounds of
MS</p>

The animation below show how 25 ions different ions (ie having
different M/Z values) are separated throughout the MS analysis and are
eventually detected (ie quantified) The final frame shows the
hypothetical spectrum

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/mstut.gif" alt="Separation and detection of ions in a mass spectrometer" width="100%" />
<p class="caption">Separation and detection of ions in a mass
spectrometer</p>

The figures below illustrate the two rounds of MS The spectrum on the
left is an MS1 spectrum acquired after 21 minutes and 3 seconds of
elution 10 peaks, highlited by dotted vertical lines, were selected for
MS2 analysis The peak at M/Z 46079 (4888) is highlighted by a red
(orange) vertical line on the MS1 spectrum and the fragment spectra are
shown on the MS2 spectrum on the top (bottom) right figure

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/MS1-MS2-spectra.png" alt="Parent ions in the MS1 spectrum (left) and two sected fragment ions MS2 spectra (right)" width="100%" />
<p class="caption">Parent ions in the MS1 spectrum (left) and two sected
fragment ions MS2 spectra (right)</p>

The figures below represent the 3 dimensions of MS data: a set of
spectra (M/Z and intensity) of retention time, as well as the
interleaved nature of MS1 and MS2 (and there could be more levels) data

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/F02-3D-MS1-scans-400-1200-lattice.png" alt="MS1 spectra over retention time" width="100%" />
<p class="caption">MS1 spectra over retention time</p>

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/F02-3D-MS1-MS2-scans-100-1200-lattice.png" alt="MS2 spectra interleaved between two MS1 spectra" width="100%" />
<p class="caption">MS2 spectra interleaved between two MS1 spectra</p>

### Accessing data

#### From the ProteomeXchange database

MS-based proteomics data is disseminated through the
[ProteomeXchange](http://www.proteomexchange.org/) infrastructure, which
centrally coordinates submission, storage and dissemination through
multiple data repositories, such as the [PRoteomics IDEntifications
(PRIDE)](https://www.ebi.ac.uk/pride/archive/) database at the EBI for
mass spectrometry-based experiments (including quantitative data, as
opposed as the name suggests),
[PASSEL](http://www.peptideatlas.org/passel/) at the ISB for Selected
Reaction Monitoring (SRM, ie targeted) data and the
[MassIVE](http://massive.ucsd.edu/ProteoSAFe/static/massive.jsp)
resource These data can be downloaded within R using the
*[rpx](https://bioconductor.org/packages/3.15/rpx)* package

    library("rpx")

Using the unique `PXD000001` identifier, we can retrieve the relevant
metadata that will be stored in a `PXDataset` object The names of the
files available in this data can be retrieved with the `pxfiles`
accessor function

    px <- PXDataset("PXD000001")

    ## Querying ProteomeXchange for PXD000001

    px

    ## Project PXD000001 with 10 files
    ## 

    ## Resource ID BFC1 in cache in C:\Users\hoge\AppData\Local/R/cache/R/rpx

    ##  [1] 'erwinia_carotovorafasta'  [10] 'PRIDE_Exp_Complete_Ac_22134pridemgfgz'
    ##  Use 'pxfiles()' to see all files

    pxfiles(px)

    ## Project PXD000001 files (10):
    ##  [remote] erwinia_carotovorafasta
    ##  [remote] F063721dat
    ##  [remote] F063721dat-mztabtxt
    ##  [remote] PRIDE_Exp_Complete_Ac_22134xmlgz
    ##  [remote] TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01raw
    ##  [remote] TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01mzXML
    ##  [remote] TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210mzXML
    ##  [remote] TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210mzML
    ##  [remote] PRIDE_Exp_Complete_Ac_22134pridemztabgz
    ##  [remote] PRIDE_Exp_Complete_Ac_22134pridemgfgz

Other metadata for the `px` data set:

    pxtax(px)

    ## [1] "Erwinia carotovora"

    pxurl(px)

    ## [1] "ftp://ftpprideebiacuk/pride-archive/2012/03/PXD000001"

    pxref(px)

    ## [1] "Gatto L, Christoforou A; Using R and Bioconductor for proteomics data analysis, Biochim Biophys Acta, 2013 May 18, doi:101016/jbbapap201304032 PMID:23692960"

Data files can then be downloaded with the `pxget` function Below, we
retrieve the raw data file The file is downloaded[2] in the working
directory and the name of the file is return by the function and stored
in the `mzf` variable for later use [3]

    fn <- "TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210mzML"
    mzf <- pxget(px, fn)

    ## Downloading TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210mzML file

    mzf

    ## [1] "C:\\Users\\hoge\\AppData\\Local/R/cache/R/rpx/7a4c58585711_TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210mzML"

#### Data packages

Some data are also distributed through dedicated packages The
*[msdata](https://bioconductor.org/packages/3.15/msdata)*, for example,
provides some general raw data files relevant for both proteomics and
metabolomics

    library("msdata")
    ## proteomics raw data
    proteomics()

    ## [1] "MRM-standmix-5mzMLgz"                                                
    ## [2] "MS3TMT10_01022016_32917-33481mzMLgz"                                 
    ## [3] "MS3TMT11mzML"                                                         
    ## [4] "TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210mzMLgz"
    ## [5] "TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01mzMLgz"

    ## proteomics identification data
    ident()

    ## [1] "TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210mzid"

    ## quantitative data
    quant()

    ## [1] "cptac_a_b_peptidestxt"

More often, such *experiment packages* distribute processed data; an
example of such is the
*[pRolocdata](https://bioconductor.org/packages/3.15/pRolocdata)*
package, that offers quantitative proteomics data

[1] Here, we will focus on data dependent acquisition (DDA), where MS1
peaks are selected In data independent acquisition (DIA), all peaks in
the MS1 spectrum are fragmented

[2] If the file is already available, it is not downloaded a second
time

[3] This and other files are also availabel in the `msdata` package,
described below
