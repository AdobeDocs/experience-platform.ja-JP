---
title: 支払者クラス
description: エクスペリエンスデータモデル（XDM）の Payer クラスについて説明します。
exl-id: 8d3e0a6d-41eb-4ffe-81dd-c7b7d532a531
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 5%

---

# [!UICONTROL  支払者 ] クラス

エクスペリエンスデータモデル（XDM）では、[!UICONTROL  支払者 ] クラスは、保険会社（健康保険など）に関連するデータを収集する支払者ビジネスエンティティを定義するプロパティの最小セットを取得します。

![ クラス構造 ](../images/classes/payer.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_id` | [!UICONTROL 文字列] | レコードに対してシステムで生成された一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのレコードを検索するために使用されます。<br><br> このフィールドはシステムで生成されるので、データ取り込み時に明示的な値は指定されません。 ただし、必要に応じて、独自の一意の ID 値を指定することもできます。 |
| `payerId` | [!UICONTROL 文字列] | 支払人の一意の ID。 |
| `payerName` | [!UICONTROL 文字列] | 支払人の名前。 |

{style="table-layout:auto"}
