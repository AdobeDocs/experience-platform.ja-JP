---
title: 外部ソース・システム監査属性データ型
description: このドキュメントでは、外部ソースシステム監査属性エクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: ebdd8707-9675-4232-a5b7-4e4a481d706a
source-git-commit: 7e07ba8b5d7bc7df809a9a122d2a58837c933674
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 6%

---

# [!UICONTROL 外部ソースシステム監査属] 性データ型

>[!NOTE]
>
>このデータ型は、Real-time Customer Data Platformの B2B エディションにアクセスできる組織でのみ使用できます。

[!UICONTROL 外部ソースシステムの監査属] 性は、外部ソースシステムに関する監査の詳細を取り込む、標準のエクスペリエンスデータモデル (XDM) データ型です。

![](../images/data-types/external-source-system-audit-attributes.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `externalKey` | [[!UICONTROL B2B ソース]](./b2b-source.md) | 監査に使用するソースの複合識別子。 |
| `createdBy` | 文字列 | このレコードを作成したユーザーの名前。 |
| `createdDate` | DateTime | このレコードが作成された日付。 |
| `externalID` | 文字列 | ソースの外部の一意の識別子。 この値は、必要に応じての識別と重複排除に役立ちます。 |
| `lastActivityDate` | DateTime | ソース・システムの最後の活動日。 |
| `lastReferencedDate` | DateTime | ソース・システムの最後の参照日。 |
| `lastUpdatedBy` | 文字列 | このレコードを最後に更新した人の名前。 |
| `lastUpdatedDate` | DateTime | ソース・システムの最終更新日。 |
| `lastViewedDate` | DateTime | ソースシステムの最終表示日。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/auditing/external-source-system-audit.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/auditing/external-source-system-audit.schema.json)
