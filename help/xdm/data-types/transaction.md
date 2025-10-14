---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；トランザクション；データタイプ；データタイプ；データタイプ；
title: トランザクションデータタイプ
description: トランザクションエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 47b7152f-a853-44f0-8962-e902631ad8a4
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 7%

---

# [!UICONTROL &#x200B; トランザクション &#x200B;] データタイプ

[!UICONTROL &#x200B; トランザクション &#x200B;] は、金融取引の詳細を記述する標準の Experience Data Model （XDM）データタイプです。

![&#x200B; トランザクションの構造 &#x200B;](../images/data-types/transaction.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `transactionAmount` | [[!UICONTROL 通貨]](./currency.md) | トランザクションの一部として交換される通貨の金額を表します。 |
| `transactionDate` | [!UICONTROL &#x200B; 日時 &#x200B;] | トランザクションが発生したときのタイムスタンプ。 |
| `transactionId` | [!UICONTROL 文字列] | トランザクションの一意の ID。 |
| `transactionType` | [!UICONTROL 文字列] | 訪問者が使用するトランザクションのタイプ。 |

{style="table-layout:auto"}
