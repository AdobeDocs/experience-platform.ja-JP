---
title: XDM ビジネスキャンペーン詳細スキーマフィールドグループ
description: XDM ビジネスキャンペーン詳細スキーマフィールドグループについて説明します。
exl-id: 3ef6c0b9-cba1-449e-8868-46446c00465f
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 5%

---

# [!UICONTROL XDM ビジネスキャンペーン詳細 &#x200B;] スキーマフィールドグループ

[!UICONTROL XDM ビジネスキャンペーン詳細 &#x200B;] は、[[!UICONTROL XDM ビジネスキャンペーン &#x200B;] クラス &#x200B;](../../classes/b2b/business-campaign.md) の標準スキーマフィールドグループで、ビジネスキャンペーンに関する詳細情報をキャプチャするものです。

![UI に表示される XDM ビジネスキャンペーン詳細フィールドグループの構造 &#x200B;](../../images/field-groups/b2b/business-campaign-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `actualCost` | [[!UICONTROL 通貨]](../../data-types/currency.md) | ビジネスキャンペーンの実際のコストを表します。 |
| `budgetedCost` | [[!UICONTROL 通貨]](../../data-types/currency.md) | ビジネスキャンペーンの予算コストを表します。 |
| `expectedRevenue` | [[!UICONTROL 通貨]](../../data-types/currency.md) | ビジネスキャンペーンによって生み出されると予想される売上高を表します。 |
| `parentCampaignKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | 親キャンペーンの複合 ID （該当する場合）。 |
| `campaignEndDate` | [!UICONTROL &#x200B; 日時 &#x200B;] | キャンペーンが終了または終了する時点の ISO 8601 タイムスタンプ。 |
| `campaignProgressionName` | [!UICONTROL 文字列] | キャンペーンの進行名。 |
| `campaignStartDate` | [!UICONTROL &#x200B; 日時 &#x200B;] | キャンペーンが開始または開始される時点の ISO 8601 タイムスタンプ。 |
| `campaignStatus` | [!UICONTROL 文字列] | キャンペーンの現在のステータス。 |
| `channelName` | [!UICONTROL 文字列] | このキャンペーンに関連付けられているチャネルの名前。 |
| `expectedResponse` | [!UICONTROL 文字列] | キャンペーンに対して期待される応答。 |
| `integrationPartnerName` | [!UICONTROL 文字列] | このキャンペーンと統合しているパートナーの名前。 |
| `isActive` | [!UICONTROL &#x200B; ブール値 &#x200B;] | このキャンペーンがアクティブかどうかを示します。 |
| `isDeleted` | [!UICONTROL &#x200B; ブール値 &#x200B;] | このキャンペーンがMarketo Engageで削除されているかどうかを示します。<br><br>[Marketo ソースコネクタを使用 &#x200B;](../../../sources/connectors/adobe-applications/marketo/marketo.md) すると、Marketoで削除されたすべてのレコードがリアルタイム顧客プロファイルに自動的に反映されます。 ただし、これらのプロファイルに関連するレコードは、引き続きデータレイクに保持される場合があります。 `isDeleted` を `true` に設定すると、フィールドを使用して、データレイクのクエリ時にソースから削除されたレコードをフィルターで除外できます。 |
| `lastActivityDate` | [!UICONTROL &#x200B; 日時 &#x200B;] | キャンペーンに関連付けられた最後のアクティビティの ISO 8601 タイムスタンプ。 |
| `timeZone` | [!UICONTROL 文字列] | キャンペーンが動作するタイムゾーン。 |
| `timeZoneDelivery` | [!UICONTROL 文字列] | キャンペーンが動作する配信タイムゾーン。 |
| `timeZoneName` | [!UICONTROL 文字列] | キャンペーンが動作するタイムゾーンの名前。 |
| `webinarHistorySyncDate` | [!UICONTROL &#x200B; 日時 &#x200B;] | このキャンペーンの最後のウェビナー履歴同期の ISO 8601 タイムスタンプ。 |
| `webinarHistorySyncStatus` | [!UICONTROL 文字列] | このキャンペーンのウェビナー履歴同期のステータス。 |
| `webinarSessionDescription` | [!UICONTROL 文字列] | このキャンペーンに関連付けられたウェビナーセッションの説明。 |
| `webinarSessionName` | [!UICONTROL 文字列] | このキャンペーンに関連付けられているウェビナーセッションの名前。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[&#x200B; 公開 XDM リポジトリ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/campaign/campaign-details.schema.json) を参照してください。
