# Semana 01 — Fundamentos e Primeiro Dashboard (CNES)

## 🎯 Objetivo
Aprender o fluxo básico do Power BI (Obter dados → Power Query →
Modelo → DAX → Visualizações) construindo um dashboard real com
dados públicos do CNES (Cadastro Nacional de Estabelecimentos de
Saúde).

## 📊 Fonte de Dados
- CNES Nacional — opendatasus.saude.gov.br
- 636 mil estabelecimentos de saúde, 6 mil municípios, 27 UFs

## 🔧 Power Query — Tratamentos Aplicados
- Correção de tipo de coluna: CO_UNIDADE e outros códigos de
  Número para Texto (evitar notação científica e permitir
  liderança de zeros)
- Criação da tabela de apoio Dim_UF (CO_UF, SIGLA_UF, REGIAO,
  NOME_UF) e mesclagem (merge) com fato_estabelecimentos
- Criação da tabela de-para de_para_tp_unidade (32 dos 40
  códigos de tipo de estabelecimento mapeados via pesquisa em
  portarias oficiais do Ministério da Saúde) e mesclagem com
  fato_estabelecimentos

## 📐 Medidas DAX
- Total de Estabelecimentos = DISTINCTCOUNT(fato_estabelecimentos[CO_CNES])
- Total de Municípios = DISTINCTCOUNT(fato_estabelecimentos[CO_IBGE])
- Total de UFs = DISTINCTCOUNT(fato_estabelecimentos[CO_UF])

## 📈 Dashboard
- 3 Cartões KPI (Estabelecimentos, Municípios, UFs)
- Gráfico de barras: Estabelecimentos por UF (sigla)
- Gráfico de barras: Estabelecimentos por Região
- Gráfico de barras: Estabelecimentos por Tipo
- Mapa de bolhas por estado (geocodificado por NOME_UF)

## 🔐 Achados da Análise
- São Paulo lidera em número absoluto de estabelecimentos, mas
  esse número sozinho não mede "cobertura de saúde" — seria
  necessário normalizar por população (estabelecimentos por
  100 mil habitantes) para uma comparação justa entre estados.
- "Consultório Isolado" é o tipo de estabelecimento mais comum
  no Brasil, seguido de Clínica Especializada e Centro de
  Saúde/Unidade Básica.
- Região Sudeste concentra a maior parte dos estabelecimentos
  do país, coerente com concentração populacional.

## 🧠 O que aprendi
1. Bind/tipo de dado importa: notação científica em códigos de
   identificação é um bug silencioso que pode quebrar
   relacionamentos.
2. Mesclagem (merge) com Externa Esquerda preserva todas as
   linhas da tabela principal, mesmo sem correspondência —
   Interna descartaria dados sem aviso.
3. Medida explícita (DAX) vs. medida implícita: a primeira dá
   controle e reuso; a segunda é rápida mas opaca.
4. Um número absoluto nunca conta a história toda — comparação
   justa exige normalização (ex: por população).
5. Nem todo dado público vem limpo ou documentado: tabelas de
   código como TP_UNIDADE exigem pesquisa e validação manual.

## ⚠️ Observações
Laboratório realizado em ambiente pessoal, com dados públicos
e de licença aberta (CNES/DATASUS).
