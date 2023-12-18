---
title: プロバイダークラス
description: Experience Data Model(XDM) の Provider クラスについて説明します。
exl-id: acb9b8a3-f911-49c5-9d2a-3a0d6aeebef9
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 4%

---

# [!UICONTROL プロバイダー] クラス

エクスペリエンスデータモデル (XDM) では、 [!UICONTROL プロバイダー] クラスは、プロバイダのビジネスエンティティ（医療プロバイダや保険プロバイダなど）を定義する最小のプロパティセットをキャプチャします。

![クラス構造](../images/classes/provider.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `providerName` | [[!UICONTROL 人物名]](../data-types/person-name.md) | プロバイダーの名前。 |
| `_id` | [!UICONTROL 文字列] | レコードの、システムで生成される一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのレコードを検索するために使用します。<br><br>このフィールドはシステムで生成されるので、データの取り込み中に明示的な値は指定されません。 ただし、必要に応じて独自の一意の ID 値を指定することもできます。 |
| `providerId` | [!UICONTROL 文字列] | プロバイダーの一意の ID。 |

{style="table-layout:auto"}

クラスは、 [[!UICONTROL 医療機関] フィールドグループ](../field-groups/provider/healthcare-provider.md) 医療機関の詳細を説明する。
