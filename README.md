# Projeto de Análise de Dados - Beneficiários ProUni

Este projeto foi desenvolvido para a disciplina de Software Product, com o objetivo de analisar o perfil dos beneficiários do ProUni utilizando uma estrutura de BI (Business Intelligence) com banco de dados persistente.

## 🛠️ Stack Tecnológica
* **Storage:** Google Cloud Storage (Bucket Regional).
* **Data Warehouse:** Google BigQuery (Native Table).
* **Ferramenta de BI:** Looker Studio
* **Fonte de Dados:** Datasets do Kaggle (CSV) - https://www.kaggle.com/datasets/lfarhat/brasil-students-scholarship-prouni-20052019?resource=download

## 📂 Estrutura do Repositório
* `dashboard`: Visualização das funcionalidades.
* `\prints`: Capturas de tela do projeto .
* `README.md`: Documentação principal e acompanhamento de entregas.
* `modelo_de_dados.pdf`: Diagrama solicitado para a Prova Final.

## 🔗 Links do Projeto-
* **Board do Projeto:** https://github.com/users/PatsOliv/projects/1/views/1
* ** AC1 - Dashboard Interativo:** https://lookerstudio.google.com/u/0/reporting/3421967f-de49-4edc-bda6-59ab66df94c5/page/OHerF
* ** AC2 - Dashboard Interativo:** https://lookerstudio.google.com/u/0/reporting/c4de8f1b-1f49-40bb-935d-bfd8de672def/page/OHerF

## 📊 Acompanhamento de Entregas

### 📍 AC1 - Funcionalidade 1: Distribuição Regional
* **Objetivo:** Analisar a volumetria de bolsas concedidas por Unidade Federativa (UF).
* **Status:** Concluído.
* **Técnica:** Agrupamento por `SIGLA_UF_BENEFICIARIO_BOLSA` com filtros temporais integrados.

### 📍 AC2 - Funcionalidade 2: Perfil de Gênero e Raça
* **Objetivo:** Identificar o perfil demográfico e a representatividade dos beneficiários, mapeando a inclusão social no programa ao longo dos anos.
* **Status:** Concluído.
* **Técnica:** Análise multidimensional utilizando as colunas SEXO_BENEFICIARIO_BOLSA e RACA_COR_BENEFICIARIO_BOLSA, com visualização em barras empilhadas e gráfico de área para cruzamento de dados.

## 📊 Modelo de Dados (Requisito Prova Final)
O modelo segue a estrutura relacional para otimização de consultas de BI.
