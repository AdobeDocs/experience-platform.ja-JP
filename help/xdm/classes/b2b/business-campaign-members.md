---
title: XDM ビジネスキャンペーンメンバークラス
description: エクスペリエンスデータモデル（XDM）の XDM ビジネスキャンペーンメンバークラスについて説明します。
exl-id: a39eac7d-46ee-4e9c-a1c0-4dbb63f2c813
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 2%

---

# [!UICONTROL XDM Business Campaign Members] クラス

>[!IMPORTANT]
>
>このクラスは、[Adobe Real-time Customer Data Platform B2B Edition](../../../rtcdp/b2b-overview.md) にアクセスできる組織で使用することを目的としています。 このクラスを [&#x200B; リアルタイム顧客プロファイル &#x200B;](../../../profile/home.md) に加えるには、Real-Time CDP B2B Edition へのアクセス権が必要です。

[!UICONTROL XDM ビジネスキャンペーンメンバー &#x200B;] は、ビジネスキャンペーンに関連する連絡先またはリードを記述する標準のエクスペリエンスデータモデル（XDM）クラスです。

![UI に表示される XDM ビジネスキャンペーンメンバークラスの構造 &#x200B;](../../images/classes/b2b/business-campaign-members.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | 関連付けられたキャンペーンの複合識別子。 |
| `campaignMemberKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | キャンペーンメンバーシップエンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL &#x200B; 外部Source システム監査属性 &#x200B;]](../../data-types/external-source-system-audit-attributes.md) | キャンペーンメンバーシップが外部ソースシステムから取得される場合、このオブジェクトはそのシステムの監査属性を取得します。 |
| `personKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | 関連するキャンペーンのメンバーであるユーザーの複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`campaignMemberID` とは別の、システムで生成された値です。 |

{style="table-layout:auto"}

このクラスが他の B2B クラスとどのように概念的に関係し、Real-Time CDP UI でこれらの関係を確立する方法については、Adobe Experience Platform B2B Edition の [&#x200B; スキーマの関係に関するガイドを参照してください &#x200B;](../../tutorials/relationship-b2b.md)

このクラスと互換性のあるその他のフィールドについては、[[!UICONTROL XDM ビジネスキャンペーンメンバーの詳細 &#x200B;]](../../field-groups/b2b-campaign-members/details.md) のフィールドグループ参照を参照してください。
