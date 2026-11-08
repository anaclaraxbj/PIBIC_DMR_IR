# PIBIC_DMR_IR

# Transformadores Visuais e Inteligência Artificial Explicável para Triagem Termográfica do Câncer de Mama

Projeto de Iniciação Científica (PIBIC/CNPq — UFU), desenvolvido no contexto da pesquisa em Inteligência Artificial aplicada à análise de imagens termográficas da mama.

O projeto investiga modelos de aprendizado profundo, com foco em **Vision Transformers (ViTs)**, para a classificação de imagens termográficas, explorando a integração de diferentes ângulos de captura e técnicas de Inteligência Artificial Explicável (XAI).

## Objetivos

- Preparar e padronizar as imagens da base pública DMR-IR.
- Investigar modelos de Vision Transformers para classificação de imagens termográficas da mama.
- Explorar estratégias de fusão de informações provenientes de múltiplos ângulos de captura.
- Aplicar técnicas de XAI, incluindo Grad-CAM e mapas de atenção, para visualizar regiões que influenciam as decisões dos modelos.
- Avaliar o desempenho dos classificadores e analisar as explicações geradas, considerando sua relevância para a interpretação dos resultados.

## Tecnologias e ferramentas

- **Linguagem:** Python
- **Aprendizado profundo:** PyTorch e Torchvision
- **Manipulação de dados:** NumPy e Pandas
- **Visualização:** Matplotlib
- **Experimentação:** Jupyter Notebook
- **Ambiente de desenvolvimento:** Visual Studio Code e Git

As ferramentas serão utilizadas conforme as necessidades de cada etapa da pesquisa.

## Base de dados

A pesquisa utiliza a base pública **DMR-IR**, composta por imagens termográficas relacionadas à investigação do câncer de mama.

Fonte de referência: [Digital Medical Images — Universidade Federal Fluminense](https://visual.ic.uff.br/dmi/).

Os dados originais são mantidos localmente.

## Estrutura do projeto

```text
PIBIC_DMR_IR/
├── data/
│   ├── raw/            # Dados originais, mantidos localmente
│   └── processed/      # Dados após o pré-processamento
├── models/             # Pesos e checkpoints gerados
├── results/            # Métricas, gráficos e visualizações
├── src/
│   ├── data/           # Carregamento e preparação dos dados
│   ├── training/       # Treinamento e validação dos modelos
│   └── xai/            # Implementação das técnicas de XAI
├── study/              # Estudos, tutoriais e exercícios
├── notebooks/          # Exploração dos dados e experimentos
```

Os diretórios `raw/`, `processed/`, `models/` e `results/` podem conter arquivos locais que não são versionados no Git. O repositório prioriza o código-fonte, a documentação e os materiais de estudo.

## Etapas da pesquisa

1. **Fundamentação teórica:** estudo de aprendizado profundo, Vision Transformers e Inteligência Artificial Explicável.
2. **Preparação dos dados:** organização, normalização e redimensionamento das imagens termográficas.
3. **Desenvolvimento dos modelos:** implementação e treinamento de modelos de classificação baseados em ViTs, considerando CNNs como referência de comparação.
4. **Fusão multiangular:** investigação da integração de informações provenientes de diferentes perspectivas das imagens.
5. **Explicabilidade:** geração e análise de mapas de calor com técnicas como Grad-CAM e mapas de atenção.
6. **Avaliação:** análise do desempenho dos modelos por métricas como acurácia, sensibilidade, especificidade, F1-score e AUC-ROC, além da avaliação visual das explicações.
7. **Produção e divulgação científica:** elaboração de relatório técnico, apresentação de resultados e produção de materiais de divulgação científica.

As etapas representam o planejamento da pesquisa; sua execução e seus resultados serão documentados à medida que o trabalho avançar.

## Status atual

**Em desenvolvimento — fase preparatória.**

As atividades iniciais incluem o estudo dos fundamentos de redes neurais e PyTorch, a configuração do ambiente de desenvolvimento, a organização da estrutura do projeto e a obtenção da base DMR-IR.

O pré-processamento completo dos dados, o treinamento e a avaliação dos modelos e a implementação das técnicas de XAI permanecem como etapas a serem desenvolvidas e documentadas.

