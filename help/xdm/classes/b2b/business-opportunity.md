---
title: XDM ビジネスオポチュニティクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM Business Opportunity クラスの概要を説明します。
exl-id: d816b0f9-fd37-45da-aa55-247f7f662da0
source-git-commit: edf7afc5db219430232a3226dc691570b50a32bd
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 7%

---

# [!UICONTROL XDM ビジネスオポチュニティ] クラス

[!UICONTROL XDM ビジネスオポチュニティ] は、ビジネスオポチュニティに最低限必要なプロパティを取り込む、標準 Experience Data Model(XDM) クラスです。

![](../../images/classes/b2b/business-opportunity.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | このオポチュニティが関連付けられているアカウントの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | オポチュニティが外部のソースシステムから来た場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `opportunityKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | オポチュニティエンティティの複合識別子。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `opportunityID`. |
| `accountID` | 文字列 | このオポチュニティが関連付けられているアカウントの一意の ID。 |
| `opportunityDescription` | 文字列 | オポチュニティの説明。 |
| `opportunityID` | 文字列 | オポチュニティエンティティの一意の ID。 |
| `opportunityName` | 文字列 | オポチュニティの名前。 |
| `opportunityStage` | 文字列 | オポチュニティの販売ステージ。 |
| `opportunityType` | 文字列 | オポチュニティのタイプ。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [リアルタイム CDP B2B エディションのスキーマの関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
