# 🔬 Pipeline de Análise de Qualidade para Imagens de Ressonância Magnética

Este projeto apresenta um pipeline completo em Python para a avaliação automática da qualidade de imagens de Ressonância Magnética (RM), obtidas a partir de um fantoma sintético. O código foi desenvolvido para ser executado em um ambiente como o Google Colab e é capaz de analisar dados de bobinas de canal único e multicanal.

O objetivo principal é extrair métricas de qualidade de forma padronizada e eficiente, reduzindo a variabilidade manual e acelerando o processo de análise.

## ✨ Principais Funcionalidades

  - **Leitura de Dados NIfTI**: Carrega imagens e mapas de ruído no formato `.nii`.
  - **Análise de Ruído**: Avalia o perfil de ruído, calcula estatísticas em ROIs (Regiões de Interesse) e testa a estacionariedade.
  - **Processamento de Imagem**: Aplica filtros (Gaussiano) e segmentação (limiar de Otsu) para isolar o fantoma.
  - **Detecção de Geometria**: Utiliza a Transformada de Hough para detectar o contorno circular do fantoma.
  - **Extração de Métricas de Qualidade**: Calcula automaticamente um conjunto de indicadores importantes:
      - Relação Sinal-Ruído (SNR)
      - Distorção Geométrica (baseada no diâmetro)
      - Homogeneidade e Contraste (via Matriz de Co-ocorrência de Níveis de Cinza - GLCM)
      - Esfericidade do objeto detectado
      - Coeficiente de Variação (CV)
  - **Geração de Relatórios**: Salva os resultados em arquivos `.csv` e gera gráficos para visualização das métricas por corte (slice).

## Workflow do Pipeline

O processo de análise é dividido nas seguintes etapas sequenciais:

1.  **Leitura e Visualização**: Carregamento dos arquivos NIfTI e exibição dos cortes da imagem.
2.  **Análise de Ruído**: Análise do histograma e estatísticas das regiões de ruído.
3.  **Filtragem Gaussiana**: Suavização da imagem para reduzir o ruído e facilitar a segmentação.
4.  **Criação de Máscara Binária**: Aplicação do método de Otsu para separar o fantoma do fundo.
5.  **Detecção de Círculos**: Identificação do contorno do fantoma em cada corte.
6.  **Cálculo de Métricas**: Extração dos indicadores de qualidade com base na imagem e na máscara.
7.  **Compilação dos Resultados**: Agregação de todas as métricas em um DataFrame final, remoção de outliers e salvamento dos dados.
8.  **Visualização Final**: Plotagem de gráficos que correlacionam as principais métricas com a posição do corte.

## 🚀 Como Começar

Este pipeline foi projetado para rodar no Google Colab. Siga os passos abaixo para executá-lo.

### Pré-requisitos

Certifique-se de que suas imagens de fantoma e de ruído (formato `.nii`) estão acessíveis no seu Google Drive.

### Instalação

As bibliotecas necessárias podem ser instaladas diretamente no ambiente Colab executando o seguinte comando em uma célula:

```bash
!pip install PyDrive nibabel numpy scipy scikit-image matplotlib seaborn pandas opencv-python
```

### Configuração do Ambiente

1.  Abra o notebook (`.ipynb`) no Google Colab.
2.  A primeira etapa no código irá solicitar a montagem do seu Google Drive. Autorize o acesso para que o script possa ler seus arquivos.

<!-- end list -->

```python
from google.colab import drive
drive.mount('/content/drive')
```

## kullanım

Para executar a análise, utilize as funções principais `pipeline_completo_single` ou `pipeline_completo_mult`.

### 1\. Análise para Bobina de Canal Único

Use a função `pipeline_completo_single`. Você precisa fornecer os caminhos para a imagem e o arquivo de ruído.

```python
# Defina os caminhos para seus arquivos
imagem_path = "/content/drive/MyDrive/seu_caminho/Image_Phantom.nii"
ruido_path = "/content/drive/MyDrive/seu_caminho/Noise.nii"

# Execute o pipeline
df_resultados, df_diametro = pipeline_completo_single(
    imagem_path=imagem_path,
    ruido_path=ruido_path,
    mostrar_plots=True, # Defina como False para uma execução mais limpa
    gerar_videos=True   # Defina como False se não precisar dos vídeos
)

# Exibe o DataFrame com os resultados finais
display(df_resultados)
```

### 2\. Análise para Bobina Multicanal

Use a função `pipeline_completo_mult`.

```python
# Defina os caminhos para seus arquivos
imagem_path_mult = "/content/drive/MyDrive/seu_caminho/Image_Phantom_multicanal.nii"
ruido_path_mult = "/content/drive/MyDrive/seu_caminho/Noise_multicanal.nii"

# Execute o pipeline
df_resultados_mult, df_diametro_mult = pipeline_completo_mult(
    imagem_path1=imagem_path_mult,
    ruido_path1=ruido_path_mult
)

# Exibe os resultados
display(df_resultados_mult)
```

## 📊 Saídas do Pipeline

Ao final da execução, o pipeline irá gerar:

1.  **Arquivos CSV**:

      - `resultados_finais.csv`: Uma tabela contendo as métricas (SNR, CV, homogeneidade, contraste, etc.) para cada corte analisado.
      - `distorcao_diametro.csv`: Uma tabela com o resultado do cálculo de distorção geométrica.

2.  **Gráficos de Visualização**:

      - Um grid de plots mostrando a variação de SNR, Contraste, Homogeneidade e Coeficiente de Variação ao longo dos cortes da imagem.

3.  **Saídas Visuais (Opcional)**:

      - Visualização dos cortes da imagem original.
      - Vídeos mostrando a imagem original, a imagem filtrada, as máscaras e a detecção de círculos.

-----
