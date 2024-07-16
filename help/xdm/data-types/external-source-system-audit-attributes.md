---
title: 外部Source システム監査属性データタイプ
description: 外部Source システム監査属性エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: ebdd8707-9675-4232-a5b7-4e4a481d706a
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 7%

---

# [!UICONTROL  外部Source システム監査属性 ] データタイプ

[!UICONTROL  外部Source システム監査属性 ] は、外部ソースシステムに関する監査詳細を取得する標準の Experience Data Model （XDM）データタイプです。

![](../images/data-types/external-source-system-audit-attributes.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `externalKey` | [[!UICONTROL B2B Source]](./b2b-source.md) | 監査に使用するソースの複合識別子。 |
| `createdBy` | 文字列 | このレコードを作成したユーザーの名前。 |
| `createdDate` | 日時 | このレコードが作成された日付。 |
| `externalID` | 文字列 | ソースの外部一意 ID。 この値は、必要に応じて識別および重複排除するために使用されます。 |
| `lastActivityDate` | 日時 | ソースシステムの最後のアクティビティの日付。 |
| `lastReferencedDate` | 日時 | ソースシステムの最終参照日。 |
| `lastUpdatedBy` | 文字列 | このレコードを最後に更新した人物の名前。 |
| `lastUpdatedDate` | 日時 | ソースシステムの最終更新日。 |
| `lastViewedDate` | 日時 | ソースシステムの最終表示日。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/auditing/external-source-system-audit.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/auditing/external-source-system-audit.schema.json)
