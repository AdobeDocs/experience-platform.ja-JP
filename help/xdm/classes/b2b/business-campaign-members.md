---
title: XDM ビジネスキャンペーンメンバークラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスキャンペーンメンバークラスの概要を説明します。
exl-id: a39eac7d-46ee-4e9c-a1c0-4dbb63f2c813
source-git-commit: 8718512a9768158183b9fb6b9e336081e47cd889
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 4%

---

# [!UICONTROL XDM ビジネスキャンペーンメンバー] クラス

>[!IMPORTANT]
>
>このクラスは、 [Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-time CDP B2B Edition へのアクセス権が必要です。 [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスキャンペーンメンバー] は、ビジネスキャンペーンに関連付けられた連絡先またはリードを記述する標準 Experience Data Model(XDM) クラスです。

![](../../images/classes/b2b/business-campaign-members.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 関連するキャンペーンの複合識別子。 |
| `campaignMemberKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | キャンペーンメンバーシップエンティティの複合 ID。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | キャンペーンのメンバーシップが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `personKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 関連するキャンペーンのメンバーである人物の複合 ID。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `campaignMemberID`. |
| `campaignID` | 文字列 | 関連するキャンペーンの一意の ID。 |
| `campaignMemberID` | 文字列 | キャンペーンメンバーシップエンティティの一意の ID。 |
| `personId` | 文字列 | 関連するキャンペーンのメンバーである人物の一意の ID。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [リアルタイム CDP B2B エディションのスキーマの関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
