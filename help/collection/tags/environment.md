---
title: environment
description: タグプロパティが現在使用しているビルド環境。
source-git-commit: 434d6913ea391b127b4b52c8494730c496bbcfe2
workflow-type: tm+mt
source-wordcount: '61'
ht-degree: 6%

---

# `environment`

`_satellite.environment` オブジェクトは、タグプロパティが現在使用しているビルド環境を示します。

```js
readonly _satellite.environment: Environment
```

## 使用可能なフィールド

このオブジェクトを呼び出す際には、次のフィールドを使用できます。

```json
{
  "id": "EN6b2...d6ff2",
  "stage": "production"
}
```

| 名前 | タイプ | 説明 |
|---|---|---|
| **`id`** | `string` | 環境の一意の ID。 環境 ID は、タグ UI の **[!UICONTROL Install]** の下にある [[!UICONTROL Environments]](/help/tags/ui/publishing/environments.md) アイコンを選択すると見つかります。 |
| **`stage`** | `development \| staging \| production` | 環境タイプ。 |
