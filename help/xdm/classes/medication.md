---
title: 医薬品クラス
description: エクスペリエンスデータモデル（XDM）の医薬品クラスについて説明します。
exl-id: e5786241-dd6e-450f-98c8-2de46affb3e2
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 4%

---

# [!UICONTROL  医薬品 ] クラス

エクスペリエンスデータモデル（XDM）の [!UICONTROL  投薬 ] クラスは、医療（特に薬や薬）に使用される物質を定義するプロパティの最小セットをキャプチャします。

![ クラス構造 ](../images/classes/medication.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_id` | [!UICONTROL 文字列] | レコードに対してシステムで生成された一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのレコードを検索するために使用されます。<br><br> このフィールドはシステムで生成されるので、データ取り込み時に明示的な値は指定されません。 ただし、必要に応じて、独自の一意の ID 値を指定することもできます。 |
| `medicationId` | [!UICONTROL 文字列] | 薬の一意の ID。 |
| `medicationName` | [!UICONTROL 文字列] | 薬の名前。 |

{style="table-layout:auto"}

クラスは、[[!UICONTROL  ヘルスケア医薬品 ] フィールドグループ ](../field-groups/medication/healthcare-medication.md) で拡張して、薬や薬に関する詳細を説明することができます。
