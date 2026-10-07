# Processo de Modelagem Dimensional e Transformação de Dados com DAX no Power BI

Este repositório contém a solução do desafio de modelagem em estrela (**Star Schema**) e aplicação de funções DAX desenvolvida a partir da base **Financial Sample**, como parte do programa de formação em Power BI da **DIO**.

---

## 📐 Diagrama do Modelo Star Schema

![Diagrama Star Schema](imagens/Diagrama_Star_Schema.png)

---

## 📌 Objetivo do Projeto
Desmembrar uma tabela única relacional (`Financials`) em uma estrutura dimensional Star Schema, composta por uma tabela central **Fato_Vendas** e tabelas de **Dimensão** detalhadas e agregadas, complementadas por uma dimensão temporal gerada via código **DAX**.

---

## 🏗️ Estrutura do Modelo Dimensional

### **1. Tabela Fato (`F_Vendas`)**
Contém as transações e métricas numéricas observacionais:
* `SK_ID` (Surrogate Key gerada no Power Query)
* `ID_Produto` (FK - Chave Estrangeira de Ligação)
* `Product`
* `Units Sold`
* `Sale Price`
* `Discount Band`
* `Segment`
* `Country`
* `Sales`
* `Profit`
* `Date` (FK - Dimensão Temporal)

---

### **2. Tabelas Dimensão**

* **`D_Produtos`** (Agrupamento e Agregações de Produtos):
  * `ID_Produto` (PK)
  * `Product`
  * `Média de Unidades Vendidas`
  * `Média do Valor de Vendas`
  * `Mediana do Valor de Vendas`
  * `Valor Máximo de Venda`
  * `Valor Mínimo de Venda`

* **`D_Produtos_Detalhes`**:
  * `ID_Produto` (FK)
  * `Discount Band`
  * `Sale Price`
  * `Units Sold`
  * `Manufacturing Price`

* **`D_Descontos`**:
  * `ID_Produto` (FK)
  * `Discounts`
  * `Discount Band`

* **`D_Detalhes`**:
  * `Segment`
  * `Country`
  * `Gross Sales`
  * `COGS`

* **`D_Calendario`** (Tabela Temporal gerada via DAX):
  * `Date` (PK)
  * `Ano`
  * `Número Mês`
  * `Nome Mês`
  * `Trimestre`
  * `Semestre`

* **`financials_origem`**: Tabela original mantida como backup em modo oculto.

---

## 🛠️ Etapas do Desenvolvimento

1. **Criação da Base de Backup:** Duplicação e renomeação da tabela `financials` para `financials_origem` em modo oculto.
2. **Mapeamento de Chaves Padrão (`ID_Produto`):** Implementação de Coluna Condicional no Power Query para mapear os produtos numericamente de 0 a 5.
3. **Agrupamento de Dados (*Group By*):** Construção da tabela `D_Produtos` utilizando agregadores estatísticos (Média, Mediana, Máximo e Mínimo).
4. **Modelagem Temporal em DAX:** Construção da tabela `D_Calendario` utilizando as funções `CALENDAR()`, `ADDCOLUMNS()`, `YEAR()`, `MONTH()`, `FORMAT()` e `IF()`.
5. **Relacionamentos:** Conexão das tabelas dimensão com a tabela fato em esquema estrela (Star Schema).
