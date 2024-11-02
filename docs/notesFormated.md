<div align="center">

# Análise do Dataset de Imóveis de Ames

<div align="left" style="max-width: 750px; margin: 0 auto; font-family: 'Segoe UI', Arial, sans-serif; font-size: 105%">

## Sobre o Dataset

Este conjunto de dados contém informações detalhadas do Escritório de Avaliação de Ames, utilizadas para calcular os valores avaliados de propriedades residenciais individuais vendidas em Ames, Iowa, entre 2006 e 2010.

## ⚠️ Limitações e Escopo do Modelo

### Não Aplicável Para:

#### **Imóveis Não Residenciais**:
* Propriedades comerciais
* Propriedades industriais
* Áreas agrícolas

#### **Bairros Excluídos**:
* Blueste
* Greens
* GrnHill
* Landmrk

Devido à baixa representatividade estatística, previsões para estes bairros não serão confiáveis

#### **Características Raras**:
* Casas com piscina (apenas 1% da amostra)
* Ruas não pavimentadas (0.2% dos casos)
* Sistemas de aquecimento não-GasA

Estas características foram simplificadas ou removidas do modelo

### Restrições Temporais:
* Dados limitados ao período 2006-2010
* Não captura tendências de mercado pós-2010
* Pode não refletir mudanças significativas no mercado imobiliário atual

### Restrições Geográficas:
* Específico para Ames, Iowa
* Pode não ser generalizável para:
  * Outras cidades do estado
  * Mercados imobiliários de outros estados
  * Áreas metropolitanas maiores

## 🎯 Recomendações de Uso

### Melhor Aplicável Para:
* Imóveis residenciais unifamiliares
* Vendas convencionais (Warranty Deed)
* Bairros bem representados no dataset
* Propriedades com características padrão do mercado de Ames

## Tratamento das Variáveis

### 📊 Abordagens Gerais

Para lidar com variáveis que possuem categorias pouco representativas, foram adotadas as seguintes estratégias:

- **Remoção de categorias** com poucas ocorrências.
- **Agrupamento em uma nova categoria** chamada "Outros".
- **Exclusão total da coluna** quando apropriado.

Para variáveis com muitos valores ausentes, as seguintes abordagens foram implementadas:

- **Exclusão da coluna** quando a quantidade de dados faltantes era significativa.
- **Criação de uma categoria** "Desconhecido" ou valor específico para representar dados ausentes.
- **Preenchimento com valores calculados** (como média ou mediana) quando apropriado.

### 🏘️ Variáveis Categóricas

#### `MS.Zoning` - Classificação de Zoneamento
- Foco exclusivo em imóveis residenciais para análise mais precisa do mercado habitacional
- Remoção de propriedades classificadas como Industrial, Comercial e Agricultura para manter consistência nos dados

#### `Sale.Type` e `Sale.Condition` - Características da Venda
- Baixa representatividade de algumas condições.
- Os tipos de venda WD(Warranty Deed) 'Cenventional', 'Cash' e 'VA Loan' são agrupados em uma categoria de vendas convencionais `GroupedWD`
- Outros tipos 'COD', 'ConLI', 'Con', 'ConLD', 'Oth' e 'ConLw' são alocados em uma categoria "Others" para uma análise mais direta

#### `Street` - Tipo de Acesso
- Removida após análise de relevância estatística
- Baixíssima variabilidade: Mais de  99% das propriedades em ruas pavimentadas

#### `Condition.1` e `Condition.2` - Proximidade a Características Especiais
- Exemplo: linha de trem
- Unificação em "`Combined.Condition`" para melhor análise e interpretabilidade
- Simplificação de categorias redundantes para análise mais eficiente

#### `Misc.Feature` e `Alley` - Características Adicionais
- Referem-se a recursos não cobertos em outras categorias como por exemplo uma segunda garagem (`Misc.Feature`) e tipo de acesso a propriedade como cascalho ou pavimentado (`Alley`)
- Muitos dados faltando (*Nan*)
- Transformação em indicadores binários mais práticos:
  - `Misc.Feature` → `Shed` (True/False): Presença de galpão/depósito
  - `Alley` → `HasAlley` (True/False): Acesso por beco

#### `Exterior1st` e `Exterior2nd` - Mateial de Revestimento Exterior
- Padronização de nomenclatura e correção de inconsistências (Algumas vezes o mesmo material estava presente tanto no *Exterior1st* quanto no *Exterior2nd*)
- Consolidação de materiais menos comuns em "Others"
- Padronização de nomenclatura e correção de inconsistências (haviam muitas categorias distintas e com poucos valores)
- Eliminação de redundâncias entre camadas de revestimento 

#### `Heating` - Sistema de Aquecimento
- Removida por predominância massiva de sistemas *GasA* (98%)
- Mantida apenas como referência em análises específicas

#### `Roof.Style` e `Roof.Matl` - Características do Telhado 
- Referem-se ao tipo de material e ao estilo de teto respectivamente
- Remoção de `Roof.Matl`, pela baixa representatividade causada por praticamente todos os tetos serem feitos do material 'CompShg'
- Foco em estilos principais: *Gable* e *Hip*
- Criação da categoria *Others* para os demais estilos de telhado

#### `Mas.Vnr.Type` - Revestimento em Alvenaria
- Racionalização de categorias para análise mais efetiva
- Tratamento específico para ausência de revestimento (dados Nan classificados como None)
- Agrupamento das categorias menos representadas em *Others*

#### `MS.SubClass` e `Foundation` - Características Estruturais
- Consolidação de classes construtivas menos frequentes
- Preservação de categorias estatisticamente significativas

#### `Neighborhood` - Localização
- Remoção de micro-bairros para análise mais robusta e evitar overfit (*Blueste*, *Greens*, *GrnHill* *Landmrk*)
- Foco em áreas com representatividade estatística adequada

#### `Garage.Type` - Tipo de Garagem
- Introdução de categoria 'NoGarage' para análise completa
- Permitindo diferenciação clara entre ausência e tipos específicos

### 📐 Variáveis Ordinais

#### `Utilities` Infraestrutura
- Tipos de utilidades disponíveis no momento da venda como água, gás, eletricidade, etc
- Removemos a coluna por baixa variabilidade e relevância estatística

#### `Pool.QC` Qualidade da Pisicna
- Removemos a coluna pela falta de dados
- Apenas 13 das quase 3000 casas possuem piscina


#### `Fence` - Cercamento
- Classificação clara de ausência como "NoFence"
- Mantendo diferenciação entre tipos quando presentes

#### `Fireplace` e `Fireplace.Qu` - Qualidade da Lareira
- A variável `Fireplaces` já transmite a ideia de existência de uma lareira e é mais relevante do que a qualidade
- Removemos a coluna *Fireplace.Qu*

#### `Garage.Cond`, `Garage.Qual`, `Garage.Finish` - Características da Garagem
- Representam respectivamente a condição, qualidade e acabamento interior da garagem.
- *Garage.Finish* de uma variável ordinal para uma variável nominal (ignorar a ordem dos níveis de acabamento). 
- Tratamento uniforme de ausências como "NoGarage"
- Removemos as colunas *Garage.Cond* e *Garage.Qual* pois consideramos como irrelevântes

#### `Electrical` - Sistema Elétrico
- Representa  o sistema elétrico dos imóveis e possui categorias ordinais, indicando o tipo e a qualidade do sistema elétrico.
- Padronização baseada no padrão mais comum (SBrkr)
- Tratamento consistente de casos atípicos

#### `Basement` Features - Características do Porão
- *Bsmt.Qual* (altura), *Bsmt.Cond* (condição geral), *Bsmt.Exposure* (exposição de um imóvel com relação a um porão), *BsmtFin.Type.1* (classificação da área acabada), *BsmtFin.Type.2* (segundo tipo de acabamento no porão)
- Tratamento unificado de ausências como "NA"
- Agrupamento estratégico de características similares (Exemplo: 'Excellent' com 'Good' em *Bsmt.Cond*)

### 🔢 Variáveis Numéricas

#### `SalePrice` - Preço de Venda (TARGET)
- Transformação logarítmica para análise proporcional
    1. Erros percentuais permitem comparações diretas entre diferentes faixas de preço, oferecendo insights mais práticos sobre a precisão do modelo.
    2.  Essa abordagem estabelece uma base sólida para o desenvolvimento de um modelo preditivo eficaz.
- Aplicamos log10 nos valores target
- Melhor tratamento de outliers e distribuição dos dados

#### `Lot.Frontage` - Medida da Frente do Lote
- Preenchimento estratégico de ausências com mediana
- Manutenção da distribuição original dos dados
- Muitos dados faltando mas por estar bastante relacionado a `Lot.Area` 

#### `Garage.Yr.Blt` - Idade da Garagem
- Conversão para `Garage.Age` para análise temporal mais relevante (*Garage.Age* = *Yr.Sold* - *Garage.Yr.Blt*)
- Tratamento específico de outliers históricos

#### `Year.Remod.Add` e `Year.Built` - Idades do Imóvel
- Transformação em métricas de idade para análise temporal (*Yr.Sold* - *Year.Remod.Add* e *Yr.Sold* - *Year.Built*)
- Correção de inconsistências cronológicas

#### `Mas.Vnr.Area` - Área de Revestimento
- Tratamento de ausências como ausência real (0)
- Mantendo consistência com outras métricas de área

#### `Pool.Area` - Piscina
- Simplificação para indicador binário `HasPool`
- Foco na presença/ausência devido à baixa frequência

</div>

---

</div>