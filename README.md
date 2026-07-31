# SBC Doenças — Validação Cruzada Bidirecional entre Dengue e Malária

> Um estudo de generalização de modelos preditivos de Machine Learning aplicados a hemogramas, avaliando se um classificador treinado para uma doença febril consegue diagnosticar outra.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Status](https://img.shields.io/badge/status-research-orange)]()

## Sobre o projeto

Doenças febris como **dengue** e **malária** compartilham sintomas iniciais inespecíficos (febre, cefaleia, dores musculares), o que dificulta o diagnóstico clínico isolado, especialmente em regiões onde ambas circulam simultaneamente.

Este projeto investiga se **modelos de aprendizado de máquina treinados com dados de hemograma** para diagnosticar uma dessas doenças conseguem **generalizar** para identificar a outra — testando a transferência preditiva nos dois sentidos (dengue → malária e malária → dengue).

O trabalho foi desenvolvido como artigo científico e submetido a um evento da SBC (Sociedade Brasileira de Computação).

## Pergunta de pesquisa

> Modelos treinados com hemogramas para diagnóstico de uma doença infecciosa possuem capacidade de generalização preditiva para identificar infecções em cenários epidemiológicos distintos?

## Metodologia

- **Dados:** dois conjuntos independentes de hemogramas (dengue e malária), com pacientes sobrepostos entre as bases separados em um conjunto extra de validação externa ("compartilhado") para evitar vazamento de dados.
- **Atributos:** 10 variáveis hematológicas em comum — Hemoglobina, Leucócitos totais, Neutrófilos, Linfócitos, Eosinófilos, Hematócrito, HCM, CHCM, RDW-CV e Plaquetas.
- **Pré-processamento:** normalização Min-Max, ajustada de forma independente em cada base de treino.
- **Divisão:** 60% treino/otimização e 40% teste interno, por doença.
- **Algoritmos testados:** KNN, Random Forest, XGBoost e SVM.
- **Otimização de hiperparâmetros:** busca automatizada com [Optuna](https://optuna.org/), validação cruzada de 5 dobras (CV=5) maximizando F1-Score.
- **Validação cruzada bidirecional:** modelos treinados em uma doença são testados (a) no teste interno da mesma doença, (b) na base completa da outra doença, e (c) no conjunto compartilhado — processo replicado invertendo os papéis.
- **Métricas:** Acurácia, Precisão, Recall, F1-Score, MCC e AUC-ROC (com ênfase no Recall pela importância clínica de minimizar falsos negativos).

## Principais resultados

| Cenário | Melhor modelo | Acurácia | AUC |
|---|---|---|---|
| Treino Malária → Teste Malária | Random Forest | 72,43% | 0,7843 |
| Treino Malária → Validação Externa (Dengue) | Random Forest | 80,43% | 0,8833 |
| Treino Dengue → Teste Dengue | XGBoost | 89,06% | 0,9293 |
| Treino Dengue → Validação Externa (Malária) | KNN | 64,67% | 0,6953 |

**Principal achado:** há uma **assimetria expressiva** na generalização. Modelos treinados em malária transferem bem o conhecimento para dengue, mas o inverso não se sustenta — sugerindo que assinaturas hematológicas mais restritas (dengue) não capturam a complexidade de padrões mais amplos (malária), o que se agrava ainda mais nos casos de sobreposição clínica entre as duas doenças.

## Tecnologias

- Python
- Scikit-learn
- XGBoost
- Optuna
- Pandas / NumPy

## Estrutura do projeto

```
sbc_doencas/
├── database/            # bases de dados brutas (dengue, malária, compartilhado)
├── treino_dengue/       # notebooks de treino (KNN, RF, SVM, XGB) + saidas/
├── treino_malaria/      # notebooks de treino (KNN, RF, SVM, XGB) + saidas/
├── tratamento.ipynb     # limpeza e pré-processamento dos dados
└── README.md
```

Cada doença tem seu próprio conjunto de notebooks de treino — um por algoritmo — e uma pasta `saidas/` com `predicoes.csv`, `probabilidades.csv` e `estatisticas.ipynb`, este último contendo os **resultados consolidados** (Acurácia, Precisão, Recall, F1, MCC, AUC) de cada modelo.

## Referências principais

- Bhatt et al. (2013) — *The global distribution and burden of dengue*, Nature.
- Ralapanawa et al. (2015) — *Predictive value of laboratory biomarkers in dengue and malaria*, BMC Infectious Diseases.
- Sarker Siam et al. (2025a, 2025b) — Datasets hematológicos de dengue e malária (Bangladesh).

## Autores

- **Renan C. Amaral** — Universidade Federal de Alfenas (UNIFAL-MG)
- **Reginaldo J. Silva** — Universidade Estadual Paulista (UNESP)
- **Angela L. Moreno** — Universidade Federal de Alfenas (UNIFAL-MG)

## Licença

Este projeto está sob a licença MIT — veja o arquivo [LICENSE](LICENSE) para mais detalhes.