Aqui está uma versão refinada e formatada do seu texto, ideal para ser inserida diretamente no seu Notion ou GitHub (`.md`). Ajustei pontualmente a coesão de algumas frases, corrigi os números "0" que estavam no texto original (assumindo que se referiam a "20 termografias") e apliquei recursos visuais do Markdown para facilitar a leitura.

---

# Estrutura da Base de Dados DMR-IR

## 1. Organização dos Diretórios

A estrutura local da base de dados está organizada em dois grandes blocos, separando a fase de desenvolvimento metodológico do conjunto final de testes:

```text
DMR-IR/
├── 12 Novos Casos de Testes/
└── Desenvolvimento da Metodologia/
    ├── Doentes/
    │   ├── Matrizes/
    │   │   └── (matrizes de temperatura em .txt)
    │   ├── Segmentadas/
    │   │   └── (arquivos de demarcação .png)
    │   └── Png da mama/
    │
    └── Saudáveis/
        └── (mesma estrutura interna)

```

Essa arquitetura hierárquica garante a separação lógica entre:

* **Dados térmicos:** Matrizes numéricas (`.txt`) contendo as leituras de temperatura em graus Celsius.
* **Dados segmentados:** Imagens (`.png`) utilizadas como máscaras (ROI), delimitando estritamente a região do tecido mamário.
* **Classes de diagnóstico:** Pacientes saudáveis e pacientes com anomalias confirmadas.
* **Conjuntos de avaliação:** Separação isolada de 12 novos casos reservados exclusivamente para a etapa de testes do modelo.

Conforme validado por **Silva et al. (2020)**, o conjunto experimental principal é composto por **64 casos de aprendizado** (32 saudáveis e 32 doentes), além dos 12 novos casos de teste. Cada paciente possui uma série temporal de 20 imagens termográficas sequenciais.

## 2. Inventário e Correspondência de Arquivos

| Característica | Resultado Observado |
| --- | --- |
| **Arquivos PNG** | 1.522 |
| **Arquivos TXT** | 1.128 |
| **Dimensões padrão PNG** | 640 × 480 pixels |
| **Modos de cor PNG** | RGBA, LA e L |
| **Dimensões matriz TXT** | 480 × 640 (linhas × colunas) |
| **Arquivos TXT validados** | 1.126 (leitura numérica bem-sucedida) |
| **Arquivos TXT com falha** | 2 (erro na leitura inicial) |

### 2.1. Matrizes Térmicas (`.txt`)

Os arquivos em formato de texto (`.txt`) armazenam tensores bidimensionais (480 × 640). De acordo com Silva et al. (2020), cada célula dessa matriz representa a temperatura absoluta medida em **graus Celsius (°C)** do pixel correspondente, gerada diretamente pelo sensor termográfico.

### 2.2. Imagens Segmentadas (`.png`)

Localizadas nos diretórios de segmentação, essas imagens apresentam resolução padrão (640 × 480) e variam nos modos de cor (RGBA, LA e L). Elas frequentemente carregam sufixos nominais como `-esq` (esquerda) e `-dir` (direita), fundamentais para a distinção anatômica lateral.

> **Nota Metodológica:** A principal função dessas imagens (`.png`) é atuar como **máscaras binárias**. A metodologia de Silva et al. (2020) sobrepõe essas imagens segmentadas sobre as matrizes térmicas (`.txt`) para isolar, extrair e computar apenas as temperaturas pertinentes ao tecido mamário (Região de Interesse - ROI), ignorando o fundo da imagem.

## 3. Protocolos de Aquisição: Estático vs. Dinâmico

A base oficial **DMR-IR** (*Database for Mastology Research with Infrared Image*) engloba exames capturados sob dois protocolos distintos.

### Protocolo Estático (Static Protocol)

Focado no equilíbrio térmico basal do paciente, captura **5 incidências angulares** padrão:

1. **Front:** Visão frontal anatômica.
2. **Right Lateral (45°):** Oblíqua direita.
3. **Right Lateral (90°):** Perfil absoluto direito.
4. **Left Lateral (45°):** Oblíqua esquerda.
5. **Left Lateral (90°):** Perfil absoluto esquerdo.

### Protocolo Dinâmico (Dynamic Protocol - DIT)

Focado na análise transiente, monitora a recuperação térmica da pele após um estresse físico provocado por resfriamento local (ventilação dirigida). O subset utilizado para a presente pesquisa opera **exclusivamente sob o protocolo dinâmico**, cujas características são:

* **Sequência Frontal:** 20 imagens termográficas sequenciais capturadas na vista frontal durante a fase de recuperação de temperatura.
* **Volume de Dados:** Utilizando 64 pacientes no conjunto de aprendizado com 20 termogramas cada, totaliza-se **1.280 imagens sequenciais**.
* **Isolamento da ROI:** As ilustrações e diretrizes metodológicas do método de extração demonstram explicitamente que todo o pipeline de processamento (segmentação e classificação) foca apenas na **vista frontal do tórax**. Embora a base contenha imagens laterais complementares de recuperação térmica, o treinamento é centralizado na incidência frontal.

---

### Referências Bibliográficas

1. **SILVA, Thiago Alves Elias da et al.** *A Computational Method to Assist the Diagnosis of Breast Disease Using Dynamic Thermography*. Sensors, v. 20, n. 14, art. 3866, 2020. DOI: [10.3390/s20143866](https://doi.org/10.3390/s20143866).
2. **SILVA, Lincoln Faria da et al.** *A New Database for Mastology Research with Infrared Image*. Journal of Medical Imaging and Health Informatics, 2014. DOI: [10.1166/jmihi.2014.1226](https://doi.org/10.1166/jmihi.2014.1226).
