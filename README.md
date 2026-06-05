# Projeto de Análise de Dados - Beneficiários ProUni

Este projeto foi desenvolvido para a disciplina de Software Product, com o objetivo de analisar o perfil dos beneficiários do ProUni utilizando uma estrutura de BI (Business Intelligence) com banco de dados persistente.

## 🛠️ Stack Tecnológica
* **Storage:** Google Cloud Storage (Bucket Regional).
* **Data Warehouse:** Google BigQuery (Native Table).
* **Ferramenta de BI:** Data Studio (antigo Looker Studio)
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
* ** AC3 - Dashboard Interativo:** https://datastudio.google.com/u/0/reporting/a123c204-157a-4f5c-ae36-032345a3cbc3/page/OHerF

## 📊 Acompanhamento de Entregas

### 📍 AC1 - Funcionalidade 1: Distribuição Regional
* **Objetivo:** Analisar a volumetria de bolsas concedidas por Unidade Federativa (UF).
* **Status:** Concluído.
* **Técnica:** Agrupamento por `SIGLA_UF_BENEFICIARIO_BOLSA` com filtros temporais integrados.

### 📍 AC2 - Funcionalidade 2: Perfil de Gênero e Raça
* **Objetivo:** Identificar o perfil demográfico e a representatividade dos beneficiários, mapeando a inclusão social no programa ao longo dos anos.
* **Status:** Concluído.
* **Técnica:** Análise multidimensional utilizando as colunas SEXO_BENEFICIARIO_BOLSA e RACA_COR_BENEFICIARIO_BOLSA, com visualização em barras empilhadas e gráfico de área para cruzamento de dados.

### 📍 AC3 - Funcionalidade 3: Tipo de Bolsa por Modalidade
* **Objetivo:** Analisar a relação entre o percentual do benefício (Integral/Parcial) e idade (Range de idades), identificando tendências de oferta.
* **Status:** Concluído.
* **Técnica:** Cruzamento de dados entre `TIPO_BOLSA` e `IDADE` utilizando gráficos de série temporal e tabelas dinâmica com mapa de calor.

## 📊 Modelo de Dados

### 📐 Modelo Entidade-Relacionamento (MER)

Para a modelagem de dados deste projeto, optou-se por uma estrutura de **Tabela Única (Flat Table / Tabela Desnormalizada)**. Por se tratar de um ambiente de Data Warehouse voltado para Analytics (BigQuery) focado no consumo direto pelo Looker Studio, não há relacionamentos com outras tabelas externas. Toda a carga de dados histórica (2005-2019) foi consolidada em uma única entidade de granularidade máxima, o que otimiza a performance das consultas, elimina a necessidade de operações de `JOIN` e garante latência zero na renderização dos filtros do dashboard.

![Modelo MER do Projeto](evidencias/modelo_mer.png)

.
.
.
.
.



### 📋 Dicionário de Dados e Engenharia do Modelo Relacional


| Nome do Campo (Atributo) | Tipo no BigQuery | Função Individual no Modelo Relacional / Analítico |
| :--- | :--- | :--- |
| **`ANO_CONCESSAO_BOLSA`** | `INTEGER` | **Atributo Categórico Temporal:** Funciona como a chave de corte cronológico do modelo. É essencial para indexar as séries temporais e permitir o agrupamento de todas as métricas ano a ano (2005-2019). |
| **`CODIGO_EMEC_IES_BOLSA`** | `INTEGER` | **Identificador Numérico Estático:** Atua como o código de identificação único da instituição de ensino no Ministério da Educação (MEC). No modelo relacional, funcionaria como uma *Chave Estrangeira (FK)* para conectar a uma tabela dimensional de faculdades. |
| **`NOME_IES_BOLSA`** | `STRING` | **Dimensão Institucional:** Atributo textual que armazena a razão social ou nome fantasia da faculdade. É usado para fazer agrupamentos, rankings e filtros das instituições que mais ofertam bolsas. |
| **`TIPO_BOLSA`** | `STRING` | **Dimensão de Benefício:** Atributo categórico de baixa cardinalidade (Integral ou Parcial). Define a regra de negócio do benefício concedido e serve para cruzar o nível de gratuidade com as demais variáveis do modelo. |
| **`MODALIDADE_ENSINO_BOLSA`** | `STRING` | **Dimensão Operacional de Ensino:** Classifica o formato da entrega do curso (Presencial ou EAD). No modelo, é uma variável qualitativa fundamental para analisar a transição e a expansão do ensino a distância ao longo das décadas. |
| **`NOME_CURSO_BOLSA`** | `STRING` | **Dimensão de Produto Acadêmico:** Armazena o nome oficial da graduação. É o atributo principal usado na **AC4** para realizar a função de agregação de contagem e gerar o *Ranking dos Top 10 Cursos* mais demandados. |
| **`NOME_TURNO_CURSO_BOLSA`** | `STRING` | **Dimensão de Segmentação:** Indica o período em que o curso é ministrado (Matutino, Vespertino, Noturno ou Integral). Serve para analisar o perfil de conveniência e acessibilidade do estudante trabalhador. |
| **`CPF_BENEFICIARIO_BOLSA`** | `STRING` | **Identificador Único Mascarado:** Atributo de granularidade máxima que identifica o estudante. No modelo relacional original, serve como a chave primária natural do aluno, usada para garantir que o mesmo estudante não seja contabilizado de forma duplicada no mesmo período. |
| **`SEXO_BENEFICIARIO_BOLSA`** | `STRING` | **Dimensão Demográfica de Gênero:** Filtro categórico qualitativo. Utilizado na **AC2** para criar as métricas de distribuição proporcional e equidade entre homens e mulheres atendidos pelo programa. |
| **`RACA_BENEFICIARIO_BOLSA`** | `STRING` | **Dimensão Demográfica Étnico-Racial:** Atributo categórico declaratório do beneficiário. Essencial na **AC2** para extrair os indicadores sociais de representatividade e inclusão social da população preta, parda, branca, amarela e indígena. |
| **`DT_NASCIMENTO_BENEFICIARIO`** | `DATE` | **Atributo Temporal do Ciclo de Vida:** Armazena a data exata de nascimento no formato `YYYY-MM-DD`. Serve como dado de origem imutável para o cálculo dinâmico da idade do estudante no momento exato em que ele ingressou no programa. |
| **`BENEFICIARIO_DEFICIENTE_FISICO`** | `STRING` | **Dimensão de Acessibilidade / Inclusão:** Variável booleana/categórica (Sim ou Não). Permite rastrear e auditar as cotas e o volume de bolsas direcionadas a pessoas com deficiência (PCD). |
| **`REGIAO_BENEFICIARIO_BOLSA`** | `STRING` | **Dimensão Macrogeográfica:** Agrupamento regional de nível superior (Sudeste, Nordeste, Norte, etc.). Facilita a análise macroeconômica e a renderização rápida de mapas de calor antes da abertura detalhada por estado. |
| **`SIGLA_UF_BENEFICIARIO_BOLSA`** | `STRING` | **Dimensão Geográfica Política:** Armazena a sigla do estado (SP, RJ, MG...). Foi o atributo principal da **AC1**, utilizado para realizar agrupamentos espaciais e identificar a concentração regional de incentivos federais. |
| **`MUNICIPIO_BENEFICIARIO_BOLSA`** | `STRING` | **Dimensão Geográfica de Alta Granularidade:** Atributo textual com o nome da cidade do aluno. Permite realizar análises de capilaridade e impacto do ProUni em municípios do interior vs. capitais. |
| **`idade`** | `INTEGER` | **Métrica Numérica Contínua / Fato:** Representa a idade calculada do estudante. No modelo de dados, funciona tanto como uma dimensão para filtros de faixas etárias quanto como um campo quantitativo para o cálculo de médias aritméticas globais ou regionais. |
