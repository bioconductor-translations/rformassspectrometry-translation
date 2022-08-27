## Contents
{:.no_toc}

-   [どのようにマススペクトロメトリーは機能するのか？](#how-does-mass-spectrometry-work)
-   [データへのアクセス方法](#accessing-data)
    -   [ProteomeXchange から](#from-the-proteomexchange-database)
    -   [データパッケージ でのアクセス](#data-packages)
{:toc} \# イントロダクション {#sec-msintro}

### どのようにマススペクトロメトリーは機能するのか？

質量分析法(MS)は、 質量とチャージ比(M/Z)に基づいて帯電した 分子(イオン)を *分離する* 技術です。 それはしばしばクロマトグラフィー (液クロ、またはガスクロ) とカップルで使われます。 測定する物質がクロマトのカラムから溶解するのにかかる時間は *リテンションタイム* と呼ばれます。

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/chromatogram.png" alt="A chromatogram, illustrating the total amount of analytes over the retention time." width="100%" />

<p class="caption">リテンションタイムにわたる測定物質の合計量を示すクロマトグラム。</p>

質量分析計は以下の3つの構成要素で構成されています:

1.  *ソース*, 分子をイオン化するもの: 例えば Matrix-assisted laser desorption/ionisation (MALDI) もしくは electrospray ionisation. (ESI)
2.  *アナライザー*, イオンを分離するもの: Time of flight (TOF) もしくは Orbitrap.
3.  *検出器* イオンを定量化するもの。

プロテオミクスにマススペクトロメトリーを使う場合、タンパクはトリプシンのようなプロテアーゼでまず分解されます。 ショットガンプロテオミクスにおいては、マススペクトロメトリーで測定される被験物質はペプチドです。

多くの場合、イオンは単一のMSラウンド以上になります。 最初のラウンドの分離後、MS1スペクトルと呼ばれるスペクトルのピークは、ペプチドを表します。 この段階では、そのペプチドについて私達が持ちうる情報はリテンションタイムとmass-to-chargeです。 (私達はまたその isotopic envelope を調べることでも charge を推測できます。例えば個々のアイソトープのピークです。) それはペプチドの出自(たとえばその配列)を推測するには不十分です。

MSMS（またはMS2）において マススペクトロメーターの設定は、 自動的にMS1のピーク数(例えば 20) を選択するように設定されます[1]。 一旦 M/Z の狭いレンジが選択されたら(一つの高いインテンシティのピーク、ペプチド、そしていくつかのバックグラウンドノイズに対応するもの)、 それはフラグメント化されます。(例えば collision-induced dissociation (CID)、higher energy collisional dissociation (HCD)、もしくはelectron-transfer dissociation (ETD) を用いて。) その後、フラグメントイオンは分析器 で分離され、MS2スペクトルを生みます。 ユニークなフラグメントイオン化パターンは de novo シーケンシングを用いた、もしくは Mascot、MSGF+、… のようなサーチエンジンを用いたペプチド配列を推測するのに用いられ、それは観測された実験のスペクトラムを理論的なスペクトラムに対応付けられます (下記参照)。

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/SchematicMS2.png" alt="Schematics of a mass spectrometer and two rounds of MS." width="100%" />

<p class="caption">マススペクトロメーターとMSの2ラウンドの概要。</p>

以下のアニメーションは、25個のイオン (すなわち異なる M/Z の値を持つもの) がどのように異なるかを示しています。 それらは、MS分析を通じて分離され、最終的に検出され(すなわち定量化され)ます。 アニメーションの最後のフレームは仮定のスペクトラムを示しています。

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/mstut.gif" alt="Separation and detection of ions in a mass spectrometer." width="100%" />

<p class="caption">マススペクトロメーターにおけるイオンの分離と検出</p>

下記の図はMSの2ラウンドを表しています。 The spectrum on the left is an MS1 spectrum acquired after 21 minutes and 3 seconds of elution. 10 peaks, highlited by dotted vertical lines, were selected for MS2 analysis. The peak at M/Z 460.79 (488.8) is highlighted by a red (orange) vertical line on the MS1 spectrum and the fragment spectra are shown on the MS2 spectrum on the top (bottom) right figure.

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/MS1-MS2-spectra.png" alt="Parent ions in the MS1 spectrum (left) and two sected fragment ions MS2 spectra (right)" width="100%" />

<p class="caption">Parent ions in the MS1 spectrum (left) and two sected
fragment ions MS2 spectra (right)</p>

The figures below represent the 3 dimensions of MS data: a set of spectra (M/Z and intensity) of retention time, as well as the interleaved nature of MS1 and MS2 (and there could be more levels) data.

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/F02-3D-MS1-scans-400-1200-lattice.png" alt="MS1 spectra over retention time." width="100%" />
<p class="caption">MS1 spectra over retention time.</p>

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/F02-3D-MS1-MS2-scans-100-1200-lattice.png" alt="MS2 spectra interleaved between two MS1 spectra." width="100%" />
<p class="caption">MS2 spectra interleaved between two MS1 spectra.</p>

### Accessing data

#### From the ProteomeXchange database

MS-based proteomics data is disseminated through the [ProteomeXchange](http://www.proteomexchange.org/) infrastructure, which centrally coordinates submission, storage and dissemination through multiple data repositories, such as the [PRoteomics IDEntifications (PRIDE)](https://www.ebi.ac.uk/pride/archive/) database at the EBI for mass spectrometry-based experiments (including quantitative data, as opposed as the name suggests), [PASSEL](http://www.peptideatlas.org/passel/) at the ISB for Selected Reaction Monitoring (SRM, i.e. targeted) data and the [MassIVE](http://massive.ucsd.edu/ProteoSAFe/static/massive.jsp) resource. These data can be downloaded within R using the *[rpx](https://bioconductor.org/packages/3.15/rpx)* package.

    library("rpx")

Using the unique `PXD000001` identifier, we can retrieve the relevant metadata that will be stored in a `PXDataset` object. The names of the files available in this data can be retrieved with the `pxfiles` accessor function.

    px <- PXDataset("PXD000001")
    
    ## Loading PXD000001 from cache.
    
    px
    
    ## Project PXD000001 with 10 files
    ## 
    
    ## Resource ID BFC1 in cache in /home/rstudio/.cache/R/rpx.
    
    ##  [1] 'erwinia_carotovora.fasta' ... [10] 'PRIDE_Exp_Complete_Ac_22134.pride.mgf.gz'
    ##  Use 'pxfiles(.)' to see all files.
    
    pxfiles(px)
    
    ## Project PXD000001 files (10):
    ##  [remote] erwinia_carotovora.fasta
    ##  [remote] F063721.dat
    ##  [remote] F063721.dat-mztab.txt
    ##  [remote] PRIDE_Exp_Complete_Ac_22134.xml.gz
    ##  [remote] TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01.raw
    ##  [remote] TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01.mzXML
    ##  [remote] TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzXML
    ##  [remote] TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzML
    ##  [remote] PRIDE_Exp_Complete_Ac_22134.pride.mztab.gz
    ##  [remote] PRIDE_Exp_Complete_Ac_22134.pride.mgf.gz

Other metadata for the `px` data set:

    pxtax(px)
    
    ## [1] "Erwinia carotovora"
    
    pxurl(px)
    
    ## [1] "ftp://ftp.pride.ebi.ac.uk/pride-archive/2012/03/PXD000001"
    
    pxref(px)
    
    ## [1] "Gatto L, Christoforou A; Using R and Bioconductor for proteomics data analysis., Biochim Biophys Acta, 2013 May 18, doi:10.1016/j.bbapap.2013.04.032 PMID:23692960"

Data files can then be downloaded with the `pxget` function. Below, we retrieve the raw data file. The file is downloaded[2] in the working directory and the name of the file is return by the function and stored in the `mzf` variable for later use [3].

    fn <- "TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzML"
    mzf <- pxget(px, fn)
    
    ## Loading TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzML from cache.
    
    mzf
    
    ## [1] "/home/rstudio/.cache/R/rpx/1922b2f30fe_TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzML"

#### Data packages

Some data are also distributed through dedicated packages. The *[msdata](https://bioconductor.org/packages/3.15/msdata)*, for example, provides some general raw data files relevant for both proteomics and metabolomics.

    library("msdata")
    ## proteomics raw data
    proteomics()
    
    ## [1] "MRM-standmix-5.mzML.gz"                                                
    ## [2] "MS3TMT10_01022016_32917-33481.mzML.gz"                                 
    ## [3] "MS3TMT11.mzML"                                                         
    ## [4] "TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzML.gz"
    ## [5] "TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01.mzML.gz"
    
    ## proteomics identification data
    ident()
    
    ## [1] "TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzid"
    
    ## quantitative data
    quant()
    
    ## [1] "cptac_a_b_peptides.txt"

More often, such *experiment packages* distribute processed data; an example of such is the *[pRolocdata](https://bioconductor.org/packages/3.15/pRolocdata)* package, that offers quantitative proteomics data.

[1] Here, we will focus on data dependent acquisition (DDA), where MS1 peaks are selected. In data independent acquisition (DIA), all peaks in the MS1 spectrum are fragmented.

[2] If the file is already available, it is not downloaded a second time.

[3] This and other files are also availabel in the `msdata` package, described below
