---
title: XDM ビジネスキャンペーン詳細スキーマフィールドグループ
description: このドキュメントでは、「XDM ビジネスキャンペーン詳細」スキーマフィールドグループの概要を説明します。
exl-id: 3ef6c0b9-cba1-449e-8868-46446c00465f
source-git-commit: 7805e4c45a48070adefbaba25a57140efc3e86b1
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 8%

---

# [!UICONTROL XDM ビジネスキャンペーンの詳細] スキーマフィールドグループ

[!UICONTROL XDM ビジネスキャンペーンの詳細] は、 [[!UICONTROL XDM ビジネスキャンペーン] クラス](../../classes/b2b/business-campaign.md)：ビジネスキャンペーンに関する詳細情報をキャプチャします。

![UI に表示される XDM ビジネスキャンペーン詳細フィールドグループの構造](../../images/field-groups/b2b/business-campaign-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `actualCost` | [[!UICONTROL 通貨]](../../data-types/currency.md) | ビジネスキャンペーンの実際のコストを表します。 |
| `budgetedCost` | [[!UICONTROL 通貨]](../../data-types/currency.md) | ビジネスキャンペーンの予算コストを表します。 |
| `expectedRevenue` | [[!UICONTROL 通貨]](../../data-types/currency.md) | ビジネスキャンペーンが生み出す売上高を表します。 |
| `parentCampaignKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 親キャンペーンの複合 ID（該当する場合）。 |
| `campaignEndDate` | [!UICONTROL 日時] | キャンペーンが終了した時点または終了する時点を示す ISO 8601 タイムスタンプ。 |
| `campaignProgressionName` | [!UICONTROL 文字列] | キャンペーンの進行名。 |
| `campaignStartDate` | [!UICONTROL 日時] | キャンペーンが開始した日時、またはキャンペーンが開始する時刻の ISO 8601 タイムスタンプ。 |
| `campaignStatus` | [!UICONTROL 文字列] | キャンペーンの現在のステータス。 |
| `channelName` | [!UICONTROL 文字列] | このキャンペーンに関連付けられたチャネルの名前。 |
| `expectedResponse` | [!UICONTROL 文字列] | キャンペーンに対して期待される応答。 |
| `integrationPartnerName` | [!UICONTROL 文字列] | このキャンペーンと統合したパートナーの名前。 |
| `isActive` | [!UICONTROL ブール値] | このキャンペーンがアクティブかどうかを示します。 |
| `isDeleted` | [!UICONTROL ブール値] | このキャンペーンがMarketo Engageで削除されたかどうかを示します。<br><br>を使用する場合、 [Marketoソースコネクタ](../../../sources/connectors/adobe-applications/marketo/marketo.md)を指定した場合、Marketoで削除されたレコードは自動的にリアルタイム顧客プロファイルに反映されます。 ただし、これらのプロファイルに関連するレコードは、データレイク内で引き続き保持される場合があります。 設定別 `isDeleted` から `true`の場合は、フィールドを使用して、データレイクに対するクエリを実行する際に、ソースから削除されたレコードを除外できます。 |
| `lastActivityDate` | [!UICONTROL 日時] | キャンペーンに関連付けられた最後のアクティビティのタイムスタンプ（ISO 8601 形式）。 |
| `timeZone` | [!UICONTROL 文字列] | キャンペーンのタイムゾーン。 |
| `timeZoneDelivery` | [!UICONTROL 文字列] | キャンペーンが運用される配信タイムゾーン。 |
| `timeZoneName` | [!UICONTROL 文字列] | キャンペーンが運用されるタイムゾーンの名前。 |
| `webinarHistorySyncDate` | [!UICONTROL 日時] | このキャンペーンの最後のウェビナー履歴同期の ISO 8601 タイムスタンプ。 |
| `webinarHistorySyncStatus` | [!UICONTROL 文字列] | このキャンペーンのウェビナー履歴同期のステータス。 |
| `webinarSessionDescription` | [!UICONTROL 文字列] | このキャンペーンに関連付けられたウェビナーセッションの説明。 |
| `webinarSessionName` | [!UICONTROL 文字列] | このキャンペーンに関連付けられたウェビナーセッションの名前。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/campaign/campaign-details.schema.json).
