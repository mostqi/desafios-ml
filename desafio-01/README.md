# Desafio Face Presentation Attack Detection Challenge

## Introdução

Atualmente, com o avanço da tecnologia, sistemas de segurança que utilizam reconhecimento facial estão se tornando cada vez mais comuns e, ao mesmo tempo, cada vez mais vulneráveis. Conforme o passar dos anos, técnicas cada vez mais sofisticadas para burlar sistemas de reconhecimento facial estão surgindo, como o uso de máscaras faciais 3D (de silicone, gel, dentre outros materiais), o uso de deep fakes, além do uso de técnicas mais clássicas, como tirar uma foto ou gravar um vídeo da tela de um celular ou computador, contendo a imagem do alvo, de modo que, com o aumento da qualidade das câmeras e da resolução das telas, acabam se tornando também ataques mais fortes com o tempo.

Uma tentativa de fraude é chamada de presentation attack, ou também de spoof attack. Face PAD systems, são sistemas de segurança com o objetivo de identificar quando um vídeo ou uma imagem que chega ao sistema é real (capturada pelo pŕóprio indivíduo), ou uma tentativa de fraude, como quando alguém tenta tirar uma foto de uma foto daquele alvo (photo attack), ou gravar um vídeo de um vídeo existente do alvo (replay attack), com o uso de 3D masks, deep fakes, ou meramente colocando uma foto do alvo em frente a sua face. 

Esta é uma tarefa que apresenta muitos desafios nos tempos atuais, pois são muito diversos os tipos de ataques, e obter técnicas que conseguem generalizar bem é um desafio que instiga bastante a comunidade acadêmica atualmente. 

A MOST, sendo uma empresa que se preocupa com inovações tecnológicas, em especial na área de inteligência artificial, visão computacional e NLP, espera que você, candidato(a), seja capaz e demonstre interesse em pesquisar novidades nos mais diversos campos de machine learning, bem como ler artigos científicos, pesquisar por novas técnicas que estão surgindo, criar seus próprios projetos, suas próprias arquiteturas e fazer experimentos.

## Desafio

Você, candidato(a), deve criar um sistema utilizando deep learning capaz de detectar quando uma imagem enviada ao seu sistema é real ou fraude. Para isso, você deverá:
- Utilizar um dataset chamado “CelebA-Spoof Dataset”, que pode ser baixado através desse link: https://github.com/ZhangYuanhan-AI/CelebA-Spoof. Porém, neste desafio, será oferecido para você, candidato(a), um conjunto
parcial do dataset total (um sample), contendo aproximadamente 4 GBs, para que você possa realizar seus experimentos. Você deve utilizar o conjunto oferecido por nós em seu desafio, e não baixar o dataset completo. Para baixar o sample use o link ao lado:
https://mostqi-infra-sp-public.s3.sa-east-1.amazonaws.com/desafios/MLChallenge_Dataset.zip
- Você deve, preferencialmente, criar sua própria arquitetura para esta tarefa (e não apenas importar uma rede já existente). Pode ser utilizado como backbone redes pré-treinadas, porém tentativas de melhorar a arquitetura e inovar são muito bem vistas. Para isso, você pode usar o framework de sua preferência (como pytorch ou keras). Aqui é onde você pode explorar sua criatividade, utilizar diferentes técnicas e ideias para criar uma rede neural. Embora machine learning seja um processo iterativo e experimental, é importante saber entender e explicar porque determinadas escolhas de arquiteturas e estratégias de treinamento funcionam melhor para sua tarefa, ou seja, ter uma boa interpretabilidade de sua arquitetura.
- Criar e estruturar um projeto no github para a melhor solução (arquitetura) encontrada em seus experimentos.
- O projeto deve ser feito utilizando Python.
