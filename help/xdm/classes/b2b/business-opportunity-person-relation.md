---
title: XDM Business Opportunity ユーザー関係クラス
description: エクスペリエンスデータモデル（XDM）の XDM Business Opportunity ユーザー関係クラスについて説明します。
exl-id: 7be193d2-52eb-4b28-953b-5e0fc21d8f93
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 3%

---

# [!UICONTROL XDM Business Opportunity ユーザー関係 &#x200B;] クラス

>[!IMPORTANT]
>
>このクラスは、[Adobe Real-time Customer Data Platform B2B Edition](../../../rtcdp/b2b-overview.md) にアクセスできる組織で使用することを目的としています。 このクラスを [ リアルタイム顧客プロファイル ](../../../profile/home.md) に加えるには、Real-Time CDP B2B Edition へのアクセス権が必要です。

[!UICONTROL XDM Business Opportunity ユーザー関係 &#x200B;] は、ビジネスオポチュニティに関連付けられたユーザーの必要最小限のプロパティをキャプチャする、標準のエクスペリエンスデータモデル（XDM）クラスです。

![UI に表示される XDM Business Opportunity ユーザークラスの構造 ](../../images/classes/b2b/business-opportunity-person-relation.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL &#x200B; 外部Source システム監査属性 &#x200B;]](../../data-types/external-source-system-audit-attributes.md) | ビジネスパーソンの関係が外部ソースシステムから取得されたものである場合、このオブジェクトはそのシステムの監査属性を取得します。 |
| `opportunityKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | 機会とユーザーの関係における機会の複合識別子。 |
| `opportunityPersonKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | 機会 – 人物関係エンティティの複合識別子。 |
| `personKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | 機会とユーザーの関係におけるユーザーの複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、クラスによって取り込まれる他の ID フィールドとは別のシステム生成の値です。 |
| `isDeleted` | ブール値 | このマーケティング リスト エンティティがMarketo Engageで削除されているかどうかを示します。<br><br>[Marketo ソースコネクタを使用 ](../../../sources/connectors/adobe-applications/marketo/marketo.md) すると、Marketoで削除されたすべてのレコードがリアルタイム顧客プロファイルに自動的に反映されます。 ただし、これらのプロファイルに関連するレコードは、引き続きデータレイクに保持される場合があります。 `isDeleted` を `true` に設定すると、フィールドを使用して、データレイクのクエリ時にソースから削除されたレコードをフィルターで除外できます。 |
| `isPrimary` | ブール値 | 人物がこのオポチュニティの主要連絡先であるかどうかを示します。 |
| `opportunityID` | 文字列 | 機会とユーザーの関係における機会の一意の ID。 |
| `opportunityPersonID` | 文字列 | 商談 – 人物関係エンティティの一意の ID |
| `personID` | 文字列 | 機会とユーザーの関係におけるユーザーの一意の ID。 |
| `personRole` | 文字列 | オポチュニティと人物の関係における人物の役割。 |

{style="table-layout:auto"}

このクラスが他の B2B クラスと概念的にどのように関連し、Real-Time CDP UI でこれらの関係を確立する方法については、[Adobe Experience Platform B2B Edition のスキーマの関係 ](../../tutorials/relationship-b2b.md) に関するガイドを参照してください。
