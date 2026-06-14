# Hairstyle Transfer Skill

English | [Português](README.pt.md) | [中文](README.zh-Hant.md) | [日本語](README.ja.md)

`hairstyle-transfer` is an AI agent Skill for changing a portrait's hairstyle while preserving the target person's identity.

Instead of directly blending two portrait images, this Skill uses a safer two-step workflow:

1. Extract the hairstyle and hair color from the reference hairstyle image as text.
2. Apply that text to a structured JSON workflow that edits only the target person's hair.

The goal is to preserve the face, expression, makeup, clothing, background, and overall identity from the target image while transferring only the hairstyle and hair color from the reference image.

## Features

- Uses `REFERENCE_0` as the only identity source.
- Uses `REFERENCE_1` only for hairstyle and hair color analysis.
- Avoids transferring the reference person's face, makeup, age, mood, or identity.
- Produces one final side-by-side image:
  - left: front view with the new hairstyle
  - right: back view showing the same hairstyle
- Adds a Traditional Chinese hairstyle label in the top-left corner.
- Includes a reusable JSON workflow template.
- Designed for portrait hairstyle exploration, salon previews, and AI-assisted visual styling.

## How It Works

The Skill treats the two input images differently:

| Reference | Purpose |
| --- | --- |
| `REFERENCE_0` | Target portrait. This is the only source for face, identity, expression, skin texture, makeup, clothing, composition, and background. |
| `REFERENCE_1` | Hairstyle reference. This is used only to analyze hairstyle shape, length, layers, curl pattern, texture, and hair color. |

The final generation step receives a structured workflow where `scope` and `hair_color` are filled from the hairstyle analysis. This helps prevent the reference image from contaminating the target person's facial identity.

## Repository Structure

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

## Installation

Install the Skill in one of the following locations.

For global use across projects:

```text
~/.agents/skills/hairstyle-transfer/
```

For project-local use:

```text
<project-root>/.agents/skills/hairstyle-transfer/
```

The folder should contain at least:

```text
SKILL.md
templates/base-workflow.json
```

## Usage

Upload two images:

1. Target portrait image
2. Hairstyle reference image

Then ask the agent to use the `hairstyle-transfer` Skill, for example:

```text
Use the hairstyle-transfer Skill to change the first person's hairstyle to match the second image.
```

The Skill will first analyze only the hairstyle and hair color from the second image, then apply that result to the first image through the JSON workflow.

## Output

The final output should be a single image:

- left side: the target person with the new hairstyle, front view
- right side: the same person and hairstyle, back view
- top-left label: Traditional Chinese hairstyle name or short description
- label color: `#721D24`

The Skill should not output multiple separate images.

## Design Principles

This Skill prioritizes identity preservation over perfect hairstyle similarity.

If a hairstyle would hide or distort the target person's recognizable facial features, the workflow should preserve the target identity first and reduce hairstyle aggressiveness if needed.

The reference hairstyle image must not be used as a face, makeup, age, expression, or identity reference.

## Version

Current version: `1.2.0`

See [CHANGELOG.md](CHANGELOG.md) for release notes.

## License

No license file has been added yet. Add a `LICENSE` file before publishing if you want to define reuse, redistribution, or contribution terms.
