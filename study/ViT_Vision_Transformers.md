# Fundamentação Teórica e Revisão de Trabalhos Acadêmicos

Objetivo de estudo: compreender os conceitos de patches, embeddings, positional embeddings e a estrutura geral de um Vision Transformer.


---

# I — Revisão de CNNs
As Convolutional Neural Networks (CNNs) são arquiteturas de redes neurais desenvolvidas para processar dados com estrutura espacial, especialmente imagens.
Uma CNN utiliza filtros (kernels) que percorrem a imagem e identificam padrões locais, como bordas, texturas e formas. Esses filtros possuem pesos aprendidos durante o treinamento.


## 1.1. Receptive Fields
O campo receptivo corresponde à região da entrada que influencia a ativação de determinado neurônio ou característica de uma camada.
- Nas primeiras camadas, os campos receptivos geralmente são menores, permitindo identificar características locais.
- Nas camadas mais profundas, os campos receptivos tendem a aumentar, permitindo combinar informações de regiões maiores da imagem.
Exemplo: uma rede pode identificar inicialmente bordas e texturas e, em camadas posteriores, combinar essas informações para reconhecer estruturas mais complexas.


## 1.2. ResNet-101
A ResNet-101 é uma arquitetura de CNN com 101 camadas, baseada em conexões residuais (skip connections).
Essas conexões permitem que informações atravessem determinados blocos da rede por caminhos alternativos, facilitando o treinamento de redes profundas.
.


## 1.3. Limitações e motivação para Transformers
As CNNs possuem um forte viés indutivo para padrões locais: suas convoluções exploram regiões próximas e compartilham os mesmos filtros espacialmente.
Isso é uma vantagem para aprender características visuais com eficiência, mas a interação entre regiões distantes da imagem depende da combinação de várias camadas ou de mecanismos adicionais.
Os Transformers oferecem outra abordagem: o mecanismo de self-attention permite relacionar diferentes regiões da imagem diretamente.


---

# II — Introdução aos Transformers


Transformers são arquiteturas de redes neurais baseadas principalmente no mecanismo de atenção (attention mechanism).
Originalmente desenvolvidos para processamento de linguagem natural, eles trabalham com sequências de tokens.


No processamento de texto:
1. Uma sentença é dividida em tokens.
2. Cada token é representado por um vetor (embedding).
3. Informações posicionais são adicionadas à representação.
4. A sequência é processada por blocos Transformer.
5. Uma camada de saída produz os resultados da tarefa.
No Vision Transformer, a lógica geral é semelhante, mas a entrada não é uma sentença: é uma imagem.



---

## III - Estrutura geral de um Vision Transformer (ViT)

Podemos organizar o fluxo de um ViT em três grandes etapas:
1. Embedding: transforma a imagem em uma sequência de representações vetoriais.
2. Transformer Encoder: processa os vetores utilizando mecanismos de atenção.
3. Classification Head: utiliza a representação final para produzir a classificação.


### 3.1. Embedding

O objetivo dessa etapa é transformar a imagem em uma sequência de vetores que possa ser processada pelo Transformer.
A) Patchification — divisão da imagem em patches
Diferentemente de um Transformer de texto, que recebe palavras ou partes de palavras, o ViT divide a imagem em pequenas regiões chamadas patches.
Exemplo:
Considere uma imagem RGB de 224 × 224 pixels e patches de 16 × 16 pixels.
- Altura: 224 pixels.
- Largura: 224 pixels.
- Tamanho de cada patch: 16 × 16 pixels.
- Quantidade de patches: 14 × 14 = 196.
Portanto, uma imagem é transformada em uma sequência de 196 patches.
B) Flattening — transformação em vetores
Cada patch é convertido em um vetor unidimensional (flattening).
Considerando uma imagem RGB, cada patch possui:
16 × 16 × 3 = 768 valores.
Assim, cada patch passa a ser representado por um vetor de 768 elementos.
C) Patch Embedding — projeção linear
Os vetores resultantes do flattening são transformados por uma camada de projeção linear.
Essa operação converte cada patch em um vetor de dimensão D, chamada de dimensão do embedding.
Por exemplo, se D = 768, cada patch será representado por um vetor de 768 dimensões.
Atenção: o vetor resultante é uma representação aprendida do patch, e não simplesmente a lista original de pixels.
D) Positional Embedding — informação posicional
Um problema dos Transformers é que o mecanismo de atenção, por si só, não identifica a posição original de cada token na sequência.
Por isso, o ViT adiciona informações posicionais aos embeddings dos patches.
Em geral, os embeddings posicionais são somados elemento a elemento aos embeddings dos patches:
Representação final = Patch Embedding + Positional Embedding
Essa operação permite que o modelo tenha acesso à informação sobre a posição de cada patch na imagem.
Não se trata de somar vetores para calcular a distância entre palavras ou de interpretar diretamente o ângulo entre eles. A soma incorpora informação posicional à representação que será processada pela rede.
E) CLS Token — token de classificação
Na arquitetura ViT original, um token especial chamado [CLS] é adicionado à sequência de patches.
Esse token funciona como uma representação que pode agregar informações da imagem durante o processamento pelo Transformer.
Ao final, sua representação é utilizada pela cabeça de classificação.
Exemplo: imagem RGB de 224 × 224, patches de 16 × 16 e dimensão de embedding D = 768.

| Etapa | Operação                   | Resultado                             |
| :---- | :------------------------- | :------------------------------------ |
| 1     | Imagem de entrada          | 224 × 224 × 3                         |
| 2     | Divisão em patches         | 196 patches                           |
| 3     | Flattening                 | 196 vetores de 768 valores            |
| 4     | Patch Embedding            | 196 embeddings de dimensão 768        |
| 5     | Adição de CLS Token        | 197 tokens                            |
| 6     | Positional Embedding       | 197 vetores com informação posicional |
| 7     | Transformer Encoder        | 197 representações contextualizadas   |
| 8     | Classification Head        | Logits das classes                    |
| 9     | Softmax (quando utilizado) | Probabilidades das classes            |
---

# IV — Referências

1. **Dive into Deep Learning.** Capítulo sobre Vision Transformer.
    
    [https://d2l.ai/chapter_attention-mechanisms-and-transformers/vision-transformer.html](https://d2l.ai/chapter_attention-mechanisms-and-transformers/vision-transformer.html)
    
2. **Vizuara.** *Introduction to Vision Transformer(ViT)| AN image is wotth 16x16 words | Computer Vision Serires* 
    
    [[https://repositorio.ufu.br/handle/123456789/39556](https://youtu.be/aIi5FsdURUA?si=8nKDYJe6Pbj7EMfv)]
    
3. **James Briggs.** *Vision Transformers (ViT) Explained + Fine-tuning in Python*.
    [https://youtu.be/qU7wO02urYU?si=9fB6SMhDn7g15H9A](https://youtu.be/qU7wO02urYU?si=9fB6SMhDn7g15H9A)
    
