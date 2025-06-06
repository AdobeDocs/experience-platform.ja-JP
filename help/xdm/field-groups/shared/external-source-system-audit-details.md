---
title: 外部ソースシステム監査の詳細
description: 外部Source システム監査詳細エクスペリエンスデータモデル（XDM）フィールドグループについて説明します。
source-git-commit: 656070cf69e3713c7889f53d51937e0e70085d96
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 6%

---

# [!UICONTROL &#x200B; 外部Source システム監査の詳細 &#x200B;] フィールドグループ

[!UICONTROL &#x200B; 外部Source システム監査詳細 &#x200B;] は、標準のエクスペリエンスデータモデル（XDM）フィールドグループで、プロパティを参照し、コンテキストメタデータを追加することで、コアの「外部Source システム監査属性」データタイプを拡張します。 これにより、詳細な監査トラッキングと、外部ソースからの柔軟なデータ統合が可能になります。

![ 外部Source システム監査詳細フィールドグループのスキーマ図。](../../images/field-groups/shared/external-source-system-audit-details.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| -------------------------------------------------| ---------------------------------------- | --------- | --- |
| [!UICONTROL &#x200B; 外部Source システム監査の詳細 &#x200B;] | `external-source-system-audit-details` | [[!UICONTROL &#x200B; 外部Source システム監査属性 &#x200B;]](../../data-types/external-source-system-audit-attributes.md) | 「[!UICONTROL External Source System Audit Details]」フィールドグループは、プロパティを参照し、コンテキストメタデータを追加することで、コアの「External Source System Audit Attributes」データタイプを拡張します。 これにより、詳細な監査トラッキングと、外部ソースの柔軟なデータ統合が容易になり、プロファイル取り込みの非同期特性に対応できます。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/shared/external-source-system-audit-details.schema.json)