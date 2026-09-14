# Looker Studio Dashboards

[![Looker Studio](https://img.shields.io/badge/Looker%20Studio-4285F4?logo=looker&logoColor=white)](https://lookerstudio.google.com/)
[![License](https://img.shields.io/badge/License-MIT-lightgrey.svg)](LICENSE)

Reports built in Looker Studio during the **Data Analyst program at EBAC**.

> 🇧🇷 Versão em português: [README.md](README.md)

Looker Studio reports live in the cloud: there is no file to download. What this repository holds is the **live link**, captures of each page, and documentation of how each dashboard was built.

---

## Gatito e-commerce

**[▶ Open the report](https://datastudio.google.com/s/t32z_HwoxpA)**

![Gatito dashboard overview](ecommerce-gatito/img/01-visao-geral.png)

An e-commerce dashboard covering acquisition and behaviour, fed by the Google Analytics connector. Two pages, a period selector and automatic comparison against the previous period.

### Page 1 — Overview

| Indicator | Value | vs. previous |
|---|---:|---:|
| Total revenue | $211,461.94 | ▲ 20.3% |
| Transactions | 974 | ▼ 13.7% |
| Average order value | $217.11 | ▲ 39.3% |
| Conversion rate | 0.85% | ▼ 20.7% |

The combination is the interesting part: **revenue up 20% while transactions fall 14%**. All the growth came from average order value (+39%), not volume — and conversion fell alongside. Fewer people buying, each spending more.

Two competing explanations the available data cannot separate: a shift in product mix, or a shift in audience. Telling them apart would need revenue by category and by acquisition channel, neither of which is in the report.

### Page 2 — User behaviour

![User behaviour](ecommerce-gatito/img/02-comportamento-usuario.png)

| Indicator | Value | vs. previous |
|---|---:|---:|
| Sessions | 114,541 | ▲ 8.8% |
| Engaged sessions | 42,507 | ▲ 0.9% |
| Active users | 88,869 | ▲ 7.9% |
| Engagement rate | 37.11% | ▼ 7.3% |

Sessions grow 8.8%, but engaged sessions barely move (+0.9%) — hence the drop in engagement. More traffic, of lower quality.

---

## Figure consistency

The derived indicators reconcile against the base metrics:

```
Average order value = 211,461.94 / 974     = $217.11   ✓
Conversion rate     = 974 / 114,541        = 0.85%     ✓
Engagement rate     = 42,507 / 114,541     = 37.11%    ✓
Sessions per user   = 114,541 / 88,869     = 1.29      ✓
```

The user card uses `Active users`, not `7-day active users` — the latter is a rolling window that Looker Studio sums across the period, yielding a figure larger than total sessions, and whose aggregation the connector will not let you change.

📄 **[Full documentation and open points](docs/ecommerce-gatito.md)** *(in Portuguese)*

---

## Structure

```
looker-studio-dashboards/
├── README.md / README.en.md
├── LICENSE
├── ecommerce-gatito/
│   └── img/
│       ├── 01-visao-geral.png
│       └── 02-comportamento-usuario.png
└── docs/
    └── ecommerce-gatito.md
```

Further Looker Studio reports get their own subfolder as the course progresses.

## Author

**Gustavo Hortêncio da Silva** — Economist (UFPB), training as a Data Analyst at EBAC.

[![GitHub](https://img.shields.io/badge/GitHub-gstvhortencio-181717?logo=github&logoColor=white)](https://github.com/gstvhortencio)

📊 Power BI dashboards: [powerbi-dashboards](https://github.com/gstvhortencio/powerbi-dashboards)
📚 Course notes: [ebac-analise-de-dados](https://github.com/gstvhortencio/ebac-analise-de-dados)

## Licence

Released under the MIT licence. See [`LICENSE`](LICENSE).
