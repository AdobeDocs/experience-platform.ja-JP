---
title: プロバイダークラス
description: エクスペリエンスデータモデル（XDM）の Provider クラスについて説明します。
exl-id: acb9b8a3-f911-49c5-9d2a-3a0d6aeebef9
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 4%

---

# [!UICONTROL Provider] クラス

エクスペリエンスデータモデル（XDM）では、[!UICONTROL &#x200B; プロバイダー &#x200B;] クラスは、プロバイダービジネスエンティティ（ヘルスケアプロバイダー、保険会社など）を定義するプロパティの最小セットを取得します。

![ クラス構造 ](../images/classes/provider.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `providerName` | [[!UICONTROL &#x200B; 人物名 &#x200B;]](../data-types/person-name.md) | プロバイダーの名前。 |
| `_id` | [!UICONTROL 文字列] | レコードに対してシステムで生成された一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのレコードを検索するために使用されます。<br><br> このフィールドはシステムで生成されるので、データ取り込み時に明示的な値は指定されません。 ただし、必要に応じて、独自の一意の ID 値を指定することもできます。 |
| `providerId` | [!UICONTROL 文字列] | プロバイダーの一意の ID。 |

{style="table-layout:auto"}

このクラスを [[!UICONTROL Healthcare Provider] フィールドグループ ](../field-groups/provider/healthcare-provider.md) で拡張すると、ヘルスケア提供組織に関する詳細を記述できます。
