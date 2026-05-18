# 💳 Valore — Modelo de Risco e Estresse de Crédito
 
> **Lufi Data Consulting** × **CFO de Fintech**
> `Python` · `PostgreSQL` · `SQL` · `Power BI` · `Excel` · `pytest`
 
---
 
## O Problema

A carteira crescia. O capital encolhia.

A Valore Capital registrou um crescimento agressivo de 40% em 2023 através da concessão de crédito para PMEs. No entanto, o cenário inverteu-se rapidamente nos anos seguintes: em 2024, a inadimplência escalou em um ritmo superior ao crescimento da carteira e, em 2025, o spread encolheu drasticamente.

Diante da crise, o Conselho de Administração substituiu o CFO e contratou Renata Foz, ex-diretora de gestão de risco em um dos maiores conglomerados financeiros do país. A missão de Renata era clara, mas complexa: diagnosticar se a deterioração do resultado era fruto de um ciclo macroeconômico inevitável ou de um problema estrutural e silencioso na originação dos ativos.

---
 
## O Mandato
 
> *"Precisamos saber qual safra está destruindo capital antes que o cenário de estresse vire realidade."*
>
> **CFO** — Fintech de crédito estruturado · carteira com múltiplos segmentos e safras · contratou a Lufi Data Consulting em 2026
 
A fintech operava com análise de risco agregada — inadimplência total da carteira, sem granularidade por coorte de originação.
Quando o custo de captação subiu e a margem começou a comprimir, o board precisou saber uma coisa que os relatórios existentes não respondiam:
 
**qual safra estava destruindo capital silenciosamente — e quanto sobrava se o cenário piorasse?**
 
---
 
## A Pergunta Central
 
> **Qual safra está destruindo capital silenciosamente — e quanto sobra se a inadimplência subir 10 pontos percentuais?**
 
---
 
## O Diferencial
 
A maioria das análises de carteira mede inadimplência pontual — quantos contratos estão em atraso hoje.
 
**O Valore mede inadimplência acumulada por mês de vida de contrato.**
 
Isso significa: para cada safra de originação, o modelo acompanha o comportamento de cada contrato desde o mês 1 até o vencimento — construindo a curva real de deterioração por coorte.
 
A safra 2022 foi o caso de validação: seu padrão de inadimplência era visivelmente distinto das demais já aos 6 meses de vida. Nos relatórios agregados, esse sinal estava invisível.
 
---
 
## As Quatro Frentes Analíticas
 
### 🏗️ Frente 1 — Dataset Sintético e ETL
> *Antes de qualquer análise, uma carteira que simula uma operação real com problemas reais.*
 
Dataset gerado em Python com `SEED=42` para reprodutibilidade total. Duas tabelas relacionais com granularidade mensal: `contratos` (originação) e `pagamentos` (série temporal de comportamento). Cinco anomalias de risco injetadas de forma controlada e documentada — incluindo a quebra de padrão da safra 2022.
 
Pipeline completo de ETL: geração → validação → ingestão no PostgreSQL via `psycopg2`. Testes de reprodutibilidade com `pytest`.
 
### 🔍 Frente 2 — SQL Avançado e Análise Exploratória
> *A curva de safra que os relatórios agregados não mostram.*
 
PostgreSQL com schema tipado e view consolidada `vw_carteira_consolidada` como única fonte de verdade para Power BI e Excel.
 
Quatro queries analíticas principais:
- **Curva de safra** — inadimplência acumulada por mês de vida, particionada por ano de originação via window functions
- **Score de risco por segmento** — cruzamento de inadimplência realizada com ticket, prazo e garantia
- **Concentração HHI** — Índice Herfindahl-Hirschman por dimensão da carteira
- **Spread líquido** — taxa contratada vs. custo de inadimplência realizado, por safra
### 📊 Frente 3 — Power BI e DAX
> *Onde o diagnóstico vira decisão.*
 
Três páginas integradas com modelo dimensional, medidas DAX e drill-through por dimensão:
 
| Página | Foco |
|---|---|
| 1 — Curva de Safra | Inadimplência acumulada por coorte · safra 2022 destacada |
| 2 — Risco por Segmento | Matriz de risco · HHI · feature importance por dimensão |
| 3 — Spread Líquido | Waterfall de spread · YoY · safras com spread negativo |
 
### 📋 Frente 4 — Excel e Modelo de Estresse
> *O que acontece com o capital regulatório se o cenário piorar?*
 
Modelo de estresse em Excel com três cenários: base, +5pp e +10pp de inadimplência. Conexão ao Python via Power Query — atualização automática ao abrir o arquivo. Exportação de relatório PDF via `openpyxl`, sem dependência de VBA.
 
---
 
## Arquitetura
 
```
[GERAÇÃO DO DATASET]
Python (NumPy vetorizado, SEED=42)
gerar_dataset.py → ingestao.py → transformacao.py
2 tabelas · granularidade mensal · 5 anomalias de risco controladas
 
        ↓
 
[BANCO DE DADOS]
PostgreSQL
create_tables.sql → carga via psycopg2 → vw_carteira_consolidada
 
        ↓
 
[ANÁLISE SQL]
curva_safra.sql · score_segmento.sql · concentracao_hhi.sql · spread_liquido.sql
Window functions · CTEs · séries temporais mensais
 
        ↓
 
[DASHBOARD]
Power BI Desktop
Modelo dimensional · DAX · 3 páginas · drill-through
 
        ↓
 
[MODELO DE ESTRESSE]
Excel + Power Query → 3 cenários de inadimplência
Python (openpyxl) → exportação PDF sem VBA
```
 
---
 
## Stack
 
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=flat&logo=postgresql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoftexcel&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)
![pytest](https://img.shields.io/badge/pytest-0A9EDC?style=flat&logo=pytest&logoColor=white)
 
---
 
## Status do Projeto
 
🚧 **Em desenvolvimento** — início: Jun/2026
 
| Milestone | Prazo | Status |
|---|---|---|
| 1 — Setup e Dataset Sintético | 01/Jun/2026 | ⏳ Aguardando início |
| 2 — SQL Avançado e EDA | 15/Jun/2026 | ⏳ Aguardando M1 |
| 3 — Power BI e DAX | 29/Jun/2026 | ⏳ Aguardando M2 |
| 4 — Excel e Modelo de Estresse | 13/jul/2026 | ⏳ Aguardando M3 |
| 5 — Entrega Final e Publicação | 03/Oago/2026 | ⏳ Aguardando M4 |
 
---
 
## Como Reproduzir o Dataset
 
O dataset é sintético — gerado em Python com regras de negócio reais e anomalias de risco controladas.
Reprodutível com `SEED=42`. Não requer download externo.
 
```bash
pip install -r requirements.txt
 
python src/etl/gerar_dataset.py
python src/etl/ingestao.py
python src/etl/transformacao.py
python src/etl/carga_postgresql.py
```
 
Para carregar no PostgreSQL, configure as variáveis de ambiente conforme `.env.example`.
 
```bash
pytest tests/test_reproducibility.py -v
```
 
> ⏱️ Tempo estimado de geração: ~2 minutos
 
---
 
## Sobre
 
Projeto desenvolvido pela **Lufi Data Consulting** como parte do portfólio de transição de carreira para dados.
 
Engenheiro de Produção com 10 anos de experiência em operações, processos e análise administrativa — agora usando dados como ferramenta de decisão de negócio.
 
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/filholuiz)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/DataLufi)
 
---
 
*Dataset sintético gerado para fins analíticos. Cliente e narrativa fictícios criados para contextualizar a análise.*
*Período simulado: carteira com histórico de safras 2019–2024.*
