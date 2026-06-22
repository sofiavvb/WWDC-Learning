# Configurando o Obsidian

Guia para quem nunca usou o Obsidian. Em poucos minutos você abre este repositório como um
vault e passa a ver o grafo de conexões entre as notas — e suas notas sincronizam com o
GitHub automaticamente.

> As capturas de tela podem ser adicionadas depois — por enquanto, siga os passos no texto.

## 1. O que é o Obsidian (e por quê)

O Obsidian é um app gratuito de notas em Markdown puro. A partir dos `[[wikilinks]]` e das
tags das notas, ele gera um **grafo** que mostra visualmente como os assuntos se conectam.
Como nossas notas já são `.md` com wikilinks, o grafo sai de graça.

## 2. Instale o Obsidian

Baixe em [obsidian.md](https://obsidian.md). É gratuito para uso pessoal. Instale
normalmente para o seu sistema (macOS, Windows ou Linux).

## 3. Clone o repositório

Você precisa ter o **Git** instalado. No terminal:

```
git clone https://github.com/sofiavvb/WWDC-Learning.git
```

Isso baixa uma cópia do repositório para uma pasta local.

## 4. Abra o repositório como vault

No Obsidian, escolha **"Open folder as vault"** (Abrir pasta como vault) e aponte para a
pasta do repositório que você acabou de clonar. Pronto: as notas aparecem na barra lateral.

## 5. Instale o plugin Obsidian Git

Para não precisar rodar comandos de Git manualmente, use o plugin que sincroniza tudo
sozinho:

1. **Settings** (Configurações) → **Community plugins** → **Browse**.
2. Pesquise por **"Obsidian Git"**.
3. **Install** e depois **Enable**.

O plugin faz **pull e push automáticos**, então suas notas ficam em sincronia com o GitHub
sem comandos manuais. Nas configurações do plugin, vale definir um intervalo de auto-pull e
auto-push sensato (por exemplo, **a cada 10 minutos**).

## 6. Abra a visualização de grafo

Clique no ícone de **grafo** na barra lateral esquerda. As notas se conectam pelos
`[[wikilinks]]` da seção **"Conexões com outras sessões"** e pelas **tags** em comum.

## 7. Etiqueta e solução de problemas

- **Dê pull antes de editar** (o plugin geralmente faz isso sozinho, mas confira) para
  pegar as notas mais recentes dos colegas.
- O plugin cuida do **push** automaticamente — você não precisa fazer commit/push à mão.
- Lembre-se: **uma nota por pessoa, por sessão.** Não edite o arquivo de outra pessoa; crie
  o seu com o seu handle. Assim não há conflitos de Git.
