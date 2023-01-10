---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；デバイス；データ型；データ型；データ型；通貨；
solution: Experience Platform
title: 通貨データタイプ
description: このドキュメントでは、通貨 XDM データタイプの概要を説明します。
exl-id: eaf4812e-32ec-4b07-82ef-60777f03623d
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 7%

---

# [!UICONTROL 通貨] データタイプ

[!UICONTROL 通貨] は、通貨タイプや換算日などの金額を表す標準的な XDM データ型です。

![](../images/data-types/currency.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `amount` | Double | 通貨の金額。 `currencyCode`. |
| `conversionDate` | 日時 | 通貨換算が行われた時点のタイムスタンプ。 |
| `currencyCode` | 文字列 | 通貨のタイプを示す ISO 4217 コード `amount` は、を表します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/currency.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/currency.schema.json)
