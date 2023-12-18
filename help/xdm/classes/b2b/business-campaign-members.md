---
title: XDM ビジネスキャンペーンメンバークラス
description: エクスペリエンスデータモデル (XDM) の XDM ビジネスキャンペーンメンバークラスについて説明します。
exl-id: a39eac7d-46ee-4e9c-a1c0-4dbb63f2c813
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 2%

---

# [!UICONTROL XDM ビジネスキャンペーンメンバー] クラス

>[!IMPORTANT]
>
>このクラスは、 [Adobe Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-Time CDP B2B Edition へのアクセス権が必要です [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスキャンペーンメンバー] は、ビジネスキャンペーンに関連付けられた連絡先またはリードを記述する標準 Experience Data Model(XDM) クラスです。

![UI に表示される XDM ビジネスキャンペーンメンバークラスの構造](../../images/classes/b2b/business-campaign-members.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 関連するキャンペーンの複合 ID。 |
| `campaignMemberKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | キャンペーンメンバーシップエンティティの複合 ID。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | キャンペーンのメンバーシップが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `personKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 関連するキャンペーンのメンバーである人物の複合 ID。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `campaignMemberID`. |

{style="table-layout:auto"}

このクラスが概念上他の B2B クラスとどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法については、 [Real-Time CDP B2B Edition のスキーマ関係](../../tutorials/relationship-b2b.md)

このクラスと互換性のある追加のフィールドについては、 [[!UICONTROL XDM ビジネスキャンペーンメンバーの詳細]](../../field-groups/b2b-campaign-members/details.md).
