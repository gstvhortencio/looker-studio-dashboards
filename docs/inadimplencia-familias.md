# Inadimplência das famílias brasileiras — documentação

**[▶ Abrir o relatório](https://datastudio.google.com/reporting/863e566d-9912-46a7-b902-ac9d8ac9dbeb)** · [PDF do relatório](https://github.com/gstvhortencio/inadimplencia-familias-br/blob/main/docs/looker_studio_relatorio.pdf) · [Repositório do projeto](https://github.com/gstvhortencio/inadimplencia-familias-br)

> O Looker Studio pede login em uma conta Google. Sem login, use o PDF.

Dashboard de seis páginas sobre a inadimplência no crédito a pessoas físicas, feito como entrega do projeto de parceria **EBAC × Semantix**. Dados do SCR.data (Banco Central), de jan/2025 a set/2026, com séries do SGS para endividamento e comprometimento de renda.

A coleta, a limpeza e a modelagem estão no [repositório do projeto](https://github.com/gstvhortencio/inadimplencia-familias-br), que também tem uma versão do dashboard em Python (Dash) publicada no Render. Este documento trata só do relatório no Looker Studio.

---

## Fontes de dados

Quatro CSVs agregados, gerados por `src/gerar_bases.py` no repositório do projeto e enviados pelo conector **Upload de arquivo**:

| Arquivo | Linhas | Uso |
|---|---:|---|
| `fato_credito_pf.csv` | ~61 mil | Base principal: mês × UF × produto × escopo × renda |
| `segmento_mes.csv` | ~1,3 mil | Tipo de instituição |
| `ocupacao_mes.csv` | ~2 mil | Ocupação |
| `serie_nacional.csv` | 21 | Taxas nacionais e séries do SGS |

## Campos calculados

```
Taxa de inadimplência (%)  = SUM(carteira_inadimplida_mi) / SUM(carteira_ativa_mi) * 100
Carteira (R$ bi)           = SUM(carteira_ativa_mi) / 1000
Inadimplido (R$ bi)        = SUM(carteira_inadimplida_mi) / 1000
UF (geo)                   = CONCAT("BR-", uf)
Escopo do crédito          = CASE escopo → rótulos legíveis
```

A taxa é **razão de somas**, nunca média de uma coluna de taxa. Cada linha tem um saldo diferente, e a média simples daria o mesmo peso a um estado pequeno e a São Paulo.

## Controles

Três controles no topo de todas as páginas:

- **Escopo do crédito:** lista com seleção múltipla. O padrão é "Famílias: consumo e habitação". Crédito rural de pessoa física e um saldo legado de cartão ficam de fora por padrão, porque distorcem o diagnóstico do endividamento das famílias (ver página 1).
- **Região.**
- **Período.**

---

## Páginas

| Página | Título (a conclusão) | Componentes |
|---|---|---|
| 1. Visão geral | A inadimplência subiu mês a mês | 5 cartões, série mensal da taxa |
| 2. Produtos | Rotativo, cheque especial e cartão parcelado concentram o risco | Barras por produto, tabela com carteira e variação |
| 3. Estados | O Norte lidera a inadimplência e a Paraíba está entre as menores taxas | Barras por UF, mapa preenchido |
| 4. Perfil do tomador | Quem não tem renda declarada atrasa mais | Colunas por faixa de renda, barras por ocupação |
| 5. Crescimento × risco | Fintechs dobraram a carteira e quase dobraram a inadimplência | Tabela por tipo de instituição, início × fim do período |
| 6. Recomendações | Sete ações para três públicos | Texto: instituições, governo e famílias |

Os títulos são frases com a conclusão, não o nome da métrica. Quem abre a página lê a resposta antes de olhar o gráfico.

## Indicadores (consumo e habitação, set/2026)

| Indicador | Valor |
|---|---:|
| Dívida em atraso (% da carteira) | 4,49% |
| Variação desde jan/2025 | +1,19 p.p. |
| Total emprestado | R$ 4,03 tri |
| Dívida em atraso | R$ 180,78 bi |
| Dívida total × renda do ano (Brasil, ago/2026) | 50,11% |
| Renda mensal comprometida com dívidas (Brasil, ago/2026) | 28,99% |

Os três primeiros fecham entre si: 180,78 / 4.030 = 4,49% ✓.

---

## Leitura dos números

**A alta é contínua, mas parte dela não é das famílias.** No crédito de consumo e habitação, a taxa foi de 3,3% para 4,5% em 21 meses. No total de pessoa física a alta é maior, e quase metade dela vem do crédito rural e de um estoque legado de cartão. Por isso o filtro padrão os exclui: crédito rural e endividamento das famílias são problemas diferentes, com instrumentos diferentes (recomendação 4 da página 6).

**O risco está no crédito caro e sem garantia.** Rotativo (21,0%), cheque especial (15,2%) e cartão parcelado pelo emissor (14,1%) lideram. Habitacional (1,5%) e consignado (3,9%) ficam embaixo, protegidos por garantia real e desconto em folha.

**Geografia.** O Norte lidera, com Tocantins (6,6%) e Roraima (6,4%) no topo. A Paraíba tem a segunda menor taxa do país (3,6%), atrás só do DF (3,4%), e também a segunda menor alta desde jan/2025 (+0,5 p.p.).

**Perfil.** Quem não tem renda declarada tem 15,1% da carteira em atraso, contra 2,5% a 7,3% nas faixas com renda. Na ocupação, autônomos (5,7%) e MEI (4,7%) atrasam mais que servidores (3,2%) e empregados do setor privado (3,4%). A renda variável pesa mais que o nível de renda.

**Fintechs: crescimento rápido, risco subindo junto.** A carteira das fintechs passou de R$ 2,3 bi para R$ 5,3 bi e a taxa foi de 11,2% para 21,2%. Nos bancos, a carteira cresceu 17% e a taxa foi de 3,0% para 3,8%. O segmento é pequeno no total, mas é o de piora mais rápida.

---

## Uma armadilha que o relatório trata

A classificação de renda na base do Banco Central **oscila muito entre meses**: saldos migram entre "Sem rendimento", "Até 1 SM" e "Indisponível" sem que as pessoas tenham mudado de renda. Uma série temporal por faixa de renda mostraria saltos que são de cadastro, não de comportamento.

A página 4 compara as faixas **dentro de um mesmo mês** (set/2026) e não mostra tendência por faixa. Uma nota fixa na página explica o motivo.

---

## Pontos a revisar

### O delta do cartão principal está em verde

O cartão de inadimplência mostra "▲ 1,19%" em verde. Dois problemas:

1. **A cor está invertida.** Inadimplência subindo é uma notícia ruim, e o verde passa a mensagem oposta. No Looker Studio, isso se corrige marcando "inverter cores" na comparação do cartão.
2. **A unidade.** A variação é de 1,19 **ponto percentual** (3,30% → 4,49%), e não 1,19%. A alta relativa seria de cerca de 36%. O texto de apoio abaixo dos cartões diz "p.p.", mas o cartão sozinho mostra "%".

### Variação relativa em base pequena

Na tabela da página 2, "Cartão: à vista e parcelado lojista" aparece com +1.339% de variação na taxa. A taxa saiu de quase zero para 0,27%, e qualquer alta sobre uma base tão pequena vira um número enorme. Para taxas, a variação em pontos percentuais informa melhor.

### Paraíba sem destaque

O texto da página 3 fala da Paraíba, mas a barra de PB tem a mesma cor das outras. Uma cor de destaque só nela levaria o olho direto ao ponto do texto.

### Datas diferentes nos cartões

A taxa e os saldos são de set/2026; dívida/renda e renda comprometida são de ago/2026, porque a série do SGS sai com um mês a mais de defasagem. A nota abaixo dos cartões avisa, mas fica fácil passar batido.

## O que está bem resolvido

**Escopo como decisão de análise, não como filtro escondido.** Separar consumo e habitação do crédito rural e do legado de cartão muda a conclusão, e o controle no topo deixa o leitor ver a diferença sozinho.

**Taxa como razão de somas.** O cálculo correto para uma taxa ponderada, em todas as páginas.

**Títulos com a conclusão** e rodapé de fonte em todas as páginas.

**Recomendações separadas por público.** O relatório termina com o que fazer, não só com o diagnóstico.
