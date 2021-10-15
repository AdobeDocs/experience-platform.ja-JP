---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；トランザクション；データ型；データ型；
title: トランザクションデータタイプ
description: このドキュメントでは、トランザクションエクスペリエンスデータモデル (XDM) のデータタイプの概要を説明します。
exl-id: 47b7152f-a853-44f0-8962-e902631ad8a4
source-git-commit: afbbdfff4346ab5240927f5703d3a06676776ea8
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 11%

---

#  Transactiondata type

 トランザクションは、通貨トランザクションの詳細を説明する標準のエクスペリエンスデータモデル (XDM) データ型です。

![トランザクション構造](../images/data-types/transaction.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `transactionAmount` | [[!UICONTROL 通貨]](./currency.md) | トランザクションの一部として交換される通貨の金額を示します。 |
| `transactionDate` | [!UICONTROL DateTime] | トランザクションが発生した日時のタイムスタンプ。 |
| `transactionId` | [!UICONTROL 文字列] | トランザクションの一意の識別子。 |
| `transactionType` | [!UICONTROL 文字列] | 訪問者が使用するトランザクションのタイプ。 |

{style=&quot;table-layout:auto&quot;}
