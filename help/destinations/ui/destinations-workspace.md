---
keywords: プラットフォーム；宛先；宛先ワークスペース；ワークスペース；ui；宛先ui;catalog；宛先カタログ；
title: 宛先ワークスペース
description: 宛先ワークスペースは、「カタログ」、「参照」、「アカウント」、「システム表示」の 4 つのセクションで構成されます。これらは、以下の節で説明します。
seo-description: Adobe Experience Platformで、左側のナビゲーションバーから「宛先」を選択し、宛先ワークスペースにアクセスします。
translation-type: tm+mt
source-git-commit: 709908196bb5df665c7e7df10dc58ee9f3b0edbf
workflow-type: tm+mt
source-wordcount: '1016'
ht-degree: 35%

---


# 宛先ワークスペース {#destinations-workspace}

## 概要 {#overview}

Adobe Experience Platformで、左のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択して、[!UICONTROL 宛先]ワークスペースにアクセスします。

[!UICONTROL 宛先]ワークスペースは、「[!UICONTROL カタログ]」、「[!UICONTROL 参照]」、「[!UICONTROL アカウント]」、「[!UICONTROL システム表示]」の 4 つのセクションで構成されます。これらは、以下の節で説明します。

![宛先 — 概要](../assets/ui/workspace/destinations-overview.png)

## [!UICONTROL カタログ] {#catalog}

「**[!UICONTROL カタログ]**」タブには、プラットフォームで使用可能なすべての宛先のリストが表示されます。この宛先には、データを送信できます。

プラットフォームユーザーインターフェイスは、宛先カタログページに多数の検索およびフィルターオプションを提供します。

* ページの検索機能を使用して、特定の行き先を見つけます。
* [!UICONTROL カテゴリ]コントロールを使用して宛先をフィルターします。
* [!UICONTROL すべての宛先]と[!UICONTROL 宛先]を切り替えます。 「**[!UICONTROL すべての宛先]**」が選択されている場合、使用可能なプラットフォームの宛先がすべて表示されます。 **[!UICONTROL 宛先]**&#x200B;を選択した場合は、接続を確立した宛先のみが表示されます。
* 表示&#x200B;**[!UICONTROL 接続]**&#x200B;または&#x200B;**[!UICONTROL 拡張子]**&#x200B;を選択します。 2つのカテゴリの違いについては、「[宛先の種類とカテゴリ](../destination-types.md)」を参照してください。

![リンク先のフィルタリングと検索デモ](../assets/ui/workspace/destinations-search-and-filter.gif)

宛先カードには、**[!UICONTROL Configure]**&#x200B;または&#x200B;**[!UICONTROL Activate]**&#x200B;コントロールと、より多くのオプションを表示するセカンダリコントロールが含まれます。 これらはすべて次のとおりです。

| 制御 | 説明 |
---------|----------
| [!UICONTROL 設定] | 宛先への接続を作成できます。 |
| [!UICONTROL アクティブ化] | 宛先への接続を確立すると、セグメントをアクティブ化できます。 |
| [!UICONTROL 表示勘定] | 宛先に接続したアカウントの表示。 |
| [!UICONTROL 表示データフロー] | 宛先に存在するデータアクティベーションフローの表示。 |
| [!UICONTROL 表示ドキュメント] | 特定のドキュメントページへのリンクを開き、そのドキュメントページの詳細と設定に役立ちます。 |

![宛先カードの制御](../assets/ui/workspace/destination-card-options.png)

カタログ内で目的のカードを選択し、右側のパネルを開きます。  ここで、宛先の説明を確認できます。 右側のレールには、上の表で説明したのと同じコントロール、宛先の説明、宛先のカテゴリとタイプの表示が表示されます。

![宛先カタログオプション](../assets/ui/workspace/destination-right-rail.png)

宛先カテゴリーと各宛先の情報について詳しくは、[宛先カタログ](../catalog/overview.md)および[宛先の種類とカテゴリ](../destination-types.md)を参照してください。

## [!UICONTROL アカウント] {#accounts}

「**[!UICONTROL アカウント]**」タブでは、様々な宛先との接続を確立した場合の詳細を確認できます。各宛先について取得できるすべての情報については、次の表を参照してください。

>[!TIP]
>
>**[!UICONTROL プラットフォーム]**&#x200B;列の追加![データボタン](../assets/ui/workspace/add-data-symbol.png)ボタンを使用して、そのアカウントの新しい宛先接続を作成します。

![「アカウント」タブ](../assets/ui/workspace/edit-account-destinations.png)

| 要素 | 説明 |
---------|----------
| [!UICONTROL プラットフォーム] | 接続を設定した宛先。 |
| [!UICONTROL 接続タイプ] | ストレージバケットまたは宛先への接続タイプを表します。 <ul><li>電子メールマーケティングの宛先の場合：S3 または FTP</li><li>リアルタイム広告の宛先の場合：サーバー間</li><li>Amazon S3 クラウドストレージの宛先：アクセスキー </li><li>SFTP クラウドストレージの宛先：SFTP の基本認証</li></ul> |
| [!UICONTROL ユーザー名] | [「宛先の接続」ウィザード](../catalog/email-marketing/overview.md#connect-destination)で選択したユーザー名。 |
| [!UICONTROL 宛先] | 宛先に対して作成された基本情報に接続された、一意に成功した宛先フローの数を表します。 |
| [!UICONTROL 認証済み] | この宛先への接続が承認された日付。 |

また、アカウント情報を編集または更新できます。 **[!UICONTROL プラットフォーム]**&#x200B;列の![アカウントの編集ボタン](../assets/ui/workspace/pencil-icon.png)を選択し、アカウントの情報を編集します。

`OAuth2`接続の種類を使用するアカウントの場合は、「**[!UICONTROL OAuthに再接続]**」を選択して、アカウントの資格情報を更新できます。

![OAUTHイメージ](../assets/ui/workspace/reconnect-oauth.png)

`Access Key`または`ConnectionString`の接続タイプを使用するアカウントの場合は、アクセスID、秘密鍵、接続文字列などの情報を含むアカウント認証情報を編集できます。

![アカウント情報の画像](../assets/ui/workspace/edit-account-details.png)

アカウントの詳細の編集が終了したら、「**[!UICONTROL 保存]**」を選択して更新を完了します。

## [!UICONTROL 参照] {#browse}

「**[!UICONTROL 参照]**」タブには、接続を確立した宛先が表示されます。**[!UICONTROL 「有効]**」の切り替えがオンになっている宛先は有効に設定され、逆も同様に設定されます。 **[!UICONTROL セグメント]**/**[!UICONTROL 参照]**&#x200B;を選択し、検査するセグメントを選択して、データの流れる宛先を表示することもできます。 「参照」タブで各宛先に対して提供されるすべての情報については、次の表を参照してください。

>[!TIP]
>
> * **[!UICONTROL 追加名前]**&#x200B;列の![セグメントボタン](../assets/ui/workspace/add-data-symbol.png)ボタンを使用して、その宛先に対する追加のセグメントをアクティブにします。
> * **[!UICONTROL 名前]**&#x200B;列の![宛先を削除ボタン](../assets/ui/workspace/delete-destination-symbol.png)を使用して、宛先への既存の接続を削除します。


![「参照」タブ](../assets/ui/workspace/browse-tab.png)

| 要素 | 説明 |
---------|----------
| 名前 | この宛先へのアクティベーションフローに指定した名前。同じ列には、次の2つのコントロールが含まれます。と[!UICONTROL 宛先]を削除します。 |
| 最後のフロー実行ステータス | 最後のデータフロー実行のステータス。 データフローの実行の詳細については、[表示宛先の詳細](destination-details-page.md)を参照してください。 |
| 最終フロー実行日 | 最後のデータフローが実行された日時。 データフローの実行の詳細については、[表示宛先の詳細](destination-details-page.md)を参照してください。 |
| [!UICONTROL 宛先] | アクティベーションフローに対して選択した宛先プラットフォームです。 |
| [!UICONTROL 接続タイプ] | ストレージバケットまたは宛先への接続タイプを表します。 <ul><li>電子メールマーケティングの宛先の場合：S3、FTP、または[!DNL Azure Blob]を指定できます。</li><li>リアルタイム広告の宛先の場合：サーバー間.</li><li>ストリーミング先の場合：[!DNL Azure Event Hubs]または[!DNL Amazon Kinesis]を指定できます。</li></ul> |
| [!UICONTROL ユーザー名] | 宛先フローに対して選択したアカウント資格情報。 |
| [!UICONTROL アクティベーションデータ] | この宛先に対してアクティブ化されているセグメントの数を示します。 このコントロールを選択すると、アクティブ化されたセグメントの詳細を確認できます。 アクティブ化されたセグメントの詳細については、宛先の詳細ページの[アクティベーションデータ](/help/destinations/ui/destination-details-page.md#activation-data)を参照してください。 |
| [!UICONTROL 作成] | 宛先へのアクティベーションフローが作成された日時（UTC 時間）。 |
| [!UICONTROL ステータス] | `Active` または `Inactive`.データが現在この宛先に対してアクティブ化されているかどうかを示します。ステータスを編集するには、「[アクティベーションの無効化](./activate-destinations.md#disable-activation)」を参照してください。 |

目的の行をクリックすると、目的の行に関する詳細情報が右側のレールに表示されます。

![宛先行をクリック](../assets/ui/workspace/click-destination-row.png)

宛先名を選択して、この宛先に対してアクティブ化されたセグメントに関する情報を表示します。「**[!UICONTROL アクティベーションの編集]**」をクリックして、この宛先に送信されるセグメントを変更または追加します。

## [!UICONTROL システム表示]{#system-view}

「**[!UICONTROL システム表示]**」タブには、Adobe Experience Platformで設定したアクティベーションフローの図が表示されます。

![Data-flows1](../assets/ui/workspace/data-flows1.png)

ページに表示されている宛先を選択し、**[!UICONTROL 表示フロー]**&#x200B;をクリックして、各宛先に対して設定したすべての接続に関する情報を表示します。

![Data-flows2](../assets/ui/workspace/data-flows2.png)