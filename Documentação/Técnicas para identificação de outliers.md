# Técnicas para detecção de anomalias
---
## A **detecção de anomalias** é a identificação de observações/eventos raros que **desviam do comportamento normal/esperado**.

<br>

Para o presente projeto, utilizaremos a linguagem Python vinculada à biblioteca Darts. Esta biblioteca possui diversas ferramentas que usam inteligência artificial para detecção e pontuação de outliers.
<br> <br>

Tais ferramentas serão listadas abaixo:

### Pontuadores de Anomalias

Os pontuadores são o núcleo do módulo de detecção de anomalias. Eles produzem séries temporais com pontuações de anomalias. Quanto maior for a pontuação de anomalias, mais anômalo é o período correspondente. <br> <br>
Os pontuadores podem trabalhar com janelas, e o tamanho da janela está relacionado com a escala temporal sobre a qual se espera que as anomalias ocorram. A interpretabilidade do pontuador depende do tipo de pontuador. <br> <br>
Alguns pontuadores são treináveis e outros não. Visando explorar e comparar técnicas de inteligência artificial (IA) para tratamento de outliers (em detrimento do uso de técnicas clássicas de estatística), neste projeto os pontuadores utilizados/comparados são **treináveis**.

A maioria dos pontuadores possuem os seguintes parâmetros:

- **window**: Valor valor Integer indicando o tamanho da janela W usada pelo pontuador para transformar a série temporal em pontuações de anomalias. O pontuador separa a série temporal em subconjuntos de W elementos e retorna o quão anômalo é o comportamento dos dados dessa janela (de forma pontual ou sobre toda a janela). O tamanho da janela deve ser definido de acordo com a duração estimada das anomalias.

- **component_wise**: valor Boolean que indica como o pontuador deve se comportar com séries multivariadas, definindo se vai tratar as dimensões de forma separada ou de forma conjunta.

- **window_transform**: valor Boolean que indica se o pontuador precisa fazer o processamento de sua sáida quando o tamanho da janela (W) for maior que 1. Em outras palavras, este parâmetro indica se o pontuador trabalhará de forma pontual ou sobre toda a janela.

Os pontuadores de anomalias do Darts serão listados abaixo:

1. **K-Means Scorer**
   O K-Means Scorer é um pontuador que utiliza a técnica K-Means Clustering [1], responsável por treinar em uma série temporal e separar os dados pontuais em K clusters com base na similaridade entre tais dados, que pode ser calculada utilizando algoritmos como a distância euclidiana quadrática ou métodos similares.
    A pontuação é feita calculando a distância do mais próximo entre os K centroides em relação à janela W. Este pontuador gera **predições determinísticas**, ou seja, gera predições replicáveis que se alinham a ambientes controlados.

2. **Negative Log-likelihood (NLL) Scorers**
   Os NLL Scorers formam um conjunto de pontuadores que utilizam distribuições de probabilidades clássicas para gerar **predições estocásticas** para cada janela W da série temporal. Em seguida, o modelo calcula o NLL ao comparar a previsão estocástica com os valores reais da distribuição para cada janela W. A pontuação resultante é o próprio NLL.
   O Darts disponibiliza as seguintes distribuições de probabilidades: exponencial, Cauchy, Gamma, Gaussiana, Laplace e Poisson.

3. **Norm Scorer**
   O Norm Scorer atua comparando duas séries temporais (uma predição e uma série temporal real, por exemplo). O modelo gera vetores para cada ponto das séries e calcula, como resultado, a norma [2] de ordem n (a ordem 1 implica a distância de Manhattan, a ordem 2 implica a distância euclidiana, etc). Assim, o Norm Scorer exibe predições determinísticas.     
   Quanto maior o valor da norma entre as séries, maior a pontuação de anomalia deste modelo.

4. **Wasserstein Scorer**
   O Wasserstein Scorer é um pontuador que utiliza a distância de mínimo esforço de Wasserstein para calcular a diferença entre duas distribuições de probabilidades, obtendo **predições determinísticas**. Assim, o modelo divide as duas séries temporais comparadas em janelas de tamanho W e compara as distribuições de probabilidades em cada janela.
   Quanto maior a distância de Wasserstein entre as janelas, maior a pontuação resultante. 



### Detectores de Anomalias

Os detectores de anomalias fazem classificações binárias nas séries temporais. Tipicamente, são usados para transformar séries temporias de pontuações de anomalias em séries temporais binárias de anomalias.

Alguns detectores são treináveis, como o QuantileDetector.

O princípio de funcionamento dos detectores está baseado no treinamento em uma ou várias séries temporais. Em seguida, o modelo é alimentado com uma série temporal de pontuações de anomalias e faz uso do detector para gerar a série temporal binária de anomalias.
Também é possível obter métricas de desempenho (como acurácia, precisão, recall, ou f1) em relação a uma predição binária e uma série temporal conhecida com a presença de anomalias, configurando uma interação supervisionada.

Os detectores de anomalias do Darts serão listados abaixo:
