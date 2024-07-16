---
title: XDM ビジネスキャンペーンクラス
description: エクスペリエンスデータモデル（XDM）の XDM ビジネスキャンペーンクラスについて説明します。
exl-id: 4e3228a1-74be-43af-b355-45d84afb1611
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 3%

---

# [!UICONTROL XDM Business Campaign] クラス

>[!IMPORTANT]
>
>このクラスは、[Adobe Real-time Customer Data Platform B2B Edition](../../../rtcdp/b2b-overview.md) にアクセスできる組織で使用することを目的としています。 このクラスを [ リアルタイム顧客プロファイル ](../../../profile/home.md) に加えるには、Real-Time CDP B2B Edition へのアクセス権が必要です。

[!UICONTROL XDM ビジネスキャンペーン ] は、ビジネスキャンペーンに必要な最小限のプロパティをキャプチャする、標準のエクスペリエンスデータモデル（XDM）クラスです。

![UI に表示される XDM ビジネスキャンペーンクラスの構造 ](../../images/classes/b2b/business-campaign.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | キャンペーンエンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL  外部Source システム監査属性 ]](../../data-types/external-source-system-audit-attributes.md) | キャンペーンが外部ソースシステムから作成される場合、このオブジェクトはそのシステムの監査属性を取得します。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`campaignID` とは別の、システムで生成された値です。 |
| `campaignDescription` | 文字列 | キャンペーンの説明。 |
| `campaignID` | 文字列 | キャンペーンエンティティの一意の ID。 |
| `campaignName` | 文字列 | キャンペーンの名前。 |
| `campaignType` | 文字列 | キャンペーンタイプまたはターゲットオーディエンス。 |

{style="table-layout:auto"}

このクラスが他の B2B クラスとどのように概念的に関係し、Real-Time CDP UI でこれらの関係を確立する方法については、Adobe Experience Platform B2B Edition の [ スキーマの関係に関するガイドを参照してください ](../../tutorials/relationship-b2b.md)

このクラスと互換性のあるその他のフィールドについては、[[!UICONTROL XDM ビジネスキャンペーンの詳細 ]](../../field-groups/b2b-campaign/details.md) のフィールドグループ参照を参照してください。
