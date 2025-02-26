# Classificação de Imagens em uma Linha de Produção
Este repositório contém um projeto de classificação de imagens focado na identificação de objetos danificados a partir de imagens capturadas de diferentes perspectivas (topo e lateral) de uma linha de produção. O projeto foi estruturado para ser executado diretamente no [Google Colab](http://colab.research.google.com/), aproveitando a utilização de GPUs para acelerar o treinamento e análise dos modelos.

Os treinamentos foram realizados utilizando técnicas de transfer learning com redes neurais profundas, aplicando o PyTorch como framework principal. Além disso, a interpretabilidade dos resultados foi abordada com o uso do método Integrated Gradient, permitindo maior transparência nas decisões dos modelos.


## Estrutura do Repositório
- `notebooks/`: Contém os notebooks principais utilizados para as diferentes etapas do projeto:
    - `01-DataPreparation.ipynb`: Notebook responsável pela preparação dos dados.
	- `02-ModelTraining.ipynb`: Notebook para o treinamento dos modelos de classificação de imagens.
	- `03-ModelInterpretability.ipynb`: Notebook que explora a interpretabilidade dos modelos treinados.
- `transfer/`: Diretório para transferir arquivos entre notebooks, como os conjuntos de dados comprimidos e checkpoints dos modelos treinados. Optou-se por essa abordagem em vez de montar o drive para evitar o consumo desnecessário de espaço na nuvem, garantindo uma solução mais eficiente e sem a necessidade de armazenamento adicional.


## Fluxo de execução do projeto
O fluxo do projeto de classificação de imagens segue uma sequência lógica de etapas organizadas através de arquivos comprimidos e notebooks específicos:

1. `01-DataPreparation.ipynb`: O projeto começa baixando o arquivo que contém as imagens dos itens na linha de produção. O notebook de preparação de dados (`01-DataPreparation.ipynb`) é responsável por processar o conteúdo de `raw-data.zip` e organizar as imagens em treino e teste. Ao final desta etapa, o resultado é salvo no arquivo `data.zip`, que contém o conjunto de dados pronto para ser utilizado no treinamento dos modelos.

2. `02-ModelTraining.ipynb`: Em seguida, o notebook de treinamento de modelos (`02-ModelTraining.ipynb`) utiliza os dados preparados em `data.zip` para treinar diferentes arquiteturas de deep learning, como ResNet, EfficientNet e MobileNet. Após o treinamento, os modelos são salvos em um arquivo chamado `models.zip`, que armazena os checkpoints dos modelos treinados.

3. `03-ModelInterpretability.ipynb`: O notebook de interpretabilidade de modelos (`03-ModelInterpretability.ipynb`) utiliza os modelos treinados, extraídos de `models.zip`, para gerar insights e análises sobre como os modelos tomam suas decisões, aplicando a técnica Integrated Gradients para interpretar as áreas de foco nas imagens.

O fluxo do projeto foi organizado dessa forma para facilitar sua execução no Google Colab, permitindo a transferência eficiente de dados entre notebooks. Ao dividir as etapas em arquivos comprimidos e notebooks específicos, como `raw-data.zip`, `data.zip`, e `models.zip`, foi possível realizar o processamento, treinamento e interpretação dos modelos de forma modular.

Essa abordagem permite que os dados sejam manipulados e carregados em diferentes momentos do projeto, sem a necessidade de manter todas as informações na memória simultaneamente. Isso também facilita o download e upload dos arquivos entre diferentes notebooks, característica essencial para uma execução otimizada no ambiente do Colab.


## Conclusões
Após o treinamento dos modelos **MobileNet**, **EfficientNet** e **ResNet**, observou-se uma clara melhoria nas métricas de acurácia e perda no conjunto de treinamento, embora o teste mostrasse variações que indicam uma possível tendência ao overfitting, especialmente se o número de épocas aumentar. **MobileNet** se destacou com a maior acurácia no conjunto de teste, mas **EfficientNet** obteve o melhor recall para a classe `damaged`, enquanto **MobileNet** foi mais confiável na classificação de embalagens intactas (`intact`). Em geral, os modelos performaram consideravelmente melhor ao classificar imagens laterais em comparação com as de topo, obtendo maior precisão e recall para ambas as classes. No entanto, a escolha do melhor modelo não deve se basear apenas em métricas técnicas, sendo necessário considerar os aspectos críticos para o negócio.

Em termos de interpretabilidade, os mapas de atribuições de **ResNet** e **MobileNet** indicaram que os modelos focaram corretamente nas áreas relevantes das caixas de detecção. No entanto, em alguns casos **EfficientNet** deu importância excessiva à esteira, especialmente nas imagens superiores, o que sugere uma deficiência em sua interpretação. As regiões danificadas foram bem destacadas nas imagens laterais, reforçando o melhor desempenho dos modelos nessa perspectiva. 

Como melhorias, sugere-se treinar modelos específicos para cada perspectiva (lateral e superior), além de aumentar o volume de dados e o número de épocas de treinamento, utilizando checkpoints para monitorar o progresso e obter modelos mais precisos e interpretáveis.