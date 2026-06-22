# WWDC Notes Hub — Repository Build Spec
**Working spec · v1.2 · build instructions for Claude Code**

> **v1.2 changelog:** all contributor-facing repo content (README, OBSIDIAN_SETUP, TAGS, assets/README, template section prompts) must be written in Portuguese — see §1.1. Technical identifiers (filenames, frontmatter field names, category folder names) stay as-is.
> **v1.1 changelog:** multiple-authors-per-session resolved as separate per-author files (§2, §4, §8). Added `author` frontmatter field; filename convention now `{session-slug}-{author}.md`; image convention now includes author suffix; seed note authored by `sofiavvb`. README contribution flow updated (§3).

This document captures the complete design and file structure for a collaborative GitHub repository where the Apple Developer Academy organizes notes about WWDC video sessions, integrated with Obsidian for a graph of connections between topics. It is written to be executed directly by Claude Code: every file, folder, and piece of content needed for the first commit is specified here. Everything here is settled unless marked *open*.

The only pre-existing asset is a local working folder (no `git init` yet). Claude Code should initialize the repository, create the full structure below, and populate it with the starter content.

---

## 1. Goal & principles

- **Purpose:** a shared, low-friction knowledge base of WWDC session notes that Academy colleagues can contribute to freely, with an Obsidian-generated connection graph layered on top.
- **Core principle — friction-zero contribution:** colleagues push directly to `main`. The bet is that engagement matters more than enforced consistency; if contributing is hard, people simply won't contribute. Consistency comes from a clear template and README, not from review gates. (Declined: Pull Request workflow — correct for quality, wrong for this audience's adoption.)
- **Obsidian-native:** notes are plain `.md` files with frontmatter and `[[wikilinks]]`, so any contributor can open the repo as an Obsidian vault and get the graph for free.

### 1.1 Language of the repository

**All contributor-facing content must be written in Portuguese (pt-BR).** The Academy group works in Portuguese and the notes are in Portuguese, so every file a contributor reads should be too. This applies to:
- `README.md` — all prose
- `OBSIDIAN_SETUP.md` — the entire step-by-step guide
- `TAGS.md` — the usage instructions
- `AI-Machine-Learning/assets/README.md` — the image-naming note
- the comment/prompt text inside `TEMPLATE.md` (the section headings are already Portuguese; the seed note is already Portuguese)
- the commit message in build step 6

**Stays in English / unchanged (do NOT translate):** filenames and slugs, frontmatter field *names* (`title`, `year`, `category`, `url`, `author`, `tags`), category folder names (they mirror Apple's official category names), and this spec document itself (it is build instruction for Claude Code, not contributor-facing). Translating technical identifiers would break consistency with tooling and with Apple's official naming.

---

## 2. Repository structure

```
wwdc-notes-hub/
├── README.md
├── TEMPLATE.md
├── TAGS.md
├── OBSIDIAN_SETUP.md
├── .gitignore
└── WWDC26/
    ├── Accessibility-Inclusion/
    ├── AI-Machine-Learning/
    │   ├── meet-the-evaluations-framework-sofiavvb.md
    │   └── assets/
    │       ├── meet-the-evaluations-framework-sofiavvb-1.png   (placeholder — see §6)
    │       └── meet-the-evaluations-framework-sofiavvb-2.png   (placeholder — see §6)
    ├── App-Services/
    ├── App-Store-Distribution-Marketing/
    ├── Audio-Video/
    ├── Business-Education/
    ├── Design/
    ├── Developer-Tools/
    ├── Essentials/
    ├── Graphics-Games/
    ├── Health-Fitness/
    ├── Maps-Location/
    ├── Photos-Camera/
    ├── Privacy-Security/
    ├── Safari-Web/
    ├── Spatial-Computing/
    ├── Swift/
    └── SwiftUI-UI-Frameworks/
```

**Decisions embedded here:**

- **Multi-year structure, starts with WWDC26 only.** Top-level folders are years (`WWDC26/`, later `WWDC25/`, etc.). Only WWDC26 is created now; adding a past year later requires no refactor. (Declined: single-year — would force restructuring later. Declined: multi-year pre-filled — too much upfront work.)
- **All 18 official Apple categories created as folders**, even empty ones, mirroring the Apple Developer videos portal exactly. This means no "where do I put this?" friction for any future session. Empty-folder noise is solved with context in the README (§3), not by omitting categories. (Declined: curated subset — creates orphan sessions with no home.)
- **The 18 categories** (from developer.apple.com/videos/): Accessibility & Inclusion, AI & Machine Learning, App Services, App Store / Distribution & Marketing, Audio & Video, Business & Education, Design, Developer Tools, Essentials, Graphics & Games, Health & Fitness, Maps & Location, Photos & Camera, Privacy & Security, Safari & Web, Spatial Computing, Swift, SwiftUI & UI Frameworks. Folder names use hyphens and drop ampersands for filesystem friendliness.
- **Empty category folders need a `.gitkeep`** file so Git tracks them (Git does not track empty directories). Claude Code should add an empty `.gitkeep` to every category folder that has no note yet.
- **Images live in an `assets/` subfolder inside each category folder**, named after the session slug **plus the author handle** with a numeric suffix (e.g. `meet-the-evaluations-framework-sofiavvb-1.png`). The author suffix prevents filename collisions when two people annotate the same session and both have screenshots. Referenced in notes via Obsidian embed syntax `![[filename.png]]`.
- **One note file per author per session.** When several people annotate the same session, each creates their own file rather than co-editing one. Filename convention: `{session-slug}-{author}.md` (e.g. `meet-the-evaluations-framework-sofiavvb.md`). This is the only structure consistent with push-to-main + friction-zero: separate files mean zero merge conflicts. (Declined: single collaborative note per session — reintroduces the merge-conflict friction that push-to-main was chosen to avoid, and Git conflicts are exactly what trips up contributors new to Git.) The trade-off — the Obsidian graph shows multiple nodes for one session — is accepted; shared tags and `[[wikilinks]]` still connect them, and the `author` frontmatter field credits each contributor.

---

## 3. README.md

Root README. **Written in Portuguese (pt-BR)** — see §1.1. Must contain, in this order:

1. **Title + one-line description** of what the repo is.
2. **What this is / how it works** — short paragraph: collaborative WWDC notes, organized by Apple's own categories, Obsidian-compatible for a connection graph.
3. **How to contribute** — the friction-zero flow: clone, copy `TEMPLATE.md` into the right `WWDC{year}/{category}/` folder, rename it to `{session-slug}-{your-handle}.md` (e.g. `meet-the-evaluations-framework-sofiavvb.md`), fill it in, push directly to `main`. Explain the per-author convention explicitly: **if someone already annotated a session, don't edit their file — create your own with your handle.** This keeps everyone's notes separate and avoids Git conflicts. Put screenshots in that category's `assets/` folder, named `{session-slug}-{your-handle}-{n}.png`.
4. **Academy focus categories** — the full list of 18 categories, with the Academy's priority categories visually marked (a ⭐ prefix). Mark these four as focus: **AI & Machine Learning**, **Swift**, **SwiftUI & UI Frameworks**, **Design**. The rest are listed normally — available, just not the current focus.
5. **Obsidian** — one line pointing to `OBSIDIAN_SETUP.md`.
6. **Tags** — one line pointing to `TAGS.md` and explaining tags are controlled but grow via convention.

Tone: welcoming, written for people who may be new to Git. Keep it short.

---

## 4. TEMPLATE.md

The copyable note template. Exact content:

```markdown
---
title:
year:
category:
url:
author:
tags: []
---

## Resumo em uma frase

## Conceitos principais

## APIs e código mencionados

## Casos de uso práticos
> Onde esse conteúdo se aplicaria num app real?

## Conexões com outras sessões

## Recursos oficiais
```

**Template decisions:**
- **Frontmatter fields:** `title`, `year`, `category`, `url`, `author`, `tags`. The `author` field holds the contributor's handle (GitHub handle recommended, for consistency with who actually made the commit). (Declined: `speakers`, `session_id`, `platforms` — friction without clear payoff for this audience. Declined: `related_sessions` in frontmatter — connections live in the body instead, see below.)
- **`tags` are controlled** — must come from `TAGS.md` (see §5). New tags are added to `TAGS.md` by convention when needed.
- **Connections between sessions live only in the body**, in the "Conexões com outras sessões" section, written as Obsidian `[[wikilinks]]`. Not duplicated in frontmatter — keeps notes readable; the graph still picks up body wikilinks. (Declined: duplicating in a frontmatter `related_sessions` field — redundant for this use.)
- **"Casos de uso práticos"** is a deliberate addition: contributors note where the session's content would apply in a real app. This is also the seed for a future skill that will help generate these. The blockquote line is a built-in prompt.
- The template is in Portuguese because that's the working language of this Academy group; contributors write notes in Portuguese.

---

## 5. TAGS.md

The controlled, living list of tags. **Instructions written in Portuguese (pt-BR).** Structure:

1. Short intro: tags are controlled to keep the graph clean; to add a new tag, add it here in the same commit as the note that uses it.
2. A flat list of the starting tags (from the seed note): `Foundation-Models`, `testing`, `on-device-AI`, `evaluation`, `AI-ML`.
3. A one-line convention note: use hyphens, no spaces; prefer reusing an existing tag over inventing a near-duplicate.

---

## 6. OBSIDIAN_SETUP.md

Step-by-step guide **written in Portuguese (pt-BR)** for people who have never used Obsidian. Must cover, in order:

1. **What Obsidian is and why** — plain-markdown notes app that generates a graph of connections from `[[wikilinks]]` and tags. One or two sentences.
2. **Install Obsidian** — download from obsidian.md, free for personal use.
3. **Clone the repo locally** — the `git clone` command, with a note that they need Git installed.
4. **Open the repo as a vault** — "Open folder as vault" in Obsidian, point it at the cloned repo folder.
5. **Install the Obsidian Git plugin** — Settings → Community plugins → Browse → search "Obsidian Git" → install → enable. Explain it auto-syncs (pull/push) so notes stay in sync with GitHub without manual Git commands. Mention a sensible auto-pull/auto-push interval.
6. **Open the graph view** — the graph icon in the left sidebar; explain that notes connect via the `[[wikilinks]]` in the "Conexões" section and via shared tags.
7. **A short troubleshooting / etiquette note** — pull before editing, the plugin handles pushing.

Keep each step concrete and beginner-safe. Note that screenshots can be added later (link placeholders are fine).

---

## 7. .gitignore

Include Obsidian's per-user workspace cruft so personal Obsidian settings don't get committed and cause noise:

```
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/cache
.DS_Store
.trash/
```

Note: do NOT ignore the whole `.obsidian/` folder — if the group later wants to share vault config (graph colors, etc.) that can be committed. For now only the per-user/volatile files are ignored.

---

## 8. Worked example — the seed note

The first real note, used throughout the design to validate the template. Lives at `WWDC26/AI-Machine-Learning/meet-the-evaluations-framework-sofiavvb.md`. This is real content from contributor `sofiavvb`'s notes on the session "Meet the Evaluations framework" (developer.apple.com/videos/play/wwdc2026/298). Write it exactly as below:

```markdown
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
```

**Image placeholders:** the contributor's original notes had two screenshots ("metodologia" and the EDD diagram). Claude Code cannot recreate these. Create the `assets/` folder and add a short `assets/README.md` instructing the contributor to drop the two real screenshots in with the exact filenames `meet-the-evaluations-framework-sofiavvb-1.png` and `meet-the-evaluations-framework-sofiavvb-2.png` (no fake binary files in the repo). The `assets/README.md` should also state the image naming convention for future contributors: `{session-slug}-{author}-{n}.png`.

---

## 9. Build steps for Claude Code

1. `git init` in the working folder.
2. Create all 18 category folders under `WWDC26/`, each with a `.gitkeep` (except `AI-Machine-Learning/`, which gets the seed note instead).
3. Create `AI-Machine-Learning/assets/` with an `assets/README.md` (see §8).
4. Write the seed note at `WWDC26/AI-Machine-Learning/meet-the-evaluations-framework-sofiavvb.md` (§8).
5. Write `README.md` (§3), `TEMPLATE.md` (§4), `TAGS.md` (§5), `OBSIDIAN_SETUP.md` (§6), `.gitignore` (§7).
6. Make an initial commit (e.g. "Initial structure: WWDC26 notes hub + Evaluations framework seed note").
7. Print the final tree and a one-paragraph summary of what was created, plus the exact commands the user runs to create the GitHub remote and push (`gh repo create` or manual `git remote add` + `git push -u origin main`). Do not assume a GitHub account name — leave a placeholder.

---

## 10. Open / next

- **Future skill — connection & use-case helper:** a planned skill that, given a note, suggests related sessions to fill the "Conexões" section and helps draft "Casos de uso práticos". The template already reserves both sections for it. *(future work, owned by the user)*
- **[CONVENTION RISK — watch as it grows] Tag sprawl:** controlled tags only work if contributors actually check `TAGS.md` before inventing tags. With push-to-main and no review, nothing enforces this. Mitigation is cultural (clear note in `TAGS.md`). Revisit if the tag list gets messy. *(open)*
- **[CONSISTENCY RISK — accepted] No review gate:** push-to-main means malformed notes can land. Accepted deliberately for adoption; the template + README are the only guardrails. Revisit only if quality becomes a real problem. *(open)*
- **Shared Obsidian config:** `.obsidian/` vault settings (graph colors, etc.) are not committed yet — left as a future option if the group wants a shared look. *(open)*
- **Past years:** WWDC25 and earlier can be added as sibling folders when someone wants to; no structural change needed. *(open)*
