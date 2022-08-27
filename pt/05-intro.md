## Conteúdo
{:.no_toc}

-   [Como funciona a espectrometria de massa?](#how-does-mass-spectrometry-work)
-   [Acesso a dados](#accessing-data)
    -   [Da base de dados ProteomeXchange](#from-the-proteomexchange-database)
    -   [Pacotes de dados](#data-packages)
{:toc} \# Introdução {#sec-msintro}

### Como funciona a espectrometria de massa?

A espectrometria de massa (EM) é uma tecnologia que *separa as* moléculas carregadas (iões) com base na sua relação massa/carga (M/Z). É frequentemente acoplada à cromatografia (liquid LC, mas também pode ser baseada em gás GC). O tempo que um analito demora a eluir da coluna de cromatografia é o tempo de retenção (*retention time*.)

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/chromatogram.png" alt="Um cromatograma, ilustrando a quantidade total de analitos ao longo do tempo de retenção." width="100%" />

<p class="caption">Um chromatograma, ilustrando a quantidade total de analitos sobre o tempo de retenção.</p>

Um espectrómetro de massa é composto por três componentes:

1.  A fonte (*source*), que ioniza as moléculas: os exemplos são dessorção/ionização de laser assistida por matriz (MALDI) ou ionização de electrospray. (ESI.)
2.  O *analyser*, que separa os iões: Tempo do voo (TOF) ou Orbitrap.
3.  O *detector* que quantifica as iões.

Quando se utiliza a espectrometria de massa para proteómica, as proteínas são previamente digeridas com uma protease como a tripsina. Em proteómica shotgun de massa, os analitos testados no espectrómetro de massa são peptídeos.

Muitas vezes, os iões são sujeitos a mais do que uma única ronda de MS. Depois de uma primeira ronda de separação, os picos nos espectros, chamados espectros MS1, representam peptídeos. Nesta fase, a única informação que possuímos sobre estes peptídeos são o seu tempo de retenção e a sua massa/carga (podemos também inferir a sua carga inspeccionando o seu envelope isotópico, ou seja, o picos dos isótopos individuais, ver abaixo), o que não é suficiente para inferir a sua identidade (ou seja, a sua sequência).

Na MSMS (ou MS2), as definições do espectrómetro de massa são definidas automaticamente para seleccionar um certo número de picos de MS1 (por exemplo 20)[1]. Uma vez seleccionado um intervalo M/Z estreito (correspondente a um pico de alta intensidade, um peptídeo, e algum ruído de fundo), ele é fragmentado (utilizando, por exemplo, a dissociação induzida pela colisão (CID), dissociação colisória de alta energia (HCD) ou dissociação por transferência de electrões (ETD)). Os fragmentos de iões são então eles próprios separados no analisador para produzir um espectro MS2. O padrão de fragmento de ião único pode então ser usado para inferir a sequência de peptídeo usando nova sequenciação (quando o espectro é de qualidade suficientemente elevada) ou utilizando um motor de busca como, por exemplo, Mascot, MSGF+, ..., que se adequará ao espectro experimental a espectros teóricos observado (ver detalhes abaixo).

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/SchematicMS2.png" alt="Esquema de um espectrómetro de massa e duas rondas de MS." width="100%" />

<p class="caption">Esquema de um espectrómetro de massa e duas rondas de MS.</p>

A animação abaixo mostra como 25 iões diferentes (ou seja, com diferentes valores de M/Z) são separados ao longo da análise de MS e são eventualmente detectados (ou seja, quantificados). A imagem final mostra o espectro hipotético.

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/mstut.gif" alt="Separação e detecção de iões num espectrómetro de massa." width="100%" />

<p class="caption">Separação e detecção de iões num espectrómetro de massa.</p>

Os figuras abaixo ilustram as duas rondas de MS. O espectro à esquerda é um espectro MS1 adquirido após 21 minutos e 3 segundos de eluição. 10 picos, realçados por linhas verticais pontilhadas, foram seleccionados para análise MS2. O pico com M/Z 460,79 (488,8) é realçado por uma linha vertical vermelha (laranja) no espectro MS1 e os espectros dos fragmentos são mostrado no espectro MS2 na figura superior (inferior) à direita.

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/MS1-MS2-spectra.png" alt="Iões progenitores no espectro MS1 (esquerda) e dois espectros de
fragmentos de iões de MS2 seccionados (direita)" width="100%" />

<p class="caption">Iões progenitores no espectro MS1 (esquerda) e dois espectros de
fragmentos de iões de MS2 seccionados (direita)</p>

Os números abaixo representam as 3 dimensões dos dados de MS: um conjunto de espectros (M/Z e intensidade) do tempo de retenção, e  a natureza intercalada dos dados de MS1 e MS2 (e poderia haver mais níveis).

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/F02-3D-MS1-scans-400-1200-lattice.png" alt="Espectros MS1 sobre o tempo de retenção." width="100%" />
<p class="caption">O espectro MS1 sobre o tempo de retenção.</p>

<img src="https://github.com/rformassspectrometry/docs/raw/main/img/F02-3D-MS1-MS2-scans-100-1200-lattice.png" alt="Os espectros MS2 intercalados entre dois espectros MS1." width="100%" />
<p class="caption">Os espectros MS2 intercalados entre dois espectros MS1.</p>

### Acesso aos dados

#### A partir da base de dados ProteomeXchange

Os dados proteómicos baseados em EM são divulgados através das infra-estruturas [ProteomeXchange](http://www.proteomexchange.org/), que coordenam centralmente a submissão, armazenamento e disseminação através de múltiplos repositórios de dados, tais como as base de dados [PRoteomics IDEntifications (PRIDE)](https://www.ebi.ac.uk/pride/archive/) na EBI para ensaios baseados na espectrometria de massa (incluindo dados quantitativos, contrariamente ao que o nome sugere), [PASSEL](http://www.peptideatlas.org/passel/) no ISB para Monitorização Seleccionada das Reacções (SRM, ou seja, direccionada) e os dados dos recursos [MassIVE](http://massive.ucsd.edu/ProteoSAFe/static/massive.jsp). Estes dados podem ser descarregados dentro do R usando o pacote *[rpx](https://bioconductor.org/packages/3.15/rpx).*

    library("rpx")

Usando o identificador único `PXD000001`, podemos recuperar o identificador de metadados relevante que serão armazenados num objecto `PXDataset`. Os nomes dos os ficheiros disponíveis nestes dados podem ser obtidos com a função `pxfiles`.

    px <- PXDataset("PXD000001")
    
    px
    
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

Outros metadados para o conjunto de dados `px`:

    pxtax(px)
    
    ## [1] "Erwinia carotovora"
    
    pxurl(px)
    
    ## [1] "ftp://ftp.pride.ebi.ac.uk/pride-archive/2012/03/PXD000001"
    
    pxref(px)
    
    ## [1] "Gatto L, Christoforou A; Using R and Bioconductor for proteomics data analysis., Biochim Biophys Acta, 2013 May 18, doi:10.1016/j.bbapap.2013.04.032 PMID:23692960"

Os ficheiros de dados podem então ser descarregados com a função `pxget`. Abaixo, recuperamos o ficheiro de dados em bruto. O ficheiro é descarregado[2] no ambiente de trabalho e o nome do ficheiro é devolvido pela função e armazenado na variável `mzf` para uso posterior [3].

    fn <- "TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzML"
    mzf <- pxget(px, fn)
    
    ## Loading TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzML from cache.
    
    mzf
    
    ## [1] "/home/rstudio/.cache/R/rpx/1922b2f30fe_TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzML"

#### Pacotes de dados

Alguns dados são também distribuídos através de pacotes dedicados. O *[msdata](https://bioconductor.org/packages/3.15/msdata)*, por exemplo, fornece alguns ficheiros gerais de dados em bruto relevantes tanto para a proteómica como para metabolómica.

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

Mais frequentemente, tais * pacotes de experimentação* distribuem dados processados; um exemplo de tal é o pacote *[pRolocdata](https://bioconductor.org/packages/3.15/pRolocdata)*, que oferece dados proteómicos quantitativos.

[1] Aqui, centrar-nos-emos na aquisição dependente de dados (DDA), onde os picos MS1 são seleccionados. Na aquisição independente de dados (DIA), todos os picos em no espectro MS1 são fragmentados.

[2] Se o ficheiro já estiver disponível, não é descarregado uma segunda vez.

[3] Este e outros ficheiros também estão disponíveis no pacote `msdata`, descrito abaixo
