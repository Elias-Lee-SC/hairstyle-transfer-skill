# Hairstyle Transfer Skill

[English](README.md) | Português | [中文](README.zh-Hant.md) | [日本語](README.ja.md)

`hairstyle-transfer` é uma Skill compatível com Codex / Cortex para trocar o penteado de um retrato mantendo a identidade da pessoa original.

Em vez de misturar diretamente duas imagens de pessoas, esta Skill usa um fluxo mais seguro em duas etapas:

1. Extrai o penteado e a cor do cabelo da imagem de referência em forma de texto.
2. Aplica esse texto a um fluxo JSON estruturado que edita apenas o cabelo da pessoa-alvo.

O objetivo é preservar rosto, expressão, maquiagem, roupa, fundo e identidade geral da imagem-alvo, transferindo somente o penteado e a cor do cabelo da imagem de referência.

## Recursos

- Usa `REFERENCE_0` como a única fonte de identidade.
- Usa `REFERENCE_1` apenas para analisar penteado e cor do cabelo.
- Evita transferir rosto, maquiagem, idade, expressão, estilo pessoal ou identidade da pessoa de referência.
- Gera uma única imagem final lado a lado:
  - esquerda: vista frontal com o novo penteado
  - direita: vista de costas mostrando o mesmo penteado
- Adiciona uma etiqueta em chinês tradicional no canto superior esquerdo.
- Inclui um template JSON reutilizável.
- Foi criada para exploração de penteados, prévias de salão e styling visual assistido por IA.

## Como Funciona

A Skill trata as duas imagens de entrada de formas diferentes:

| Referência | Finalidade |
| --- | --- |
| `REFERENCE_0` | Retrato-alvo. É a única fonte para rosto, identidade, expressão, textura da pele, maquiagem, roupa, composição e fundo. |
| `REFERENCE_1` | Referência de penteado. É usada apenas para analisar formato, comprimento, camadas, ondulação, textura e cor do cabelo. |

Na etapa final, a geração recebe um fluxo estruturado no qual `scope` e `hair_color` são preenchidos a partir da análise do penteado. Isso ajuda a impedir que a imagem de referência contamine a identidade facial da pessoa-alvo.

## Estrutura do Repositório

```text
.
├── SKILL.md
├── CHANGELOG.md
├── README.md
├── README.pt.md
├── README.zh-Hant.md
├── README.ja.md
└── templates/
    └── base-workflow.json
```

## Instalação

Instale a Skill em um dos seguintes locais.

Para uso global em vários projetos:

```text
~/.agents/skills/hairstyle-transfer/
```

Para uso local em um projeto:

```text
<project-root>/.agents/skills/hairstyle-transfer/
```

A pasta deve conter pelo menos:

```text
SKILL.md
templates/base-workflow.json
```

## Uso

Envie duas imagens:

1. Retrato da pessoa-alvo
2. Imagem de referência do penteado

Depois, peça ao agente para usar a Skill `hairstyle-transfer`, por exemplo:

```text
Use the hairstyle-transfer Skill to change the first person's hairstyle to match the second image.
```

A Skill primeiro analisa apenas o penteado e a cor do cabelo da segunda imagem. Em seguida, aplica esse resultado à primeira imagem por meio do fluxo JSON.

## Saída

A saída final deve ser uma única imagem:

- lado esquerdo: pessoa-alvo com o novo penteado, vista frontal
- lado direito: a mesma pessoa e o mesmo penteado, vista de costas
- etiqueta no canto superior esquerdo: nome ou breve descrição do penteado em chinês tradicional
- cor da etiqueta: `#721D24`

A Skill não deve gerar várias imagens separadas.

## Princípios de Design

Esta Skill prioriza a preservação da identidade acima da semelhança perfeita do penteado.

Se um penteado ocultar ou distorcer características faciais reconhecíveis da pessoa-alvo, o fluxo deve preservar primeiro a identidade da pessoa-alvo e reduzir a intensidade da mudança quando necessário.

A imagem de referência do penteado não deve ser usada como referência de rosto, maquiagem, idade, expressão ou identidade.

## Versão

Versão atual: `1.2.0`

Veja [CHANGELOG.md](CHANGELOG.md) para notas de versão.

## Licença

Ainda não há um arquivo de licença. Adicione um arquivo `LICENSE` antes de publicar se quiser definir termos de reutilização, redistribuição ou contribuição.
