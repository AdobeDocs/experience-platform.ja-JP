---
title: エラーの詳細情報のデータタイプ
description: エラーの詳細情報エクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: 65f3dcf1cacfbc4e8a598244810d238bd88f64bd
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 5%

---

# [!UICONTROL エラーの詳細情報] データタイプ

[!UICONTROL エラーの詳細情報] は、エラーの詳細を説明する標準の Experience Data Model(XDM) データ型です。 以下を使用します。 [!UICONTROL エラーの詳細情報] エラーソースと ID の詳細を取り込むためのデータタイプ。 エラー ID はエラーを識別し、エラーソースは、エラーの送信元がプレーヤーか外部ソースかを指定します。

![Error Details Information データ型の図です。](../images/data-types/error-details-information.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------|----------------|-----------|----------------------------------------------|
| [!UICONTROL エラー ID] | `name` | 文字列 | エラー ID。 |
| [!UICONTROL エラーソース] | `source` | 文字列 | エラーソース。 列挙：「プレーヤー」、「外部」、それぞれの意味を持つ。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/datatypes/errordetails.schema.json)
