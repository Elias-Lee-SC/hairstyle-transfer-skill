# Hairstyle Transfer Skill

[English](README.md) | [Português](README.pt.md) | 中文 | [日本語](README.ja.md)

`hairstyle-transfer` 是一個相容於 Codex / Cortex 的人像換髮型 Skill，用於替換人像照片中的髮型，同時盡量保持目標人物的身份一致性。

它不是把兩張人像直接混合，而是採用更穩定的兩步流程：

1. 先把髮型範例圖中的髮型與髮色提取成文字描述。
2. 再把文字結果套入結構化 JSON 工作流，只針對目標人物的頭髮區域進行編輯。

這個設計的目標，是在轉換髮型與髮色時，保留目標照片中的臉部、表情、妝容、服裝、背景與整體身份辨識度。

## 功能特色

- 以 `REFERENCE_0` 作為唯一人物身份來源。
- `REFERENCE_1` 只用於分析髮型與髮色。
- 避免把範例圖人物的臉、妝容、年齡感、氣質或身份轉移到目標人物。
- 最終只輸出一張左右拼接圖：
  - 左側：換髮後的正面效果
  - 右側：同款髮型的背視效果
- 左上角加入繁體中文髮型名稱或簡短描述。
- 內含可重複使用的 JSON 工作流模板。
- 適合用於髮型探索、沙龍預覽與 AI 輔助造型視覺化。

## 工作原理

本 Skill 會把兩張輸入圖片分成不同角色：

| 參考圖 | 用途 |
| --- | --- |
| `REFERENCE_0` | 目標人物圖像。這是臉部、身份、表情、皮膚質感、妝容、服裝、構圖與背景的唯一來源。 |
| `REFERENCE_1` | 髮型範例圖。只用於分析髮型形狀、長度、層次、捲度、質感與髮色。 |

最終生成時，系統會把髮型分析結果填入 `scope` 與 `hair_color`，再交給結構化 JSON 工作流處理。這可以降低第二張圖的人臉特徵污染第一張圖人物身份的風險。

## 專案結構

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

## 安裝方式

可以將 Skill 安裝到以下任一位置。

跨專案全域使用：

```text
~/.agents/skills/hairstyle-transfer/
```

只在單一專案中使用：

```text
<project-root>/.agents/skills/hairstyle-transfer/
```

資料夾至少需要包含：

```text
SKILL.md
templates/base-workflow.json
```

## 使用方式

上傳兩張圖片：

1. 目標人物頭像
2. 髮型範例圖

接著要求代理使用 `hairstyle-transfer` Skill，例如：

```text
Use the hairstyle-transfer Skill to change the first person's hairstyle to match the second image.
```

Skill 會先只分析第二張圖的髮型與髮色，再透過 JSON 工作流把結果套用到第一張圖。

## 輸出結果

最終輸出應該只有一張圖片：

- 左側：目標人物換髮後的正面效果
- 右側：同一人物與同款髮型的背視效果
- 左上角：繁體中文髮型名稱或簡短描述
- 文字顏色：`#721D24`

本 Skill 不應輸出多張分離圖片。

## 設計原則

本 Skill 優先保留人物身份，其次才追求髮型相似度。

如果某個髮型會遮擋或改變目標人物可辨識的臉部特徵，工作流應優先保留 `REFERENCE_0` 的人物身份，必要時降低髮型相似度或減弱臉周遮擋。

髮型範例圖不得作為臉部、妝容、年齡感、表情或人物身份參考。

## 版本

目前版本：`1.2.0`

更新紀錄請見 [CHANGELOG.md](CHANGELOG.md)。

## 授權

目前尚未加入授權檔案。若要在 GitHub 上開放他人重用、散布或貢獻，建議發布前加入 `LICENSE` 檔案。
