---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；デバイス；データ型；データ型；データ型；通貨；
solution: Experience Platform
title: 通貨データタイプ
topic-legacy: overview
description: このドキュメントでは、通貨 XDM データタイプの概要を説明します。
exl-id: eaf4812e-32ec-4b07-82ef-60777f03623d
source-git-commit: 5e92b288bb8c996cfcf343d8ac1ab1665b0d3ad0
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 7%

---

#  通貨データ型

 通貨は、通貨タイプや換算日など、通貨の金額を示す標準 XDM データ型です。

![](../images/data-types/currency.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `amount` | Double | `currencyCode` で定義される通貨の量。 |
| `conversionDate` | DateTime | 通貨換算が行われた日時のタイムスタンプ。 |
| `currencyCode` | 文字列 | `amount` が表す通貨のタイプを示す ISO 4217 コード。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/currency.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/currency.schema.json)
