---
title: XDM ビジネスアカウントクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスアカウントクラスの概要を説明します。
exl-id: abe4c919-a680-4aad-918e-6e56cae8bd4d
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 6%

---

# [!UICONTROL XDM ビジネスアカウント] クラス

>[!IMPORTANT]
>
>このクラスは、 [Adobe Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-Time CDP B2B Edition へのアクセス権が必要です [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスアカウント] は、ビジネスアカウントに最低限必要なプロパティを取り込む、標準の Experience Data Model(XDM) クラスです。

![UI に表示される XDM Business Account クラスの構造](../../images/classes/b2b/business-account.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | アカウントエンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | アカウントが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `accountKey` 識別子。 |
| `isDeleted` | ブール値 | このアカウントエンティティがMarketo Engageで削除されたかどうかを示します。<br><br>を使用する場合、 [Marketoソースコネクタ](../../../sources/connectors/adobe-applications/marketo/marketo.md)を指定した場合、Marketoで削除されたレコードは自動的にリアルタイム顧客プロファイルに反映されます。 ただし、これらのプロファイルに関連するレコードは、データレイク内で引き続き保持される場合があります。 設定別 `isDeleted` から `true`の場合は、フィールドを使用して、データレイクに対するクエリを実行する際に、ソースから削除されたレコードを除外できます。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [Real-Time CDP B2B Edition のスキーマ関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
