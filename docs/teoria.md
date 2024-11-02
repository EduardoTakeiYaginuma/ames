# Feature Engineering

Feature Engineering é o processo de transformar dados brutos em características significativas que podem ser utilizadas em modelos de machine learning. Esse processo é crucial porque as características adequadas podem melhorar a precisão do modelo e influenciar diretamente sua capacidade de generalização.

## Conceitos Fundamentais

### 1. Variáveis Categóricas
- Variáveis categóricas são variáveis que representam grupos, categorias ou classes e não possuem uma ordem natural ou um valor numérico com significado quantitativo. Elas podem ser textos, rótulos ou símbolos que identificam diferentes categorias dentro de um conjunto de dados.

- **Exemplos de variáveis categóricas:**
  1. Cor: categorias como "vermelho", "azul" e "verde".
  2. Tipo de construção: categorias como "residencial", "comercial", "industrial".
  3. Estado civil: categorias como "solteiro", "casado", "divorciado".
  4. Classe de um imóvel (no dataset Ames): categorias como "1 andar", "2 andares", "com porão".

### 2. Variáveis Ordinais
- Variáveis ordinais são um tipo de variável categórica que possuem uma ordem ou hierarquia inerente entre suas categorias. Diferente das variáveis nominais, que não têm um significado de ordem, as variáveis ordinais indicam uma relação de maior ou menor entre as categorias.

- **Exemplos de variáveis ordinais:**
  1. Nível de escolaridade: categorias como "fundamental", "médio" e "superior".
  2. Escala de satisfação: categorias como "muito insatisfeito", "insatisfeito", "neutro", "satisfeito" e "muito satisfeito".
  3. Classificação de produtos: categorias como "1 estrela", "2 estrelas", "3 estrelas" e assim por diante.
  4. Grau de urgência: categorias como "baixo", "médio" e "alto".

### 3. Transformações de Variáveis
- **Normalização**: Ajustar a escala das variáveis numéricas para que estejam na mesma faixa (por exemplo, entre 0 e 1), melhorando a convergência de algoritmos de aprendizado.
- **Padronização**: Reduzir as características para que tenham média zero e desvio padrão um.
- **Transformações Logarítmicas**: Reduzir a influência de outliers e linearizar relações exponenciais.

### 4. Manipulação de Dados Ausentes
- **Remoção**: Excluir registros ou colunas com muitos dados faltantes.
- **Imputação**: Preencher valores ausentes usando a média, mediana, moda ou valores preditivos.

### 5. Codificação de Variáveis Categóricas
- **Label Encoding**: Converter categorias em números inteiros.
- **One-Hot Encoding**: Criar variáveis binárias para cada categoria.

### 6. Criação de Novas Variáveis
- **Interações entre Variáveis**: Criar novas características que representam a interação entre duas ou mais variáveis.
- **Variáveis Derivadas**: Criar novas características a partir de variáveis existentes.

### 7. Tratamento de Outliers
- **Identificação**: Usar métodos estatísticos (como o IQR ou z-score) para identificar e tratar outliers.

### 8. Análise de Variância
- **Análise de Variância (ANOVA)**: Utilizada para identificar se as características categóricas têm um efeito significativo sobre uma variável numérica.

### 9. Dimensionalidade
- **Redução de Dimensionalidade**: Técnicas como PCA (Análise de Componentes Principais) são usadas para reduzir o número de variáveis mantendo a maior parte da informação possível.

### 10. Validação de Recursos
- **Validação Cruzada**: Usada para avaliar a eficácia das características criadas, assegurando que o modelo não esteja superajustado a dados específicos.

## Importância do Feature Engineering

O Feature Engineering é vital porque:
- Melhora a precisão dos modelos preditivos.
- Reduz o tempo de treinamento ao eliminar variáveis irrelevantes.
- Ajuda na interpretação dos resultados, proporcionando insights significativos.

## Explorando a Relação entre Recursos e Alvo

Quando estamos explorando a relação entre uma característica individual e o alvo (neste caso, o preço de venda), é fundamental adotar a abordagem correta para garantir que nossas análises e decisões de modelagem sejam válidas e generalizáveis. Aqui está um guia sobre como proceder corretamente nessa fase de exploração.

### Importância da Separação do Conjunto de Dados

- **Evitar o Data Snooping**: O data snooping refere-se à prática de usar informações do conjunto de testes durante o desenvolvimento do modelo, o que pode levar a overfitting. Isso significa que o modelo pode se ajustar demais aos dados de treinamento e não se generalizar bem para novos dados. Para evitar isso, todas as análises que envolvem o alvo devem ser feitas somente após a divisão do conjunto de dados em conjuntos de treinamento e teste.
  
- **Separação em Treinamento e Teste**: A divisão do conjunto de dados deve ser feita antes de qualquer análise mais profunda. Uma divisão comum é usar 70-80% dos dados para treinamento e 20-30% para teste.

### Análise Pré-Treino

Antes de dividir os dados, você pode fazer uma análise inicial, como:
- **Análise Descritiva**: Obtenha uma visão geral das estatísticas descritivas das variáveis numéricas.
- **Visualização de Correlações**: Utilize gráficos de dispersão para verificar a relação entre cada recurso numérico e o alvo.

## Análise de Correlações entre Variáveis Numéricas

Quando estamos lidando com um conjunto de dados que contém várias variáveis numéricas, a análise de correlações é uma etapa importante para entender as interações entre essas variáveis. 

### Por que Procurar Correlações Altas?

- **Multicolinearidade**: Em alguns modelos, a presença de correlações altas entre variáveis independentes pode causar problemas, dificultando a distinção da contribuição de cada variável para a previsão do alvo.
- **Interpretação e Simplicidade**: Identificar e remover variáveis altamente correlacionadas pode simplificar o modelo, tornando-o mais interpretável e evitando o overfitting.

### Analisando Correlações entre Variáveis Numéricas

Abaixo, descreverei como você pode analisar as correlações entre variáveis numéricas em seu conjunto de dados:

- **Cálculo da Matriz de Correlação**: Você pode usar a função `corr()` do pandas para calcular a matriz de correlação entre as variáveis numéricas.
  
- **Visualização da Matriz de Correlação**: Uma matriz de calor (heatmap) é uma maneira eficaz de visualizar as correlações entre variáveis. Vamos usar o seaborn para isso.
