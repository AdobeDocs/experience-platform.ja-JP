---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；トランザクション；データ型；データ型；
title: トランザクションデータタイプ
description: このドキュメントでは、トランザクションエクスペリエンスデータモデル (XDM) のデータタイプの概要を説明します。
exl-id: 47b7152f-a853-44f0-8962-e902631ad8a4
source-git-commit: afbbdfff4346ab5240927f5703d3a06676776ea8
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 8%

---

# [!UICONTROL トランザクション] データタイプ

[!UICONTROL トランザクション] は、金融トランザクションの詳細を説明する標準の Experience Data Model(XDM) データ型です。

![トランザクション構造](../images/data-types/transaction.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `transactionAmount` | [[!UICONTROL 通貨]](./currency.md) | トランザクションの一環として交換される通貨の量を表します。 |
| `transactionDate` | [!UICONTROL 日時] | トランザクションが発生した日時のタイムスタンプ。 |
| `transactionId` | [!UICONTROL 文字列] | トランザクションの一意の ID。 |
| `transactionType` | [!UICONTROL 文字列] | 訪問者が使用するトランザクションのタイプ。 |

{style="table-layout:auto"}
