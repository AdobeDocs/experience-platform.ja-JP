---
title: XDM ビジネスキャンペーンメンバー詳細スキーマフィールドグループ
description: XDM ビジネスキャンペーンメンバー詳細スキーマフィールドグループについて説明します。
exl-id: 597629c8-7f41-4c1c-95b6-aed5e16cee72
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 3%

---

# [!UICONTROL XDM ビジネスキャンペーンメンバー詳細 &#x200B;] スキーマフィールドグループ

[!UICONTROL XDM ビジネスキャンペーンメンバー詳細 &#x200B;] は、[[!UICONTROL XDM ビジネスキャンペーンメンバー &#x200B;] クラス &#x200B;](../../classes/b2b/business-campaign-members.md) の標準スキーマフィールドグループで、ビジネスキャンペーンに関する詳細情報をキャプチャします。

![UI に表示される XDM ビジネスキャンペーンメンバーの詳細フィールドグループの構造 &#x200B;](../../images/field-groups/b2b/business-campaign-member-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `acquiredByCampaignKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | このキャンペーンメンバーを取得したキャンペーンの複合 ID。 |
| `acquiredByCampaignID` | [!UICONTROL 文字列] | このキャンペーンメンバーを取得したキャンペーンの文字列識別子。 |
| `firstRespondedDate` | [!UICONTROL &#x200B; 日時 &#x200B;] | 人物が最初にキャンペーンに応答した際の ISO 8601 タイムスタンプ。 |
| `hasReachedSuccess` | [!UICONTROL &#x200B; ブール値 &#x200B;] | このキャンペーンメンバーが成功したコンバージョンにつながったかどうかを示します。 |
| `hasResponded` | [!UICONTROL &#x200B; ブール値 &#x200B;] | このキャンペーンメンバーがキャンペーンに応答したかどうかを示します。 |
| `isDeleted` | [!UICONTROL &#x200B; ブール値 &#x200B;] | このキャンペーンメンバーがMarketo Engageで削除されたかどうかを示します。<br><br>[Marketo ソースコネクタを使用 &#x200B;](../../../sources/connectors/adobe-applications/marketo/marketo.md) すると、Marketoで削除されたすべてのレコードがリアルタイム顧客プロファイルに自動的に反映されます。 ただし、これらのプロファイルに関連するレコードは、引き続きデータレイクに保持される場合があります。 `isDeleted` を `true` に設定すると、フィールドを使用して、データレイクのクエリ時にソースから削除されたレコードをフィルターで除外できます。 |
| `isExhausted` | [!UICONTROL &#x200B; ブール値 &#x200B;] | このキャンペーンメンバーがすべてのキャンペーンインタラクションを使い果たしたかどうかを示します。 |
| `lastStatus` | [!UICONTROL 文字列] | キャンペーンメンバーの最新のステータス。 |
| `memberStatus` | [!UICONTROL 文字列] | キャンペーンメンバーの現在のステータス。 |
| `memberStatusReason` | [!UICONTROL 文字列] | キャンペーンメンバーの現在のステータスの背後にある理由。 |
| `membershipDate` | [!UICONTROL &#x200B; 日時 &#x200B;] | キャンペーンメンバーの現在のステータスの背後にある理由。 |
| `nurtureCadence` | [!UICONTROL 文字列] | キャンペーンメンバーに提示されるキャンペーン関連情報の頻度。 |
| `nurtureTrackName` | [!UICONTROL 文字列] | このキャンペーンメンバーが対象となる育成プログラムの名前。 |
| `reachedSuccessDate` | [!UICONTROL &#x200B; 日時 &#x200B;] | キャンペーンメンバーのコンバージョンが成功した時点の ISO 8601 タイムスタンプ。 |
| `webinarConfirmationUrl` | [!UICONTROL 文字列] | キャンペーンメンバーのウェビナー確認 URL。 |
| `webinarRegistrationID` | [!UICONTROL 文字列] | キャンペーンメンバーのウェビナー登録 ID。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/campaign-member/campaign-member-details.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/campaign-member/campaign-member-details.schema.json)
