---
title: What's new in image understanding
year: 2026
category: AI-Machine-Learning
url: https://developer.apple.com/videos/play/wwdc2026/237/
author: sofiavvb
tags:
  - Vision-Framework
  - Foundation-Models
  - image-understanding
  - tool-calling
  - on-device-AI
---

## Resumo

A sessão apresenta três conjuntos de novidades em compreensão de imagens no WWDC 2026. A **nova API tap-to-segment no Vision** permite selecionar e segmentar qualquer objeto de uma imagem com toques, traços lasso ou retângulos, gerando uma máscara refinável de forma iterativa. O **Foundation Models passa a aceitar imagens como entrada via** `Attachment`, viabilizando tarefas descritivas como geração de legendas e sugestão de receitas, em contraste com o Vision, ajustado para tarefas específicas de visão computacional e muito mais rápido para isso. A sessão cobre também como criar ferramentas baseadas em imagem para estender o modelo, além de ferramentas nativas do Vision como `BarcodeReaderTool` e `OCRTool` para leitura de barcodes e texto em mais de 30 idiomas.

## Conceitos principais

- **Vision vs Foundation Models:** Vision é rápido e fine-tuned para tarefas específicas de visão computacional; Foundation Models usa LLMs para compreensão mais ampla e tarefas descritivas. A escolha depende do tipo de tarefa.
- **Tap-to-segment:** permite selecionar qualquer objeto de uma imagem com um ponto, traço lasso ou retângulo e gerar uma máscara de segmentação.
- **Coordenadas normalizadas:** Vision usa um sistema de 0.0 a 1.0; a largura mínima do traço lasso é 1% da largura total da imagem.
- **`ImageRequestHandler`:** centraliza o processamento de imagens no Vision — a imagem é carregada uma vez e reutilizada por múltiplos requests.
- **Segmentação iterativa:** a máscara pode ser refinada adicionando novos pontos com `addIncludedPoint(_:)` sem precisar reprocessar do zero.
- **Imagens como entrada para LLMs:** via `Attachment(image)` dentro de um `Prompt`, o Foundation Models consegue raciocinar sobre o conteúdo visual.
- **Image-based tools e `ImageReference`:** ferramentas (`Tool`) podem declarar parâmetros do tipo `ImageReference` com `@Generable`. Dentro de `call(arguments:)`, a imagem é recuperada via `imageReference.resolve(in: transcript)`, onde `transcript` é construído a partir do histórico da sessão (`@SessionProperty(\.history)`). O modelo passa a imagem pelo histórico e a ferramenta a recupera de lá.
- **Ferramentas nativas do Vision:** `BarcodeReaderTool` e `OCRTool` são fornecidas pela Apple e passadas diretamente no `LanguageModelSession(model:tools:)` — não é necessário implementar o protocolo `Tool` para elas.
- **Download do modelo:** a API tap-to-segment requer download do modelo on-device via `downloadAssets` antes de usar.

## APIs e código mencionados

**Tap-to-segment:**
```swift
// Gera uma máscara de segmentação com um ponto inicial
let handler = ImageRequestHandler(image)
let request = GenerateIterativeSegmentationRequest(seed: point)
let observation = try await handler.perform(request)
let mask = observation?.pixelBuffer

// Refina a máscara com um novo ponto
request.addIncludedPoint(newPoint)
let refinedObservation = try await handler.perform(request)
```

**Geração de legenda com Foundation Models:**
```swift
import FoundationModels

let prompt = Prompt {
    "Generate a caption for this image"
    Attachment(image)
}
let response = try await session.respond(to: prompt)
let caption = response.content
```

**Ferramenta baseada em imagem (exemplo: PlantIdentifierTool):**
```swift
struct PlantIdentifierTool: Tool {
    @SessionProperty(\.history) var history

    @Generable
    struct Arguments {
        var image: ImageReference
    }

    func call(arguments: Arguments) async throws -> String {
        let imageReference = arguments.image
        let transcript = Transcript(history)
        guard let imageAttachment = imageReference.resolve(in: transcript) else {
            throw AppError.imageNotFound
        }
        let image = try imageAttachment.pixelBuffer()
        return classifyPlant(image)
    }
}
```

**Usando ferramentas nativas do Vision:**
```swift
import FoundationModels
import Vision

let session = LanguageModelSession(model: model, tools: [BarcodeReaderTool()])
let response = try await session.respond(generating: EventInfo.self) {
    "Get the date, location, and website from this flyer"
    Attachment(image).label("flyer")
}
```

**APIs mencionadas:**
- `ImageRequestHandler` — carrega e centraliza o processamento de uma imagem no Vision
- `GenerateIterativeSegmentationRequest(seed:)` — request de segmentação iterativa
- `request.addIncludedPoint(_:)` — refinamento da máscara com novos pontos
- `downloadAssets` — download do modelo on-device para tap-to-segment
- `Attachment(image)` — passa imagens para o Foundation Models
- `Tool` + `ImageReference` + `@Generable` — criação de ferramentas com parâmetros de imagem
- `@SessionProperty(\.history)` — acesso ao histórico da sessão dentro de uma ferramenta
- `imageReference.resolve(in: transcript)` — recupera a imagem do histórico
- `BarcodeReaderTool` / `OCRTool` — ferramentas nativas de leitura de código e texto (prontas da Apple)

## Casos de uso práticos
> Onde esse conteúdo se aplicaria num app real?

**App de culinária:** o usuário fotografa a geladeira e o app usa Foundation Models com `Attachment(image)` para identificar os ingredientes e gerar uma receita com base no que está disponível.

**App de natureza:** um `PlantIdentifierTool` personalizado recebe a imagem fotografada pelo usuário e classifica a planta ou fungo usando modelos de visão computacional locais, com o Foundation Models coordenando a resposta em linguagem natural.

## Conexões com outras sessões

- [[What's new in the Foundation Models framework]] (WWDC26)
- [[Deep dive into the Foundation Models framework]] (WWDC25)
- [[Discover Swift enhancements in the Vision framework]] (WWDC24)

## Recursos oficiais

- https://developer.apple.com/videos/play/wwdc2026/237/
- https://developer.apple.com/documentation/Vision/segmenting-objects-using-taps-scribbles-or-rectangles
