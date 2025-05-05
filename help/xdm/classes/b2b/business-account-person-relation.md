---
title: XDM ビジネスアカウント人物関係クラス
description: エクスペリエンスデータモデル（XDM）の XDM ビジネスアカウント人物関係クラスについて説明します。
exl-id: d51abe9b-d936-4c84-96e2-35a81ca6b67f
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 12%

---

# [!UICONTROL XDM Business Account Person Relation] クラス

>[!IMPORTANT]
>
>このクラスは、[Adobe Real-time Customer Data Platform B2B Edition](../../../rtcdp/b2b-overview.md) にアクセスできる組織で使用することを目的としています。 このクラスを [ リアルタイム顧客プロファイル ](../../../profile/home.md) に加えるには、Real-Time CDP B2B Edition へのアクセス権が必要です。

[!UICONTROL XDM ビジネスアカウントユーザー関係 &#x200B;] は、ビジネスアカウントに関連付けられたユーザーの必要最小限のプロパティをキャプチャする、標準のエクスペリエンスデータモデル（XDM）クラスです。

![UI に表示される XDM ビジネスアカウントユーザー関係クラスの構造 ](../../images/classes/b2b/business-account-person-relation.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | アカウントとユーザーの関係におけるアカウントの複合識別子。 |
| `accountPersonKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | アカウントと人物の関係エンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL &#x200B; 外部Source システム監査属性 &#x200B;]](../../data-types/external-source-system-audit-attributes.md) | アカウントと人物の関係が外部ソースシステムからのものである場合、このオブジェクトはそのシステムの監査属性を取得します。 |
| `personKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | アカウントとユーザーの関係におけるユーザーの複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、クラスによって取り込まれる他の ID フィールドとは別のシステム生成の値です。 |
| `accountID` | 文字列 | アカウントと人物の関係におけるアカウントの一意の ID。 |
| `accountPersonID` | 文字列 | アカウント – 人物関係エンティティの一意の ID。 |
| `currencyCode` | 文字列 | アカウントと人物の間の関係に使用される ISO 4217 通貨コード。 |
| `isActive` | ブール値 | アカウントと人物の間の関係がアクティブかどうかを示します。 |
| `isDeleted` | ブール値 | このアカウントと担当者の関係がMarketo Engageで削除されたかどうかを示します。<br><br>[Marketo ソースコネクタを使用 ](../../../sources/connectors/adobe-applications/marketo/marketo.md) すると、Marketoで削除されたすべてのレコードがリアルタイム顧客プロファイルに自動的に反映されます。 ただし、これらのプロファイルに関連するレコードは、引き続きデータレイクに保持される場合があります。 `isDeleted` を `true` に設定すると、フィールドを使用して、データレイクのクエリ時にソースから削除されたレコードをフィルターで除外できます。 |
| `isDirect` | ブール値 | これがアカウントと人物の間の直接の関係であるかどうかを示します。 |
| `isPrimary` | ブール値 | ユーザーがこのアカウントのプライマリ連絡先かどうかを示します。 |
| `personID` | 文字列 | アカウントと人物の関係における人物の一意の ID。 |
| `personRoles` | 文字列の配列 | アカウントと人物の関係における人物の役割をリストします。 |
| `relationEndDate` | 日時 | アカウントと人物の間の関係が終了する日付。 |
| `relationStartDate` | 日時 | アカウントと人物の間の関係が開始する日付。 |
| `relationshipSource` | 文字列 | アカウントと人物の関係のソース。 |

{style="table-layout:auto"}

このクラスが他の B2B クラスと概念的にどのように関連し、Real-Time CDP UI でこれらの関係を確立する方法については、[Adobe Experience Platform B2B Edition のスキーマの関係 ](../../tutorials/relationship-b2b.md) に関するガイドを参照してください。
