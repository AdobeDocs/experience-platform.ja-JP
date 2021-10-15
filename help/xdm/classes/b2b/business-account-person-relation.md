---
title: XDM ビジネスアカウント担当者関係クラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスアカウント担当者関係クラスの概要を説明します。
exl-id: d51abe9b-d936-4c84-96e2-35a81ca6b67f
source-git-commit: b5cdd72238f7b4519de1c789f4294b9698415327
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 7%

---

# [!UICONTROL XDM ビジネスアカウント担当者] 関係クラス（ベータ版）

>[!IMPORTANT]
>
>このクラスは、現在ベータ版のReal-time Customer Data Platform B2B Edition の一部です。 ドキュメントと機能は変更される場合があります。

[!UICONTROL XDM Business Account Person ] Relationations は、ビジネスアカウントに関連付けられている個人の最低限必要なプロパティを取得する、標準の Experience Data Model(XDM) クラスです。

![](../../images/classes/b2b/business-account-person-relation.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 勘定科目と個人関係の勘定科目の複合識別子。 |
| `accountPersonKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 勘定 — 個人関係エンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | アカウントと個人の関係が外部ソース・システムから取得された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `personKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 勘定 — 個人関係の個人の複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、クラスで取り込まれる他の ID フィールドとは別の、システムで生成される値です。 |
| `accountID` | 文字列 | アカウントと個人の関係にあるアカウントの一意の識別子。 |
| `accountPersonID` | 文字列 | 勘定 — 個人関係エンティティの一意の識別子。 |
| `currencyCode` | 文字列 | アカウントと個人の関係に使用される ISO 4217 通貨コード。 |
| `isActive` | Boolean | アカウントと個人の間の関係がアクティブかどうかを示します。 |
| `isDirect` | Boolean | アカウントと個人の間の直接の関係であるかどうかを示します。 |
| `isPrimary` | Boolean | 個人がこのアカウントの主要連絡先であるかどうかを示します。 |
| `personID` | 文字列 | アカウントと個人の関係にある個人の一意の識別子。 |
| `personRole` | 文字列 | アカウントと個人の関係における個人の役割。 |
| `relationEndDate` | DateTime | アカウントと個人の関係が終了した日付。 |
| `relationStartDate` | DateTime | アカウントと個人の関係が開始された日付。 |

{style=&quot;table-layout:auto&quot;}

このクラスが概念的に他の B2B クラスと関連付けられる方法と、Adobe Experience Platform UI でこれらの関係を確立する方法については、Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md) の [ スキーマの関係に関するガイドを参照してください。
