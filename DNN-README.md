Detecção de Intrusões e Anomalias em Redes de Computadores com Deep Learning (DNN)

Este repositório contém o código-fonte e os experimentos de um modelo de **Rede Neural Profunda (DNN)** supervisionado desenvolvido para a classificação de tráfego de rede (tráfego normal vs. ataques cibernéticos). 

O projeto utiliza o conjunto de dados moderno **CIC-IDS2017** pré-processado em formato Parquet para avaliar a precisão de redes neurais na detecção preditiva de ameaças em tempo de execução.


Sumário Executivo

* Objetivo: Classificar o tráfego de rede em requisições benígnas (`Normal`) ou maliciosas (`Ataque`) utilizando aprendizado profundo.
* Dataset: CIC-IDS2017 (armazenado em arquivos `.parquet` consolidados).
* Desempenho Geral:
  * Acurácia: `99%`
  * F1-Score (Ataque): `0.98`
  * AUC (Curva ROC): `0.9995`


O Dataset (CIC-IDS2017)

O dataset utilizado contém fluxos de tráfego de rede capturados em ambiente controlado, contemplando requisições benignas e os principais vetores de ataques modernos:

1. **Benign-Monday-no-metadata.parquet** (Tráfego Normal)
2. **Botnet-Friday-no-metadata.parquet** (Ataques de Botnet)
3. **Bruteforce-Tuesday-no-metadata.parquet** (Força Bruta FTP/SSH)
4. **DDoS-Friday-no-metadata.parquet** (Ataques de Negação de Serviço Distribuída)
5. **DoS-Wednesday-no-metadata.parquet** (Ataques DoS/Slowloris/Hulk)
6. **Infiltration-Thursday-no-metadata.parquet** (Infiltração de Rede)
7. **Portscan-Friday-no-metadata.parquet** (Varredura de Portas)
8. **WebAttacks-Thursday-no-metadata.parquet** (Ataques Web: XSS, SQL Injection, Brute Force)


Arquitetura do Modelo

A arquitetura da Rede Neural Classificadora foi implementada no TensorFlow/Keras utilizando a API `Sequential`:

```text
=================================================================
Camada (Tipo)               Shape de Saída       Nº de Parâmetros
=================================================================
InputLayer                  (None, input_dim)    0
Dense (ReLU)                (None, 128)          Paramétros variáveis
Dropout (0.3)               (None, 128)          0
Dense (ReLU)                (None, 64)           8,256
Dropout (0.2)               (None, 64)           0
Dense (ReLU)                (None, 32)           2,080
Dense (Sigmoid) - Saída     (None, 1)            33
=================================================================
Total de Parâmetros Treináveis: ~19,329 (75.50 KB)
