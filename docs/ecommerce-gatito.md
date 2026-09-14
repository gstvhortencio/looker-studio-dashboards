# E-commerce Gatito — documentação

**[▶ Abrir o relatório](https://datastudio.google.com/s/t32z_HwoxpA)**

Dashboard de duas páginas com métricas de e-commerce e comportamento web, no padrão de análise do Google Analytics. Período coberto: 17 de agosto a 13 de setembro, com comparação automática contra o período anterior.

---

## Estrutura

| Página | Foco | Componentes |
|---|---|---|
| Visão Geral | Resultado comercial | 4 cartões com delta, série diária de taxa de conversão |
| Comportamento do usuário | Tráfego e engajamento | 4 cartões com delta, série diária de usuários ativos |

Elementos comuns às duas: cabeçalho com logotipo, seletor de período, link para a base de dados.

## Indicadores

### Página 1 — Visão Geral

| Indicador | Valor | Δ |
|---|---:|---:|
| Receita total | $211.461,94 | ▲ 20,3% |
| Transações | 974 | ▼ 13,7% |
| Ticket médio | $217,11 | ▲ 39,3% |
| Taxa de conversão | 0,85% | ▼ 20,7% |

### Página 2 — Comportamento do usuário

| Indicador | Valor | Δ |
|---|---:|---:|
| Sessões | 114.541 | ▲ 8,8% |
| Sessões engajadas | 42.507 | ▲ 0,9% |
| Usuários ativos | 88.869 | ▲ 7,9% |
| Taxa de engajamento | 37,11% | ▼ 7,3% |

---

## Leitura dos números

**Receita cresce sem volume.** Receita +20,3% com transações −13,7%. Todo o crescimento veio de ticket médio (+39,3%). Menos pedidos, cada um maior.

Duas explicações concorrentes que os dados presentes não separam: mudança de mix (produtos mais caros ganhando participação) ou mudança de público (um segmento de menor volume e maior valor). Distinguir exigiria receita por categoria de produto e por canal de aquisição — nenhuma das duas está no relatório.

**Tráfego cresce, qualidade cai.** Sessões +8,8%, mas sessões engajadas apenas +0,9%. O tráfego incremental praticamente não engajou, e a taxa de engajamento caiu 7,3 p.p. em termos relativos. Combinado com a queda de 20,7% na conversão, o padrão sugere aquisição de tráfego menos qualificado no período.

**A série de conversão é ruidosa.** Oscila entre 0,3% e 1,4% dia a dia, sem tendência clara. Com 974 transações em 28 dias — cerca de 35 por dia — a variação diária é dominada por ruído amostral. Uma média móvel de 7 dias mostraria a tendência que a série bruta esconde.

---

## Uma armadilha que o relatório evita

O conector do Google Analytics expõe a métrica `_active7DayUsers_` ("usuários ativos por 7 dias"). Ela parece a escolha natural para um cartão de KPI e **não é** — é uma janela móvel, e o Looker Studio a agrega por soma ao longo do período, contando o mesmo usuário até sete vezes.

Num período de 28 dias com cerca de 23 mil usuários ativos por dia, o cartão exibiria algo em torno de 646 mil — mais que o total de sessões do período, que é impossível.

Pior: **a agregação não pode ser corrigida**. Métricas vindas de conector têm a agregação definida pelo próprio conector; no painel de propriedades do campo o seletor aparece travado em *Automático*.

A saída é trocar a métrica. O cartão usa `Usuários ativos` (`activeUsers`), que agrega corretamente sobre qualquer período — daí os 88.869, coerentes com as 114.541 sessões (cerca de 1,29 sessão por usuário).

A série diária, essa sim, usa `_active7DayUsers_` — e está correta, porque num gráfico por dia a métrica é exatamente o que promete. O problema existia só na soma de um cartão.

---

## Pontos a revisar

### Eixo truncado na página 2

O gráfico de usuários ativos começa em 20 mil, não em zero. A variação real no período é de cerca de 20%, mas o gráfico faz parecer uma queda dramática no final.

Truncar o eixo em série temporal é defensável — partir do zero achataria a série. Vale saber que amplifica a percepção de movimento.

### A série de conversão é ruidosa demais para leitura diária

Oscila entre 0,3% e 1,4% dia a dia, sem tendência clara. Com 974 transações em 28 dias — cerca de 35 por dia — a variação diária é dominada por ruído amostral.

**Sugestão** — acrescentar uma média móvel de 7 dias, que mostraria a tendência hoje escondida pela oscilação.

## O que está bem resolvido

**Comparação contra período anterior em todos os cartões.** Um valor absoluto sozinho não informa; `$211.461,94 ▲ 20,3%` informa. É o tipo de decisão que separa um painel útil de uma lista de números.

**Seletor de período funcional.** O relatório é navegável, não estático — quem abre pode recortar o período que interessa.

**Separação entre resultado e comportamento.** Página 1 responde "quanto vendemos", página 2 responde "o que aconteceu no site". A divisão é a mesma que se usa em análise de e-commerce de verdade.

**Os indicadores derivados fecham.** Ticket médio, taxa de conversão e taxa de engajamento batem exatamente com as divisões das métricas base — sinal de que os campos calculados estão corretos.
