---
title: XDM ビジネスマーケティングリストメンバークラス
description: エクスペリエンスデータモデル（XDM）の XDM ビジネスマーケティングリストメンバークラスについて説明します。
exl-id: 069002c2-5583-4c59-84ee-c071e2acaaec
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 2%

---

# [!UICONTROL XDM Business Marketing List Members] クラス

>[!IMPORTANT]
>
>このクラスは、[Adobe Real-time Customer Data Platform B2B Edition](../../../rtcdp/b2b-overview.md) にアクセスできる組織で使用することを目的としています。 このクラスを [ リアルタイム顧客プロファイル ](../../../profile/home.md) に加えるには、Real-Time CDP B2B Edition へのアクセス権が必要です。

[!UICONTROL XDM ビジネスマーケティングリストメンバー &#x200B;] は、マーケティングリストに関連付けられたメンバー、人物または連絡先を記述する、標準のエクスペリエンスデータモデル（XDM）クラスです。

![UI に表示される XDM ビジネスマーケティングリストメンバークラスの構造 ](../../images/classes/b2b/business-marketing-list-members.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL &#x200B; 外部Source システム監査属性 &#x200B;]](../../data-types/external-source-system-audit-attributes.md) | マーケティングリストメンバーシップが外部ソースシステムから取得される場合、このオブジェクトはそのシステムの監査属性を取得します。 |
| `marketingListKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | ユーザーがメンバーになっているマーケティングリストの複合識別子。 |
| `marketingListMemberKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | マーケティングリストメンバーシップエンティティの複合識別子。 |
| `personKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | マーケティングリストのメンバーであるユーザーのための複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`marketingListMemberID` とは別の、システムで生成された値です。 |
| `isDeleted` | ブール値 | このマーケティング リスト メンバーエンティティがMarketo Engageで削除されているかどうかを示します。<br><br>[Marketo ソースコネクタを使用 ](../../../sources/connectors/adobe-applications/marketo/marketo.md) すると、Marketoで削除されたすべてのレコードがリアルタイム顧客プロファイルに自動的に反映されます。 ただし、これらのプロファイルに関連するレコードは、引き続きデータレイクに保持される場合があります。 `isDeleted` を `true` に設定すると、フィールドを使用して、データレイクのクエリ時にソースから削除されたレコードをフィルターで除外できます。 |
| `marketingListID` | 文字列 | マーケティングリストの一意の ID。 |
| `marketingListMemberID` | 文字列 | マーケティングリストのメンバーシップエンティティの一意の ID。 |
| `personId` | 文字列 | 人物の一意の ID。 |

{style="table-layout:auto"}

このクラスが他の B2B クラスと概念的にどのように関連し、Real-Time CDP UI でこれらの関係を確立する方法については、[Adobe Experience Platform B2B Edition のスキーマの関係 ](../../tutorials/relationship-b2b.md) に関するガイドを参照してください。
