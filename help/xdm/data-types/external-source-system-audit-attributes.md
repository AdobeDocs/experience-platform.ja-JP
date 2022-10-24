---
title: 外部ソースシステム監査属性データタイプ
description: このドキュメントでは、外部ソースシステム監査属性エクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: ebdd8707-9675-4232-a5b7-4e4a481d706a
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 8%

---

# [!UICONTROL 外部ソースシステム監査属性] データタイプ

>[!NOTE]
>
>このデータタイプは、Adobe Real-time Customer Data Platformの B2B エディションにアクセスできる組織でのみ使用できます。

[!UICONTROL 外部ソースシステム監査属性] は、外部ソースシステムに関する監査の詳細を取り込む、標準の Experience Data Model(XDM) データ型です。

![](../images/data-types/external-source-system-audit-attributes.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `externalKey` | [[!UICONTROL B2B ソース]](./b2b-source.md) | 監査に使用するソースの複合識別子。 |
| `createdBy` | 文字列 | このレコードを作成したユーザーの名前。 |
| `createdDate` | 日時 | このレコードが作成された日付。 |
| `externalID` | 文字列 | ソースの外部の一意の ID。 この値は、必要に応じての識別と重複排除に使用されます。 |
| `lastActivityDate` | 日時 | ソースシステムの最後のアクティビティの日付。 |
| `lastReferencedDate` | 日時 | ソースシステムの最終参照日。 |
| `lastUpdatedBy` | 文字列 | このレコードを最後に更新した人の名前。 |
| `lastUpdatedDate` | 日時 | ソースシステムの最終更新日。 |
| `lastViewedDate` | 日時 | ソースシステムの最終表示日です。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/auditing/external-source-system-audit.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/auditing/external-source-system-audit.schema.json)
