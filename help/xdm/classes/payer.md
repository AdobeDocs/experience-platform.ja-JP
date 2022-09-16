---
title: 支払者クラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の Payer クラスの概要を説明します。
exl-id: 8d3e0a6d-41eb-4ffe-81dd-c7b7d532a531
source-git-commit: 2fd35c4ac29f43391f9dc03c636d20558b701be7
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 7%

---

# [!UICONTROL 支払者] クラス

エクスペリエンスデータモデル (XDM) では、 [!UICONTROL 支払者] クラスは、保険会社（健康保険など）に関するデータを収集する支払者ビジネスエンティティを定義するプロパティの最小セットをキャプチャします。

![クラス構造](../images/classes/payer.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_id` | [!UICONTROL 文字列] | レコードの、システムで生成される一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのレコードを検索するために使用します。<br><br>このフィールドはシステムで生成されるので、データの取り込み中に明示的な値は指定されません。 ただし、必要に応じて独自の一意の ID 値を指定することもできます。 |
| `payerId` | [!UICONTROL 文字列] | 支払者の一意の識別子。 |
| `payerName` | [!UICONTROL 文字列] | 支払者の名前。 |

{style=&quot;table-layout:auto&quot;}
