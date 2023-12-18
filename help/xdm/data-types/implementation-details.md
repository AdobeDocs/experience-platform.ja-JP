---
title: 実装の詳細データタイプ
description: 実装の詳細のエクスペリエンスデータモデル (XDM) データタイプについて説明します。
exl-id: d3d16bae-196b-489d-8590-fd22150eedf1
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '101'
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

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/implementationdetails.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/implementationdetails.schema.json)
