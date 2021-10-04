---
title: XDM ビジネスアカウントクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスアカウントクラスの概要を説明します。
exl-id: abe4c919-a680-4aad-918e-6e56cae8bd4d
source-git-commit: b5cdd72238f7b4519de1c789f4294b9698415327
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 9%

---

# [!UICONTROL XDM ビジネスア] カウントクラス（ベータ版）

>[!IMPORTANT]
>
>このクラスは、現在ベータ版のリアルタイム顧客データプラットフォーム B2B エディションの一部です。 ドキュメントと機能は変更される場合があります。

[!UICONTROL XDM ビジネスア] カウントは、ビジネスアカウントの最低限必要なプロパティを取得する標準の Experience Data Model(XDM) クラスです。

![](../../images/classes/b2b/business-account.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 勘定エンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | アカウントが外部ソース・システムから取得された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`accountID` とは別のシステム生成値です。 |
| `accountID` | 文字列 | アカウントエンティティの一意の識別子。 |

{style=&quot;table-layout:auto&quot;}

このクラスが概念的に他の B2B クラスと関連付けられる方法と、Adobe Experience Platform UI でこれらの関係を確立する方法については、Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md) の [ スキーマの関係に関するガイドを参照してください。
