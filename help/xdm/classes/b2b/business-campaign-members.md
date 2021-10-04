---
title: XDM Business Campaign メンバークラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM Business Campaign Members クラスの概要を説明します。
exl-id: a39eac7d-46ee-4e9c-a1c0-4dbb63f2c813
source-git-commit: b5cdd72238f7b4519de1c789f4294b9698415327
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 8%

---

# [!UICONTROL XDM Business Campaign メンバ] ークラス（ベータ版）

>[!IMPORTANT]
>
>このクラスは、現在ベータ版のリアルタイム顧客データプラットフォーム B2B エディションの一部です。 ドキュメントと機能は変更される場合があります。

[!UICONTROL XDM Business Campaign メンバ] ーは、ビジネスキャンペーンに関連付けられた連絡先またはリードを記述する標準のエクスペリエンスデータモデル (XDM) クラスです。

![](../../images/classes/b2b/business-campaign-members.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 関連するキャンペーンの複合識別子。 |
| `campaignMemberKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | キャンペーンのメンバーシップエンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | キャンペーンのメンバーシップが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `personKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 関連するキャンペーンのメンバーである個人の複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`campaignMemberID` とは別のシステム生成値です。 |
| `campaignID` | 文字列 | 関連するキャンペーンの一意の ID。 |
| `campaignMemberID` | 文字列 | キャンペーンのメンバーシップエンティティの一意の ID。 |
| `personId` | 文字列 | 関連するキャンペーンのメンバーである個人の一意の ID。 |

{style=&quot;table-layout:auto&quot;}

このクラスが概念的に他の B2B クラスと関連付けられる方法と、Adobe Experience Platform UI でこれらの関係を確立する方法については、Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md) の [ スキーマの関係に関するガイドを参照してください。
