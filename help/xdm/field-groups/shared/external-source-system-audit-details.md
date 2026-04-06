---
title: 外部Source システム監査の詳細
description: External Source System Audit Details Experience Data Model （XDM）フィールドグループについて説明します。
exl-id: 6aa154f3-620f-4a2e-9e33-a0757d0491c1
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 4%

---

# [!UICONTROL External Source System Audit Details] フィールドグループ

[!UICONTROL External Source System Audit Details]は、コアの「外部Source System Audit Attributes」データタイプを拡張し、そのプロパティを参照してコンテキストメタデータを追加する標準Experience Data Model （XDM）フィールドグループです。 これにより、監査の詳細な追跡と、外部ソースからの柔軟なデータ統合が可能になります。

![外部Source System Audit Details フィールド グループのスキーマ ダイアグラム。](../../images/field-groups/shared/external-source-system-audit-details.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| -------------------------------------------------| ---------------------------------------- | --------- | --- |
| [!UICONTROL External Source System Audit Details] | `external-source-system-audit-details` | [[!UICONTROL External Source System Audit Attributes]](../../data-types/external-source-system-audit-attributes.md) | &#39;[!UICONTROL External Source System Audit Details]&#39; フィールド グループは、プロパティを参照し、コンテキスト メタデータを追加することで、コア &#39;外部Source System Audit Attributes&#39; データ型を拡張します。 これにより、詳細な監査追跡と外部ソース向けの柔軟なデータ統合が容易になり、プロファイル取り込みの非同期性を考慮できます。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/shared/external-source-system-audit-details.schema.json)
