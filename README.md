<h1 align="center">
TB x-rAI Net 2.0: Evolução no Apoio ao Diagnóstico de Tuberculose em Imagens Radiológicas através de Rede Neural Convolucional
</h1>

<p align="justify">Este repositório apresenta um modelo de Rede Neural Convolucional desenvolvido para o apoio ao diagnóstico da Tuberculose (TB) a partir de imagens radiológicas. O objetivo deste estudo é desenvolver um método que seja capaz de oferecer apoio aos profissionais da área da saúde a tomarem decisões com maior rapidez e precisão. Cabe ressaltar que a TB x-rAI Net 2.0 não substitui o diagnóstico médico.

O treinamento do modelo foi realizado no ambiente do <em>Google Colab</em> utilizando uma GPU Nvidia T4 Tensor Core a partir de um <em>script</em> em linguagem Python com a utilização das bibliotecas <em>TensorFlow</em>, <em>Keras</em>, <em>NumPy</em>, <em>Matplotlib</em> e <em>Scikit-Learn</em>. </p>


## Base de dados:

Para o desenvolvimento e construção da CNN proposta foram utilizados quatro *datasets* públicos de raio-x torácicos de casos normais e de TB:
- *Belarus Tuberculosis Portal* - [Frapa 2021]
- *DA and DB - TB Chest X-ray Dataset* (Nova Délhi - Índia) - [Chauhan 2014]
- *Montgomery Conty* (EUA) - [Raddar 2020]
- *Shenzhen* (China) - [Mengzhang 2024]

A base de Belarus contém 304 radiografias todas elas TB-positivas;
A base de Nova Délhi contém 278 radiografias com 138 casos normais e 125 casos de Tuberculose;
A base de Montgomery contém 138 radiografias com 80 casos normais e 58 TB-positivos;
A base de Shenzhen contém 662 radiografias com 326 casos normais e 336 casos de Tuberculose.

**Total de imagens : 1382**


## Pré-processamento:

<p align="justify">Nesta etapa, as imagens passam por uma transformação para uma escala de cinza, são todas redimensionadas para 256x256 <em>pixels</em>. Além disso, a fim de evitar <em>data leakage</em> foram removidas imagens duplicadas, resultando em um total de 1378 imagens, após a remoção de 4 imagens do <em>dataset</em> de Belarus. Ademais, foi aplicado o filtro <em>CLAHE</em>, permitindo melhorar o contraste dos raios-x. </p>


## Arquitetura da rede:

<p align="justify">A arquitetura da rede desenvolvida utiliza cinco blocos convolucionais residuais, permitindo que informações vindas das entradas (radiografias) sejam preservadas e combinadas com as características adquiridas ao longo das operações de convolução. Em cada um dos blocos são aplicadas duas convoluções 3x3, seguidas de um <em>BatchNormalization</em> de tamanho 16, função de ativação ReLU, mecanismo de atenção <em>Squeeze-and-Excitation</em> (<em>SE</em>), para a definição da importância dos canais da rede e recalibração desses canais. Além disso, é utilizada uma operação de <em>Max Pooling</em>, na redução do custo computacional da CNN, diminuição espacial e na preservação de ativações mais elevadas, uma camada <em>GAP</em> (<em>Global Average Pooling</em>), realizando o cálculo da média dos valores dos canais, obtidos anteriormente no <em>Max Pooling</em>. Ao fim, o método aplica <em>dropout</em> para reduzir o <em>overfitting</em> e utiliza uma camada totalmente conectada com uma ativação <em>Sigmoid</em>, para a geração binária das classificações (TB e Saudável). </p>

O modelo também utiliza três modelos idênticos porém com configurações distintas de filtros iniciais e taxas de *dropout*, entre os quais:

- Modelo 1: Filtro de 32 e *dropout* de 0,3;
- Modelo 2: Filtro de 64 e *dropout* de 0,4;
- Modelo 3: Filtro de 128 e *dropout* de 0,5.

<p align="center">
  <img src="https://github.com/Isadoura/TB-x-rAI-Net-2.0-Apoio-ao-Diagnostico-de-Tuberculose-atraves-de-Rede-Neural-Convolucional/blob/main/imagens/arquiteturacnn.drawio.png" alt="Arquitetura da CNN proposta" width="800">
  <br>
  <strong>Figura 1</strong>: Arquitetura da CNN proposta.
</p>


## Divisão dos dados de treinamento, validação e teste

