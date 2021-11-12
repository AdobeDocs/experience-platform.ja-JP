---
title: XDM ビジネスアカウントクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスアカウントクラスの概要を説明します。
exl-id: abe4c919-a680-4aad-918e-6e56cae8bd4d
source-git-commit: edf7afc5db219430232a3226dc691570b50a32bd
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 6%

---

# [!UICONTROL XDM ビジネスアカウント] クラス

[!UICONTROL XDM ビジネスアカウント] は、ビジネスアカウントに最低限必要なプロパティを取り込む、標準の Experience Data Model(XDM) クラスです。

![](../../images/classes/b2b/business-account.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | アカウントエンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | アカウントが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `accountID`. |
| `accountID` | 文字列 | アカウントエンティティの一意の ID。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [リアルタイム CDP B2B エディションのスキーマの関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
