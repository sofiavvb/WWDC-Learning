# WWDC Notes Hub

Notas colaborativas sobre as sessões em vídeo da WWDC, organizadas e conectadas em um grafo via Obsidian.

## O que é / como funciona

Este repositório é uma base de conhecimento colaborativa com notas sobre as sessões em
vídeo da WWDC. As notas ficam organizadas pelas categorias oficiais da Apple (as mesmas
do portal de vídeos para desenvolvedores) e são arquivos `.md` simples com `[[wikilinks]]`
e tags, então qualquer pessoa pode abrir o repositório como um **vault do Obsidian** e
ganhar de graça um grafo de conexões entre os assuntos.

A filosofia é **contribuição sem fricção**: você dá push direto na `main`. A consistência
vem do template e deste README, não de revisões. Se contribuir for difícil, ninguém
contribui :)

## Como contribuir

1. **Clone** o repositório:
   ```
   git clone https://github.com/sofiavvb/WWDC-Learning.git
   ```
2. **Copie** o `TEMPLATE.md` para a pasta da categoria certa, dentro do ano correspondente:
   `WWDC{ano}/{categoria}/`.
3. **Renomeie** o arquivo para `{session-slug}-{seu-handle}.md`
   (ex: `meet-the-evaluations-framework-sofiavvb.md`). O `session-slug` é o título da
   sessão em minúsculas, com hifens no lugar dos espaços; o `handle` é o seu usuário do
   GitHub.
4. **Preencha** as seções do template (em português).
5. **Dê push direto na `main`.**

**Uma nota por pessoa, por sessão.** Se alguém já anotou uma sessão, **não edite o arquivo
dessa pessoa**: crie o seu próprio, com o seu handle. Isso mantém as notas de cada um
separadas e evita conflitos de Git.

**Screenshots?** Coloque na pasta `assets/` da categoria, nomeados como
`{session-slug}-{seu-handle}-{n}.png`, e referencie na nota com `![[nome-do-arquivo.png]]`.

## Categorias

As 18 categorias oficiais da Apple. As marcadas com ⭐ são as **categorias de foco** do
momento — as demais estão disponíveis normalmente, só não são a prioridade atual.

- ⭐ AI & Machine Learning
- ⭐ Swift
- ⭐ SwiftUI & UI Frameworks
- ⭐ Design
- Accessibility & Inclusion
- App Services
- App Store / Distribution & Marketing
- Audio & Video
- Business & Education
- Developer Tools
- Essentials
- Graphics & Games
- Health & Fitness
- Maps & Location
- Photos & Camera
- Privacy & Security
- Safari & Web
- Spatial Computing

## Obsidian

Nunca usou o Obsidian? Veja o passo a passo em [`OBSIDIAN_SETUP.md`](OBSIDIAN_SETUP.md).

## Tags

As tags são controladas (para manter o grafo limpo), mas crescem por convenção conforme
necessário. Veja a lista e as regras em [`TAGS.md`](TAGS.md).
