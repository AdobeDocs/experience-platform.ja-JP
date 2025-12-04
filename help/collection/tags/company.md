---
title: 会社
description: 実装されたタグプロパティを所有する IMS 組織に関する情報を取得します。
source-git-commit: 434d6913ea391b127b4b52c8494730c496bbcfe2
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 3%

---

# `company`

`_satellite.company` オブジェクトは、タグプロパティを所有する IMS 組織に関する情報を表示します。

```ts
readonly _satellite.company: Company
```

## 使用可能なフィールド

このオブジェクトを呼び出す際には、次のフィールドを使用できます。

```json
{
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "dynamicCdnEnabled": true,
  "cdnAllowList": [
    "assets.adoberesources.cn",
    "assets.adobedtm.com"
  ]
}
```

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| **`orgId`** | `string` | タグプロパティの IMS 組織 ID |
| **`dynamicCdnEnabled`** | `boolean` | タグプロパティでAdobeの動的 CDN 切り替え機能を使用しているかどうかを判断します。 `true` に設定すると、訪問者の場所に基づいて、訪問者がタグをリクエストする CDN が自動的に切り替わります。 |
| **`cdnAllowList`** | `string[]` | タグプロパティの読み込み元となる CDN の許可。 |

同様の情報も `_satellite._container.company` に含まれています。 詳細は、[`_container`](container.md) を参照してください。
