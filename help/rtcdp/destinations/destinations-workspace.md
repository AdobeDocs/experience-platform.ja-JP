---
title: 宛先ワークスペース
seo-title: 宛先ワークスペース
description: アドビのリアルタイム顧客データプラットフォームで、左側のナビゲーションバーから「宛先」を選択して、宛先ワークスペースにアクセスします。
seo-description: アドビのリアルタイム顧客データプラットフォームで、左側のナビゲーションバーから「宛先」を選択して、宛先ワークスペースにアクセスします。
translation-type: tm+mt
source-git-commit: 336aa90cf1e059a92a36dd0ef3222ef6a6f5123b

---


# 宛先ワークスペース {#destinations-workspace}

アドビのリアルタイム顧客データプラットフォームで、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択して、宛先ワークスペースにアクセスします。

The [!UICONTROL Destinations] workspace consists of four sections, **[!UICONTROL Catalog]**, **[!UICONTROL Browse]**, **[!UICONTROL Accounts]**, and **[!UICONTROL System View]**, which are described in the sections below.

![宛先 — 概要](/help/rtcdp/destinations/assets/destinations-overview.png)

## [!UICONTROL カタログ] {#catalog}

「**[!UICONTROL カタログ]**」タブには、アドビが提供するすべての宛先のリストが表示されます。このリストには、データを送信できます。

ページ上の検索機能を使用して、 **[!UICONTROL カテゴリコントロールを使用して、特定の宛先を見つけたり、宛先をフィルターしたりします]** 。

カタログ内の宛先を選択して、右側のレールを開きます。Here, you can set up a connection to the destination (**[!UICONTROL Connect destination]**), view existing destination connections (**[!UICONTROL Browse destinations]**) or learn more detailed information about each destination by viewing the documentation (**[!UICONTROL View documentation]**).

![宛先カタログオプション](/help/rtcdp/destinations/assets/destination-ui-catalog-options.png)

宛先カテゴリと各宛先の情報について詳しくは、[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)を参照してください。

## [!UICONTROL 参照] {#browse}

「**[!UICONTROL 参照]**」タブには、接続を確立した宛先が表示されます。Destinations with the **[!UICONTROL enabled]** toggle turned on set the destination to active and vice-versa. また、**[!UICONTROL セグメント／参照]**&#x200B;を選択し、検査するセグメントを選択すると、データのフロー先を表示することもできます。「参照」タブで各宛先に対して提供されるすべての情報については、次の表を参照してください。

![「参照」タブ](/help/rtcdp/destinations/assets/browse-tab.png)

| 要素 | 説明 |
---------|----------
| 名前 | この宛先へのアクティベーションフローに指定した名前。 |
| [!UICONTROL 宛先] | アクティベーションフローに対して選択した宛先プラットフォームです。 |
| [!UICONTROL 接続タイプ] | 接続グループまたは接続先へのストレージの種類を表します。 <ul><li>電子メールマーケティングの宛先の場合：S3またはFTPを使用できます。</li><li>リアルタイム広告の宛先の場合：サーバー間</li></ul> |
| [!UICONTROL ユーザー名] | 宛先フローに対して選択したアカウント資格情報。 |
| [!UICONTROL セグメント] | この宛先に対してアクティブ化されているセグメントの数。 |
| [!UICONTROL 作成] | 宛先へのアクティベーションフローが作成された日時（UTC 時間）です。 |
| [!UICONTROL Status] | `Active` または `Inactive`.データが現在この宛先に対してアクティブ化されているかどうかを示します。ステータスを編集するには、「[アクティベーションの無効化](/help/rtcdp/destinations/activate-destinations.md#disable-activation)」を参照してください。 |

目的の行をクリックすると、目的の行に関する詳細情報が右側のレールに表示されます。

![宛先行をクリック](/help/rtcdp/destinations/assets/click-destination-row.png)

宛先名を選択して、この宛先に対してアクティブ化されたセグメントに関する情報を表示します。「**[!UICONTROL アクティベーションの編集]**」をクリックして、この宛先に送信されるセグメントを変更または追加します。

## [!UICONTROL アカウント] {#accounts}

「**[!UICONTROL アカウント]**」タブでは、様々な宛先との接続を確立した場合の詳細を確認できます。各宛先について取得できるすべての情報については、次の表を参照してください。

![「アカウント」タブ](/help/rtcdp/destinations/assets/accounts-tab.png)

| 要素 | 説明 |
---------|----------
| [!UICONTROL プラットフォーム] | 接続を設定した宛先。 |
| [!UICONTROL 接続タイプ] | 接続グループまたは接続先へのストレージの種類を表します。 <ul><li>電子メールマーケティングの宛先の場合：S3またはFTPを使用できます。</li><li>リアルタイム広告の宛先の場合：サーバー間</li><li>Amazon S3クラウドのストレージ先：アクセスキー </li><li>SFTPクラウドのストレージ先：SFTPの基本認証</li></ul> |
| [!UICONTROL ユーザー名] | [「宛先の接続」ウィザード](/help/rtcdp/destinations/email-marketing-destinations.md#connect-destination)で選択したユーザー名。 |
| [!UICONTROL データフロー] | 宛先に対して作成された基本情報に接続された、一意に成功した宛先フローの数を表します。 |
| [!UICONTROL 認証済み] | この宛先への接続が承認された日付。 |
| [!UICONTROL Status] | `Active` または `Inactive`.データが現在この宛先に対してアクティブ化されているかどうかを示します。ステータスを編集するには、「[アクティベーションの無効化](/help/rtcdp/destinations/activate-destinations.md#disable-activation)」を参照してください。 |

## [!UICONTROL システム表示]{#system-view}

The **[!UICONTROL System View]** tab displays a graphic representation of the activation flows that you have set up in the Real-time Customer Data Platform.

![Data-flows1](/help/rtcdp/destinations/assets/data-flows1.png)

Select any of the destinations displayed on the page and press **[!UICONTROL View flows]** to see information on all the connections you have set up for each destination.

![Data-flows2](/help/rtcdp/destinations/assets/data-flows2.png)