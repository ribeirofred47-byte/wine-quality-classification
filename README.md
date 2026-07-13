# Classificação da Qualidade de Vinhos com Machine Learning

> **Tech Challenge — Fase 2 | FIAP Pós Tech em Data Analytics**

Projeto desenvolvido por **Frederico Sousa Ribeiro** e **Júlio César do Amaral Francisco** para o Tech Challenge da Fase 2 da Pós Tech em Data Analytics da FIAP.

## Sobre o projeto

A avaliação da qualidade de um vinho é tradicionalmente realizada por especialistas por meio de análises sensoriais. Esse processo pode demandar tempo e apresentar certo grau de subjetividade.

Neste projeto, utilizamos características físico-químicas de amostras de vinho para desenvolver e comparar modelos de Machine Learning capazes de apoiar a classificação da qualidade do produto.

## Objetivo

Desenvolver e comparar modelos de classificação capazes de prever se um vinho pertence à categoria de **alta qualidade** ou de **baixa/média qualidade**, utilizando suas características físico-químicas.

A variável original `quality` foi transformada em uma classificação binária:

- **Alta Qualidade (classe 1):** nota maior ou igual a 7;
- **Baixa/Média Qualidade (classe 0):** nota menor que 7.

## Base de dados

Foi utilizada a base **Wine Quality Dataset**, contendo informações físico-químicas de amostras de vinho. A base utilizada no projeto está disponível em `data/WineQT.csv`.

As variáveis analisadas incluem:

- acidez fixa (`fixed acidity`);
- acidez volátil (`volatile acidity`);
- ácido cítrico (`citric acid`);
- açúcar residual (`residual sugar`);
- cloretos (`chlorides`);
- dióxido de enxofre livre (`free sulfur dioxide`);
- dióxido de enxofre total (`total sulfur dioxide`);
- densidade (`density`);
- pH;
- sulfatos (`sulphates`);
- teor alcoólico (`alcohol`);
- nota de qualidade (`quality`).

A coluna identificadora `Id` não foi utilizada como variável preditora.

## Metodologia

O desenvolvimento foi organizado nas seguintes etapas:

1. compreensão do problema e definição da variável-alvo;
2. análise inicial e verificação da qualidade dos dados;
3. análise da distribuição das notas e do balanceamento das classes;
4. análise exploratória das variáveis físico-químicas;
5. análise de correlações;
6. identificação e avaliação de outliers pelo método IQR;
7. identificação e tratamento de registros duplicados;
8. separação estratificada entre treino e teste;
9. padronização das variáveis para a Regressão Logística;
10. treinamento de Regressão Logística e Random Forest;
11. experimentos adicionais com pesos balanceados;
12. comparação de desempenho;
13. análise da importância das variáveis;
14. interpretação dos resultados e implicações para o negócio.

## Cuidados metodológicos

Algumas decisões foram adotadas para tornar a avaliação mais confiável:

- registros duplicados nas características físico-químicas foram analisados e removidos antes da divisão entre treino e teste, reduzindo o risco de amostras idênticas aparecerem nos dois conjuntos;
- a divisão entre treino e teste utilizou estratificação para preservar aproximadamente a proporção das classes;
- o `StandardScaler` foi ajustado somente nos dados de treino e posteriormente aplicado aos dados de teste, evitando vazamento de informação;
- os outliers não foram removidos automaticamente, pois valores estatisticamente extremos podem representar amostras válidas;
- correlação e importância de variáveis foram interpretadas como associações e contribuições preditivas, não como evidências de causalidade.

## Análise exploratória

A análise revelou forte concentração de vinhos nas notas 5 e 6. Após a transformação da variável-alvo, os vinhos de alta qualidade representaram aproximadamente **13,5%** das amostras utilizadas na modelagem, caracterizando um cenário de desbalanceamento.

Entre os principais achados:

- o **teor alcoólico** apresentou a maior associação positiva com a qualidade;
- a **acidez volátil** apresentou a principal associação negativa com a qualidade;
- sulfatos, densidade e ácido cítrico também se destacaram durante as análises e a modelagem.

Essas associações não devem ser interpretadas, isoladamente, como relações de causa e efeito.

## Modelos avaliados

Foram avaliadas quatro configurações:

- Regressão Logística;
- Random Forest;
- Regressão Logística com pesos balanceados;
- Random Forest com pesos balanceados.

A Regressão Logística foi utilizada como um modelo linear e interpretável de referência. O Random Forest foi utilizado para avaliar a capacidade de capturar relações não lineares e interações entre as características físico-químicas.

## Comparação dos modelos

| Modelo | Acurácia | Precisão — Alta Qualidade | Recall — Alta Qualidade | F1-score — Alta Qualidade |
|---|---:|---:|---:|---:|
| Regressão Logística | 89,7% | 65,0% | 48,1% | 55,3% |
| Random Forest | 90,7% | 72,2% | 48,1% | 57,8% |
| Regressão Logística balanceada | 78,4% | 35,6% | 77,8% | 48,8% |
| Random Forest balanceado | 90,7% | 90,0% | 33,3% | 48,6% |

Como a classe de alta qualidade representa uma parcela minoritária da base, a análise não foi limitada à acurácia. Foram consideradas especialmente precisão, recall e F1-score da classe positiva.

## Modelo selecionado

Entre as configurações avaliadas, o **Random Forest sem balanceamento** apresentou o melhor equilíbrio geral nas métricas observadas no conjunto de teste:

- **Acurácia:** 90,7%;
- **Precisão para alta qualidade:** 72,2%;
- **Recall para alta qualidade:** 48,1%;
- **F1-score para alta qualidade:** 57,8%.

Apesar do melhor equilíbrio geral entre os modelos testados, o recall de 48,1% evidencia uma limitação importante: o modelo deixa de identificar parte relevante dos vinhos realmente pertencentes à classe de alta qualidade.

A escolha do modelo também depende do objetivo de negócio. A **Regressão Logística balanceada**, por exemplo, alcançou recall de 77,8%, identificando uma parcela maior dos vinhos de alta qualidade, porém com redução de precisão, F1-score e acurácia.

## Principais variáveis para o modelo

Segundo a importância calculada pelo Random Forest, as cinco variáveis de maior contribuição foram:

1. teor alcoólico;
2. acidez volátil;
3. sulfatos;
4. densidade;
5. ácido cítrico.

A importância das variáveis representa sua contribuição para as decisões preditivas do modelo e não comprova relações de causalidade.

## Implicações para o negócio

Os resultados indicam que características físico-químicas podem apoiar produtores e especialistas na avaliação e na padronização da qualidade dos vinhos.

Um modelo preditivo pode funcionar como ferramenta de apoio durante o processo produtivo, auxiliando na identificação de amostras com maior probabilidade de alcançar uma classificação de alta qualidade.

Entretanto, o modelo não deve substituir a avaliação técnica e sensorial realizada por especialistas. Além disso, decisões sobre o modelo final devem considerar o custo de falsos positivos e falsos negativos para o negócio.

## Limitações e oportunidades de melhoria

Entre as principais limitações e possibilidades de evolução estão:

- desbalanceamento entre as classes;
- número reduzido de amostras positivas no conjunto de teste;
- necessidade de validação cruzada estratificada para avaliar a estabilidade dos resultados;
- possibilidade de incluir matriz de confusão, ROC-AUC e PR-AUC;
- ajuste de hiperparâmetros;
- avaliação de permutation importance ou SHAP para aprofundar a interpretação;
- investigação de novas features com fundamentação no domínio.

## Estrutura do repositório

```text
wine-quality-classification/
│
├── data/
│   └── WineQT.csv
│
├── notebooks/
│   └── Tech_Challenge_2.ipynb
│
├── results/
│   ├── comparacao_modelos.csv
│   ├── correlacoes_com_qualidade.png
│   ├── distribuicao_classes.png
│   ├── distribuicao_qualidade.png
│   ├── distribuicao_variaveis.png
│   ├── importancia_variaveis.png
│   ├── matriz_correlacao.png
│   └── tabela_outliers.csv
│
├── src/
│   └── README.md
│
├── requirements.txt
├── README.md
└── .gitignore
```

## Como executar o projeto

### 1. Clonar o repositório

```bash
git clone https://github.com/ribeirofred47-byte/wine-quality-classification.git
```

### 2. Acessar a pasta

```bash
cd wine-quality-classification
```

### 3. Criar um ambiente virtual

A criação de um ambiente virtual é opcional, mas recomendada para isolar as dependências do projeto:

```bash
python -m venv .venv
```

### 4. Ativar o ambiente virtual

No Windows:

```bash
.venv\Scripts\activate
```

No macOS ou Linux:

```bash
source .venv/bin/activate
```

### 5. Instalar as dependências

```bash
pip install -r requirements.txt
```

### 6. Abrir e executar o notebook

O notebook principal está disponível em:

```text
notebooks/Tech_Challenge_2.ipynb
```

O notebook possui configuração automática de caminhos e é compatível com diferentes ambientes de execução:

- execução a partir da pasta `notebooks/` do repositório;
- execução a partir da raiz do repositório;
- execução no Google Colab com o arquivo `WineQT.csv` carregado em `/content/`.

Ao executar todas as células, os principais artefatos da análise são salvos automaticamente na pasta `results/` correspondente ao ambiente utilizado.

## Resultados gerados

A pasta `results/` reúne os principais artefatos produzidos durante a análise exploratória e a modelagem:

- `distribuicao_classes.png`: distribuição das classes binárias e evidência do desbalanceamento do alvo;
- `distribuicao_qualidade.png`: distribuição das notas originais de qualidade dos vinhos;
- `distribuicao_variaveis.png`: histogramas das variáveis físico-químicas;
- `correlacoes_com_qualidade.png`: correlação individual das variáveis com a nota de qualidade;
- `matriz_correlacao.png`: matriz de correlação entre as variáveis analisadas;
- `tabela_outliers.csv`: quantificação de possíveis outliers pelo método IQR;
- `comparacao_modelos.csv`: comparação das métricas dos modelos avaliados, incluindo os experimentos com balanceamento;
- `importancia_variaveis.png`: importância das variáveis segundo o modelo Random Forest.

## Tecnologias utilizadas

- Python;
- Pandas;
- NumPy;
- Matplotlib;
- Seaborn;
- Scikit-learn;
- Jupyter Notebook / Google Colab;
- GitHub.

## Arquivos principais

- **Base de dados:** `data/WineQT.csv`
- **Notebook principal:** `notebooks/Tech_Challenge_2.ipynb`
- **Resultados:** `results/`

## Integrantes

- Frederico Sousa Ribeiro
- Júlio César do Amaral Francisco

## Instituição

**FIAP — Pós Tech em Data Analytics**  
**Tech Challenge — Fase 2: Machine Learning Applied to Business**
