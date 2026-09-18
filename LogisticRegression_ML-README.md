Detecção de Intrusão em Redes com Regressão Logística (Machine Learning)

Este repositório contém a implementação de um modelo **supervisionado de Regressão Logística** para detecção de intrusões de rede utilizando a base de dados **CIC-IDS2017**.


Visão Geral do Projeto

A detecção de anomalias e intrusões no tráfego de rede é fundamental para Centros de Operações de Segurança (SOC). O objetivo deste projeto é treinar um classificador linear capaz de discriminar fluxos benignos de tentativas de ataque em tempo real, priorizando a **minimização de Falsos Negativos (aumento do Recall de ataques)**.

* **Dataset:** CIC-IDS2017 (em formato `.parquet`).
* **Tipo de Aprendizado:** Supervisionado Binário.
* **Paradigma:** Modelo Linear Interpretável.
* **Linguagem & Frameworks:** Python, Scikit-Learn, Pandas, NumPy.


Pipeline de Engenharia de Dados

1. **Carregamento e Concatenação:** Leitura automatizada dos arquivos `.parquet` contendo os fluxos de tráfego.
2. **Limpeza de Dados:**
   * Remoção de duplicatas e tratamento de valores nulos/infinitos (`NaN`, `inf`).
   * Descarte de atributos com variância zero (colunas sem sinal de informação).
3. **Divisão Estratificada:**
   * Divisão do dataset em treino e teste mantendo a proporção de classe (85% Benigno / 15% Ataque) via `stratify=y`.
4. **Padronização das Features:**
   * Aplicação do `StandardScaler` (com ajustamento `fit_transform` estritamente no conjunto de treino) para evitar *data leakage*.


Estratégia de Balanceamento de Classes

Em monitoramento de segurança, **perder um ataque (Falso Negativo)** é consideravelmente mais custoso do que gerar um **alarme falso (Falso Positivo)**. Para contornar o desbalanceamento da base original, foi adotada a parametrização `class_weight='balanced'`:


Essa abordagem penaliza severamente os erros na classe minoritária (Ataques), reajustando a hiperpsuperfície de decisão linear para maximizar a sensibilidade da detecção.



### **Métricas Globais**
* **Acurácia Geral:** **94.59%**
* **Área Sob a Curva ROC (AUC-ROC):** **0.9930**

### **Relatório de Classificação Detalhado**

| Classe | Precision | Recall | F1-Score | Support |
| :--- | :---: | :---: | :---: | :---: |
| **Normal (0)** | 0.99 | 0.94 | 0.97 | 379.063 |
| **Ataque (1)** | **0.75** | **0.97** | **0.84** | 67.299 |
| **Macro Average** | 0.87 | 0.96 | 0.91 | 446.362 |
| **Weighted Average** | 0.96 | 0.95 | 0.95 | 446.362 |


Análise Crítica dos Resultados (SOC / Cibersegurança)

* **Alta Sensibilidade (Recall de 97%):** Com o uso do `class_weight='balanced'`, o modelo capturou 97% das intrusões reais, reduzindo substancialmente o risco de ataques passarem despercebidos.
* **Custo de Falsos Alarmes (Precisão de 75%):** O limite linear do modelo resulta em cerca de 25% de alarmes falsos entre os alertas gerados para a classe de Ataques. Em um SOC, isso demanda triagem manual por parte dos analistas.
* **Capacidade de Separação (AUC 0.9930):** Demonstra que a ordenação das probabilidades do modelo é quase perfeita, permitindo refinar o ponto de decisão (*Threshold Tuning*) conforme a tolerância a risco da organização.

---

Como Executar o Código

**Pré-requisitos**
```bash
pip install pandas numpy scikit-learn matplotlib
