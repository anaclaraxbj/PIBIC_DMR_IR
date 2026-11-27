# Pré- Processamento
Normalização térmica e redimensionamento da DMR-IR
O objetivo é entender quais transformações são necessárias, quais informações podemos alterar e propor um procedimento inicial que possa ser testado nos dados reais.
O pré-processamento precisa preservar tanto a estrutura espacial quanto a informação térmica.

## Etapa A — Inspecionar os dados originais
O que é normalização?
Normalizar significa transformar os valores numéricos para uma escala mais adequada ao processamento pelo modelo.
| Característica      | Resultado              | Interpretação                           |
| :------------------ | :--------------------- | :-------------------------------------- |
| Modo                | LA                     | Luminosidade + transparência            |
| Dimensões           | 640 × 480              | Compatíveis com a matriz TXT            |
| Canal L             | 0 a 212                | Possui diferentes intensidades de cinza |
| Valores únicos de L | 212                    | Não é uma máscara binária convencional  |
| Canal A             | 255 em todos os pixels | Imagem totalmente opaca                 |Interpretação:
- O fundo preto corresponde aos pixels de valor zero.
- A região mamária apresenta diferentes intensidades de cinza, que formam a textura visível.
- O canal alfa não contém informação de segmentação: todos os pixels são totalmente opacos.
Portanto, nossa hipótese de trabalho é que esse PNG representa uma imagem em tons de cinza com a região de interesse preservada e o fundo zerado.
- PNG e TXT possuem dimensões de 480 × 640 pixels.
- O contorno da máscara acompanha a região mamária correspondente.
- A condição canal_L > 0 produziu uma máscara que delimita a região observada.
Quantidade de pixels: 35263
Temperatura mínima: 24.89 °C
Temperatura máxima: 32.87 °C
Temperatura média: 29.58 °C
Desvio-padrão: 0.89 °C
- 1.522 registros, um para cada PNG.
- 1.121 identificadores distintos.
- 401 identificadores com duas máscaras (possivelmente esquerda e direita).
- 720 identificadores com apenas uma máscara.
A proporção da ROI apresenta variação considerável:
| Estatística   | Proporção |
| :------------ | :-------- |
| Mínima        | 4,93%     |
| 25º percentil | 11,67%    |
| Mediana       | 14,26%    |
| 75º percentil | 17,27%    |
| Máxima        | 25,72%    || Estatística                 | Temperatura |
| :-------------------------- | :---------- |
| Menor média entre as ROIs   | 25,77 °C    |
| Média das médias das ROIs   | 29,59 °C    |
| Mediana das médias das ROIs | 29,29 °C    |
| Maior média entre as ROIs   | 33,53 °C    |Observei que:
- Delimitação: o contorno ciano acompanha os limites da região segmentada.
- Alinhamento: a máscara está posicionada corretamente sobre a matriz térmica, sem deslocamento aparente.
- Inclusão: a região delimitada corresponde à área mamária de interesse, sem incluir o fundo ou o artefato quadrangular inferior.
- Consistência: a máscara apresenta limites definidos, sem regiões isoladas aparentes.
- Orientação: a máscara está posicionada no lado correspondente à identificação do arquivo, considerando a convenção de orientação utilizada na base.

## Etapa B — Comparar normalizações
Foi realizada uma análise exploratória comparando duas estratégias de normalização Min-Max: individual e com limites comuns. Foram selecionados quatro quadros temporais do paciente PAC_38, lateral direita: DN0, DN1, DN10 e DN19. As transformações foram aplicadas exclusivamente aos pixels pertencentes às regiões de interesse (ROI).
Min-Max individual: cada quadro foi normalizado utilizando seus próprios valores mínimo e máximo. Consequentemente, todas as imagens passaram a apresentar valores entre 0 e 1. Essa abordagem preserva a distribuição relativa das temperaturas dentro de cada imagem, mas elimina a utilização de uma escala térmica compartilhada. Observou-se, inclusive, alteração na ordenação das médias normalizadas em relação às médias originais.
Min-Max com limites comuns: foi utilizado um único mínimo (22,78 °C) e um único máximo (30,55 °C), calculados a partir das quatro amostras. Essa transformação manteve uma escala compartilhada entre os quadros, preservando proporcionalmente as diferenças entre suas temperaturas médias.
Na escala original, a temperatura média da ROI diminuiu de 28,61 °C em DN0 para 27,32 °C em DN19, correspondendo a uma redução aproximada de 1,29 °C. A mesma tendência foi observada após a normalização com limites comuns.
Para análises que dependem da comparabilidade térmica entre quadros temporais, a normalização com limites comuns apresenta uma propriedade desejável em relação ao Min-Max individual: mantém uma escala compartilhada e preserva proporcionalmente as diferenças de temperatura entre imagens.

## Etapa C — Comparar redimensionamentos
Amostra analisada: PAC_38_DN0-dir e Dimensão original: 640 × 480 pixels (largura × altura).
Foram testadas quatro resoluções reduzidas: 320 × 240, 256 × 192, 224 × 168 e 160 × 120 pixels. A matriz térmica foi redimensionada por interpolação bilinear, enquanto a máscara da ROI foi redimensionada pelo método do vizinho mais próximo, preservando sua natureza binária.
A proporção da ROI permaneceu próxima de 12,45% em todas as resoluções. As dimensões relativas da caixa delimitadora e o centroide normalizado também apresentaram pequenas variações, indicando preservação da geometria e da posição relativa da região segmentada.
A quantidade absoluta de pixels da ROI diminuiu de 38.256 para 2.394 na resolução 160 × 120, acompanhando a redução da quantidade total de pixels da imagem.
As estatísticas globais apresentaram boa estabilidade. A média térmica variou de 28,6011 °C a 28,6074 °C, em comparação com 28,6053 °C na imagem original. O desvio-padrão também permaneceu próximo de 0,9338 °C. Os percentis centrais apresentaram alterações pequenas. Entretanto, os valores extremos foram mais sensíveis ao redimensionamento: na resolução 160 × 120, o máximo diminuiu de 30,3500 °C para 30,2738 °C, uma diferença de -0,0762 °C.
Para a amostra analisada, todas as resoluções candidatas preservaram satisfatoriamente a geometria da ROI e as estatísticas térmicas globais. A redução espacial apresentou maior efeito sobre os valores extremos, indicando a necessidade de investigar a preservação de detalhes térmicos localizados.

## Etapa D — Registro das decisões de pré-processamento
Adota-se provisoriamente a normalização Min-Max com limites comuns entre as imagens, em substituição à normalização individual por imagem.
A estratégia foi escolhida por preservar uma escala térmica compartilhada, permitindo maior comparabilidade entre quadros temporais e diferentes pacientes.
Os parâmetros mínimo e máximo serão estimados exclusivamente a partir dos dados de treinamento, após a divisão dos pacientes entre treinamento, validação e teste. Os mesmos parâmetros serão aplicados aos demais conjuntos, evitando vazamento de dados.
Serão utilizadas estratégias distintas para a matriz térmica e para a máscara da ROI:
- Matriz térmica: interpolação bilinear, adequada para dados numéricos contínuos, como as temperaturas expressas em °C.
- Máscara da ROI: interpolação por vizinho mais próximo, preservando a natureza binária da segmentação e evitando a criação de valores intermediários.
A combinação apresentou boa preservação geométrica e estatística na amostra exploratória PAC_38_DN0-dir.
Foram definidas quatro resoluções candidatas, preservando a proporção original de 4:3:
- 320 × 240 pixels;
- 256 × 192 pixels;
- 224 × 168 pixels;
- 160 × 120 pixels.
A escolha definitiva dependerá da avaliação da preservação das informações térmicas e espaciais, do custo computacional e da compatibilidade com a arquitetura Vision Transformer selecionada.
A normalização Min-Max com limites comuns e a combinação de interpolação bilinear para a matriz térmica e vizinho mais próximo para a máscara foram estabelecidas como escolhas provisórias.
As dimensões candidatas foram definidas para experimentação, sem seleção definitiva.
A configuração final será determinada após validação em múltiplos pacientes, avaliação da classificação e análise da qualidade das explicações produzidas pelo modelo.
