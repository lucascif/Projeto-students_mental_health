# Projeto-students_mental_health
🧩 Students Mental Health Assessments — Predição de Depression Score
📘 Sobre o Projeto

Este projeto tem como objetivo prever o nível de depressão dos estudantes a partir de dados de saúde mental e características demográficas, utilizando regressão linear múltipla.

A análise é baseada no dataset “Students Mental Health Assessments”, publicado por Saint (Owner) no Kaggle. O estudo busca compreender quais fatores influenciam o Depression Score e avaliar a precisão de um modelo estatístico capaz de estimar esse valor com base em outras variáveis relacionadas ao bem-estar estudantil.

## 📊 Conjunto de Dados

Fonte: Kaggle – Students Mental Health Assessments

Autor: Saint (Owner)

Ano: 2023

Tipo: Dataset em formato CSV

O dataset contém informações sobre:

Dados demográficos (idade, gênero etc.)

Indicadores de ansiedade, estresse e depressão

Hábitos de estudo e fatores acadêmicos

Aspectos relacionados à saúde mental e emocional

## 🎯Objetivos

### 1. Descrição do Problema
O aumento de sintomas de depressão e estresse entre estudantes universitários vem se tornando uma preocupação crescente no ensino superior. Fatores como carga acadêmica excessiva, privação de sono, pressão por desempenho e baixo suporte emocional podem contribuir significativamente para o agravamento de problemas de saúde mental nesta população.

Embora o problema seja urgente, as evidências sobre fatores de risco modificáveis, como hábitos de vida e rotina acadêmica, ainda são limitadas.

### Objetivo
O objetivo deste projeto é investigar a associação entre um conjunto específico de fatores da vida universitária e os níveis de depressão relatados por estudantes.

Para isso, foi construído um modelo de regressão para estimar a variável alvo Depression_Score (uma escala de 0 a 5). A análise foi focada em oito variáveis de interesse, selecionadas intencionalmente a partir do dataset "Students Mental Health Assessments" (Kaggle, autor: Saint), para testar como diferentes dimensões da experiência estudantil se correlacionam com a saúde mental.

Dataset
O estudo foi conduzido utilizando o dataset público "Students Mental Health Assessments". O conjunto de dados original continha 7.022 registros.

Limpeza: Uma inspeção inicial revelou 27 valores ausentes (NaN) nas colunas CGPA (12) e Substance_Use (15). Todas as 27 linhas com dados ausentes foram removidas.

Dataset Final: A análise foi conduzida em um conjunto de dados limpo com 6.995 registros completos.

Variáveis Selecionadas
Variável Alvo (Y): Depression_Score (Pontuação de 0-5)

Variáveis Preditoras (X):

Fatores Acadêmicos:

CGPA (Média de notas acumulada)

Semester_Credit_Load (Carga de créditos no semestre)

Course (Área de estudo)

Fatores de Estilo de Vida:

Sleep_Quality (Qualidade percebida do sono)

Physical_Activity (Nível de atividade física)

Diet_Quality (Qualidade percebida da dieta)

Fatores de Contexto Pessoal:

Financial_Stress (Nível de estresse financeiro)

Social_Support (Percepção de apoio social)

🧩 Metodologia
O projeto foi dividido em duas fases: (Semana 2) construção de um modelo base e (Semana 3) refinamento, validação e comparação de modelos.

Pré-processamento e Transformação de Dados
Para preparar os dados para a modelagem, as variáveis categóricas foram convertidas em formatos numéricos:

Codificação Ordinal (Label Encoding): Variáveis com uma ordem intrínseca clara foram mapeadas para inteiros:

Sleep_Quality / Diet_Quality: ['Poor', 'Average', 'Good'] -> [0, 1, 2]

Physical_Activity / Social_Support: ['Low', 'Moderate', 'High'] -> [0, 1, 2]

Codificação Nominal (One-Hot Encoding): A variável Course foi transformada em múltiplas colunas binárias (ex: Course_Engineering, Course_Law), pois não possui uma ordem lógica.

Semana 2: Análise Exploratória (EDA) e Modelo Base
EDA: A relação entre as 8 variáveis preditoras e o Depression_Score foi investigada usando um Heatmap de Correlação e Boxplots para validar visualmente as hipóteses (ex: a relação negativa esperada entre CGPA e Depression_Score).

Modelo Base: Um modelo de LinearRegression (Scikit-learn) foi treinado usando uma divisão simples 80/20 (treino/teste) para estabelecer um baseline inicial de performance.

Semana 3: Refinamento e Validação
Métricas de Avaliação: Para avaliar os modelos de regressão, foram utilizadas as métricas:

R² (Coeficiente de Determinação): Percentual da variância da Depression_Score que o modelo consegue explicar.

RMSE (Root Mean Squared Error): O erro médio das previsões, na mesma unidade da variável alvo (pontos de 0-5).

Validação Cruzada (CV): Para obter uma estimativa de performance robusta, todos os modelos da Semana 3 foram avaliados usando K-Fold Cross-Validation (K=10).

Padronização (Pipeline): Foi criado um Pipeline com ColumnTransformer para aplicar StandardScaler (padronização de média 0 e desvio 1) apenas às features numéricas (CGPA, Financial_Stress, Semester_Credit_Load), o que é essencial para os modelos de regularização.

Otimização de Hiperparâmetros (Modelos Lineares): Foram testados modelos de regressão linear regularizada para lidar com a complexidade e selecionar features:

Ridge (L2): Otimizado com GridSearchCV para encontrar o melhor alpha.

Lasso (L1): Otimizado com GridSearchCV para encontrar o melhor alpha, permitindo a seleção automática de features (zerando coeficientes).

Testes Estatísticos: Um modelo Statsmodels OLS foi executado nos dados padronizados para obter p-values (valores-p) e determinar a significância estatística de cada variável preditora.

Otimização (Modelo Não-Linear): Um RandomForestRegressor foi treinado e otimizado usando RandomizedSearchCV (versão rápida, K=5, n=10) para capturar relações não-lineares.
## 📈 Resultados

A seguir estão os principais resultados obtidos durante as etapas de **modelagem, validação e refinamento** do projeto.

### 🔹 1. Baseline — Regressão Linear (Divisão 80/20)

**Modelo:** `LinearRegression` (Scikit-learn)  
**Divisão:** 80% treino / 20% teste (`random_state=42`)

| Métrica | Valor |
|----------|--------|
| R² | **0.0782** |
| MAE | **1.3020** |
| MSE | **2.4188** |
| RMSE | **1.5552** |

📊 **Interpretação:**  
O modelo linear simples explica cerca de **7,8% da variabilidade** de `Depression_Score`.  
O erro médio (MAE ≈ 1.30, RMSE ≈ 1.56) indica previsões em torno de **±1.5 pontos** na escala 0–5.

### 🔹 2. Validação Cruzada (K-Fold, K=10)

Para obter um baseline mais robusto, foi aplicada a **Validação Cruzada K-Fold** com K=10.

| Métrica | Média | Desvio Padrão |
|----------|--------|---------------|
| **R² (CV=10)** | **0.0682** | ± 0.0194 |
| **RMSE (CV=10)** | **1.5672** | ± 0.0303 |

📊 **Interpretação:**  
Em média, o modelo base explica **~6.8% da variância** e apresenta um erro médio de **~1.57 pontos**.  
Este resultado foi considerado o **Baseline Robusto** para comparação futura.

### 🔹 3. Padronização (Pipeline)

Um **Pipeline** com `ColumnTransformer` foi implementado para aplicar `StandardScaler`  
apenas nas colunas numéricas:

- `Semester_Credit_Load`
- `Financial_Stress`
- `CGPA`

As demais colunas (ordinais e one-hot) foram mantidas inalteradas (`passthrough`).

📘 **Resultado:**  
As métricas permaneceram idênticas ao baseline linear, confirmando que a **Regressão Linear não é sensível à escala**.  
O pipeline foi validado com sucesso e utilizado nas próximas etapas (Lasso e Ridge).

### 🔹 4. Regularização — Lasso (L1) e Ridge (L2)

Modelos de regressão regularizada foram otimizados com `GridSearchCV`:

| Modelo | Melhor Alpha | R² Médio (CV) | RMSE Médio (CV) |
|---------|---------------|---------------|-----------------|
| **Lasso (L1)** | 0.01 | **0.0689** | **1.5666** |
| **Ridge (L2)** | 10.0 | **0.0682** | **1.5672** |

📊 **Análise Lasso:**  
O modelo **Lasso** manteve 7 variáveis e **zerou 5 features**, realizando seleção automática.

| Feature | Coeficiente |
|----------|--------------|
| Course_Computer Science | **+1.1419** |
| Semester_Credit_Load | **+0.0290** |
| CGPA | **–0.0286** |
| Financial_Stress | **–0.0176** |
| Social_Support | **–0.0170** |
| Course_Medical | **–0.0152** |
| Diet_Quality | **+0.0149** |

### 🔹 5. Testes Estatísticos — OLS (Statsmodels)

Um modelo **Ordinary Least Squares (OLS)** foi treinado para verificar a significância estatística (p-value < 0.05).

| Métrica | Valor |
|----------|--------|
| R² | **0.0733** |
| R² Ajustado | **0.0717** |
| F-statistic | **46.02 (p < 0.001)** |

📘 **Variáveis Estatisticamente Significantes (p < 0.05):**

| Variável | Coeficiente | P-Value | Interpretação |
|-----------|--------------|---------|---------------|
| `Course_Computer Science` | +1.1988 | 1.4e-54 | Forte impacto positivo |
| `Semester_Credit_Load` | +0.0391 | 0.037 | Leve impacto positivo |
| `CGPA` | –0.0390 | 0.037 | Impacto negativo significativo |

📊 **Conclusão:**  
Estudantes com **maior CGPA** tendem a apresentar **menor Depression Score**,  
enquanto alunos de **Ciência da Computação** mostraram pontuações mais altas.


### 🔹 6. Modelo Não-Linear — Random Forest

Um modelo **RandomForestRegressor** foi testado com `RandomizedSearchCV` (cv=5, n_iter=10).

| Hiperparâmetros Otimizados |
|-----------------------------|
| max_depth = 10 |
| max_features = 'log2' |
| min_samples_leaf = 2 |
| min_samples_split = 10 |
| n_estimators = 139 |

| Métrica | Valor |
|----------|--------|
| R² (CV=5) | **0.0565** |
| RMSE (CV=5) | **1.5780** |

📊 **Comparação com Baseline:**

| Modelo | R² | RMSE |
|--------|-----|------|
| **Linear Regression (Baseline)** | 0.0682 | 1.5672 |
| **Random Forest** | 0.0565 | 1.5780 |

🧠 **Interpretação:**  
O modelo não-linear **não superou** o desempenho da regressão linear,  
indicando que as relações no dataset são majoritariamente **lineares e simples**.

**Top 10 Features (Importância Random Forest):**

| Feature | Importância |
|----------|-------------|
| Course_Computer Science | 0.2591 |
| CGPA | 0.2292 |
| Semester_Credit_Load | 0.1416 |
| Financial_Stress | 0.0970 |
| Social_Support | 0.0527 |
| Physical_Activity | 0.0491 |
| Diet_Quality | 0.0487 |
| Sleep_Quality | 0.0486 |
| Course_Medical | 0.0271 |
| Course_Law | 0.0185 |


### 🔹 7. Análise de Resíduos — Lasso (Melhor Modelo)

O modelo **Lasso (α = 0.01)** foi selecionado como o melhor, com **R² = 0.0689**.

Foram geradas 3 análises gráficas:

1. **Resíduos vs. Valores Previstos:** distribuição aleatória centrada em 0 ✅  
2. **Histograma dos Resíduos:** curva de sino simétrica ✅  
3. **Q-Q Plot:** pontos alinhados à diagonal ✅  

Análise de Resíduos: O modelo com melhor performance foi diagnosticado através da análise de seus erros (resíduos) para verificar vieses.
📚 Referência

SAINT. Students Mental Health Assessments. Kaggle, 2023. Disponível em: https://www.kaggle.com/datasets/sonia22222/students-mental-health-assessments
. Acesso em: 4 nov. 2025.
