## Conteúdo
{:.no_toc}

-   [Como funciona a espectrometria de massa?](#how-does-mass-spectrometry-work)
-   [Acesso a dados](#accessing-data)
    -   [Da base de dados ProteomeXchange](#from-the-proteomexchange-database)
    -   [Pacotes de dados](#data-packages)
{:toc} \# Introdução {#sec-msintro}

### Como funciona a espectrometria de massa?

A espectrometria de massa (EM) é uma tecnologia que *separa as* moléculas carregadas (iões) com base na sua relação massa/carga (M/Z). É frequentemente acoplada à cromatografia (liquid LC, mas também pode ser baseada em gás GC). O tempo que um analito demora a eluir da coluna de cromatografia é o tempo de retenção (*retention time*.)

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/chromatogram.png" alt="A chromatogram, illustrating the total amount of analytes over the retention time." width="100%" />

<p class="caption">Um chromatograma, ilustrando a quantidade total de analitos sobre o tempo de retenção.</p>

Um espectrómetro de massa é composto por três componentes:

1.  A fonte (*source*), que ioniza as moléculas: os exemplos são dessorção/ionização de laser assistida por matriz (MALDI) ou ionização de electrospray. (ESI.)
2.  O *analyser*, que separa os iões: Tempo do voo (TOF) ou Orbitrap.
3.  O *detector* que quantifica as iões.

Quando se utiliza a espectrometria de massa para proteómica, as proteínas são previamente digeridas com uma protease como a tripsina. Em proteómica shotgun de massa, os analitos testados no espectrómetro de massa são peptídeos.

Muitas vezes, os iões são sujeitos a mais do que uma única ronda de MS. Depois de uma primeira ronda de separação, os picos nos espectros, chamados espectros MS1, representam peptídeos. Nesta fase, a única informação que possuímos sobre estes peptídeos são o seu tempo de retenção e a sua massa/carga (podemos também inferir a sua carga inspeccionando o seu envelope isotópico, ou seja, o picos dos isótopos individuais, ver abaixo), o que não é suficiente para inferir a sua identidade (ou seja, a sua sequência).

Na MSMS (ou MS2), as definições do espectrómetro de massa são definidas automaticamente para seleccionar um certo número de picos de MS1 (por exemplo 20)[1]. Uma vez seleccionado um intervalo M/Z estreito (correspondente a um pico de alta intensidade, um peptídeo, e algum ruído de fundo), ele é fragmentado (utilizando, por exemplo, a dissociação induzida pela colisão (CID), dissociação colisória de alta energia (HCD) ou dissociação por transferência de electrões (ETD)). Os fragmentos de iões são então eles próprios separados no analisador para produzir um espectro MS2. O padrão de fragmento de ião único pode então ser usado para inferir a sequência de peptídeo usando nova sequenciação (quando o espectro é de qualidade suficientemente elevada) ou utilizando um motor de busca como, por exemplo, Mascot, MSGF+, ..., que se adequará ao espectro experimental a espectros teóricos observado (ver detalhes abaixo).

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/SchematicMS2.png" alt="Schematics of a mass spectrometer and two rounds of MS." width="100%" />

<p class="caption">Esquema de um espectrómetro de massa e duas rondas de MS.</p>

A animação abaixo mostra como 25 iões diferentes (ou seja, com diferentes valores de M/Z) são separados ao longo da análise de MS e são eventualmente detectados (ou seja, quantificados). The final frame shows the hypothetical spectrum.

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/mstut.gif" alt="Separation and detection of ions in a mass spectrometer." width="100%" />

<p class="caption">Separation and detection of ions in a mass
spectrometer.</p>

The figures below illustrate the two rounds of MS. The spectrum on the left is an MS1 spectrum acquired after 21 minutes and 3 seconds of elution. 10 peaks, highlited by dotted vertical lines, were selected for MS2 analysis. The peak at M/Z 460.79 (488.8) is highlighted by a red (orange) vertical line on the MS1 spectrum and the fragment spectra are shown on the MS2 spectrum on the top (bottom) right figure.

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
