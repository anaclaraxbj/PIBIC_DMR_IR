# Estrutura da base DMR-IR

## Organização dos diretórios

Minha base está organizada assim:

DMR-IR/
│
├── 12 Novos Casos de Testes/
└── Desenvolvimento da Metodologia/
    │
    ├── Doentes/
    │   ├── Matrizes/
    │   │   ├── matrizes de temperatura
    │   │   └── …
    │   ├── Segmentadas/
    │   ├── …
    │   └── png da mama/
    │
    └── Saudáveis/
        │
        └── …

Essa organização tem uma separação entre:

- Dados térmicos: matrizes TXT com temperaturas.
- Dados segmentados: imagens em que as regiões das mamas foram delimitadas.
- Conjuntos por classe: doentes e saudáveis.
- Conjunto de teste: os 12 novos casos.

Silva et al. (2020) descrevem um conjunto experimental composto por 64 casos de aprendizado, sendo 32 saudáveis e 32 com anomalias, além de 12 novos casos de teste. Cada caso é descrito com 20 imagens termográficas sequenciais.

## Tipos de arquivos e correspondências

| Característica | Resultado observado |
| --- | --- |
| Arquivos PNG | 1.522 |
| Arquivos TXT | 1.128 |
| Dimensões PNG mais frequentes | 640 × 480 pixels |
| Modos PNG | RGBA, LA e L |
| Dimensões TXT | 480 × 640 (linhas × colunas) |
| TXT lidos numericamente | 1.126 |
| TXT com falha na leitura inicial | 2 |

### Matrizes térmicas — TXT

Os arquivos TXT contêm matrizes numéricas de dimensão 480 × 640. Segundo Silva et al. (2020), os arquivos de matrizes térmicas descritos no artigo representam temperaturas, em graus Celsius, associadas aos pixels dos termogramas radiométricos.

### Imagens segmentadas — PNG

A base local apresenta imagens PNG organizadas em diretórios de segmentação. Foram identificados modos de imagem RGBA, LA e L, além de dimensões predominantemente iguais a 640 × 480 pixels.

Os nomes das imagens segmentadas podem apresentar sufixos como `-esq` e `-dir`, indicando distinções entre as regiões mamárias.

### Correspondência entre TXT e PNG

A metodologia descrita por Silva et al. (2020) utiliza imagens segmentadas como máscaras para selecionar os valores térmicos correspondentes às regiões de interesse nas matrizes TXT.

### Protocolos

Com base na documentação oficial da base de dados orig **DMR-IR** (*Database for Research Mastology with Infrared Image*) presente nos artigos de referência, os identificadores de ângulo, posição e sequência nas imagens térmicas seguem uma padronização rigorosa dividida entre o protocolo estático e o dinâmico:

- **Protocolo Estático (Static Protocol):** Abrange um conjunto de **5 posições e ângulos de incidência** padrão por paciente para capturar a distribuição térmica em estado de equilíbrio:
    1. **Front** (Visão Frontal)
    2. **Right Latera** (Lateral Direita a 45 graus)
    3. **Right Lateral** (Lateral Direita a 90 graus / Perfil direito)
    4. **Left Lateral** (Lateral Esquerda a 45 graus)
    5. **Left Lateral** (Lateral Esquerda a 90 graus / Perfil esquerdo)
- **Protocolo Dinâmico (Dynamic Protocol):** Monitora a recuperação da temperatura cutânea após o estresse térmico (resfriamento por ventilação):
    - **0 imagens sequenciais na posição Frontal:** Identificadas cronologicamente em ordem de sequência (por exemplo, de 1 a 20), capturadas em intervalos regulares durante o processo de retorno ao equilíbrio térmico.
    - **Imagens laterais complementares:** Inclui adicionalmente 2 imagens nos ângulos de **Right Lateral**para abranger toda a extensão lateral das mamas.

A Base de dados que estou usando deixa claro que: O método proposto no artigo utiliza e avalia **exclusivamente o Protocolo Dinâmico (DIT)** (*Dynamic Infrared Thermography*), baseando-se em sequências de 20 termografias capturadas ao longo do tempo após o estímulo térmico com um ventilador:

- **Protocolo e Quantidade de Imagens:** O estudo utiliza um conjunto de **0 termografias sequenciais** capturadas por paciente através do protocolo DIT. Com um total de 64 pacientes avaliados na fase de aprendizado, tem-se exatamente o montante de 1.280 imagen, o que corresponde a uma única sequência frontal por paciente.
- **Visualização da Região de Interesse (ROI):** As ilustrações do método (como a Figura 1 e a Figura 4) demonstram explicitamente que a captura e a segmentação subsequente focam na **vista frontal do tórax e das mamas** para o isolamento das regiões de interesse.

# Referências

1. SILVA, Thiago Alves Elias da et al. A Computational Method to Assist the Diagnosis of Breast Disease Using Dynamic Thermography. Sensors, 2020, v. 20, n. 14, art. 3866. [https://doi.org/10.3390/s20143866](https://doi.org/10.3390/s20143866?utm_source=chatgpt.com)
2. SILVA, Lincoln Faria da et al. A New Database for Mastology Research with Infrared Image. [https://doi.org/10.1166/jmihi.2014.1226](https://doi.org/10.1166/jmihi.2014.1226)
