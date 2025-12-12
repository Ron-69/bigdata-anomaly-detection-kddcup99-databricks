# bigdata-anomaly-detection-kddcup99-databricks
Implementação de um pipeline de MLOps completo no Databricks/PySpark para Detecção de Intrusão de Rede (Anomalias) no dataset KDDCup 99.

# 🛡️ Detecção Distribuída de Intrusão em Escala com PySpark (Databricks)

## 🎯 Objetivo do Projeto

Este projeto evoluiu de uma experimentação não supervisionada para a implementação de um **pipeline de Machine Learning Supervisionado e de Governança (MLOps)** em um ambiente de Big Data (Databricks/Spark).

O objetivo principal é demonstrar a capacidade de **escalar** o pré-processamento, mitigação de **vazamento de dados (Data Leakage)** e o registro de modelos de **Detecção de Intrusão de Rede (Anomalias)** usando processamento paralelo.

## 🔎 Desafio Abordado e Soluções Implementadas

O dataset KDDCup 99 apresenta desafios críticos que exigiram intervenções estratégicas:

| Desafio | Estratégia de Mitigação | Validação (Métricas Finais) |
| :--- | :--- | :--- |
| **1. Colapso do Modelo (GMM)** | **StandardScaler e PCA:** Normalização das features e redução de dimensionalidade (38 → 10 componentes). | Transição para Random Forest (RF) permitiu alcançar métricas > 0.99. |
| **2. Vazamento de Dados (Data Leakage)** | **Ordem do Pipeline Corrigida:** Divisão Treino/Teste **antes** do treinamento e aplicação do `StandardScaler` e `PCA` (usando `Pipeline.fit` no treino).  | **Confirmação:** Acurácia e Recall de **0.9978** e **0.9981**, validadas em dados de teste *não vistos* pelo treinamento do scaler/PCA. |
| **3. Alta Dimensionalidade (Curse of Dimensionality)** | **PCA (Principal Component Analysis):** Redução de 38 features para 10, mantendo a variância essencial para o RF. | **Desempenho Final:** Matriz de Confusão extremamente limpa, comprovando a separabilidade das classes. |
| **4. Governança e Deployment** | **MLflow e Unity Catalog:** Registro do modelo (Versão 2) com Assinatura e Artefatos no Volume UC (`MLFLOW_DFS_TMP`). | Modelo pronto para Deployment e Inferência em Lote. |

## 📊 Performance do Modelo Final (Random Forest)

O modelo **Random Forest (numTrees=20, maxDepth=5)** treinado no conjunto de features PCA-escaladas atingiu a seguinte performance no conjunto de teste:

| Métrica | Valor | Análise em Segurança |
| :--- | :--- | :--- |
| **Acurácia** | **0.9978** | Classificação correta em 99.78% dos casos. |
| **Precisão** | **0.9992** | Extrema confiabilidade: 99.92% dos alarmes são reais (Baixíssimo FP). |
| **Recall (Sensibilidade)** | **0.9981** | **Prioridade de Segurança:** Captura 99.81% de todas as intrusões reais (Baixíssimo FN). |
| **F1-Score** | **0.9986** | Equilíbrio perfeito entre Precisão e Recall. |

**Matriz de Confusão:**

| Real / Previsto | **0.0 (Normal)** | **1.0 (Anomalia)** |
| :---: | :---: | :---: |
| **0 (Normal)** | **290.782 (TN)** | **955 (FP)** |
| **1 (Anomalia)** | **2.239 (FN)** | **1.175.818 (TP)** |

## 🛠️ Tecnologias e Ferramentas

| Categoria | Tecnologia | Uso no Projeto |
| :--- | :--- | :--- |
| **Ambiente Cloud/Big Data** | **Databricks Community Edition** | Plataforma unificada para execução de notebooks e gerenciamento de clusters Spark. |
| **Governança/MLOps** | **MLflow & Unity Catalog (UC)** | Rastreamento de experimentos, log de métricas e registro de modelo com governança e requisitos de `signature`. | 
| **Pré-processamento** | **StandardScaler & PCA** | Escalonamento de features e redução de dimensionalidade (38 → 10). |
| **Processamento Paralelo** | **Apache Spark (PySpark)** | ETL distribuído, Vectorização, Treinamento (RF) e Inferência. |
| **Modelagem ML** | **Random Forest Classifier** | Modelo final supervisionado para classificação de intrusão. |

## 💻 Estrutura do Workspace (Databricks)

O projeto é dividido em notebooks que seguem o ciclo de vida do MLOps:

1.  **`01_setup_environment.ipynb`:** Configuração do Unity Catalog (Catálogo, Schema, Volume) e ingestão de dados.
2.  **`02_etl_preprocessing.ipynb`:** ETL, Divisão Treino/Teste, Aplicação do **StandardScaler e PCA** via Spark ML `Pipeline` (mitigação de *leakage*).
3.  **`03_random_forest_classification.ipynb`:** Treinamento do Random Forest, Inferência e **Avaliação Detalhada** (Matriz de Confusão, F1, Recall).
4.  **`04_model_registration_mlflow.ipynb`:** Registro do Modelo (Versão 2) no **Unity Catalog** com `ModelSignature` e `MLFLOW_DFS_TMP`.

## ⚙️ Instalação e Execução

Para garantir a total reprodução do pipeline, siga os passos abaixo sequencialmente:

### 1. Preparação do Dataset e do Ambiente UC

A. **Download do Dataset:**
   * Acesse a página do dataset **[KDDCup 99 no Kaggle](https://www.kaggle.com/datasets/galaxyh/kdd-cup-1999-data)** ou outra fonte confiável.
   
B. **Criação do Volume no Unity Catalog (UC):**
   * No Databricks Workspace, navegue até a interface do **Catalog** (Catálogo).
   * **Crie o Catálogo:** `bigdata_anomaly_detection_kddcup99_catalogue`.
   * **Crie o Schema (Banco de Dados):** `ou utilize o default`.
   * **Clone o Repositório:** No Databricks Workspace, vá até 'Repos' e clone este URL [https://github.com/Ron-69/bigdata-anomaly-detection-kddcup99-databricks.git].
   * **Anexe o Cluster:** Anexe os notebooks a um cluster PySpark ativo (recomendado o uso de um cluster **Serverless** ou **Shared**).
   * **Crie o Volume:** `Executando a célula #1 do notebook  create_kdd_volume.ipynb`.
   * **Caminho do Volume:** O caminho final será semelhante a `/Volumes/bigdata_anomaly_detection_kddcup99_catalogue/default/kdd_volume`.

C. **Upload do Dataset:**
   * Utilize a interface de **Data Ingestion** do Databricks, clicando na opção **Upload files to a volume**. Arrastar ou **browse** para procurar.
   * Passe o volume criado

### 2. Execução dos Notebooks

1. **Execução Sequencial:** Execute os notebooks na seguinte ordem:
    * **`01_setup_environment.ipynb`:** (Confirma as permissões de acesso ao Volume e verifica o arquivo raw).
    * **`02_etl_preprocessing.ipynb`:** Executa ETL, Scaling, PCA e Split (Treino/Teste).
    * **`03_random_forest_classification.ipynb`:** Treinamento e Avaliação do modelo RF.
    * **`04_model_registration_mlflow.ipynb`:** Registro do modelo (Versão N) no Unity Catalog.
