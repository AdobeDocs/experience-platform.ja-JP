---
title: XDM ビジネスキャンペーンクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスキャンペーンクラスの概要を説明します。
exl-id: 4e3228a1-74be-43af-b355-45d84afb1611
source-git-commit: 0084492ed467c5996a94c5c55a79c9faf8f5046e
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 8%

---

# [!UICONTROL XDM ビジネスキャンペーン] クラス

>[!IMPORTANT]
>
>このクラスは、 [Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-time CDP B2B Edition へのアクセス権が必要です。 [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスキャンペーン] は、ビジネスキャンペーンで最低限必要なプロパティを取り込む、標準エクスペリエンスデータモデル (XDM) クラスです。

![UI に表示される XDM ビジネスキャンペーンクラスの構造](../../images/classes/b2b/business-campaign.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | キャンペーンエンティティの複合 ID。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | キャンペーンが外部ソースシステムから送信された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `campaignID`. |
| `campaignDescription` | 文字列 | キャンペーンの説明。 |
| `campaignID` | 文字列 | キャンペーンエンティティの一意の ID。 |
| `campaignName` | 文字列 | キャンペーンの名前。 |
| `campaignType` | 文字列 | キャンペーンタイプまたはターゲットオーディエンス。 |

{style=&quot;table-layout:auto&quot;}

このクラスが概念上他の B2B クラスとどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法については、 [リアルタイム CDP B2B エディションのスキーマの関係](../../tutorials/relationship-b2b.md)

このクラスと互換性のある追加のフィールドについては、 [[!UICONTROL XDM ビジネスキャンペーンの詳細]](../../field-groups/b2b-campaign/details.md).
