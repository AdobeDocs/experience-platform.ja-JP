---
title: buildInfo
description: サイトに実装されたタグビルドに関する情報を取得します。
source-git-commit: 434d6913ea391b127b4b52c8494730c496bbcfe2
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 2%

---

# `buildInfo`

`_satellite.buildInfo` オブジェクトには、実装されたタグプロパティのビルドに関する情報が含まれています。 このオブジェクトは、頻繁なビルドをデバッグして最新バージョンを使用していることを確認する場合に最も役立ちます。

```ts
readonly _satellite.buildInfo: BuildInfo
```

## 使用可能なフィールド

このオブジェクトを呼び出す際には、次のフィールドを使用できます。

```json
{
  "minified": true,
  "buildDate": "YYYY-06-13T01:22:12Z",
  "turbineBuildDate": "YYYY-08-22T17:32:44Z",
  "turbineVersion": "28.0.0"
}
```

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| **`minified`** | `boolean` | ライブラリが縮小されているかどうかを示します。 実稼動ビルドは通常、縮小（`true`）されますが、開発ビルドとステージングビルドは通常、縮小（`false`）されません。 |
| **`buildDate`** | `string (ISO-8601 datetime)` | JavaScript ファイルが作成および公開された日時。 |
| **`turbineBuildDate`** | `string (ISO-8601 datetime)` | [Turbine](https://github.com/adobe/reactor-turbine) は、タグルールを処理し、ロジックをタグ拡張機能に委任するAdobeのエンジンです。 このフィールドには、タグプロパティの公開に使用する Turbine ビルドの日時が格納されます。 |
| **`turbineVersion`** | `string` | タグプロパティの作成と公開に使用される Turbine のバージョン。 |

同様の情報も `_satellite._container.buildInfo` に含まれています。 詳細は、[`_container`](container.md) を参照してください。
