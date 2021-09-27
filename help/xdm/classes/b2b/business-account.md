---
title: XDMビジネスアカウントクラス
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)のXDMビジネスアカウントクラスの概要を説明します。
source-git-commit: 19bb39b66f3a3eb93fd0138ac021568021d77b0f
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 4%

---

# [!UICONTROL XDM Business ] Accountclass

>[!NOTE]
>
>このクラスは、リアルタイム顧客データプラットフォームのB2Bエディションにアクセスできる組織でのみ使用できます。

[!UICONTROL XDM Business Account] は、ビジネスアカウントの最低限必要なプロパティを取り込む、標準のExperience Data Model(XDM)クラスです。

![](../../images/classes/b2b/business-account.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 勘定エンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | アカウントが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`accountID`とは別のシステム生成値です。 |
| `accountID` | 文字列 | アカウントエンティティの一意の識別子。 |
