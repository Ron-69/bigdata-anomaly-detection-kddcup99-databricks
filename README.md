# bigdata-anomaly-detection-kddcup99-databricks
Detecção Distribuída de Anomalias (Intrusão de Rede) usando Isolation Forest no PySpark (Databricks). Projeto focado em Big Data, processamento paralelo e escalabilidade.

# 🛡️ Detecção de Anomalias em Escala com PySpark (Databricks)

## 🎯 Objetivo do Projeto

Este projeto foca na experimentação e implementação de um pipeline de Machine Learning não supervisionado em um ambiente de Big Data (Databricks/Spark). O objetivo principal é demonstrar a capacidade de **escalar** a detecção de anomalias (Intrusão de Rede) no vasto dataset **KDDCup 99** utilizando processamento paralelo.

## 🔎 Desafio Abordado

O dataset KDDCup 99 possui milhões de registros e um alto grau de desbalanceamento de classes, exigindo uma abordagem distribuída para:
1.  **Manipulação de Dados Massivos:** Utilizar o poder do Spark para carregar, limpar e vetorizar dados que não caberiam na memória de uma única máquina (limitação do Databricks Community Edition).
2.  **Modelagem Escalável:** Aplicar o algoritmo **Isolation Forest** (ou substituto não supervisionado como GMM, devido às restrições do Spark MLlib base) de forma distribuída para identificar padrões de intrusão com eficiência.

## 🛠️ Tecnologias e Ferramentas

| Categoria | Tecnologia | Uso no Projeto |
| :--- | :--- | :--- |
| **Ambiente Cloud/Big Data** | **Databricks Community Edition** | Plataforma unificada para execução de notebooks e gerenciamento de clusters Spark. |
| **Processamento Paralelo** | **Apache Spark (PySpark)** | ETL (Extração, Transformação e Carga) distribuído, Vectorização e Treinamento de modelos em paralelo. |
| **Modelagem ML** | **Isolation Forest** (ou PySpark MLlib GMM) | Algoritmo não supervisionado escolhido para a detecção de *outliers* (anomalias). |
| **Linguagem/Versão** | **Python 3 / SQL (Spark)** | Lógica principal do projeto. |

## 💻 Estrutura do Workspace (Databricks)

O projeto é estruturado em um único notebook ou série de células que abordam o ciclo de vida do ML no Spark:

1.  **`Célula 1: Data Loading & Exploration`:** Carregamento do `kddcup.data` e análise do desbalanceamento.
2.  **`Célula 2: Feature Engineering & Vectorization`:** Limpeza de dados categóricos, conversão de tipos (`cast`), e uso do `VectorAssembler` do Spark MLlib.
3.  **`Célula 3: Training & Inference`:** Treinamento do modelo não supervisionado e aplicação no dataset completo para gerar os *scores* de anomalia.
4.  **`Célula 4: Evaluation`:** Análise da distribuição dos *scores* e identificação dos clusters de anomalia.

## ⚙️ Instalação e Execução

Este projeto deve ser clonado diretamente no seu ambiente **Databricks Repositories (Git Folder)**.

1.  **Clone o Repositório:** No Databricks Workspace, navegue até 'Repos' e clone este URL.
2.  **Anexe o Cluster:** Anexe o(s) notebook(s) a um cluster PySpark ativo.
3.  **Execução:** Execute as células sequencialmente para replicar a pipeline de detecção de anomalias distribuída.

---
