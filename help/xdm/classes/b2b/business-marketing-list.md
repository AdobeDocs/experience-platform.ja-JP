---
title: XDM ビジネスマーケティングリストクラス
description: エクスペリエンスデータモデル（XDM）の XDM ビジネスマーケティングリストクラスについて説明します。
exl-id: 510c5608-054d-4bed-91eb-22d84b5dc625
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 3%

---

# [!UICONTROL XDM Business Marketing List] クラス

>[!IMPORTANT]
>
>このクラスは、[Adobe Real-time Customer Data Platform B2B Edition](../../../rtcdp/b2b-overview.md) にアクセスできる組織で使用することを目的としています。 このクラスを [ リアルタイム顧客プロファイル ](../../../profile/home.md) に加えるには、Real-Time CDP B2B Edition へのアクセス権が必要です。

[!UICONTROL XDM ビジネスマーケティングリスト &#x200B;] は、マーケティングリストに必要な最小限のプロパティをキャプチャする、標準のエクスペリエンスデータモデル（XDM）クラスです。マーケティングリストを使用すると、商品を購入する可能性が最も高い見込み客を優先することができます。

![UI に表示される XDM ビジネスマーケティングリストクラスの構造 ](../../images/classes/b2b/business-marketing-list.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL &#x200B; 外部Source システム監査属性 &#x200B;]](../../data-types/external-source-system-audit-attributes.md) | マーケティングリストのソースが外部システムである場合、このオブジェクトはそのシステムの監査属性を取得します。 |
| `marketingListKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | マーケティングリストエンティティの複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`marketingListID` とは別の、システムで生成された値です。 |
| `isDeleted` | ブール値 | このマーケティング リスト エンティティがMarketo Engageで削除されているかどうかを示します。<br><br>[Marketo ソースコネクタを使用 ](../../../sources/connectors/adobe-applications/marketo/marketo.md) すると、Marketoで削除されたすべてのレコードがリアルタイム顧客プロファイルに自動的に反映されます。 ただし、これらのプロファイルに関連するレコードは、引き続きデータレイクに保持される場合があります。 `isDeleted` を `true` に設定すると、フィールドを使用して、データレイクのクエリ時にソースから削除されたレコードをフィルターで除外できます。 |
| `marketingListDescription` | 文字列 | マーケティングリストの説明。 |
| `marketingListID` | 文字列 | マーケティングリストエンティティの一意の ID。 |
| `marketingListName` | 文字列 | マーケティングリストの名前。 |

{style="table-layout:auto"}

このクラスが他の B2B クラスと概念的にどのように関連し、Real-Time CDP UI でこれらの関係を確立する方法については、[Adobe Experience Platform B2B Edition のスキーマの関係 ](../../tutorials/relationship-b2b.md) に関するガイドを参照してください。
