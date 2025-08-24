# Qualidade_MRI_Bobinas_IC
Este projeto teve como principal objetivo o desenvolvimento de um pipeline em Python para a avaliação da qualidade de imagens de ressonância magnética (RM).  Um dos principais benefícios do trabalho padronização da análise e redução das medições manuais.

\documentclass[a4paper,11pt]{article}
\input{cabecalho}
\bibliographystyle{plain}
\usepackage{pdfpages}

\begin{document}

%Remove numeração da página atual
\thispagestyle{empty}

% Como fazer um cabeçalho passo a passo com tabular: https://www.youtube.com/watch?v=NEvF9mJwOXI&list=PLF6ZF9NW0WmqUAgtkYlQmCDHP6H_bYwGk&index=3

\begin{tabular}[l]{ll}
\multirow{5}*{\includegraphics[width=50pt]{logo.png}} & 
\textbf{\resizebox{!}{0.3cm}{Fundação Universidade Federal do ABC}}\\&
\textbf{\resizebox{!}{0.3cm}{Pró reitoria de pesquisa}} \\
& \textbf{\resizebox{!}{0.25cm}{Av. dos Estados, 5001, Santa Terezinha, Santo André/SP, CEP 09210-580}}\\
& \textbf{\resizebox{!}{0.25cm}{Bloco L, 3ºAndar, Fone (11) 3356-7617}}\\
& \textbf{\resizebox{!}{0.25cm}{iniciacao@ufabc.edu.br}} \\
\end{tabular}
\\
\\
\\
\\
\\
\\
\\
\\
\\
\\
\\
\\
\\
\\
\\
\\
\\
Relatório final do projeto de Iniciação Científica submetido no Edital: 01/2024 
\\
(PIC/PIBIC/PIBITI/PIBIC-AF)
\\
\\
\\
\\
\\
\\
\\
\\
\\
\textbf{Nome do aluno:} Sarah Fernandes de Souza\\ \\
\textbf{Nome do orientador:} John Andrew Sims\\ \\
\textbf{Título do projeto:} Desenvolvimento de um Software para Avaliação da Homogeneidade e Relação Sinal/Ruído em Scanners de Imagens por Ressonância Magnética.\\ \\
\textbf{Palavras-chave do projeto:} Determination of SNR in MRI, Hhomogeneity in MRI, Uniformity testing of MRI, Periodic testing of MRI, Quality assurance for MRI systems.\\ \\
\textbf{Área do conhecimento do projeto:} Engenharia biomédica.\\ \\
\textbf{Bolsista:} Não.\\ \\

%Quebra de página
\newpage


%Sumário
\tableofcontents

\newpage

%Comando para determinar espaçamento entre linhas (1,5 nesse caso)
\onehalfspacing
{
%Define como tópico/seção do projeto (irá aparecer automaticamente no sumário)  
\section{Resumo}

Este projeto teve como principal objetivo o desenvolvimento de um pipeline em Python para a avaliação da qualidade de imagens de ressonância magnética (RM). As imagens médicas são suscetíveis a problemas como ruído, inomogeneidade e artefatos, o que pode comprometer a qualidade do diagnóstico. O software realiza a leitura de imagens no formato NIfTI e calcula algumas métricas que podem ser utilizadas como parâmetros de qualidade. As imagens foram lidas dentro do ambiente de programação do Google collaboratory, avaliadas, filtradas, segmentadas e as medidas realizadas foram de distorção geométrica, relação sinal-ruído (RSR), homogeneidade, contraste, coeficiente de variação, se o ruído é o não estacionário e a esfericidade do fantoma nas imagens. Um dos principais benefícios do trabalho padronização da análise e redução das medições manuais. O programa fornece ao usuário imagens para inspeção visual e duas tabelas com a extensão ".csv" contendo os resultados das métricas, as quais podem ser baixadas ao final da avaliação. Uma das tabelas contém a distorção do maior diâmetro, e a outra, os parâmetros para todos os cortes válidos. Os valores podem ser utilizados para comparar os cortes ao longo do tempo.

\section{Introdução}

\subsection{Bobinas de Radiofrequência}
 As bobinas de radiofrequência (RF) utilizadas em MRI são componentes que transmitem potência de RF (excitar o núcleo do 1H no corpo) e recebem (detectar o sinal emitido do núcleo) sinais de RF. Dentre os tipos de bobinas de RF para MRI, podemos classificá-las como transmissoras, receptoras e transceptoras (transmissoras e receptoras). Além disso, as bobinas podem ser categorizadas de acordo com a sua geometria e região alvo do corpo como de volume, superfície e multicanal. (GRUBER et al., 2018)

\subsection{Controle de qualidade}

Ressonância magnética, assim como as outras modalidades estão sujeitas a problemas de deterioração da qualidade, o que pode estar relacionada com limitações de hardware, duração do exame, movimentação do paciente, interações do paciente com os componentes elétricos, Eddy current e movimentação estocástica de elétrons livres presentes nas bobinas de RF e as consequências desse problemas são a adição de ruídos e artefatos às imagens. (AJA-FERNÁNDEZ; VEGAS-SÁNCHEZ-FERRERO, 2016)

O controle de qualidade (CQ) de imagens médicas na clínica ou hospital é essencial para garantir que a instrumentação está operando de forma adequada, produzindo imagens com qualidade suficiente para fins de diagnóstico ou encaminhamento do paciente para tratamento apropriado. (DOMINGOS, 2020; ACR, 2015) Normas nacionais e internacionais existem para determinar quais testes devem ser realizados na instrumentação, com qual frequência, e os resultados mínimos para garantir sua conformidade. (NEMA, 2008; ANVISA) 

\subsection{Ruído em imagens de ressonância}

O Ruído Gaussiano está presente nos dados antes da obtenção da “imagem magnitude”, portanto o espaço-k, que é a matriz de números complexos codificada em fase e frequência, apresenta ruído gaussiano. Quando a “imagem magnitude” é obtida por meio da Transformada de Fourier e do cálculo do módulo dos valores complexos, o ruído deixa de ser gaussiano. (CAMPOS, 2020)

O arquivo final visualizado pelo médico para diagnóstico poderá apresentar duas possíveis distribuições: Rician, nas regiões com sinal, e Rayleigh nas regiões sem sinal. Essas distribuições ocorrem em imagens com alto RSR (Relação sinal ruído) obtidas com uma bobina de um canal. (CAMPOS, 2020). 

Uma medida bem utilizada para medir a qualidade da imagem é a relação sinal/ruído (RSR) de RM. Porém, não existe nenhum método aceito universalmente para medir a RSR em todas as situações. 
O perfil de ruído depois da reconstrução da imagem é uma distribuição Rician. Porém, perto do centro do fantoma, onde a RSR é alta, o ruído tende à distribuição Gaussiana. No ar, fora do fantoma, o ruído pode ser considerado de Rayleigh. (AJA-FERNÁNDEZ; VEGAS-SÁNCHEZ-FERRERO, 2016) Análise das distribuições de ruído em partes diferentes da imagem poderia mostrar possíveis problemas na sua aquisição. As normas de National Equipment Manufacturers Association mostram vários métodos para determinar RSR a partir de imagens de fantoma, e dois métodos foram considerados para o presente projeto, porém, considerando que não há mais de uma aquisição para cada um dos conjuntos de imagens, não foi possível aplicar o método 1, que envolve a subtração de dois cortes iguais obtidos com os mesmos parâmetros. Algumas outras medidas também foram realizadas, como, homogeneidade, contraste, borramento e o coeficiente de variação (CV), sendo todos formas de quantificar a inomogeneidade do campo B1 calculado a partir dos pixels dentro do campo B1.

Homogeneidade Avalia a proximidade dos valores de pixels da imagem à diagonal da matriz GLCM, que é uma análise de textura que pode ser utilizada para obter características de contraste e de homogeneidade das imagens. O valor da homogeneidade aumenta à medida que os valores apresentam poucas variações entre si. (SCIKIT-IMAGE; LANCASTER; HASEGAWA, 2020).\\


\begin{center}
$Homogeneidade = \sum_{i,j=0}^{N-1} \frac{P(i,j)}{1+(i-j)^2}$     (1)\\
\end{center}

\textbf{Equação 1:} Expressão para cálculo da homogeneidade. (SCIKIT-IMAGE).\\

O contraste é definido como a diferença na intensidade ou exposição da imagem entre regiões adjacentes e refere-se à capacidade de diferenciar tecidos ou objetos vizinhos (SCIKIT-IMAGE; LANCASTER; HASEGAWA, 2020).\\

\begin{center}
$Contraste = \sum_{i,j=0}^{N-1} P(i,j) \cdot (i-j)^2$ (2)\\
\end{center}

\textbf{Equação 2:} Expressão para cálculo do contraste. (SCIKIT-IMAGE).\\

O Coeficiente de variação avalia o quanto de variação há entre os valores dos pixels da imagem. Considerando que o fantoma é feito de material homogêneo, espera-se que a variação seja a menor possível.\\

\begin{center}
$CV = \frac{\sigma}{\mu} \cdot 100\%$       (3)\\
\end{center}

\textbf{Equação 3:} Expressão para cálculo do coeficiente de variação.

\subsection{Relação sinal ruído (RSR)}
A avaliação do ruído nas imagens foi realizada por meio da seleção de 4 regiões de ruído na imagem, que podem ser chamadas de “NMROI”, que são ROIs que não possuem sinal, apenas ruído. (NEMA, 2008) Nessas regiões a distribuição do ruído é "Rayleigh", mas apenas se estiver sendo analisadas imagens de um canal. (CAMPOS, 2020) A figura 1 mostra como ficou a seleção das quatro regiões. E as distribuições que são as de RAYLEIGH e o RICIANO conforme pode ser observado na figura 2. 

\begin{figure}[h!]
    \centering
    \includegraphics[scale=0.5]{regioes.png}
    \caption{Imagem do fantoma obtida com a bobina millipede de um canal. Na imagem é possível observar em vermelho 4 regiões selecionadas, cada uma delas equivale a uma ROI que possui apenas ruído. (Fonte: Autor.)}
\end{figure}

\begin{itemize}
    \item \textbf{Distribuição do ruídos das regiões:} Considerando um sistema com apenas um canal o ruído deverá ter a mesma distribuição em todas as regiões e é um ruído estacionário, ou seja, o ruído varia com a intensidade do sinal, mas não varia com a posição do pixel, portanto, o desvio padrão e a média das regiões devem ser próximos. (AJA-FERNÁNDEZ; VEGAS-SÁNCHEZ-FERRERO, 2016).
    \item \textbf{Seleção dos pixels:} Os pixels selecionados não podem abranger a região sem sinal e as normas na NEMA para testes de qualidade, aconselha a seleção de ao menos 1000 pixels. Desta forma, cada região possui 250 pixels de área, que somados, dão os 1000 pixels necessários. (NEMA, 2008)
    \item \textbf{Ruído na região de sinal (MROI):} O ruído na ROI que engloba o sinal deve ter uma distribuição chama de RICIANA. (NEMA, 2008)
    \item \textbf{Ruído na região de ruído (NMROI):} O ruído nas NMROI é um caso particular do ruído RICIANO, chamado de RAYLEIGH. (NEMA, 2008)
\end{itemize}

\begin{figure}[h!]
    \centering
    \includegraphics[scale=0.3]{distribuicao.png}
    \caption{Imagem mostrando a distribuição de ruído esperada. Na região de sinal é a distribuição Riciana e a região de ruído de Rayleigh. (Fonte: AJA-FERNÁNDEZ; VEGAS-SÁNCHEZ-FERRERO, 2016).}
\end{figure}

Um importante parâmetro para avaliar a qualidade da imagem é a RSR e para o seu cálculo utilizou-se a equação 1. O valor “S” da equação equivale ao valor médio dos pixels da MROI, que é uma ROI que engloba ao menos 75 por cento da região de sinal do Fantoma, na equação esse valor também está representado como “mean(MROI)”. O valor “N” é o desvio padrão do ruído e também está representado na equação como “std(NMROI)”. (NEMA, 2008)\\

\begin{center}
$RSR = \frac {mean(MROI)}{std(NMROI).0,66} = \frac {S}{N.0,66}$         (4)
\end{center}
\textbf{Equação 4:} Expressão utilizada para o cálculo da relação sinal ruído da imagem.

\subsection{Distorções geométricas}
A distorção geométrica pode ser uma consequência do deslocamento dos pixels de uma imagem em relação à posição em que deveria se encontrar e um dos principais fatores de distorção é a falta de homogeneidade do campo magnético. (DOMINGUES, 2020). As distorções são um problema para as imagens médicas, visto que elas devem representar a estrutura analisada com a maior precisão possível, para permitir um bom diagnóstico. (ACR, 2015) Considerando a importância da correta representação das estruturas do corpo, o código irá avaliar as dimensões do fantoma e comparar com as dimensões reais fornecidas pelo usuário. 

Além disso, também relacionado ao diâmetro, pode-se calcular a esfericidade das imagens, que seria o quanto elas se distanciam de um circulo perfeito. 

\subsection{Transformada de Hough}
Uma forma eficiente de aplicar uma máscara e de obter informações da imagem é com a transformada de Hough para círculos. Para o seu cálculo são necessários três parâmetros, sendo eles duas coordenadas “x” e “y” e o raio da circunferência.(OPENCV) Conforme pode ser observado na equação 3.\\

\begin{center}
$C_{Hough}(x_{centro}, y_{centro}, r_{raio})$    (5)
\end{center}\\

\textbf{Equação 5:} Expressão utilizada para representar os circulos que são calculados por meio da transformada de Hough.

\section{Objetivos}
Os objetivos principais deste projeto de IC é avaliar a qualidade de imagens de ressonância magnética. Imagens médicas são frequentemente corrompidas por ruído e podem apresentar problemas de homogeniedade, contraste e artefatos. O presente trabalho irá realizar a leitura dessas imagens e devolver de forma automática parâmetros e métricas que podem ser utilizadas para garantir a qualidade das imagens.  

\section{Metodologia}

\subsection{Materiais}

\subsubsection{Imagens disponíveis para análise}

As imagens disponibilizadas foram feitas com o uso de três tipos de bobinas de RF:

\begin{itemize}
    \item Volumétrica - tipo milipede com 30 mm de diâmetro interno \textbf{(Figura 1a)};
    \item Volumétrica - tipo solenóide com 30 mm de diâmetro interno \textbf{(Figura 1b)};
    \item Multicanal com 8 canais exclusivos para recepção com base nos protocolos sugeridos pela ACR e pelo NEMA \textbf{(Figura 1c)}.
\end{itemize}

\begin{figure}[h!]
    \centering
    \includegraphics[scale=0.2]{figura1.png}
    \caption{Imagens das três bobinas utilizadas para aquisição dos três conjuntos de imagens. (a) Bobina volumétrica millipede (esquerda),  (b) Bobina volumétrica solenóide (central) e (c) Bobina multicanal com 8 canais receptora (direita).}
\end{figure}

O fantoma utilizado possui formato esférico com diâmetro de 25 mm e foi preenchido com solução aquosa de $CuSO_4$ (1 g/L) e NaCl (3,6 g/L), apresentando distribuição homogênea. Este foi posicionado no isocentro de cada bobina. As imagens foram coletadas a partir de um scanner pré-clínico de 9,4T (Bruker Biospec 94/30, Ettlingen, Germany), localizado no Department of Neurobiology, University of Pittsburgh, EUA.

As imagens do fantoma foram realizadas com as três bobinas, solenóide, millipede e 8 canais, foram adquiridas com ponderação em T1, e sequência de pulsos spin-eco com TR = 500 ms e TE = 20 ms. O campo de visão das aquisições (Field of View, FOV) foi de 50 mm × 50 mm e 1 mm de espessura de fatia, com tamanho de matriz 256 × 256 e 30 fatias no total.

\subsubsection{Ambiente e linguagem de programação}

Todo o código desenvolvido em Python 3 utilizando bibliotecas científicas e utilitárias padrão, como: skimage, pandas, glob, OpenCV, imageio, matplotlib, pydicom e numpy. OpenCV.HoughCircles implementa o HCT. O ambiente ou IDE utilizada para testar e rodar os códigos foi o Google colaboratory.  


\subsection{Fluxograma}

O fluxograma das funções pode ser observado na \textbf{figura 4} e cada uma delas será explicada a seguir.

\begin{figure}[h!]
    \centering
    \includegraphics[width=0.5\linewidth]{fluxograma.png}
    \caption{fluxograma das funções mostrando de forma geral todo o funcionamento do código, desde a leitura das imagens até o retorno do resultado final ao usuário}
    \label{fig:placeholder}
\end{figure}

\subsubsection{Leitura das imagens}

A função para leitura das imagens utiliza a biblioteca Nibabel, que pode ser utilizada para a leitura de arquivos Nifft. Ela permite acesso aos dados da imagem em formato de um array e também permite que se acesse as informações contidas no cabeçalho do arquivo. (https://nipy.org/nibabel/nifti\_images.html) 

A entrada dessa função são duas Strings com os caminhos do arquivo da imagem e do ruído e a saída são os dados e informações das imagens e do ruído.  

\subsubsection{Visualização das imagens}

A etapa de visualização das imagens pode ser realizada de duar formas, por meio do plot de todos os cortes em um grid contendo vários subplots, com a função "plotagemimagens", ou pode ser feita com a função "videoimagens", que permite a vizualização dos dados em formato de vídeo. 

\subsubsection{Análise do ruído}
A análise do ruído envolve algumas funções e análises:

\begin{enumerate}

    \item Função \textbf{"perfil\_ruido\_imagem"}: Essa função recebe os dados do ruído que foram carregados com a biblioteca Nibabel e devolve três plotes, que mostram o histograma geral ddo ruído (considerando uma média de todos os cortes), a função densidade de probabilidade (PDF) e a função de distribuição acumulada (CDF). O objetivo desse plots é a visualização da distribuição do ruído. Assim como já foi discutido na sessão de introdução, o ruído em resonância magnética segue uma distribuição chamada Reylight, para o cado de apenas uma bobina. Apartir dos dados do histograma, é possível verificar se a distribuição é igual ou não à esperada. A média do ruído deu 115 e o desvio padrão 6. 
    
    \item Função \textbf{"noise\_regions"}: realiza o que foi descrito no manual do NEMA em que 4 regiões são selecionadas para a análise do ruído. Com a seleçõa dessas quatro regiões, é possível verificar se o ruído é ou não estacionário, o que depende do número de bobinas da imagem. O ruído possui variância uniforme em todas as quatro regiões selecionadas, se a imagem do fantoma tiver sido realizada com uma bobina de apenas um canal. 
    
    Rodou-se o código para cada um dos conjuntos de imagens para verificar se o comportarmento estacionário do ruído vaira ou não com o número de canais nas bobinas. 
    
    \item Função \textbf{"resumo\_estatistica"}: realiza os calculos de média e desvio padrão de todas as ROIs de 
    cada um dos cortes. Todas elas foram comparadas. 
    
    \item Função \textbf{"teste\_ruido\_estacionario"}: É uma função que aplica o teste do quiquadrado nas quatro regiões selecionadas. Se o valor "p" for significante, então podemos recusar a hipotese nula de que o ruído nas quatro regiões são iguais. E se não for significante, são os casos em que falhamos em rejeitrar a hipótese nula e o ruído é igual nas quatro ROIs. 
    
\end{enumerate}

\subsubsection{Filtragem das imagens}

A filtragem das imagens foi realizada com o uso de um filtro gaussinano. Esse filtro é baseado na distribuição normal, também chamada de distribuição gaussiano, e é muito utilizado em casos em que a distribuição, mesmo não sendo gaussinana, pode ser aproximada para tal distribuição. O papel da filtragem é permitir a melhor aplicação de outras funções, como a de limiariação da imagem para a obtenção de uma máscara binária, detecção de bordas. 

\subsubsection{Limiarização da imagem}

O método escolhido para a formação das máscaras binárias foi método de Otsu. Esse método basea-se a identificação de classes no histograma da imagem e determinar um valor intermediário entre as classes que permita a melhor separação possível entre ambas. O seja, ele devolve, de forma automática qual valor pode ser usado para separar a região de sinal da região de ruído. 

\subsubsection{Transformada de Hougth - Detecção de círculos}

A detecção de circulos com a transformada de hought pois os fantomas utilizados para testar as bobinas são circulares. Supondo que não há tanta distorção noformato circular do fantoma nas imagens, a detecção irá abranger maior parte da região de sinal. 

\subsubsection{Medições para avaliação da Qualidade}
As medições foram realizadas para que se possa ter um parâmetro de como as imagens estão ao longo do tempo. Mais de uma medição pode ser realizada com o código a fim de se detectar uma queda na qualidade das imagens, portanto, o uso das medições sem um parâmetro ou referência pode não ser adequado. As principais medições realizadas e disponibilizadas em formato CVC para download são: 

\begin{enumerate}
    \item \textbf{Distorção do maior diâmetro:} Considerando que o maior diâmetro é conehcido, essa medição avalia o quão próximo do ideal esta o maior diâmetro capturado pelas imagens. 
    \item \textbf{SNR - Relação sinal Ruído:} Calculo da relação entre o ruído e o sinal da imagem. O objetivo é sempre que o sinal seja maior que o ruído na imagem. 
    \item \textbf{Homogeneidade e Contraste:} Avalia o quão homogênio são os valores dentro da região de sinal e se o fantoma pode ser bem vizualidado. Imagens combordas mais bem definidas possuem maior contraste. 
    \item \textbf{Limiar utilizado pelo método de Otsu:} Define o valor de ton de cinza utilizado para formação das máscaras em cada corte e também representa um piso para os valores da região de sinal da imagem. 
    \item \textbf{Esfericidade:} Avalia o quão distante o fantoma esta de ser uma esfera, como era esperado. O código calcula dois valores de raio, que deveriam ser iguais. A menor e a maior medida do raio são utilizadas para o calculo. 
    \item \textbf{Coeficiente de Variação:} Relacionado à homogeniedade das imagens, quando mais perto de zero, maior é a homogeniedade dentro da região de sinal.
    \item \textbf{Estacionaridade do Ruído}: Confere se as imagens apresentam ruído estacionário ou não. Sendo que essa característica varia de acordo com o número de canais da bobina utilizada. 
    \item \textbf{Distribuição do ruído}: Confere se as imagens apresentam ruído com a distribuição de rayleigh. 
\end{enumerate}

\subsection{Pipeline completo}

Este pipeline tem como objetivo executar uma análise completa da qualidade de imagens de ressonância magnética. O processo inicia-se com a leitura dos dados de entrada e avança até o cálculo e a compilação de métricas de qualidade, como SNR (Relação Sinal-Ruído), distorção e esfericidade. Para tal, ele foi separado em oito etapas: 

\begin{itemize}
    \item ETAPA 1: Leitura das imagens.
    \item ETAPA 2: Visualização das imagens.
    \item ETAPA 3: Análise do ruído.
    \item ETAPA 4: Filtragem das imagens.
    \item ETAPA 5: Criação de máscaras binárias.
    \item ETAPA 6: Detecção de circulos.
    \item ETAPA 7: Cálculo de métricas.
    \item ETAPA 8: Compilação dos resultados.
\end{itemize}

\subsubsection{Entradas}

Para a execução do pipeline, os seguintes parâmetros são necessários:

\begin{itemize}
    \item \textbf{Caminho da Imagem e do Ruído:} O caminho para os arquivos NIfTI da imagem e do ruído, representados por imagem\_path e ruido\_path, respectivamente.
    \item \textbf{Detalhes Geométricos:} O Campo de Visão (FOV) em milímetros, o diâmetro real do objeto em milímetros e a dimensão da matriz em pixels.
    \item \textbf{Dimensão das ROIs:} A dimensão das Regiões de Interesse (ROIs) do ruído, em pixels.
    \item \textbf{Configurações de Saída:} Duas variáveis booleanas controlam a geração de elementos visuais: mostrar\_plots para exibir gráficos intermediários e gerar\_videos para criar vídeos do processo.
\end{itemize}

\subsubsection{Saídas}

Ao final da execução, o pipeline retorna dois DataFrames do Pandas contendo os resultados da análise:

\begin{itemize}
    \item \textbf{DataFrame Final (df\_final):} Um DataFrame completo que reúne todas as métricas de qualidade calculadas e organizadas de forma limpa.
    \item \textbf{DataFrame de Diâmetro (pd\_diametro):} Um DataFrame específico com os resultados detalhados sobre a distorção do diâmetro.
\end{itemize}

\section{Resultados e Discussão}

Os resultados serão apresentados na ordem em que as funções foram sendo implementadas no pipeline completo do código. A primeira etapa consistiu no recebimento de dois arquivos NIfTI, e na apresentação das informações da imagem, como dimensões, tipo de dado e cabeçalho. 

\begin{figure}[h!]
    \centering
    \includegraphics[width=0.5\linewidth]{Etapa_1_2.png}
    \caption{Enter Caption}
    \label{fig:placeholder}
\end{figure}

A partir da leitura desses dados, a próxima etapa é a visualização dos cortes, que pode ser em um grid ou em vídeo. O grid é mostrado direto na saída do código, mas o vídeo, quando habilitado, pode ser baixado pelo usuário.

As próximas etapas envolvem a análise do ruído e algumas operações antes dos cálculos das métricas. Primeiro, o desvio padrão e a média do ruído das imagens são calculados, sendo que o vetor utilizado foi uma média geral de todos os cortes do arquivo "ruido\_data". E 3 gráficos relacionados ao ruído foram plotados: um histograma, uma função de densidade de probabilidade (PDF) e uma função de distribuição acumulada (CDF). 

\begin{figure}[h!]
    \centering
    \includegraphics[width=0.5\linewidth]{graficos.png}
    \caption{Informações sobre as imagens e cabeçalho (Etapa 1) e cortes das imagens plotadas em um grid (Etapa 2).}
    \label{fig:placeholder}
\end{figure}

Depois da análise visual do ruído, as imagens foram passadas por um filtro gaussiano para suavizar o ruído e preparar os cortes para uma melhor segmentação da região de sinal. O método utilizado para a montagem da máscara foi o de Otsu. E, por último, a Transformada de Hough foi usada para detectar os círculos nas imagens. O cálculo das métricas é então iniciado e os resultados vão sendo salvos em dataframes. A saída indicando a passagem por essas etapas pode ser visualizada na figura 7. 

\begin{figure}[h!]
    \centering
    \includegraphics[width=0.5\linewidth]{etapas_juntas.png}
    \caption{Imagem com os gráficos do ruído (Etapa 3): Da qerquerda para a direta, há um histograma, a função de densidade de probabilidade (PDF) e a de distribuição acumulada (CDF).}
    \label{fig:placeholder}
\end{figure}

O resultado final pode ser vizualizado na figura 8. Há dois dataframes, um com as métricas dos cortes e um com as informações de distorção do maior diâmetro. Além disso, há também um exemplo de como os dados ficam ao serem disponibilizados no formato .cvc. Os dados ficam separados por vírgulas. 

\begin{figure}[h!]
    \centering
    \includegraphics[width=0.5\linewidth]{saidas_final.png}
    \caption{Saídas no console referentes iniciando com a etapa 4 e terminando com a etapa 7.}
    \label{fig:placeholder}
\end{figure}

\newpage

\section{Conclusão}

O pipeline pode ser utilizado para a avaliação da qualidade de imagem, no que diz respeito às métricas de ruído, contraste e homogeneidade, e o seu principal benefício, é a automação da extração dessas métricas, reduzindo a necessidade de medições manuais. Além disso, o código é composto por um conjunto de funções independentes, que podem ser ajustadas ou rodadas separadamente, se necessário.

Algumas limitações relacionadas ao projeto são o tipo de arquivo, que precisa ser em formato NIFTI, outros tipos de arquivo podem exigir outras funções para a leitura das imagens. O formato do fantoma precisa ser circular, pois os códigos de detecção de círculos e cálculo dos valores de raio levam em consideração apenas essa geometria. E a transformada Hough fornece apenas uma estimativa de onde há uma estrutura circular na imagem, mas os objetos imagiados não são círculos perfeitos, algumas regiões de sinal podem acabar sendo excluídas da delimitação, e regiões de ruído, incluídas.

Apesar de o projeto fornecer métricas quantitativas para avaliação da qualidade, a interpretação delas requer conhecimento especializado em ressonância magnética e processamento de imagens médicas, a fim de validar a relevância clínica dos valores e indicar possíveis problemas com as bobinas ou com o equipamento. 

Não foi possível construir um site nem implementar uma função para validar os tipos de distribuições presentes nas imagens, o que poderia ser realizado em projetos futuros. Os próximos passos para uma implementação segura em ambientes hospitalares seriam validar e testar as funções com mais imagens para garantir que todos os passos estão funcionando como esperado, além da refatoração do código, para remover redundâncias e deixá-lo mais eficiente.  

\newpage

\section{Referências}

\begin{thebibliography}

% Artigos de Periódicos
\bibitem{gruber2018rf}
\textbf{GRUBER, B. et al}. RF coils: A practical guide for nonphysicists. \textit{Journal of Magnetic Resonance Imaging}, v. 48, n. 3, p. 590--604, 13 jun. 2018.

\bibitem{ballard1981generalizing}
\textbf{BALLARD, D. H}. Generalizing the Hough transform to detect arbitrary shapes. \textit{Pattern Recognition}, v. 13, n. 2, p. 111--122, 1981.

\bibitem{canny1986computational}
\textbf{CANNY, J}. A computational approach to edge detection. \textit{IEEE Transactions on Pattern Analysis and Machine Intelligence}, v. PAMI-8, n. 6, p. 679--698, 1986.

\bibitem{gudbjartsson1995rician}
\textbf{GUDBJARTSSON, H.; PATZ, S}. The Rician distribution of noisy MRI data. \textit{Magnetic Resonance in Medicine}, v. 34, n. 6, p. 910--914, Dec. 1995. DOI: 10.1002/mrm.1910340618.

\bibitem{haralick1973textural}
\textbf{HARALICK, R. M.; SHANMUGAM, K.; DINSTEIN, I}. Textural features for image classification. \textit{IEEE Transactions on Systems, Man, Cybernetics}, v. 3, n. 6, p. 610--622, 1973.

\bibitem{mazzola2009ressonancia}
\textbf{MAZZOLA, A. A}. Ressonância magnética: princípios de formação da imagem e aplicações em imagem funcional. \textit{Revista Brasileira de Física Médica}, v. 3, n. 1, p. 117--129, 2009.

\bibitem{petrou2006image}
\textbf{PETROU, M.; SEVILLA, P. G}. \textit{Image Processing: Dealing with Texture}. John Wiley & Sons, Ltd, London, 2006.

\bibitem{thakur2016poisson}
\textbf{THAKUR, K. V.; DAMODARE, O. H.; SAPKAL, A. M}. Poisson Noise Reducing Bilateral Filter. \textit{Procedia Computer Science}, v. 79, p. 861--865, 2016.

% Livros e Capítulos de Livros
\bibitem{gonzalez2018digital}
\textbf{GONZALEZ, R. C.; WOODS, R. E}. \textit{Digital Image Processing}. 4. ed. New York, NY: Pearson, 2018.

\bibitem{rangayyan2005biomedical}
\textbf{RANGAYYAN, R. M}. Image quality and information content. In: \textit{Biomedical Image Analysis}, chapter 2, p. 39--84. CRC Press, 2005.

% Teses e Dissertações
\bibitem{domingues2020controle}
\textbf{DOMINGUES, C. V. J}. \textit{Controle da qualidade de imagem em ressonância magnética}. Tese (Mestrado) - Universidade de Lisboa, Portugal, 2020.

\bibitem{campos2020metodo}
\textbf{CAMPOS, V. P}. \textit{Método baseado em estabilização de variância para filtragem de imagens volumétricas de ressonância magnética corrompidas por ruído Riciano não-estacionário}. Dissertação UFSCAR, São Carlos, 2020. Disponível em: \url{https://www.teses.usp.br/teses/disponiveis/18/18152/tde-02022022-151520/pt-br.php}. Acesso em: 29 set. 2024.

% Documentos Normativos e Técnicos
\bibitem{acr2015quality}
\textbf{AMERICAN COLLEGE OF RADIOLOGY (ACR)}. \textit{Quality Control Manual}. Reston, VA, 2015. Disponível em: \url{https://www.researchgate.net/publication/284188389_2015_ACR_MR_QCManual_Table_of_Contents}. Acesso em: 16 fev. 2025.

\bibitem{anvisa2021instrucao}
\textbf{BRASIL. Agência Nacional de Vigilância Sanitária (ANVISA)}. \textit{Instrução Normativa nº 97, de 03 de dezembro de 2021}. Disponível em: \url{https://bvsms.saude.gov.br/bvs/saudelegis/anvisa/2021/in097_27_05_2021.pdf}. Acesso em: 25 maio 2024.

\bibitem{nema2008snr}
\textbf{NATIONAL ELECTRICAL MANUFACTURERS ASSOCIATION}. \textit{NEMA Standards Publication MS 1-2008 (R2014, R2020): Determination of Signal-to-Noise Ratio (SNR) in Diagnostic Magnetic Resonance Imaging}. Rosslyn, VA, 2008. Disponível em: \url{www.nema.org}. Acesso em: 16 fev. 2025.

% Websites
\bibitem{elster2024mriquestions}
\textbf{ELSTER LLC}. Website - MRI Questions: Signal-to-Noise Ratio in MRI, 2024. Disponível em: \url{https://mriquestions.com/index.html}. Acesso em: 29 maio 2024.

\bibitem{github}
\textbf{GITHUB}. GitHub: Where the world builds software. Disponível em: \url{https://github.com/}. Acesso em: 16 fev. 2025.

\bibitem{opencv}
\textbf{OPENCV}. Hough Circle Transform. Disponível em: \url{https://docs.opencv.org/3.4/d4/d70/tutorial_hough_circle.html}. Acesso em: 16 fev. 2025.

\bibitem{streamlit}
\textbf{STREAMLIT}. Streamlit Library. Disponível em: \url{https://streamlit.io/}. Acesso em: 16 fev. 2025.

\bibitem{scikitimage}
\textbf{SCIKIT-IMAGE}. Plotting grey-level co-occurrence matrix properties. Disponível em: \url{https://scikit-image.org/docs/stable/auto_examples/features_detection/plot_glcm.html}. Acesso em: 16 fev. 2025.

\bibitem{Nibabel}
(https://nipy.org/nibabel/nifti\_images.html)

\end{thebibliography}

\newpage
\section{Anexos}

\begin{center}
    ANEXO I - Documentação de arquitetura e design do código
\end{center}

Pipeline de processamento de imagens e análise de dados para avaliar a qualidade de imagens de ressonância magnética (MRI) no formato NIfTI. O objetivo principal é automatizar a extração de métricas de qualidade, como a Relação Sinal-Ruído (SNR), homogeneidade, contraste, distorção geométrica e esfericidade. As principais bibliotecas utilizadas foram a nibabel (para arquivos NIfTI), numpy (para computação numérica), matplotlib e seaborn (para visualização), scikit-image e opencv (para processamento de imagens) e pandas (para manipulação de dados tabulares).\\

\begin{figure}[h!]
    \centering
    \includegraphics[width=1\linewidth]{tabela.png}
    \label{fig:placeholder}
\end{figure}

\newpage
\begin{center}
    ANEXO II - Orientações de Uso
\end{center}

Abaixo há algumas orientações para rodar a função que roda todas as outras funções em oito etapas. O código está disponível no link para consulta no link: LINK!

\begin{enumerate}
    \item \textbf{Instale as bibliotecas necessárias:} O código possui um conjunto de bibliotecas que precisam ser instaladas antes da utilização do código. Para rodar localmente, crie um ambiente virtual e instale as versões mais recentes. Alguns casos podem exigir a instalação de uma versão mais antiga, visto que python é uma linguagem que está em constante aprimoramento. Algumas funções podem ser deprecadas e ou removidas.
    \item \textbf{Rodando o código no colab:} Conecte-se ao Google Drive para carregar seus arquivos de imagem e ruído diretamente do Google Drive. Rodando localmente, basta criar um diretório com os arquivos. 
    \item \textbf{Execução do Pipeline:} A função principal para rodar todo o processo é a "pipeline\_completo()", que inclui todas as etapas da análise. Para rodar a análise, basta chamar a função passando os caminhos dos seus arquivos NIfTI de imagem e ruído.
\end{enumerate}

\begin{figure}[h!]
    \centering
    \includegraphics[width=1\linewidth]{exemplo.png}
    \caption{Exemplo de como rodar o Pipeline completo}
    \label{fig:placeholder}
\end{figure}

\textbf{Parâmetros da função: }

\begin{itemize}
    \item \textbf{imagem\_path (obrigatório):} Caminho do arquivo NIfTI da sua imagem.
    \item \textbf{ruido\_path (obrigatório):} Caminho do arquivo NIfTI da sua imagem de ruído.
    \item \textbf{FOV (opcional):} O Campo de Visão da imagem em milímetros (padrão: 50).
    \item \textbf{diametro\_real (opcional):} O diâmetro real do objeto (phantom) em milímetros (padrão: 25).
    \item \textbf{matrix\_dim (opcional):} A dimensão da matriz de pixels da imagem (padrão: 256).
    \item \textbf{dim\_roi (opcional):} Dimensão das ROIs de ruído usadas na análise (padrão: 50).
    \item \textbf{mostrar\_plots (opcional):} Defina como True para exibir gráficos de todas as fatias. O padrão é True.
    \item \textbf{gerar\_videos (opcional):} Defina como True para gerar vídeos das fatias. O padrão é True.
\end{itemize}

Após a execução, a função retorna e salva dois DataFrames no diretório atual:

\begin{itemize}
    \item \textbf{resultados\_finais.csv:} Contém métricas como SNR, homogeneidade, contraste, esfericidade e CV para cada fatia.
    \item \textbf{distorcao\_diametro.csv:} Apresenta a distorção geométrica do diâmetro.
\end{itemize}

\end{document}
