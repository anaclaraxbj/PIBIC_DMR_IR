# Fundamentação Teórica e Revisão de Trabalhos Acadêmicos

Este documento reúne os conceitos estudados sobre **Redes Neurais Convolucionais (CNNs)** e fichamentos de trabalhos acadêmicos relacionados à análise de imagens termográficas da mama.

O objetivo é registrar os aprendizados, compreender as metodologias existentes e identificar contribuições relevantes para o projeto de iniciação científica, que investiga **Vision Transformers e Inteligência Artificial Explicável (XAI)**.

---

# I — Fundamentos de CNNs

**Referência de estudo:** *Dive into Deep Learning*

**Material:** Capítulo sobre Redes Neurais Convolucionais — [Dive into Deep Learning](https://d2l.ai/chapter_convolutional-neural-networks/index.html)

## 1.1. O que são CNNs?

As Redes Neurais Convolucionais são arquiteturas de redes neurais projetadas para processar dados com estrutura espacial, como imagens.

Uma imagem é representada numericamente por uma matriz ou tensor. As CNNs exploram a relação espacial entre os valores da imagem por meio de operações convolucionais, que aplicam filtros aprendidos a regiões locais.

Ao longo da rede, as operações produzem representações que podem combinar informações locais em padrões progressivamente mais complexos. Essas representações são utilizadas pelas camadas posteriores para realizar tarefas como classificação e segmentação.

## 1.2. Convolução e correlação cruzada

Uma camada convolucional aplica filtros, também chamados de *kernels*, à entrada. Cada filtro possui pesos que determinam como os valores de uma região local são combinados.

Em implementações comuns de CNNs, a operação utilizada é a **correlação cruzada**.

De maneira simplificada, os valores de uma janela da entrada são multiplicados pelos pesos correspondentes do filtro, e os produtos são somados para produzir um valor de saída.

## 1.3. Stride e padding

- **Stride:** é o passo de deslocamento do filtro sobre a entrada. Um stride maior normalmente reduz a resolução espacial da saída.
- **Padding:** consiste em adicionar valores às bordas da entrada, permitindo controlar as dimensões da saída e o tratamento das regiões próximas às bordas.

Esses parâmetros influenciam o tamanho dos mapas de características e a quantidade de informação espacial preservada.

## 1.4. Pooling

O *pooling* é uma operação utilizada em determinadas arquiteturas para reduzir as dimensões espaciais dos mapas de características.

Duas modalidades comuns são:

- **Max pooling:** seleciona o maior valor de cada região.
- **Average pooling:** calcula a média dos valores de cada região.

O pooling pode reduzir o custo computacional das operações posteriores e diminuir a resolução espacial. Por outro lado, também pode eliminar detalhes importantes.

O pooling normalmente é aplicado separadamente a cada canal, preservando a quantidade de canais enquanto reduz as dimensões espaciais. O efeito exato depende do tamanho da janela, do stride, do padding e da configuração utilizada.

### Aplicação em imagens termográficas

O max pooling preserva os maiores valores locais, enquanto o average pooling representa a média de cada região.

Não se deve assumir que max pooling seja sempre a melhor escolha para imagens térmicas: a decisão precisa considerar a tarefa, os dados e os resultados experimentais.

## 1.5. Saída das camadas convolucionais

A saída de uma camada convolucional costuma ser um tensor com três dimensões por amostra:

[(C,H,W)]

Em que:

- **C:** quantidade de canais de saída;
- **H:** altura espacial do mapa;
- **W:** largura espacial do mapa.

Cada canal contém um mapa de características produzido por um conjunto de operações e pesos aprendidos. Uma arquitetura pode produzir dezenas ou centenas de canais, dependendo da camada.

É importante não atribuir automaticamente significado fisiológico a cada canal.

Por exemplo, não é correto afirmar que determinado canal representa exclusivamente hipertermia, vascularização ou tecido tumoral sem evidências específicas.

As representações aprendidas podem combinar diferentes padrões, e uma característica pode estar distribuída entre vários canais.

A resolução espacial dos mapas também pode ser menor que a da imagem original, em razão das operações realizadas ao longo da rede. Isso permite conservar alguma informação espacial, mas não garante a localização exata de uma estrutura anatômica.

## 1.6. Relação entre CNNs e Grad-CAM

O Grad-CAM utiliza gradientes da pontuação de uma classe em relação às ativações de uma camada convolucional para construir um mapa de relevância espacial.

Por isso, compreender os mapas de características é importante para estudar o método.

Contudo, o Grad-CAM não simplesmente identifica um canal que represente uma característica fisiológica: ele combina informações de múltiplos mapas e dos gradientes relacionados à classe investigada.

A região destacada indica relevância segundo o procedimento do método. Ela não comprova, por si só, que o modelo identificou um tumor ou aprendeu uma relação fisiológica correta.

---

# II — Fichamentos de Trabalhos Acadêmicos

## Trabalho 1 — Segmentação automática de região de interesse em imagens termográficas da mama utilizando redes profundas

**Autor:** Tiago da Silva e Souza Pinto

**Tipo:** Trabalho de Conclusão de Curso — Ciência da Computação, UFU, 2023

**Fonte:** Repositório Institucional da UFU

### .1. Problema e objetivo

O trabalho investiga a segmentação automática da região de interesse em imagens termográficas da mama.

O objetivo é separar a região mamária do plano de fundo, reduzindo informações que podem prejudicar etapas posteriores de análise automática.

A segmentação e a classificação são tarefas distintas:

- **Segmentação:** o modelo atribui rótulos aos pixels para delimitar regiões.
- **Classificação:** o modelo atribui uma classe à imagem ou ao exemplo analisado.

### .2. Metodologia

De acordo com o registro do repositório da UFU:

- **Tarefa:** segmentação de imagens termográficas.
- **Arquitetura:** DeepLabV3+.
- **Rede de base:** ResNet-101 pré-treinada no ImageNet.
- **Dados:** imagens termográficas acompanhadas de referências de segmentação (*ground truth*).

O trabalho relata médias de:

| Métrica | Resultado |
| --- | --- |
| Acurácia | 98,69% |
| Interseção sobre União (IoU) | 97,18% |
| Precisão | 98,48% |

Esses valores são resultados de segmentação relatados pelo trabalho, não métricas de classificação de pacientes com e sem câncer.

### .3. Interpretação dos resultados

Os resultados indicam bom desempenho segundo as métricas reportadas e o protocolo experimental do estudo.

Entretanto, para avaliar a generalização, também é necessário conhecer a quantidade e a diversidade das imagens, a divisão dos dados, a forma de validação e as condições experimentais.

### .4. Conexão com a iniciação científica

A segmentação pode ser investigada como uma etapa de pré-processamento para delimitar a região mamária antes da classificação.

Essa delimitação pode ajudar a reduzir a influência do plano de fundo. No entanto, não é possível concluir antecipadamente que a segmentação aumentará o desempenho da classificação. Isso precisa ser testado experimentalmente, com comparações apropriadas.

Para o projeto de XAI, a segmentação também pode ajudar a definir uma região anatômica de interesse para análises posteriores.

Ainda assim, uma região de interesse não deve ser tratada automaticamente como uma marcação tumoral.

---

## Trabalho 2 — Classificação de imagens térmicas para detecção do câncer de mama utilizando um comitê de classificadores

**Autor:** Paulo Vitor Costa Silva

**Tipo:** Trabalho de Conclusão de Curso — Ciência da Computação, UFU, 2023

**Fonte:** Repositório Institucional da UFU

### 3.1. Problema e objetivo

O trabalho investiga a classificação de imagens termográficas de pacientes saudáveis e doentes utilizando redes neurais convolucionais e comitês de classificadores.

A questão computacional central é verificar se a combinação das predições de diferentes modelos pode alcançar resultados superiores aos obtidos por classificadores individuais.

### 3.2. Metodologia

Conforme o registro do repositório:

- **Base de dados:** DMR-IR.
- **Tarefa:** classificação de imagens termográficas.
- **Abordagem:** CNNs pré-treinadas e dois comitês de classificadores.
- **Estratégia destacada:** votação majoritária.

O resumo do repositório relata que o comitê por votação majoritária obteve:

| Métrica | Resultado |
| --- | --- |
| Acurácia | 90,91% |
| F1-score | 92,31% |

O trabalho relata resultados superiores aos modelos individuais na comparação apresentada.

As arquiteturas individuais específicas, os procedimentos completos de treinamento e as demais métricas devem ser confirmados no texto integral da monografia antes de serem registrados como detalhes definitivos.

### 3.3. Interpretação dos resultados

Um comitê combina as predições de diferentes classificadores.

Dependendo da diversidade dos modelos e dos erros que cometem, essa combinação pode melhorar o desempenho geral.

Entretanto, combinar modelos não garante resultados melhores em todos os cenários. O desempenho depende dos classificadores escolhidos, da estratégia de combinação e do protocolo de avaliação.

### 3.4. Conexão com a iniciação científica

Esse estudo fornece uma referência para a etapa de classificação de imagens termográficas e para a comparação de estratégias baseadas em CNNs.

O projeto de iniciação científica acrescenta uma dimensão de explicabilidade: além de avaliar as predições, pretende investigar como as explicações visuais se relacionam com o comportamento dos modelos e com regiões fisiologicamente relevantes.

A combinação de classificadores e a explicabilidade são questões diferentes. Um comitê pode melhorar a classificação sem necessariamente produzir explicações mais fiéis ou mais coerentes fisiologicamente.

---

## Trabalho 3 — Comparação de modelos multiângulo

**Autor:** Gustavo Resende

**Fonte:** confirmar o título completo, os dados bibliográficos e o endereço permanente do documento antes de finalizar este fichamento.

### 4.1. Problema e objetivo

Pelas anotações realizadas durante a leitura, o trabalho compara arquiteturas de redes neurais para classificação de imagens termográficas considerando diferentes ângulos de aquisição.

A ideia central é investigar como as informações de múltiplas vistas podem contribuir para distinguir pacientes saudáveis e doentes.

### 4.2. Metodologia registrada nas anotações

- **Base de dados:** DMR-IR.
- **Tarefa:** classificação de imagens termográficas.
- **Arquiteturas anotadas:** ResNet, DenseNet e EfficientNet.
- **Preparação das imagens:** redimensionamento, adaptação do número de canais e aumento de dados.
- **Métricas anotadas:** acurácia, F1-score, sensibilidade, especificidade e precisão.

Esses detalhes foram registrados a partir das anotações de estudo e ainda devem ser conferidos no documento original.

O resultado atribuído à DenseNet também precisa ser associado à métrica, ao protocolo experimental e à configuração específica que o produziu.

### 4.3. Interpretação e conexão com a iniciação científica

O uso de múltiplos ângulos pode fornecer informações complementares sobre a distribuição térmica superficial.

Isso torna relevante investigar estratégias de fusão de imagens ou representações de diferentes vistas.

A arquitetura que apresenta melhor resultado em um experimento não é necessariamente a melhor para outro conjunto de dados ou outra estratégia de fusão.

Além disso, um bom desempenho de classificação não garante que um mapa de explicação destaque regiões relevantes em todos os ângulos. Essa questão deve ser investigada separadamente.

---

## Trabalho 4 — Algoritmos bioinspirados para otimização de CNNs aplicados na detecção de câncer de mama utilizando imagens termográficas

**Autora:** Caroline Barcelos Gonçalves

**Tipo:** Dissertação de mestrado — Ciência da Computação, UFU, 2022

**Fonte:** Repositório Institucional da UFU

### 5.1. Problema e objetivo

O trabalho investiga a otimização de CNNs pré-treinadas para classificar imagens termográficas estáticas da mama em pacientes saudáveis e doentes.

A questão computacional envolve a escolha de arquiteturas e hiperparâmetros adequados para melhorar o desempenho dos classificadores, considerando também o custo computacional da busca.

### 5.2. Metodologia

Segundo o resumo do repositório:

- **Base de dados:** imagens termográficas estáticas da base DMR.
- **Tarefa:** classificação de imagens.
- **Arquiteturas:** VGG-16, ResNet-50 e DenseNet-201.
- **Métodos de otimização:** algoritmo genético (AG) e otimização por enxame de partículas (PSO).
- **Modelo surrogate:** Random Forest, utilizado para reduzir o custo computacional associado à busca.

O resumo relata melhorias de F1-score após a otimização em determinadas configurações, incluindo VGG-16 e ResNet-50.

Também relata resultados de F1-score de 0,92 para a DenseNet-201 e de 1,0 em uma configuração com o modelo surrogate.

Esses resultados devem ser interpretados no contexto dos experimentos da dissertação.

Um F1-score de 1,0 em uma avaliação específica não demonstra, por si só, desempenho perfeito em dados independentes ou em aplicações clínicas.

### 5.3. Interpretação dos resultados

A otimização de hiperparâmetros busca encontrar configurações adequadas para o modelo e para o procedimento de treinamento.

Algoritmos bioinspirados podem explorar diferentes combinações de parâmetros, enquanto um modelo surrogate pode aproximar os resultados de avaliações mais custosas.

A utilidade dessa estratégia depende da definição do problema de otimização, do orçamento computacional e da forma como os experimentos são validados.

### 5.4. Conexão com a iniciação científica

O trabalho oferece referências para estudar CNNs pré-treinadas, estratégias de otimização e avaliação de modelos de classificação.

Essas arquiteturas podem servir como referências de comparação para a pesquisa atual.

Contudo, não se deve assumir que a arquitetura com melhor resultado nessa dissertação também será a melhor para o projeto com Vision Transformers e fusão multiângulo.

O Grad-CAM pode ser investigado em CNNs compatíveis para examinar regiões relacionadas às predições.

A otimização do classificador e a avaliação da explicabilidade devem ser tratadas como etapas distintas.

---

# III — Comparação dos Trabalhos

Os trabalhos analisados abordam etapas diferentes do processamento de imagens termográficas.

| Trabalho | Tarefa principal | Contribuição para o estudo |
| --- | --- | --- |
| Tiago Pinto | Segmentação | Delimitação da região mamária |
| Paulo Silva | Classificação com comitê | Combinação de classificadores |
| Gustavo Resende | Classificação multiângulo | Investigação de diferentes vistas |
| Caroline Gonçalves | Otimização de CNNs | Seleção de arquiteturas e hiperparâmetros |

A comparação mostra que segmentação, classificação, fusão multiângulo e otimização são problemas relacionados, mas distintos.

---

# IV — Relação com a Iniciação Científica

O projeto atual investiga **Vision Transformers e Inteligência Artificial Explicável** para a classificação de imagens termográficas da mama, com interesse na fusão de diferentes ângulos e na avaliação das explicações visuais.

Os trabalhos anteriores ajudam a fundamentar diferentes componentes dessa investigação:

- **Segmentação:** fornece referências sobre delimitação da região mamária.
- **Classificação:** apresenta estratégias e arquiteturas para distinguir classes.
- **Fusão multiângulo:** motiva a investigação de informações complementares entre vistas.
- **Otimização:** oferece exemplos de busca de hiperparâmetros e comparação de modelos.
- **XAI:** acrescenta a análise de como as predições se relacionam com regiões da entrada.

Essas conexões indicam possibilidades de investigação, não resultados já demonstrados para o projeto atual.

---

# V — Referências

1. **Dive into Deep Learning.** Capítulo sobre Redes Neurais Convolucionais.
    
    [https://d2l.ai/chapter_convolutional-neural-networks/index.html](https://d2l.ai/chapter_convolutional-neural-networks/index.html)
    
2. **PINTO, Tiago da Silva e Souza.** *Segmentação automática de região de interesse em imagens termográficas da mama utilizando redes profundas.* UFU, 2023.
    
    [https://repositorio.ufu.br/handle/123456789/39556](https://repositorio.ufu.br/handle/123456789/39556)
    
3. **SILVA, Paulo Vitor Costa.** *Classificação de imagens térmicas para detecção do câncer de mama utilizando um comitê de classificadores.* UFU, 2023.
    
    [https://repositorio.ufu.br/handle/123456789/39939](https://repositorio.ufu.br/handle/123456789/39939)
    
4. **RESENDE, Gustavo** *Comparação de modelos multiângulo***.** [https://repositorio.ufu.br/handle/123456789/4611](https://repositorio.ufu.br/handle/123456789/46119)
5. **GONÇALVES, Caroline Barcelos.** *Algoritmos bioinspirados para otimização de CNNs aplicados na detecção de câncer de mama utilizando imagens termográficas.* UFU, 2022.
    
    [https://repositorio.ufu.br/handle/123456789/36092](https://repositorio.ufu.br/handle/123456789/36092)