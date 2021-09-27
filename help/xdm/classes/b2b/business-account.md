---
title: XDMビジネスアカウントクラス
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)のXDMビジネスアカウントクラスの概要を説明します。
source-git-commit: 5fd82b02eb25f3d575de695c2f2b14a5e5b18400
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 3%

---

# [!UICONTROL XDM Business ] Accountclass

>[!NOTE]
>
>このクラスは、リアルタイム顧客データプラットフォームB2Bエディションにアクセスできる組織でのみ使用できます。

[!UICONTROL XDM Business Account] は、ビジネスアカウントの最低限必要なプロパティを取り込む、標準のExperience Data Model(XDM)クラスです。

![](../../images/classes/b2b/business-account.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 勘定エンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | アカウントが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`accountID`とは別のシステム生成値です。 |
| `accountID` | 文字列 | アカウントエンティティの一意の識別子。 |

このクラスと他のB2Bクラスとの概念的な関係と、Adobe Experience Platform UIでこれらの関係を確立する方法については、 Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md)の[スキーマの関係に関するガイドを参照してください。
