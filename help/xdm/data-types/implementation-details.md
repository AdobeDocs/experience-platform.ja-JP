---
title: 実装の詳細データタイプ
description: 実装の詳細 Experience Data Model （XDM）データタイプについて説明します。
exl-id: d3d16bae-196b-489d-8590-fd22150eedf1
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 7%

---

# [!UICONTROL &#x200B; 実装の詳細 &#x200B;] データタイプ

[!UICONTROL &#x200B; 実装の詳細 &#x200B;] は、API や SDK などのテクノロジー実装を記述する標準の Experience Data Model （XDM）データタイプです。

![&#x200B; データタイプ構造 &#x200B;](../images/data-types/implementation-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `environment` | 文字列 | 実装の環境。 |
| `name` | 文字列 | SDK またはエンドポイントの識別子。 拡張機能を含め、すべての SDK またはエンドポイントは URI で識別されます。 |
| `version` | 文字列 | API または SDK のバージョン。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/implementationdetails.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/implementationdetails.schema.json)
