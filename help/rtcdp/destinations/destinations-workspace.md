---
title: 宛先ワークスペース
seo-title: 宛先ワークスペース
description: アドビのリアルタイム顧客データプラットフォームで、左側のナビゲーションバーから「宛先」を選択して、宛先ワークスペースにアクセスします。
seo-description: アドビのリアルタイム顧客データプラットフォームで、左側のナビゲーションバーから「宛先」を選択して、宛先ワークスペースにアクセスします。
translation-type: tm+mt
source-git-commit: e87ddff936da88b1b2b3cf71c2d6c24ed28b39ab

---


# 宛先ワークスペース {#destinations-workspace}

アドビのリアルタイム顧客データプラットフォームで、左側のナビゲーションバーから「**宛先**」を選択して、宛先ワークスペースにアクセスします。

The Destinations workspace consists of four sections, **Catalog**, **Browse**, **Accounts**, and **System View**, which are described in the sections below.

![宛先 — 概要](/help/rtcdp/destinations/assets/destinations-overview.png)

## カタログ {#catalog}

The **[!UICONTROL Catalog]** tab displays a list of all destinations offered by Adobe, that you can send data to.

ページ上の検索機能を使用して、特定のリンク先を見つけたり、コントロールを使用してリンク先をフィルターし **[!UICONTROL Categories]** たりします。

カタログ内の宛先を選択して、右側のレールを開きます。Here, you can set up a connection to the destination (**Connect destination**), view existing destination connections (**Browse destinations**) or learn more detailed information about each destination by viewing the documentation (**View documentation**).

![宛先カタログオプション](/help/rtcdp/destinations/assets/destination-ui-catalog-options.png)

宛先カテゴリと各宛先の情報について詳しくは、[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)を参照してください。

## 参照 {#browse}

The **[!UICONTROL Browse]** tab displays the destinations with which you have established a connection. Destinations with the **enabled** toggle turned on set the destination to active and vice-versa. また、**セグメント／参照**&#x200B;を選択し、検査するセグメントを選択すると、データのフロー先を表示することもできます。「参照」タブで各宛先に対して提供されるすべての情報については、次の表を参照してください。

![「参照」タブ](/help/rtcdp/destinations/assets/browse-tab.png)

| 要素 | 説明 |
---------|----------
| 名前 | この宛先へのアクティベーションフローに指定した名前。 |
| 宛先 | アクティベーションフローに対して選択した宛先プラットフォームです。 |
| 接続タイプ | ストレージバケットまたは宛先への接続タイプを表します。 <ul><li>電子メールマーケティングの宛先の場合：S3またはFTPを使用できます。</li><li>リアルタイム広告の宛先の場合：サーバー間</li></ul> |
| ユーザー名 | 宛先フローに対して選択したアカウント資格情報。 |
| セグメント | この宛先に対してアクティブ化されているセグメントの数。 |
| 作成 | 宛先へのアクティベーションフローが作成された日時（UTC 時間）です。 |
| ステータス | `Active` または `Inactive`.データが現在この宛先に対してアクティブ化されているかどうかを示します。ステータスを編集するには、「[アクティベーションの無効化](/help/rtcdp/destinations/activate-destinations.md#disable-activation)」を参照してください。 |

目的の行をクリックすると、目的の行に関する詳細情報が右側のレールに表示されます。

![宛先行をクリック](/help/rtcdp/destinations/assets/click-destination-row.png)

宛先名を選択して、この宛先に対してアクティブ化されたセグメントに関する情報を表示します。Click **[!UICONTROL Edit activation]** to modify or add to the segments that are being sent to this destination.

## アカウント {#accounts}

In the **[!UICONTROL Accounts]** tab, you can learn more about the connections that you have established with various destinations. 各宛先について取得できるすべての情報については、次の表を参照してください。

![「アカウント」タブ](/help/rtcdp/destinations/assets/accounts-tab.png)

| 要素 | 説明 |
---------|----------
| プラットフォーム | 接続を設定した宛先。 |
| 接続タイプ | ストレージバケットまたは宛先への接続タイプを表します。 <ul><li>電子メールマーケティングの宛先の場合：S3またはFTPを使用できます。</li><li>リアルタイム広告の宛先の場合：サーバー間</li><li>Amazon S3クラウドストレージの場合：アクセスキー </li><li>SFTPクラウドストレージの場合：SFTPの基本認証</li></ul> |
| ユーザー名 | [「宛先の接続」ウィザード](/help/rtcdp/destinations/email-marketing-destinations.md#connect-destination)で選択したユーザー名。 |
| データフロー | 宛先に対して作成された基本情報に接続された、一意に成功した宛先フローの数を表します。 |
| 認証済み | この宛先への接続が承認された日付。 |
| ステータス | `Active` または `Inactive`.データが現在この宛先に対してアクティブ化されているかどうかを示します。ステータスを編集するには、「[アクティベーションの無効化](/help/rtcdp/destinations/activate-destinations.md#disable-activation)」を参照してください。 |

## システムビュー {#system-view}

The **[!UICONTROL System View]** tab displays a graphic representation of the activation flows that you have set up in the Real-time Customer Data Platform.

![Data-flows1](/help/rtcdp/destinations/assets/data-flows1.png)

Select any of the destinations displayed on the page and press **[!UICONTROL View flows]** to see information on all the connections you have set up for each destination.

![Data-flows2](/help/rtcdp/destinations/assets/data-flows2.png)