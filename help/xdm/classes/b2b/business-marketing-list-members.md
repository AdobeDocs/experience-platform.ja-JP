---
title: XDM ビジネスマーケティングリストメンバークラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスマーケティングリストメンバークラスの概要を説明します。
exl-id: 069002c2-5583-4c59-84ee-c071e2acaaec
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 7%

---

# [!UICONTROL XDM ビジネスマーケティングリストメンバー] クラス

>[!IMPORTANT]
>
>このクラスは、 [Adobe Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-Time CDP B2B Edition へのアクセス権が必要です [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスマーケティングリストメンバー] は、マーケティングリストに関連付けられたメンバー、個人、または連絡先を記述する標準 Experience Data Model(XDM) クラスです。

![UI に表示される XDM Business Marketing List Members クラスの構造](../../images/classes/b2b/business-marketing-list-members.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | マーケティングリストのメンバーシップが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `marketingListKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 人物がメンバーであるマーケティングリストの複合識別子。 |
| `marketingListMemberKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | マーケティングリストメンバーシップエンティティの複合識別子。 |
| `personKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | マーケティングリストのメンバーである人物の複合識別子。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `marketingListMemberID`. |
| `isDeleted` | ブール値 | このマーケティングリストメンバーエンティティがMarketo Engageで削除されたかどうかを示します。<br><br>を使用する場合、 [Marketoソースコネクタ](../../../sources/connectors/adobe-applications/marketo/marketo.md)を指定した場合、Marketoで削除されたレコードは自動的にリアルタイム顧客プロファイルに反映されます。 ただし、これらのプロファイルに関連するレコードは、データレイク内で引き続き保持される場合があります。 設定別 `isDeleted` から `true`の場合は、フィールドを使用して、データレイクに対するクエリを実行する際に、ソースから削除されたレコードを除外できます。 |
| `marketingListID` | 文字列 | マーケティングリストの一意の ID。 |
| `marketingListMemberID` | 文字列 | マーケティングリストメンバーシップエンティティの一意の ID。 |
| `personId` | 文字列 | 人物の一意の ID。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [Real-Time CDP B2B Edition のスキーマ関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
