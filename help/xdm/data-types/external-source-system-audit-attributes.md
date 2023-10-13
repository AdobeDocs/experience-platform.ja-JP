---
title: 外部ソースシステム監査属性データタイプ
description: このドキュメントでは、外部ソースシステム監査属性エクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: ebdd8707-9675-4232-a5b7-4e4a481d706a
source-git-commit: a7e6ebfe09566e6e027b13efc95dda97ff8f0315
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 7%

---

# [!UICONTROL 外部ソースシステム監査属性] データタイプ

[!UICONTROL 外部ソースシステム監査属性] は、外部ソースシステムに関する監査の詳細を取り込む、標準の Experience Data Model(XDM) データ型です。

![](../images/data-types/external-source-system-audit-attributes.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `externalKey` | [[!UICONTROL B2B ソース]](./b2b-source.md) | 監査に使用するソースの複合識別子。 |
| `createdBy` | 文字列 | このレコードを作成したユーザーの名前。 |
| `createdDate` | 日時 | このレコードが作成された日付。 |
| `externalID` | 文字列 | ソースの外部の一意の ID。 この値は、必要に応じての識別と重複排除に使用されます。 |
| `lastActivityDate` | 日時 | ソースシステムの最後のアクティビティの日付。 |
| `lastReferencedDate` | 日時 | ソースシステムの最後の参照日。 |
| `lastUpdatedBy` | 文字列 | このレコードを最後に更新した人の名前。 |
| `lastUpdatedDate` | 日時 | ソースシステムの最終更新日。 |
| `lastViewedDate` | 日時 | ソースシステムの最終表示日です。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/auditing/external-source-system-audit.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/auditing/external-source-system-audit.schema.json)
