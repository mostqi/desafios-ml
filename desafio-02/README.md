 
# Desafio MLOps

## Instruções

1. Criar uma pipeline de treinamento do dataset do MNIST no AWS Sagemaker; pipeline deve conter
  - etapa de treinamento
  - etapa de teste
  - etapa de comparação com o melhor modelo obtido nos experimentos anteriores
  - etapa de deployment no ambiente de inferência do sagemaker
  - rastreabilidade dos hyperparêmetros e artefatos de treinamento e de teste
  - as etapas de treinamento e teste devem ser executadas como containers
  - a pipeline deverá também ser também representada com grafo (neste caso, o DAG) no AWS SAgemaker Studio

2. Adicionar na pipeline mecanismos de monitoramento de métricas produzidas durante o treinamento
3. Usando a pipeline contruida ate este ponto, criar um esboço de uma arquitetura para executá-la sempre que que haja degradação da performance do modelo no amibente de inferência (assuma que o dataset de entrada esta eh atualizado frequentemente)


## Entrada e saída

- A entrada da pipeline deve ser o dataset do MNIST armazenado no S3
- O resultado do desafio deve ser o modelo sendo servido pelo ambiente de inferÊcnia do Sagemaker

## Apresentação

A apresentação consistirá:
- do código da pipeline
- do grafo da pipeline no Sagemaker Studio
- da visualização no Sagemaker Studio dos experimentos realizados ecomo elas se relacionam com os hyperparâmetros de treinamento
- das métricas produzidas durante o treinamento, e como elas se relacionam com os hyperparâmetros de treinamento
