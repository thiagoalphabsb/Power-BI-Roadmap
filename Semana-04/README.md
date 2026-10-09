# Semana 04 — DAX e Indicadores de Saúde

🎯 **Objetivo**

Aprofundar o conhecimento em DAX com foco na função `CALCULATE` e manipulação do contexto de filtro, desenvolvendo indicadores avançados sobre internações hospitalares (SIH/SUS) e estabelecimentos de saúde (CNES).

---

📊 **Fonte de Dados & Modelagem**

* **Dataset:** Produção Hospitalar do SUS (SIH/SUS) e Cadastro Nacional de Estabelecimentos de Saúde (CNES)
* **Modelagem:** Modelo dimensional consolidado integrando tabelas fato e dimensões de localização e calendário (`Dim_Calendario` e `Dim_UF`)

---

🔧 **Fórmulas e Medidas DAX Aplicadas**

* **Contagens & Agregações:** Utilização de `COUNTROWS`, `COUNTA` e `DISTINCTCOUNT` sobre a coluna `CO_CNES` para análise e contagem de estabelecimentos de saúde.
* **Médias:** Desenvolvimento de `Média Mensal` e `Média Diária de Internações` utilizando `SUMX` + `CALCULATE` para contabilizar corretamente os dias com dados.
* **Acumulados:** 
  * `Internações Acumuladas`: cálculo contínuo utilizando `FILTER` + `ALL`.
  * `Acumulado no Ano`: cálculo de acumulado anual utilizando a função de inteligência temporal `TOTALYTD`.
* **Comparações & Contextos Ampliados:**
  * `Média por UF`: cálculo de média com `AVERAGEX` + `ALL`.
  * `UFs Acima da Média`: filtragem dinâmica combinando `FILTER` + `VALUES`.
  * `Estabelecimentos Região Sul`: navegação no modelo relacional com `FILTER` + `RELATED`.
* **Classificação & Formatação Condicional:** Aplicação de lógica condicional via `SWITCH` (`Status Variação` e `Cor Status`) para alimentar regras de formatação condicional no relatório.
* **Comparativo Anual:** Criação de `Internações Ano Anterior` e `Variação % Ano` com a função `SAMEPERIODLASTYEAR`.

---

📈 **Dashboard (Página 04)**

* **Cartões e Indicadores de Saúde:** Exibição clara de taxas, médias diárias e consolidados de internações e estabelecimentos.
* **Tabelas & Matrizes:** Comparativos regionais e estaduais com destaques visuais via formatação condicional baseada na medida `Cor Status`.
* **Filtros Interativos:** Segmentadores por região e UF permitindo navegação fluida pelos contextos de filtro.

---

🐛 **Bugs Encontrados e Resoluções Técnicas**

1. **Diferença de Escopo entre `ALL(coluna)` e `ALL(tabela)`:**
   * *Problema:* A fórmula `ALL(Dim_UF[SIGLA_UF])` removia apenas o filtro da UF e mantinha o filtro de Região, gerando médias distorcidas e diferentes por linha.
   * *Solução:* Substituição por `ALL(Dim_UF)`, garantindo que todos os filtros da tabela de dimensão fossem removidos para o cálculo do total geral.

2. **Acumulado "Plano" em Meses Sem Dados:**
   * *Problema:* A `Dim_Calendario` estendia-se até jul/2026, mas os dados do sistema terminavam em jan/2026. Sem validação, o acumulado repetiu o valor fixo de 8.563.097 até o final do calendário.
   * *Solução:* Tratamento da medida com a verificação `IF(NOT ISBLANK(...))`, interrompendo a exibição nos meses sem lançamentos.

3. **Variação % Falsa de -100% em fev/2026:**
   * *Problema:* O mês existia na `Dim_Calendario`, mas não continha registros na tabela fato. A medida calculava a fórmula como "0 menos o mês anterior", resultando em uma queda irreal de -100%.
   * *Solução:* Ajuste na lógica para retornar `BLANK()` quando o mês analisado não possui internações registradas.

4. **Medida Apontando para Tabela/Métrica Incorreta:**
   * *Problema:* A medida `Total Internacoes` retornava o valor fixo de 635.786 em todos os meses (referente à contagem de estabelecimentos).
   * *Solução:* Revisão das referências de colunas/tabelas na fórmula DAX para garantir o apontamento correto à tabela fato de internações.

---

🔐 **Achados da Análise**

* **Comportamento do `TOTALYTD` no Total do Cartão:** A função `TOTALYTD` na linha de total exibe apenas o acumulado do ano corrente (1.179.372) e não a soma total de toda a série histórica.
* **Comportamento de Filtro do `CALCULATE`:** A medida `Internações Região Sudeste` utiliza `REGIAO = "Sudeste"` no `CALCULATE`, sobrescrevendo o filtro externo de linha/página e mantendo o valor fixo da região.
* **Comportamento do `% do Total`:** O uso de `ALL(Dim_UF)` no denominador garante que todas as linhas dividam a quantidade pelo total nacional, resultando na soma perfeita de 100%.

---

🧠 **O que aprendi**

* Entendimento prático sobre a diferença de escopo entre limpar o filtro de uma única coluna (`ALL(Coluna)`) e de uma tabela inteira (`ALL(Tabela)`).
* Métodos para evitar o efeito "linha reta/plano" em cálculos acumulados quando o calendário é mais extenso do que a série temporal de dados reais.
* Uso de condicionais (`IF` + `ISBLANK`) para evitar distorções de cálculo (como variações de -100%) em períodos sem movimentação administrativa.
* Utilização de `SWITCH` combinada com códigos HEX/cores para dinamizar a formatação condicional diretamente por medidas DAX.

---

⚠️ **Observações & Limitações**

* **Sem base para comparação anual:** Como a série histórica cobre apenas o período de jul/2025 a jan/2026 (7 meses consolidados), a função `SAMEPERIODLASTYEAR` retorna valores vazios por ausência de dados em 2024.
* **Cadastro de Estabelecimentos:** A base do CNES não apresenta duplicidades de estabelecimentos (`COUNTROWS`, `COUNTA` e `DISTINCTCOUNT` resultam no mesmo valor de 635.786).
* Laboratório prático executado em ambiente de estudos com dados abertos do SIH/SUS e CNES (DATASUS/Ministério da Saúde).

---

📸 **Evidência**
![Dashboard Semana 04](imagens/Lab-semana04.png)
