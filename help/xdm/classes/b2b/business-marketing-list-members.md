---
title: XDM ビジネスマーケティングリストメンバークラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスマーケティングリストメンバークラスの概要を説明します。
exl-id: 069002c2-5583-4c59-84ee-c071e2acaaec
source-git-commit: 8718512a9768158183b9fb6b9e336081e47cd889
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 4%

---

# [!UICONTROL XDM ビジネスマーケティングリストメンバー] クラス

>[!IMPORTANT]
>
>このクラスは、 [Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-time CDP B2B Edition へのアクセス権が必要です。 [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスマーケティングリストメンバー] は、マーケティングリストに関連付けられたメンバー、個人、または連絡先を記述する標準 Experience Data Model(XDM) クラスです。

![](../../images/classes/b2b/business-marketing-list-members.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | マーケティングリストのメンバーシップが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `marketingListKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 人物がメンバーであるマーケティングリストの複合識別子。 |
| `marketingListMemberKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | マーケティングリストメンバーシップエンティティの複合識別子。 |
| `personKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | マーケティングリストのメンバーである人物の複合識別子。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `marketingListMemberID`. |
| `marketingListID` | 文字列 | マーケティングリストの一意の ID。 |
| `marketingListMemberID` | 文字列 | マーケティングリストメンバーシップエンティティの一意の ID。 |
| `personId` | 文字列 | 人物の一意の ID。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [リアルタイム CDP B2B エディションのスキーマの関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
