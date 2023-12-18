---
title: プランクラス
description: Experience Data Model(XDM) の Plan クラスについて説明します。
exl-id: ccff962d-3104-482c-8d65-d2bd2602a9be
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 4%

---

# [!UICONTROL プラン] クラス

エクスペリエンスデータモデル (XDM) では、 [!UICONTROL プラン] クラスは、健康プランや保険プランなど、プランを定義する最小のプロパティセットをキャプチャします。

![クラス構造](../images/classes/plan.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_id` | [!UICONTROL 文字列] | レコードの、システムで生成される一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのレコードを検索するために使用します。<br><br>このフィールドはシステムで生成されるので、データの取り込み中に明示的な値は指定されません。 ただし、必要に応じて独自の一意の ID 値を指定することもできます。 |
| `planId` | [!UICONTROL 文字列] | プランの一意の ID。 |
| `planName` | [!UICONTROL 文字列] | プランの名前。 |

{style="table-layout:auto"}

クラスは、 [[!UICONTROL ヘルスケアプランの詳細] フィールドグループ](../field-groups/plan/healthcare-plan-details.md) 健康保険計画の詳細を説明する。
