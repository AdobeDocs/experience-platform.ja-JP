---
title: XDM ビジネスキャンペーンメンバー詳細スキーマフィールドグループ
description: このドキュメントでは、「XDM ビジネスキャンペーンメンバー詳細」スキーマフィールドグループの概要を説明します。
exl-id: 597629c8-7f41-4c1c-95b6-aed5e16cee72
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 6%

---

# [!UICONTROL XDM ビジネスキャンペーンメンバーの詳細] スキーマフィールドグループ

[!UICONTROL XDM ビジネスキャンペーンメンバーの詳細] は、 [[!UICONTROL XDM ビジネスキャンペーンメンバー] クラス](../../classes/b2b/business-campaign-members.md)：ビジネスキャンペーンに関する詳細情報をキャプチャします。

![UI に表示される XDM ビジネスキャンペーンメンバー詳細フィールドグループの構造](../../images/field-groups/b2b/business-campaign-member-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `acquiredByCampaignKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | このキャンペーンメンバーを取得したキャンペーンの複合 ID。 |
| `acquiredByCampaignID` | [!UICONTROL 文字列] | このキャンペーンメンバーを取得したキャンペーンの文字列識別子。 |
| `firstRespondedDate` | [!UICONTROL 日時] | 人物が最初にキャンペーンに応答した時点を示す ISO 8601 タイムスタンプ。 |
| `hasReachedSuccess` | [!UICONTROL ブール値] | このキャンペーンメンバーがコンバージョンに成功したかどうかを示します。 |
| `hasResponded` | [!UICONTROL ブール値] | このキャンペーンメンバーがキャンペーンに応答したかどうかを示します。 |
| `isDeleted` | [!UICONTROL ブール値] | このキャンペーンメンバーがMarketo Engageで削除されたかどうかを示します。<br><br>を使用する場合、 [Marketoソースコネクタ](../../../sources/connectors/adobe-applications/marketo/marketo.md)を指定した場合、Marketoで削除されたレコードは自動的にリアルタイム顧客プロファイルに反映されます。 ただし、これらのプロファイルに関連するレコードは、データレイク内で引き続き保持される場合があります。 設定別 `isDeleted` から `true`の場合は、フィールドを使用して、データレイクに対するクエリを実行する際に、ソースから削除されたレコードを除外できます。 |
| `isExhausted` | [!UICONTROL ブール値] | このキャンペーンメンバーがすべてのキャンペーンインタラクションを消費したかどうかを示します。 |
| `lastStatus` | [!UICONTROL 文字列] | キャンペーンメンバーの最後のステータス。 |
| `memberStatus` | [!UICONTROL 文字列] | キャンペーンメンバーの現在のステータス。 |
| `memberStatusReason` | [!UICONTROL 文字列] | キャンペーンメンバーの現在のステータスの背後にある理由。 |
| `membershipDate` | [!UICONTROL 日時] | キャンペーンメンバーの現在のステータスの背後にある理由。 |
| `nurtureCadence` | [!UICONTROL 文字列] | キャンペーンメンバーに提示するキャンペーン関連の情報のタイムケイデンス。 |
| `nurtureTrackName` | [!UICONTROL 文字列] | このキャンペーンメンバーの対象となる育成プログラムの名前。 |
| `reachedSuccessDate` | [!UICONTROL 日時] | キャンペーンメンバーに対して正常なコンバージョンがおこなわれた場合のの ISO 8601 タイムスタンプ。 |
| `webinarConfirmationUrl` | [!UICONTROL 文字列] | キャンペーンメンバーのウェビナー確認 URL。 |
| `webinarRegistrationID` | [!UICONTROL 文字列] | キャンペーンメンバーのウェビナー登録 ID。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/campaign-member/campaign-member-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/campaign-member/campaign-member-details.schema.json)
