---
title: XDM Business Opportunity クラス
description: エクスペリエンスデータモデル（XDM）の XDM Business Opportunity クラスについて説明します。
exl-id: d816b0f9-fd37-45da-aa55-247f7f662da0
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 3%

---

# [!UICONTROL XDM Business Opportunity] クラス

>[!IMPORTANT]
>
>このクラスは、[Adobe Real-time Customer Data Platform B2B Edition](../../../rtcdp/b2b-overview.md) にアクセスできる組織で使用することを目的としています。 このクラスを [ リアルタイム顧客プロファイル ](../../../profile/home.md) に加えるには、Real-Time CDP B2B Edition へのアクセス権が必要です。

[!UICONTROL XDM Business Opportunity] は、ビジネスオポチュニティに必要な最小限のプロパティをキャプチャする、標準のエクスペリエンスデータモデル（XDM）クラスです。

![UI に表示される XDM Business Opportunity クラスの構造 ](../../images/classes/b2b/business-opportunity.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | この機会が関連付けられているアカウントの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL  外部Source システム監査属性 ]](../../data-types/external-source-system-audit-attributes.md) | オポチュニティが外部ソースシステムから提供される場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `opportunityKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | 機会エンティティの複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`opportunityID` とは別の、システムで生成された値です。 |
| `accountID` | 文字列 | このオポチュニティが関連付けられているアカウントの一意の ID。 |
| `isDeleted` | ブール値 | このマーケティング リスト エンティティがMarketo Engageで削除されているかどうかを示します。<br><br>[Marketo ソースコネクタを使用 ](../../../sources/connectors/adobe-applications/marketo/marketo.md) すると、Marketoで削除されたすべてのレコードがリアルタイム顧客プロファイルに自動的に反映されます。 ただし、これらのプロファイルに関連するレコードは、引き続きデータレイクに保持される場合があります。 `isDeleted` を `true` に設定すると、フィールドを使用して、データレイクのクエリ時にソースから削除されたレコードをフィルターで除外できます。 |
| `opportunityDescription` | 文字列 | オポチュニティの説明。 |
| `opportunityID` | 文字列 | 商談エンティティの一意の ID。 |
| `opportunityName` | 文字列 | オポチュニティの名前。 |
| `opportunityStage` | 文字列 | オポチュニティの販売ステージ。 |
| `opportunityType` | 文字列 | オポチュニティのタイプ。 |

{style="table-layout:auto"}

このクラスが他の B2B クラスと概念的にどのように関連し、Real-Time CDP UI でこれらの関係を確立する方法については、[Adobe Experience Platform B2B Edition のスキーマの関係 ](../../tutorials/relationship-b2b.md) に関するガイドを参照してください。
