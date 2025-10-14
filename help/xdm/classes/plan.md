---
title: プラン クラス
description: エクスペリエンスデータモデル（XDM）のプランクラスについて説明します。
exl-id: ccff962d-3104-482c-8d65-d2bd2602a9be
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 4%

---

# [!UICONTROL &#x200B; プラン &#x200B;] クラス

エクスペリエンスデータモデル（XDM）の [!UICONTROL &#x200B; プラン &#x200B;] クラスは、医療プランや保険プランなど、プランを定義するプロパティの最小セットを取得します。

![&#x200B; クラス構造 &#x200B;](../images/classes/plan.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_id` | [!UICONTROL 文字列] | レコードに対してシステムで生成された一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのレコードを検索するために使用されます。<br><br> このフィールドはシステムで生成されるので、データ取り込み時に明示的な値は指定されません。 ただし、必要に応じて、独自の一意の ID 値を指定することもできます。 |
| `planId` | [!UICONTROL 文字列] | プランの一意の ID。 |
| `planName` | [!UICONTROL 文字列] | プランの名前。 |

{style="table-layout:auto"}

クラスを [[!UICONTROL &#x200B; ヘルスケアプランの詳細 &#x200B;] フィールドグループ &#x200B;](../field-groups/plan/healthcare-plan-details.md) で拡張して、医療保険プランの詳細を説明できます。
