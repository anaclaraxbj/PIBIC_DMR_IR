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
- **Aprendizado profundo:** PyTorch
- **Experimentação:** Jupyter Notebook
- **Ambiente de desenvolvimento:** Visual Studio Code e Git

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

## Etapas da pesquisa

1. **Revisão da literatura e estudo sobre ViT e XAI:** Etapa inicial focada no estudo e fundamentação teórica para o desenvolvimento das atividades do projeto.
2. **Preparação dos dados:** Organização e pré-processamento rigoroso das imagens da base de dados pública DMR-IR, envolvendo a normalização térmica e o redimensionamento dos arquivos para garantir a padronização necessária à entrada dos algoritmos de aprendizado profundo.
3. **Desenvolvimento dos modelos:** Implementação e treinamento das arquiteturas de Vision Transformers (ViT), focando no ajuste dos mecanismos de atenção para a classificação precisa das patologias mamárias.
4. **Implementação de Explicabilidade (XAI):** Aplicação de ferramentas de Inteligência Artificial Explicável (XAI), como o Grad-CAM, gerando mapas de calor que permitam a validação visual das decisões tomadas pelo modelo.
5. **Validação de Resultados:** Coleta e análise de métricas estatísticas de desempenho computacional e validação das predições.
6. **Disseminação e Divulgação:** Colaboração na redação de relatórios técnicos e na elaboração de artigos científicos para eventos da área, além da produção de um PITCH em vídeo para o público leigo e criação de conteúdos educativos para redes sociais.
7. **Integração de Equipe:** Integração plena à equipe multidisciplinar do grupo de pesquisa GPEND4.0.

## Status atual

**Em desenvolvimento — fase preparatória.**

