---
title: Meet the Evaluations Framework
year: 2026
category: AI-Machine-Learning
url: https://developer.apple.com/videos/play/wwdc2026/298
author: sofiavvb
tags: [Foundation-Models, testing, on-device-AI, evaluation]
---

## Resumo em uma frase
O Evaluations framework oferece uma forma estruturada de testar features de AI generativa, onde o mesmo input pode produzir outputs diferentes — algo que testes unitários tradicionais não conseguem cobrir.

## Conceitos principais

**O problema:** em software tradicional, input A sempre gera output Y. Em modelos inteligentes essa relação é imprevisível, então precisamos de uma nova abordagem de testes.

**O fluxo básico de uma Evaluation:**
1. Definir o `subject` — o serviço/código a ser testado (chamado e retornado no método subject)
2. Montar o `dataset` com `ModelSample` (inputs + valores esperados)
3. Definir uma `Metric` (ex: `tagCount`)
4. Implementar um `Evaluator` (passa/falha na métrica)
5. Definir um `aggregateMetrics` summary

**Exemplo do vídeo (BookTagging):** uma feature inteligente que gera tags a partir da review de um livro. Usaram julgamento humano para identificar comportamentos a ajustar — garantir entre 3 e 8 tags por livro, não usar o título como tag, não usar várias palavras, identificar o gênero literário. A primeira expectativa testada foi justamente garantir que cada livro tenha entre 3 e 8 tags.

**Geração sintética de samples:** dados humanos não escalam ("human data creation doesn't scale"), então o framework oferece um `SampleGenerator` que cria novos samples automaticamente a partir de um modelo.

![[meet-the-evaluations-framework-sofiavvb-1.png]]

**Evaluation Driven Development:** metodologia iterativa de desenvolver features de AI — rodar a evaluation, analisar os resultados, ajustar o prompt ou o modelo, repetir.

![[meet-the-evaluations-framework-sofiavvb-2.png]]

**Model Judge:** um modelo de linguagem usado para dar uma nota ao output da sua feature, aplicado consistentemente ao longo do dataset. Pode usar o Private Cloud Compute (mais robusto que o modelo local). Bom para avaliações qualitativas. Dá para adicionar dimensões (`ScoreDimensions`) e um prompt para dar contexto ao julgamento.

## APIs e código mencionados
- `ModelSample` — define um input do dataset com valor esperado
- `Metric` — unidade de medida (ex: contagem de tags)
- `Evaluator` — protocolo para definir se um output passa ou falha
- `aggregateMetrics` — agrega resultados estatísticos do dataset
- `SampleGenerator` — gera samples sintéticos a partir de um modelo
- `ModelJudgeEvaluator` — avaliador qualitativo via LLM
- `ScoreDimensions` — dimensões de scoring para o Model Judge

## Casos de uso práticos
- **App de minigame com avaliação de performance (Swift Student Challenge):** o modelo gera uma avaliação textual da performance do usuário num minigame, mas é difícil saber se essas avaliações estão boas. O Evaluations framework permitiria definir métricas objetivas (ex: a avaliação menciona o ponto específico em que o usuário errou?) e usar um Model Judge para medir a qualidade qualitativa do feedback gerado.

## Conexões com outras sessões
- [[Create Robust Evaluations for Agentic Apps]] — aprofunda a geração de datasets e casos avançados de ModelSample

## Recursos oficiais
- [Página da sessão](https://developer.apple.com/videos/play/wwdc2026/298)
