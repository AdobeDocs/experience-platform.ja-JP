---
title: XDM ビジネスアカウント人物関係クラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスアカウント人物関係クラスの概要を説明します。
exl-id: d51abe9b-d936-4c84-96e2-35a81ca6b67f
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 7%

---

# [!UICONTROL XDM ビジネスアカウント人物関係] クラス

>[!IMPORTANT]
>
>このクラスは、 [Adobe Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-Time CDP B2B Edition へのアクセス権が必要です [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスアカウント人物関係] は、ビジネスアカウントに関連付けられている個人の最小限必要なプロパティを取得する、標準 Experience Data Model(XDM) クラスです。

![UI に表示される XDM ビジネスアカウント人物関係クラスの構造](../../images/classes/b2b/business-account-person-relation.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | アカウントと人物の関係におけるアカウントの複合識別子。 |
| `accountPersonKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | アカウントと人物の関係エンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | アカウントと人物の関係が外部のソースシステムから来た場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `personKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | アカウントと人物の関係における人物の複合識別子。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、クラスで取り込まれる他の ID フィールドとは別の、システムで生成される値です。 |
| `accountID` | 文字列 | アカウントと人物の関係におけるアカウントの一意の識別子。 |
| `accountPersonID` | 文字列 | アカウントと人物の関係エンティティの一意の ID。 |
| `currencyCode` | 文字列 | アカウントと人物の間の関係に使用される ISO 4217 通貨コード。 |
| `isActive` | ブール値 | アカウントと人物の間の関係がアクティブかどうかを示します。 |
| `isDeleted` | ブール値 | このアカウントと人物の関係がMarketo Engageで削除されたかどうかを示します。<br><br>を使用する場合、 [Marketoソースコネクタ](../../../sources/connectors/adobe-applications/marketo/marketo.md)を指定した場合、Marketoで削除されたレコードは自動的にリアルタイム顧客プロファイルに反映されます。 ただし、これらのプロファイルに関連するレコードは、データレイク内で引き続き保持される場合があります。 設定別 `isDeleted` から `true`の場合は、フィールドを使用して、データレイクに対するクエリを実行する際に、ソースから削除されたレコードを除外できます。 |
| `isDirect` | ブール値 | これがアカウントと人物の間の直接の関係であるかどうかを示します。 |
| `isPrimary` | ブール値 | 人物がこのアカウントの主要連絡先かどうかを示します。 |
| `personID` | 文字列 | アカウントと人物の関係における人物の一意の識別子。 |
| `personRoles` | 文字列の配列 | アカウントと人物の関係における人物の役割のリストが表示されます。 |
| `relationEndDate` | 日時 | アカウントと人物の間の関係が終了した日付。 |
| `relationStartDate` | 日時 | アカウントと人物の間の関係が開始した日付。 |
| `relationshipSource` | 文字列 | アカウントと人物の関係のソース。 |

{style="table-layout:auto"}

詳しくは、 [Real-Time CDP B2B Edition のスキーマ関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
