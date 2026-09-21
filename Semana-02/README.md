# Semana 02 — Power Query (ETL com dados do SIH/SUS)

## 🎯 Objetivo
Aplicar técnicas de ETL (Extração, Transformação, Carga) em dados
reais de Produção Hospitalar do SUS, focando em Power Query.

## 📊 Fonte de Dados
- SIH/SUS — Internações por UF, via TabNet/DATASUS
- Período: Jul/2025 a Jul/2026, 27 UFs

## 🔧 Power Query — Tratamentos Aplicados
- Unpivot (Transformar Outras Colunas em Linhas): converteu uma
  tabela larga (1 coluna por mês) em uma tabela longa (1 linha
  por UF/mês), formato exigido para análise temporal no Power BI
- Correção de erro de tipo: concatenação de número com texto via
  Text.From()
- Coluna de ordenação customizada (AnoMes_Ordem = Ano*100+Mes_Num)
  e uso de "Classificar por Coluna", corrigindo a ordenação
  alfabética incorreta de Mes_Ano
- Divisão de coluna (código + nome do estado) e mesclagem com
  Dim_UF, trazendo SIGLA_UF e REGIAO
- Filtro dos últimos 6 meses da série (dados incompletos, sujeitos
  a atualização segundo nota oficial do TabNet)

## 📈 Dashboard
- Gráfico de linha: evolução mensal de internações no Brasil
  (Jul/2025 a Jan/2026, período "fechado")
- Gráfico de linha por Região: comparação entre as 5 regiões

## 🔐 Achados da Análise
- Identificado atraso de notificação/fechamento de AIH: os
  últimos meses de uma série do SIH/SUS aparecem artificialmente
  baixos e não devem ser interpretados como queda real
- Padrão sazonal real: queda de internações entre Outubro e
  Dezembro, possivelmente ligada à redução de procedimentos
  eletivos no período de festas
- Sudeste concentra a maior parte das internações (~500 mil/mês),
  seguido de Nordeste, Sul, Norte e Centro-Oeste

## 🧠 O que aprendi
1. Power Query é rígido com tipos de dado (diferente do Excel) —
   concatenar número com texto exige conversão explícita
2. Ordenação de texto é sempre alfabética; ordenação cronológica
   de meses exige uma coluna auxiliar numérica
3. Merge no Power Query não é dinâmico como um relacionamento de
   modelo — mudar o tipo de uma coluna numa tabela pode quebrar
   merges já configurados em outras consultas
4. Dados administrativos de saúde têm atraso de notificação: os
   períodos mais recentes de uma série raramente estão completos
5. Escalas compartilhadas em gráficos multi-série podem esconder
   variações reais em categorias de menor volume

## ⚠️ Observações
Laboratório realizado em ambiente pessoal, com dados públicos e
de licença aberta (SIH/SUS/DATASUS).
