# Dashboards em Looker Studio

[![Looker Studio](https://img.shields.io/badge/Looker%20Studio-4285F4?logo=looker&logoColor=white)](https://lookerstudio.google.com/)
[![License](https://img.shields.io/badge/License-MIT-lightgrey.svg)](LICENSE)

Relatórios construídos no Looker Studio durante o curso de **Analista de Dados da EBAC**, incluindo o projeto de parceria EBAC × Semantix.

> 🇬🇧 English version: [README.en.md](README.en.md)

Relatórios do Looker Studio vivem na nuvem: não há arquivo a baixar. O que este repositório guarda é o **link vivo**, as capturas de cada página e a documentação de como cada dashboard foi montado.

---

## Inadimplência das famílias brasileiras

**[▶ Abrir o relatório](https://datastudio.google.com/reporting/863e566d-9912-46a7-b902-ac9d8ac9dbeb)** · [PDF](https://github.com/gstvhortencio/inadimplencia-familias-br/blob/main/docs/looker_studio_relatorio.pdf) · [Repositório do projeto](https://github.com/gstvhortencio/inadimplencia-familias-br)

> O Looker Studio pede login em uma conta Google. Sem login, use o PDF.

![Visão geral do dashboard de inadimplência](inadimplencia-familias/img/01-visao-geral.png)

Projeto de parceria **EBAC × Semantix**: seis páginas sobre a inadimplência no crédito a pessoas físicas, com dados do SCR.data (Banco Central) de jan/2025 a set/2026 e séries do SGS. Filtros de escopo do crédito, região e período em todas as páginas.

| Página | Conclusão |
|---|---|
| Visão geral | No crédito de consumo e habitação, a inadimplência subiu de 3,3% para 4,5% em 21 meses |
| Produtos | Rotativo (21%), cheque especial (15%) e cartão parcelado (14%) concentram o risco |
| Estados | O Norte lidera; a Paraíba tem a segunda menor taxa do país (3,6%) |
| Perfil do tomador | Sem renda declarada: 15,1% em atraso, contra 2,5% a 7,3% nas faixas com renda |
| Crescimento × risco | Fintechs mais que dobraram a carteira, e a taxa foi de 11,2% para 21,2% |
| Recomendações | Sete ações para instituições, governo e famílias |

A decisão de análise mais importante está no filtro de escopo: crédito rural e um saldo legado de cartão respondem por quase metade da alta no total de pessoa física. Misturados ao consumo das famílias, eles distorcem o diagnóstico, por isso ficam fora no padrão.

<details>
<summary>Ver as outras páginas</summary>

![Produtos](inadimplencia-familias/img/02-produtos.png)
![Estados](inadimplencia-familias/img/03-estados.png)
![Perfil do tomador](inadimplencia-familias/img/04-perfil.png)
![Crescimento × risco](inadimplencia-familias/img/05-crescimento-risco.png)
![Recomendações](inadimplencia-familias/img/06-recomendacoes.png)

</details>

📄 **[Documentação completa e pontos a revisar](docs/inadimplencia-familias.md)**

---

## E-commerce Gatito

**[▶ Abrir o relatório](https://datastudio.google.com/s/t32z_HwoxpA)**

![Visão geral do dashboard Gatito](ecommerce-gatito/img/01-visao-geral.png)

Dashboard de e-commerce com métricas de aquisição e comportamento, alimentado pelo conector do Google Analytics. Duas páginas, seletor de período e comparação automática contra o período anterior.

### Página 1 — Visão Geral

| Indicador | Valor | vs. período anterior |
|---|---:|---:|
| Receita total | $211.461,94 | ▲ 20,3% |
| Transações | 974 | ▼ 13,7% |
| Ticket médio | $217,11 | ▲ 39,3% |
| Taxa de conversão | 0,85% | ▼ 20,7% |

A combinação é a parte interessante: **receita sobe 20% enquanto transações caem 14%**. O crescimento veio inteiramente de ticket médio (+39%), não de volume — e a conversão caiu junto. Menos gente comprando, cada uma gastando mais.

Duas explicações concorrentes que os dados presentes não separam: mudança de mix de produto ou mudança de público. Distinguir exigiria receita por categoria e por canal de aquisição, nenhuma das duas no relatório.

### Página 2 — Comportamento do usuário

![Comportamento do usuário](ecommerce-gatito/img/02-comportamento-usuario.png)

| Indicador | Valor | vs. período anterior |
|---|---:|---:|
| Sessões | 114.541 | ▲ 8,8% |
| Sessões engajadas | 42.507 | ▲ 0,9% |
| Usuários ativos | 88.869 | ▲ 7,9% |
| Taxa de engajamento | 37,11% | ▼ 7,3% |

Sessões crescem 8,8%, mas sessões engajadas quase não se movem (+0,9%) — daí a queda no engajamento. Mais tráfego, de qualidade menor.

---

## Consistência dos números

Os indicadores derivados fecham com as métricas base:

```
Ticket médio        = 211.461,94 / 974      = $217,11   ✓
Taxa de conversão   = 974 / 114.541         = 0,85%     ✓
Taxa de engajamento = 42.507 / 114.541      = 37,11%    ✓
Sessões por usuário = 114.541 / 88.869      = 1,29      ✓
```

O cartão de usuários usa `Usuários ativos` e não `Usuários ativos por 7 dias` — essa segunda é uma janela móvel que o Looker Studio soma ao longo do período, produzindo um número maior que o total de sessões, e cuja agregação o conector não deixa alterar.

📄 **[Documentação completa e pontos a revisar](docs/ecommerce-gatito.md)**

---

## Estrutura

```
looker-studio-dashboards/
├── README.md / README.en.md
├── LICENSE
├── inadimplencia-familias/
│   └── img/                       6 capturas, uma por página
├── ecommerce-gatito/
│   └── img/
│       ├── 01-visao-geral.png
│       └── 02-comportamento-usuario.png
└── docs/
    ├── inadimplencia-familias.md
    └── ecommerce-gatito.md
```

Novos relatórios do Looker Studio entram como subpastas próprias conforme o curso avança.

## Autor

**Gustavo Hortêncio da Silva** — Economista (UFPB), em formação como Analista de Dados pela EBAC.

[![GitHub](https://img.shields.io/badge/GitHub-gstvhortencio-181717?logo=github&logoColor=white)](https://github.com/gstvhortencio)

📊 Dashboards em Power BI: [powerbi-dashboards](https://github.com/gstvhortencio/powerbi-dashboards)
📚 Anotações do curso: [ebac-analise-de-dados](https://github.com/gstvhortencio/ebac-analise-de-dados)

## Licença

Distribuído sob a licença MIT. Veja [`LICENSE`](LICENSE).
