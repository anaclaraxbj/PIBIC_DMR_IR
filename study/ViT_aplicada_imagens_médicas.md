# Vision Transformers em imagens médicas

## 1 — Identificação da fonte
- Autores: Khalid Al-Hammuri, Fayez Gebali, Awos Kanan e Ilamparithi Thirumarai Chelvan.
- Ano: 2023.
- Título: Vision transformer architecture and applications in digital health: a tutorial and survey.
- Periódico: Visual Computing for Industry, Biomedicine, and Art.
- DOI: https://doi.org/10.1186/s42492-023-00140-9
- Tipo de publicação: Artigo de revisão/tutorial.

---

## 2 — Objetivo do trabalho
O objetivo é discutir os fundamentos do ViT e suas aplicações em saúde digital, apresentar um roteiro de implementação e discutir limitações e desafios. Já que o ViT surgiu como mecanismo computacional de última geração passando a substituir técnicas tradicionais como os CNN's. 
O objetivo de estudar ViTs no contexto de imagens médicas é aproveitar a atenção global do ViT para relacionar características distantes, sendo um possível bom substituto para CNNs.


---

## 3 - Fundamentação teórica

### 3.1. Arquitetura ViT
Na arquitetura ViT a imagem é dividida em patches que são achatados e dispostos como uma sequência linear, A Fig. 3a (p. 4) mostra a imagem repartida e o processo repetido para N imagens. Cada patch achatado passa por uma projeção linear e vira um vetor de características (embedding), os valores vão sendo atualizados durante o treino, patches com características parecidas ficam mais próximos no espaço latente.
O vetor de posição é somado ao de características, formando um novo vetor no espaço de embedding. O encoder Transformer é uma pilha de N camadas idênticas, cada uma com duas subcamadas: (1) normalização, atenção multi-cabeça (MSA) e conexão residual; (2) normalização, MLP e conexão residual.
A MSA calcula uma média ponderada das representações com base nas similaridades entre pares de representações. A entrada X (L tokens, dimensão d) é projetada em Key, Query e Value (K = XWK, Q = XWQ, V = XWV). O score S = QKᵀ é escalado por raiz de d, normalizado com softmax e multiplicado por V: Atenção(Q, K, V) = softmax(QKᵀ / raiz(d)) · V (eq. 9). Em multi-cabeça, várias atenções rodam em paralelo, cada uma pode focar em características diferentes, e os resultados são concatenados e passam por uma camada linear.

### 3.2. Aplicações em imagens médicas
| Tarefa                            | Modalidades                                              | Métodos citados (exemplos)                                                                                                       | Tabela |
| :-------------------------------- | :------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------- | :----- |
| Segmentação (38%)                 | RM, TC, raio X, colonoscopia, endoscopia                 | TransUNet, Dual/DS-TransUNet, Swin-Unet, iSegFormer, 3D UX-Net, UNesT, NestedFormer, RECIST, GT U-Net, FCN-transformer, TraSeTR  | 1      |
| Detecção (2%)                     | RM T2, colonoscopia, TC                                  | DETR, COTR, SATr, UCLT, IHD                                                                                                      | 2      |
| Classificação (19%)               | RM, raio X, histopatologia, citopatologia, RM de difusão | TransMed (ViT + CNN), GasHis-transformer, ScoreNet (câncer de mama em histopatologia), RadioTransformer (COVID-19), TractoFormer | 3      |
| Predição / prognóstico            | volumes 3D, genoma, ultrassom, raio X, laparoscopia      | 3D-SMx, GBT, RTM, CLIMAT, Sig-Former, SIGT                                                                                       | 4      |
| Reconstrução (13%) e síntese (3%) | RM, TC, endoscopia                                       | SLATER, T2Net, ReconFormer, FastMRIRecon, E-DSSR, DuTrans, MIST-net                                                              | 5      |
| Geração de laudos (16%)           | imagens médicas gerais, câmera cirúrgica                 | RTMIC, IFCC, AlignTransformer, LDASR                                                                                             | 6      |
| Segurança em telessaúde           | dados de sistemas de informação                          | criptografia em blocos, MFVT, detecção de intrusão, antispoofing, defesa contra backdoor                                         | 7      |

---

## 4- Resultados e contribuições
Os benefícios são: Atenção global, representação robusta de características, processamento paralelo dos patches, capacidade de processar dados em formatos diferentes e gerar laudos ou instruções.
As limitações: exigem muitos dados, tem capacidade limitada em localização, a ResNet foi melhor com conjuntos pequenos de dados. Estratégias contra a falta de dados: transfer learning, data augmentation, síntese de imagens, rotulagem automática, inicialização de pesos por autossupervisão (ref. 106) e aprendizado contrastivo (refs. 107 e 108).

---

## 5. Relação com a pesquisa de iniciação científica
Projeto: Inteligência artificial explicável na classificação de câncer de mama em imagens termográficas.
Um modelo híbrido (CNN extrai características locais e um Transformer relaciona regiões) é uma alternativa a comparar, já que o artigo mostra híbridos funcionando em outras modalidades.

---

## 6. Questões e lacunas identificadas

### A. Representação dos dados térmicos
1. Como representar uma matriz térmica de temperaturas em graus Celsius na entrada de um Vision Transformer? A matriz térmica pode ser representada como uma imagem de um único canal, em que cada pixel corresponde a uma temperatura em Celsius. Após o pré-processamento e a normalização, a matriz é dividida em patches, que são achatados, transformados em embeddings e associados a informações posicionais.

2. Como a escolha do tamanho dos patches pode afetar a identificação de pequenas regiões de hipertermia? Patches menores preservam mais detalhes espaciais e podem favorecer a identificação de pequenas regiões de hipertermia, mas aumentam a quantidade de tokens e o custo computacional. Patches maiores reduzem o custo, porém podem agrupar pequenas alterações térmicas com regiões maiores, dificultando sua identificação.

### B. Arquitetura e aprendizado
1. A capacidade de atenção global do ViT pode contribuir para identificar assimetrias térmicas entre as duas mamas? Sim. A atenção global permite relacionar regiões espacialmente distantes da imagem, podendo contribuir para identificar diferenças de temperatura e distribuição de hipertermia entre as duas mamas. Entretanto, para essa comparação ocorrer diretamente, ambas precisam estar representadas conjuntamente na entrada ou em uma arquitetura que permita relacionar suas representações.

2. Considerando a quantidade limitada de pacientes independentes na DMR-IR, o uso de transferência de aprendizado seria mais adequado do que treinar um ViT do zero? Sim. A transferência de aprendizado é uma estratégia promissora porque permite utilizar representações visuais previamente aprendidas em grandes conjuntos de imagens, reduzindo a necessidade de aprender todos os parâmetros a partir de uma base pequena.

## 7 - Síntese crítica
Os três aprendizados mais importantes: a estrutura do ViT: patches, embeddings com posição, pilha de blocos de atenção multi-cabeça e MLP com residual e normalização, e cabeça de classificação. 
O tamanho do conjunto de dados é o fator crítico. Com poucos dados, CNNs, transfer learning e modelos híbridos tendem a ser mais seguros que um ViT puro.
Na área médica, os ViTs aparecem sobretudo em segmentação e classificação, muitas vezes em modelos híbridos, mas o artigo é uma revisão narrativa e otimista, então as afirmações de superioridade precisam de checagem.
Mahoro e Akhloufi (2022), citado como ref. 24 neste artigo: câncer de mama em termogramas com CNN e Transformers. Ainda não li esse trabalho; vale ler antes de afirmar qualquer coisa sobre ViT em termografia. Para o problema de poucos dados: refs. 105 (Chen et al., 2022) e 106 (Gani et al., 2022), citadas no artigo.


---
