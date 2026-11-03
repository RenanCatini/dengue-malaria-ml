# Validação Cruzada Bidirecional entre Dengue e Malária

> Estudo de generalização de modelos de Machine Learning aplicados a hemogramas, avaliando se um classificador treinado para diagnosticar dengue consegue identificar malária (e vice-versa).

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![Status](https://img.shields.io/badge/status-research-orange)]()

## Sobre o projeto

Dengue e malária compartilham sintomas iniciais inespecíficos, o que dificulta o diagnóstico clínico isolado. Este projeto avalia se modelos treinados com hemogramas para uma dessas doenças conseguem **generalizar** para diagnosticar a outra, testando a transferência preditiva nos dois sentidos.

## Metodologia

- Duas bases independentes de hemograma (dengue e malária) + conjunto de validação externa "compartilhado", com 10 variáveis hematológicas em comum.
- Normalização Min-Max e divisão 60/40 (treino/teste) por doença.
- Algoritmos: **KNN, Random Forest, XGBoost e SVM**, otimizados com Optuna (CV=5).
- Validação cruzada bidirecional: cada modelo é testado na própria doença, na doença oposta e no conjunto compartilhado.
- Métricas: Acurácia, Precisão, Recall, F1, MCC e AUC-ROC.

## Principais resultados

| Cenário | Melhor modelo | Acurácia | AUC |
|---|---|---|---|
| Treino Malária → Teste Malária | Random Forest | 72,43% | 0,7843 |
| Treino Malária → Validação Externa (Dengue) | Random Forest | 80,43% | 0,8833 |
| Treino Dengue → Teste Dengue | XGBoost | 89,06% | 0,9293 |
| Treino Dengue → Validação Externa (Malária) | KNN | 64,67% | 0,6953 |

**Achado principal:** assimetria expressiva na generalização — modelos treinados em malária transferem bem para dengue, mas o inverso degrada significativamente, sugerindo que assinaturas hematológicas mais restritas (dengue) não capturam a complexidade de padrões mais amplos (malária).

## Tecnologias

Python · Scikit-learn · XGBoost · Optuna · Pandas / NumPy

## Estrutura do projeto

```
sbc_doencas/
├── database/            # bases de dados (dengue, malária, compartilhado)
├── treino_dengue/       # notebooks de treino (KNN, RF, SVM, XGB) + saidas/
├── treino_malaria/      # notebooks de treino (KNN, RF, SVM, XGB) + saidas/
├── tratamento.ipynb     # limpeza e pré-processamento dos dados
└── README.md
```

Cada pasta `saidas/` contém `predicoes.csv`, `probabilidades.csv` e `estatisticas.ipynb` — este último com os resultados consolidados de cada modelo.

## Autores

- **Renan C. Amaral** — Universidade Federal de Alfenas (UNIFAL-MG)
- **Reginaldo J. Silva** — Universidade Estadual Paulista (UNESP)
- **Angela L. Moreno** — Universidade Federal de Alfenas (UNIFAL-MG)
