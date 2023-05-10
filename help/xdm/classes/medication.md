---
title: 薬のクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の Medigure クラスの概要を説明します。
exl-id: e5786241-dd6e-450f-98c8-2de46affb3e2
source-git-commit: 2fd35c4ac29f43391f9dc03c636d20558b701be7
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 4%

---

# [!UICONTROL 薬] クラス

エクスペリエンスデータモデル (XDM) では、 [!UICONTROL 薬] クラスは、医療、特に医薬や薬物に使用される物質を定義する最小の特性セットをキャプチャします。

![クラス構造](../images/classes/medication.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_id` | [!UICONTROL 文字列] | レコードの、システムで生成される一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのレコードを検索するために使用します。<br><br>このフィールドはシステムで生成されるので、データの取り込み中に明示的な値は指定されません。 ただし、必要に応じて独自の一意の ID 値を指定することもできます。 |
| `medicationId` | [!UICONTROL 文字列] | 薬の一意の ID。 |
| `medicationName` | [!UICONTROL 文字列] | 薬の名前。 |

{style="table-layout:auto"}

クラスは、 [[!UICONTROL 医療薬] フィールドグループ](../field-groups/medication/healthcare-medication.md) 薬物や薬物に関する詳細を説明する。
