---
title: XDMビジネスアカウント担当者関係クラス
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)のXDMビジネスアカウント担当者関係クラスの概要を説明します。
source-git-commit: 19bb39b66f3a3eb93fd0138ac021568021d77b0f
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 5%

---

# [!UICONTROL XDM Business Account Person ] Relationclass

>[!NOTE]
>
>このクラスは、リアルタイム顧客データプラットフォームのB2Bエディションにアクセスできる組織でのみ使用できます。

[!UICONTROL XDM Business Account Person Relationations] は、ビジネスアカウントに関連付けられている個人の最低限必要なプロパティを取り込む、標準のExperience Data Model(XDM)クラスです。

![](../../images/classes/b2b/business-account-person-relation.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 勘定科目と個人関係の勘定科目の複合識別子。 |
| `accountPersonKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 勘定 — 個人関係エンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | アカウントと個人の関係が外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `personKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 勘定科目と個人関係の個人の複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、クラスで取り込まれる他のIDフィールドとは別の、システムで生成される値です。 |
| `accountID` | 文字列 | アカウントと個人の関係にあるアカウントの一意の識別子。 |
| `accountPersonID` | 文字列 | アカウントと個人の関係エンティティの一意の識別子。 |
| `currencyCode` | 文字列 | アカウントと個人の関係に使用されるISO 4217通貨コード。 |
| `isActive` | Boolean | アカウントと個人の間の関係がアクティブかどうかを示します。 |
| `isDirect` | Boolean | アカウントと個人の間の直接関係であるかどうかを示します。 |
| `isPrimary` | Boolean | 個人がこのアカウントの主要連絡先であるかどうかを示します。 |
| `personID` | 文字列 | アカウントと個人の関係にある個人の一意の識別子。 |
| `personRole` | 文字列 | アカウントと個人の関係における個人の役割。 |
| `relationEndDate` | DateTime | アカウントと個人の関係が終了した日付。 |
| `relationStartDate` | DateTime | アカウントと個人の関係が開始された日付。 |
