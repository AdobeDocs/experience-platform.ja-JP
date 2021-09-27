---
title: XDM Business Opportunityクラス
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)のXDM Business Opportunityクラスの概要を説明します。
source-git-commit: 5fd82b02eb25f3d575de695c2f2b14a5e5b18400
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 5%

---

# [!UICONTROL XDM Business ] Opportunityclass

>[!NOTE]
>
>このクラスは、リアルタイム顧客データプラットフォームB2Bエディションにアクセスできる組織でのみ使用できます。

[!UICONTROL XDM Business Opportunity] は、ビジネスオポチュニティに最低限必要なプロパティを取り込む、標準のExperience Data Model(XDM)クラスです。

![](../../images/classes/b2b/business-opportunity.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | このオポチュニティに関連付けられているアカウントの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | オポチュニティが外部ソース・システムから取得された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `opportunityKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 営業案件エンティティの複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`opportunityID`とは別のシステム生成値です。 |
| `accountID` | 文字列 | このオポチュニティが関連付けられているアカウントの一意のID。 |
| `opportunityDescription` | 文字列 | 営業案件の説明。 |
| `opportunityID` | 文字列 | 営業案件エンティティの一意のIDです。 |
| `opportunityName` | 文字列 | オポチュニティの名前。 |
| `opportunityStage` | 文字列 | オポチュニティの販売段階。 |
| `opportunityType` | 文字列 | オポチュニティのタイプ。 |

このクラスと他のB2Bクラスとの概念的な関係と、Adobe Experience Platform UIでこれらの関係を確立する方法については、 Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md)の[スキーマの関係に関するガイドを参照してください。
