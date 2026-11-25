# Projeto-engenharia-de-dados-Consultorio-DR-Cesar

Aqui está uma estrutura completa de documentação (`README.md`) pronta para você copiar e colar no repositório do seu projeto no GitHub, consolidando a arquitetura de banco de dados em **SQL Server**, a base histórica (`2022–2025`) e a camada analítica em **Power BI**.

---

```markdown
# 🏥 Engenharia de Dados & BI: Gestão de Custos para Clínica Médica (2022–2025)

![SQL Server](https://img.shields.io/badge/SQL_Server-CC2927?style=for-the-badge&logo=microsoft-sql-server&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Data Engineering](https://img.shields.io/badge/Data_Engineering-ETL_%26_Modeling-0078D4?style=for-the-badge)

## 📌 Contexto e Problema de Negócio

Antes deste projeto, o controle financeiro e de custos da clínica médica era realizado de forma descentralizada em planilhas manuais. Esse cenário gerava gargalos críticos para a gestão:
* **Inconsistência de dados:** Erros de digitação, espaços extras e variações de caixa (maiúsculas/minúsculas) duplicavam registros de fornecedores, categorias e status.
* **Falta de padronização e valores ausentes:** Campos essenciais sem preenchimento dificultavam o rateio por centro de custo e responsável.
* **Baixa visibilidade gerencial:** A ausência de uma fonte única da verdade (*Single Source of Truth*) impedia análises rápidas sobre onde os recursos estavam sendo alocados, comprometendo o corte de desperdícios e a proteção da margem operacional.

## 🎯 Objetivos do Projeto

1. **Centralização e Governança (SQL Server):** Estruturar um banco de dados relacional normalizado para substituir o controle em planilhas.
2. **Qualidade de Dados (Data Cleaning):** Criar rotinas SQL para higienização, tratamento de nulos e padronização de domínios da base histórica (`2022–2025`).
3. **Inteligência Analítica (Power BI):** Desenvolver um painel gerencial interativo com KPIs claros para apoiar a tomada de decisão no dia a dia da clínica.

---

## 🏗️ Arquitetura e Pipeline de Dados

O fluxo de dados foi dividido em quatro etapas principais:

```text
[Planilha Legada (.csv)] 
        │
        ▼
[1. Staging & Higienização (SQL Server)] ──► TRIM, UPPER, Tratamento de NULLs e Domínios
        │
        ▼
[2. Modelagem Relacional (Star Schema)] ──► Normalização em 5 Dimensões + 1 Tabela Fato
        │
        ▼
[3. Carga & Integridade Referencial]    ──► Povoamento via INSERT/SELECT com FKs
        │
        ▼
[4. Camada Analítica (Power BI)]        ──► Modelagem DAX, KPIs e Visualização Executiva

```

### 1. Higienização e Padronização (Data Cleansing)

Na tabela de origem (`dbo.base_custos_clinica_2022_2025`), foram aplicadas regras de qualidade antes da normalização:

* **Padronização de Strings:** Aplicação de `UPPER(TRIM())` nas colunas `FORNECEDOR`, `CATEGORIA`, `CENTRO_CUSTO`, `DESCRICAO`, `STATUS` e `RESPONSAVEL` para eliminar duplicidades por formatação.
* **Tratamento de Nulos:** Substituição de valores `NULL` em atributos dimensionais obrigatórios pelo padrão `'NÃO INFORMADO'`.
* **Uniformização de Domínio:** Consolidação dos registros da coluna `STATUS` para três estados válidos: `PAGO`, `PENDENTE` e `ATRASADO`.

### 2. Modelagem de Dados (Star Schema)

Para garantir integridade referencial, performance nas consultas e escalabilidade, os dados foram normalizados em um modelo dimensional composto por **5 tabelas de dimensão** e **1 tabela fato**:

* **Tabelas Dimensão:**
* `FORNECEDOR` (`ID_FORNECEDOR`, `NOME_FORNECEDOR`)
* `CATEGORIA` (`ID_CATEGORIA`, `NOME_CATEGORIA`)
* `CENTRO_CUSTO` (`ID_CENTRO_CUSTO`, `NOME_CENTRO_CUSTO`)
* `RESPONSAVEL` (`ID_RESPONSAVEL`, `NOME_RESPONSAVEL`)
* `STATUS_CUSTO` (`ID_STATUS`, `DESCRICAO_STATUS`)


* **Tabela Fato:**
* `CUSTO` (`ID_CUSTO`, `DATA_CUSTO`, `DESCRICAO`, `VALOR`, `ID_FORNECEDOR`, `ID_CATEGORIA`, `ID_CENTRO_CUSTO`, `ID_RESPONSAVEL`, `ID_STATUS`) — com chaves estrangeiras (`FOREIGN KEY`) garantindo a integridade entre fato e dimensões.



---

## 📊 Visualização de Dados (Power BI)

O relatório foi conectado diretamente ao banco **SQL Server** normalizado, estruturado com foco em experiência do usuário (UI/UX) e leitura executiva:

* **Indicadores Principais (KPIs):** Custo Total no período (`2022–2025`), Total Pago, Total Pendente e Volume em Atraso.
* **Análises Dimensionais:**
* Evolução temporal dos custos (anual e mensal).
* Distribuição de despesas por **Centro de Custo** e **Categoria**.
* Ranking de **Fornecedores** por volume financeiro.
* Acompanhamento de **Status de Pagamento** e gastos por **Responsável**.



---

## 📂 Estrutura Sugerida do Repositório

```text
├── 📁 data/
│   └── base_custos_clinica_2022_2025.csv   # Base histórica bruta (anonimizada/amostra)
├── 📁 sql/
│   ├── 01_data_cleaning.sql                # Script de higienização e padronização
│   ├── 02_ddl_tabelas_normalizadas.sql     # Criação das tabelas Dimensão e Fato
│   └── 03_etl_carga_dados.sql              # Povoamento das dimensões e tabela fato
├── 📁 powerbi/
│   └── dashboard_custos_clinica.pbix       # Arquivo do relatório Power BI
└── README.md                               # Documentação do projeto

```

---

## 🚀 Resultados Alcançados

* **Eliminação de retrabalho manual:** Fim da consolidação manual de planilhas propensas a falhas humanas.
* **Confiabilidade financeira:** 100% dos registros padronizados e validados por chaves estrangeiras no SQL Server.
* **Agilidade gerencial:** Visão clara e centralizada dos custos por centro de custo, fornecedor e status de pagamento, permitindo identificar gargalos e proteger a margem da clínica.

---

## 🤝 Contato

Projeto desenvolvido pela **Starts BI** — Consultoria em Engenharia de Dados e Business Intelligence.

* 📱 **WhatsApp:** [(11) 96484-9144](https://www.google.com/search?q=https://wa.me/5511964849144)

```

---

### Dicas extras antes de subir no GitHub:
1. **Prints do Dashboard:** Crie uma pasta `assets/` ou `images/` no repositório e adicione capturas de tela das páginas do seu Power BI e do diagrama de relacionamentos (modelo estrela), inserindo-as na seção **Visualização de Dados** com `![Dashboard](assets/print-dashboard.png)`.
2. **Privacidade dos Dados (LGPD):** Se o arquivo `base_custos_clinica_2022_2025.csv` contiver nomes reais de fornecedores, responsáveis ou valores confidenciais da clínica, substitua por dados fictícios ou anonimizados antes de tornar o repositório público.

Você gostaria que eu também separasse o script SQL nos três arquivos organizados (`01_data_cleaning.sql`, `02_ddl.sql` e `03_etl_carga.sql`) ou criasse uma documentação das medidas DAX usadas no Power BI?

```
