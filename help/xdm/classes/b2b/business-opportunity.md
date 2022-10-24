---
title: XDM ビジネスオポチュニティクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM Business Opportunity クラスの概要を説明します。
exl-id: d816b0f9-fd37-45da-aa55-247f7f662da0
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 8%

---

# [!UICONTROL XDM ビジネスオポチュニティ] クラス

>[!IMPORTANT]
>
>このクラスは、 [Adobe Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-Time CDP B2B Edition へのアクセス権が必要です [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスオポチュニティ] は、ビジネスオポチュニティに最低限必要なプロパティを取り込む、標準 Experience Data Model(XDM) クラスです。

![UI に表示される XDM Business Opportunity クラスの構造](../../images/classes/b2b/business-opportunity.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | このオポチュニティが関連付けられているアカウントの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | オポチュニティが外部のソースシステムから来た場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `opportunityKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | オポチュニティエンティティの複合識別子。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `opportunityID`. |
| `accountID` | 文字列 | このオポチュニティが関連付けられているアカウントの一意の ID。 |
| `isDeleted` | ブール値 | このマーケティングリストエンティティがMarketo Engageで削除されたかどうかを示します。<br><br>を使用する場合、 [Marketoソースコネクタ](../../../sources/connectors/adobe-applications/marketo/marketo.md)を指定した場合、Marketoで削除されたレコードは自動的にリアルタイム顧客プロファイルに反映されます。 ただし、これらのプロファイルに関連するレコードは、データレイク内で引き続き保持される場合があります。 設定別 `isDeleted` から `true`の場合は、フィールドを使用して、データレイクに対するクエリを実行する際に、ソースから削除されたレコードを除外できます。 |
| `opportunityDescription` | 文字列 | オポチュニティの説明。 |
| `opportunityID` | 文字列 | オポチュニティエンティティの一意の ID。 |
| `opportunityName` | 文字列 | オポチュニティの名前。 |
| `opportunityStage` | 文字列 | オポチュニティの販売ステージ。 |
| `opportunityType` | 文字列 | オポチュニティのタイプ。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [Real-Time CDP B2B Edition のスキーマ関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
