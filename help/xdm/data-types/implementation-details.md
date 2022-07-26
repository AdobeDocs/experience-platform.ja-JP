---
title: 実装の詳細データタイプ
description: このドキュメントでは、実装の詳細なエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
source-git-commit: 77fb3e348c2298fc5c325fcf2d3408da084b2b19
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 7%

---

# [!UICONTROL 実装の詳細] データタイプ

[!UICONTROL 実装の詳細] は、API や SDK などのテクノロジー実装を記述する標準の Experience Data Model(XDM) データ型です。

![データタイプの構造](../images/data-types/implementation-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `environment` | 文字列 | 実装の環境。 |
| `name` | 文字列 | SDK またはエンドポイントの識別子。 すべての SDK またはエンドポイントは、拡張機能を含め、URI を通じて識別されます。 |
| `version` | 文字列 | API または SDK のバージョン。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/implementationdetails.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/implementationdetails.schema.json)
