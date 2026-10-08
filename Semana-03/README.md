# Semana 03 — DAX Intermediário (Modelagem e Time Intelligence no SIH/SUS)

🎯 **Objetivo**

Evoluir a transformação de dados (Power Query - Semana 02) para a camada de modelagem de dados e inteligência temporal no Power BI, aplicando medidas DAX avançadas com contexto de filtro, variáveis e cálculo de variações periódicas.

---

📊 **Fonte de Dados & Modelagem**

* **Dataset:** Produção Hospitalar do SUS (SIH/SUS - TabNet/DATASUS)
* **Modelagem:** Estruturação de modelo dimensional (Star Schema) conectando a tabela fato de internações à nova dimensão temporal
* **Dim_Calendario:** Criada via DAX com a função `CALENDAR()`, configurada e devidamente **marcada como Tabela de Datas** para garantir a precisão de funções de Time Intelligence

---

🔧 **Fórmulas e Medidas DAX Aplicadas**

* **Coluna Calculada de Data:** Criação de um campo de data real (`DATE`) a partir dos atributos de `Ano` e `Mes_Num`, permitindo o relacionamento correto com a `Dim_Calendario`.
* **Variáveis (`VAR` / `RETURN`):** Desenvolvimento de medidas performáticas e legíveis (`Total_Internacoes`, `Internações Consolidadas`), isolando etapas de cálculo e otimizando a avaliação do contexto.
* **Modificação de Contexto (`CALCULATE` + `ALL`):**
* `Internações Região Sudeste`: filtro fixo de contexto.
* `% do Total`: uso da função `ALL()` para ignorar filtros de linha/categoria e calcular a representatividade percentual de cada estado ou região frente ao total nacional.


* **Inteligência Temporal (Time Intelligence):**
* `Internações Mês Anterior`: cálculo via `PREVIOUSMONTH()`.
* `Variação % MoM`: cálculo de crescimento mês a mês (Month-over-Month).



---

📈 **Dashboard (Página 03)**

* **Gráfico de Tendência:** Evolução temporal com rótulos de dados ativados para leitura rápida de picos e quedas.
* **Cartões de KPI:** Exibição da variação mensal formatada em percentual com indicação de tendência.
* **Filtros Interativos:** Segmentador de dados por UF (Unidade da Federação) para análise regionalizada.

---

🐛 **Bugs Encontrados e Resoluções Técnicas**

1. **Aritmética em AnoMes_Ordem (`AAAAMM`) na Virada de Ano:**
* *Problema:* A subtração direta sobre inteiros (ex: `202602 - 6 = 202596`) gerava valores inexistentes na tabela temporal, provocando a exclusão indevida de todo o ano de 2026 nos filtros.
* *Solução:* Substituição da aritmética inteira pela função de data `EDATE()` sobre a coluna do tipo `Date`, aplicando o deslocamento correto de meses entre anos.


2. **Eixo Temporal Desordenado:**
* *Problema:* O campo textual `AnoMes` exibia os meses em ordem alfabética no gráfico.
* *Solução:* Aplicação do recurso **Classificar por Coluna** (*Sort by Column*), vinculando o campo de exibição à coluna numérica `AnoMes_Ordem`.



---

🔐 **Achados da Análise**

* **Comportamento de Filtro do CALCULATE:** Filtros explícitos inseridos dentro do `CALCULATE` (ex: `REGIAO = "Sudeste"`) sobrescrevem os filtros externos do relatório. O cartão correspondente mantém o valor fixo da região mesmo quando o usuário seleciona uma UF de outra região no segmentador.
* **Impacto do Retardo de Notificação no MoM:** Como o corte dos dados incompletos dos últimos 6 meses (regra de negócio da Semana 02) não foi propagado nativamente para as medidas "cruas" de inteligência temporal, a *Variação % MoM* exibe quedas acentuadas em períodos recentes (ex: fev/2026 próximo a -100%), refletindo o atraso do fechamento de AIH do DATASUS e não um evento real de saúde pública.

---

🧠 **O que aprendi**

* Operações matemáticas sobre representações numéricas de datas (`YYYYMM`) causam falhas lógicas em transições de ano; deslocamentos temporais em DAX devem ser feitos sempre com funções específicas de data.
* A marcação da tabela de calendário como **Tabela de Datas** é requisito fundamental para que as funções de Time Intelligence respeitem os limites de tempo e o modelo relacional sem falhar.
* Compreensão prática da ordem de avaliação de filtros: filtros explícitos no `CALCULATE` substituem o contexto da página, enquanto funções como `ALL()` removem explicitamente os filtros aplicados.
* A necessidade de tratar rigorosamente os dados administrativos de saúde antes de expor variações percentuais em relatórios executivos para evitar interpretações equivocadas de métricas de negócio.

---

⚠️ **Observações**

Laboratório prático executado em ambiente de estudos com dados abertos do SIH/SUS (DATASUS/Ministério da Saúde).

---

📸 **Evidência**
![Dashboard Semana 03]([imagens/Lab-semana03.png](https://github.com/thiagoalphabsb/Power-BI-Roadmap/blob/main/Semana-03/imagens/Lab-semana03.png))
