---
title: 宛先ワークスペース
seo-title: 宛先ワークスペース
description: アドビのリアルタイム顧客データプラットフォームで、左側のナビゲーションバーから「宛先」を選択して、宛先ワークスペースにアクセスします。
seo-description: アドビのリアルタイム顧客データプラットフォームで、左側のナビゲーションバーから「宛先」を選択して、宛先ワークスペースにアクセスします。
translation-type: tm+mt
source-git-commit: 570c627672439a5ee0f4215b7bf7915ec3dd2bb3
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 96%

---


# 宛先ワークスペース {#destinations-workspace}

アドビのリアルタイム顧客データプラットフォームで、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択して、[!UICONTROL 宛先]ワークスペースにアクセスします。

[!UICONTROL 宛先]ワークスペースは、「**[!UICONTROL カタログ]**」、「**[!UICONTROL 参照]**」、「**[!UICONTROL アカウント]**」、「**[!UICONTROL システム表示]**」の 4 つのセクションで構成されます。これらは、以下の節で説明します。

![宛先 — 概要](/help/rtcdp/destinations/assets/destinations-overview.png)

## [!UICONTROL カタログ] {#catalog}

「**[!UICONTROL カタログ]**」タブには、アドビが提供するすべての宛先のリストが表示されます。このリストには、データを送信できます。

ページ上の検索機能を使用して、**[!UICONTROL カテゴリ]**&#x200B;コントロールを使用して、特定の宛先を見つけたり、宛先をフィルターしたりします。

カタログ内の宛先を選択して、右側のレールを開きます。ここでは、宛先への接続を設定する（**[!UICONTROL 宛先の接続]**）、既存の宛先の接続を表示する（**[!UICONTROL 宛先の閲覧]**）、ドキュメントを参照して各宛先に関する詳細情報を確認（**[!UICONTROL ドキュメントを表示]**）できます。

![宛先カタログオプション](/help/rtcdp/destinations/assets/destination-ui-catalog-options.png)

宛先カテゴリと各宛先の情報について詳しくは、[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)を参照してください。

## [!UICONTROL 参照] {#browse}

「**[!UICONTROL 参照]**」タブには、接続を確立した宛先が表示されます。切り替えを&#x200B;**[!UICONTROL 有効]**&#x200B;にした宛先はアクティブに設定し、逆も同様に設定します。You can also view the destinations where you have data flowing by selecting **[!UICONTROL Segments]** > **[!UICONTROL Browse]** and selecting a segment to inspect. 「参照」タブで各宛先に対して提供されるすべての情報については、次の表を参照してください。

![「参照」タブ](/help/rtcdp/destinations/assets/browse-tab.png)

| 要素 | 説明 |
---------|----------
| 名前 | この宛先へのアクティベーションフローに指定した名前。 |
| [!UICONTROL 宛先] | アクティベーションフローに対して選択した宛先プラットフォームです。 |
| [!UICONTROL 接続タイプ] | ストレージバケットまたは宛先への接続タイプを表します。 <ul><li>電子メールマーケティングの宛先の場合：S3 または FTP</li><li>リアルタイム広告の宛先の場合：サーバー間</li></ul> |
| [!UICONTROL ユーザー名] | 宛先フローに対して選択したアカウント資格情報。 |
| [!UICONTROL セグメント] | この宛先に対してアクティブ化されているセグメントの数。 |
| [!UICONTROL 作成] | 宛先へのアクティベーションフローが作成された日時（UTC 時間）。 |
| [!UICONTROL ステータス] | `Active` または `Inactive`.データが現在この宛先に対してアクティブ化されているかどうかを示します。ステータスを編集するには、「[アクティベーションの無効化](/help/rtcdp/destinations/activate-destinations.md#disable-activation)」を参照してください。 |

目的の行をクリックすると、目的の行に関する詳細情報が右側のレールに表示されます。

![宛先行をクリック](/help/rtcdp/destinations/assets/click-destination-row.png)

宛先名を選択して、この宛先に対してアクティブ化されたセグメントに関する情報を表示します。「**[!UICONTROL アクティベーションの編集]**」をクリックして、この宛先に送信されるセグメントを変更または追加します。

## [!UICONTROL アカウント] {#accounts}

「**[!UICONTROL アカウント]**」タブでは、様々な宛先との接続を確立した場合の詳細を確認できます。各宛先について取得できるすべての情報については、次の表を参照してください。

![「アカウント」タブ](/help/rtcdp/destinations/assets/accounts-tab.png)

| 要素 | 説明 |
---------|----------
| [!UICONTROL プラットフォーム] | 接続を設定した宛先。 |
| [!UICONTROL 接続タイプ] | ストレージバケットまたは宛先への接続タイプを表します。 <ul><li>電子メールマーケティングの宛先の場合：S3 または FTP</li><li>リアルタイム広告の宛先の場合：サーバー間</li><li>Amazon S3 クラウドストレージの宛先：アクセスキー </li><li>SFTP クラウドストレージの宛先：SFTP の基本認証</li></ul> |
| [!UICONTROL ユーザー名] | [「宛先の接続」ウィザード](/help/rtcdp/destinations/email-marketing-destinations.md#connect-destination)で選択したユーザー名。 |
| [!UICONTROL データフロー] | 宛先に対して作成された基本情報に接続された、一意に成功した宛先フローの数を表します。 |
| [!UICONTROL 認証済み] | この宛先への接続が承認された日付。 |
| [!UICONTROL ステータス] | `Active` または `Inactive`.データが現在この宛先に対してアクティブ化されているかどうかを示します。ステータスを編集するには、「[アクティベーションの無効化](/help/rtcdp/destinations/activate-destinations.md#disable-activation)」を参照してください。 |

## [!UICONTROL システム表示]{#system-view}

「**[!UICONTROL システム表示]**」タブには、リアルタイム顧客データプラットフォームで設定したアクティベーションフローの図が表示されます。

![Data-flows1](/help/rtcdp/destinations/assets/data-flows1.png)

ページに表示される任意の宛先を選択し、「**[!UICONTROL フローを表示]**」を押して、各宛先に設定したすべての接続に関する情報を表示します。

![Data-flows2](/help/rtcdp/destinations/assets/data-flows2.png)