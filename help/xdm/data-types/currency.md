---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；デバイス；データ型；データ型；データ型；通貨；
solution: Experience Platform
title: 通貨データタイプ
description: 通貨 XDM データタイプについて説明します。
exl-id: eaf4812e-32ec-4b07-82ef-60777f03623d
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 6%

---

# [!UICONTROL 通貨] データタイプ

[!UICONTROL 通貨] は、通貨タイプや換算日などの金額を表す標準的な XDM データ型です。

![](../images/data-types/currency.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `amount` | Double | 通貨の金額。 `currencyCode`. |
| `conversionDate` | 日時 | 通貨換算が行われた時点を示すタイムスタンプ。 |
| `currencyCode` | 文字列 | 通貨のタイプを示す ISO 4217 コード `amount` は、を表します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/currency.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/currency.schema.json)
