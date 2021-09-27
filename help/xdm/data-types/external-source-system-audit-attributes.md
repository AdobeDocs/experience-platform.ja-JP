---
title: 外部ソース・システム監査属性データ型
description: このドキュメントでは、外部ソースシステム監査属性エクスペリエンスデータモデル(XDM)データタイプの概要を説明します。
source-git-commit: 8bf0c63f33ae9cbfb01d4db9e5220c6762575c5b
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 6%

---

# [!UICONTROL 外部ソースシステムの監査属性] データ型

>[!NOTE]
>
>このデータタイプは、リアルタイム顧客データプラットフォームのB2Bエディションにアクセスできる組織でのみ使用できます。

[!UICONTROL 外部ソースシステムの監査属] 性は、外部ソースシステムに関する監査の詳細を取り込む、標準のエクスペリエンスデータモデル(XDM)データ型です。

![](../images/data-types/external-source-system-audit-attributes.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `externalKey` | [[!UICONTROL B2Bソース]](./b2b-source.md) | 監査に使用するソースの複合識別子。 |
| `createdBy` | 文字列 | このレコードを作成したユーザーの名前。 |
| `createdDate` | DateTime | このレコードが作成された日付。 |
| `externalID` | 文字列 | ソースの外部の一意の識別子。 この値は、必要に応じて、の識別と重複排除に使用されます。 |
| `lastActivityDate` | DateTime | ソース・システムの最後の活動日。 |
| `lastReferencedDate` | DateTime | ソース・システムの最後の参照日。 |
| `lastUpdatedBy` | 文字列 | このレコードを最後に更新した人の名前。 |
| `lastUpdatedDate` | DateTime | ソース・システムの最終更新日。 |
| `lastViewedDate` | DateTime | ソース・システムの最終表示日。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/auditing/external-source-system-audit.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/auditing/external-source-system-audit.schema.json)
