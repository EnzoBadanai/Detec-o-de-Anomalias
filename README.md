# 🛡️ Detecção de Intrusão e Ameaças em Redes (NIDS): Estudo Comparativo

Este repositório contém um estudo comparativo abrangente sobre o uso de **Aprendizado de Máquina (Machine Learning)** e **Deep Learning** para a Detecção de Intrusões e Ameaças em Redes de Computadores (NIDS), utilizando o dataset de referência **CIC-IDS2017**.

O projeto avalia a eficácia de diferentes paradigmas de Inteligência Artificial no contexto de um **Centro de Operações de Segurança (SOC)**: desde modelos lineares supervisionados ultra-rápidos até redes neurais profundas.

---

## 📌 Visão Geral da Arquitetura e Modelos

A solução foi estruturada em três abordagens complementares que simulam uma estratégia de **Defesa em Camadas (Defense-in-Depth)**:

1. **Regressão Logística (`class_weight='balanced'`):**
   * **Tipo:** Supervisionado Linear.
   * **Função no SOC:** *Baseline* leve e de baixíssimo custo computacional para filtragem e triagem preliminar de tráfego no perímetro de rede.
2. **DNN Classificadora (Multi-Layer Perceptron):**
   * **Tipo:** Supervisionado Não-Linear Profundo (Keras/TensorFlow).
   * **Função no SOC:** Classificação de alta precisão para assinaturas e categorias de ataques conhecidos, minimizando alarmes falsos.

---

## 🏗️ Pipeline de Engenharia e Pré-processamento de Dados

O pipeline de dados foi padronizado para garantir a reprodutibilidade e evitar o vazamento de dados (*data leakage*):

* **Dataset:** CIC-IDS2017 (arquivos `.parquet` consolidados).
* **Limpeza de Dados:** Remoção de duplicatas, tratamento de valores infinitos/ausentes (`inf`, `NaN`) e descarte de colunas com variância zero ($\sigma^2 = 0$).
* **Divisão Estratificada:** Separação em conjuntos de Treino, Validação e Teste mantendo a proporção real das classes via `stratify=y`.
* **Escalonamento Robusto:** Aplicação do `StandardScaler` ajustado estritamente nos dados de treino (`fit_transform`) para lidar com picos de tráfego extremos.

---

## 📊 Tabela Comparativa de Métricas e Desempenho

A tabela a seguir consolida os resultados obtidos pelos três modelos no conjunto de teste:

| Modelo / Métrica | Regressão Logística (`ML`) | DNN Classificadora |
| :--- | :---: | :---: |
| **Tipo de Aprendizado** | Supervisionado Linear | Supervisionado Não-Linear |
| **Dados de Treino** | Normais + Ataques | Normais + Ataques |
| **Acurácia Geral** | 94.59% | **~99.0%+** |
| **Área Sob a Curva ROC (AUC)** | 0.9930 | **~0.9980+** |
| **Recall / Sensibilidade (Ataques)** | **97.00%** | **~98.00% - 99.00%** |
| **Precision / Precisão (Ataques)** | 75.00% | **~98.00% - 99.00%** |
| **F1-Score (Ataques)** | 0.8400 | **~0.9850** |

---

## 🔍 Análise Crítica dos Resultados (Cibersegurança & SOC)

* **Trade-off da Regressão Logística:**
  O uso da hiperparametrização `class_weight='balanced'` elevou a sensibilidade do modelo linear para **97% de Recall em ataques**. No entanto, devido à incapacidade de separar fronteiras não lineares complexas, a precisão ficou em **75%**, gerando cerca de 25% de alarmes falsos (Falsos Positivos) que demandariam análise manual.
* **Superioridade da DNN Classificadora:**
  A rede neural profunda superou as limitações do modelo linear ao mapear as interações entre as 69 métricas de tráfego de rede. Ela reduziu drasticamente os Falsos Positivos, elevando tanto a *Precision* quanto o *Recall* para a faixa de **98%-99%**.

---

## 🎓 Conclusões do Estudo

1. **A complexidade da rede neural é demonstrada e justificada:** Para classificação supervisionada de tráfego de rede, a conversão de uma solução linear (Regressão Logística) para uma não linear (DNN) elimina o gargalo de alarmes falsos mantendo a detecção de invasões no nível máximo.
2. **Possivel Defesa em Camadas:** O modelo linear serve para descarte rápido no perímetro já a DNN supervisionada resolve a classificação exata de ameaças conhecidas
---

## 📂 Estrutura de Documentos do Repositório

* [`LogisticRegression_ML-README.md`](./LogisticRegression_ML-README.md) — Documentação detalhada do modelo baseline de Regressão Logística.
* [`DNN-README.md`](./DNN-README.md) — Documentação técnica e arquitetura da Rede Neural Classificadora.
* [`LogisticRegression_ML-Threat_Detection.ipynb`](./LogisticRegression_ML-Threat_Detection.ipynb) — Notebook do experimento de ML clássico.
* [`DNN-Threat_Detection.ipynb`](./DNN-Threat_Detection.ipynb) — Notebook do experimento de Deep Learning.
