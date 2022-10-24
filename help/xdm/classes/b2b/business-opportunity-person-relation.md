---
title: XDM ビジネスオポチュニティ人物関係クラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスオポチュニティ人物関係クラスの概要を説明します。
exl-id: 7be193d2-52eb-4b28-953b-5e0fc21d8f93
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 7%

---

# [!UICONTROL XDM ビジネスオポチュニティ人物関係] クラス

>[!IMPORTANT]
>
>このクラスは、 [Adobe Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-Time CDP B2B Edition へのアクセス権が必要です [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスオポチュニティ人物関係] は、ビジネスオポチュニティに関連付けられている個人の最小限必要なプロパティをキャプチャする、標準 Experience Data Model(XDM) クラスです。

![UI に表示される XDM Business Opportunity Person クラスの構造](../../images/classes/b2b/business-opportunity-person-relation.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | ビジネス人物関係が外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `opportunityKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | オポチュニティと人物の関係におけるオポチュニティの複合識別子。 |
| `opportunityPersonKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | オポチュニティと人物の関係エンティティの複合識別子。 |
| `personKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | オポチュニティと人物の関係にある人物の複合識別子。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、クラスで取り込まれる他の ID フィールドとは別の、システムで生成される値です。 |
| `isDeleted` | ブール値 | このマーケティングリストエンティティがMarketo Engageで削除されたかどうかを示します。<br><br>を使用する場合、 [Marketoソースコネクタ](../../../sources/connectors/adobe-applications/marketo/marketo.md)を指定した場合、Marketoで削除されたレコードは自動的にリアルタイム顧客プロファイルに反映されます。 ただし、これらのプロファイルに関連するレコードは、データレイク内で引き続き保持される場合があります。 設定別 `isDeleted` から `true`の場合は、フィールドを使用して、データレイクに対するクエリを実行する際に、ソースから削除されたレコードを除外できます。 |
| `isPrimary` | ブール値 | 人物がこのオポチュニティの主要連絡先かどうかを示します。 |
| `opportunityID` | 文字列 | オポチュニティと人物の関係におけるオポチュニティの一意の識別子。 |
| `opportunityPersonID` | 文字列 | オポチュニティと人物の関係エンティティの一意の識別子 |
| `personID` | 文字列 | オポチュニティと人物の関係にある人物の一意の識別子。 |
| `personRole` | 文字列 | オポチュニティと人物の関係における人物の役割。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [Real-Time CDP B2B Edition のスキーマ関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
