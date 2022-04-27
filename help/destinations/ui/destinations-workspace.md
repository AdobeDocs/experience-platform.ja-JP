---
keywords: プラットフォーム；宛先；宛先ワークスペース；ワークスペース；ui；宛先 ui；カタログ；宛先カタログ；
title: 宛先ワークスペース
description: 「宛先」ワークスペースは、「カタログ」、「参照」、「アカウント」、「システム表示」の 4 つのセクションで構成されます。 以下の節で説明します。
seo-description: In Adobe Experience Platform, select Destinations from the left navigation bar to access the destinations workspace.
exl-id: 0f46f08d-0fe3-441d-933a-86bc146c0f19
source-git-commit: b275621d9c6552327e0e55c00c8fcf0397088168
workflow-type: tm+mt
source-wordcount: '1156'
ht-degree: 19%

---

# 宛先ワークスペース {#destinations-workspace}

Adobe Experience Platformで、 **[!UICONTROL 宛先]** 左側のナビゲーションバーから [!UICONTROL 宛先] ワークスペース。

この [!UICONTROL 宛先] ワークスペースは 5 つのセクションで構成されています。 [!UICONTROL 概要], [!UICONTROL カタログ], [!UICONTROL 参照], [!UICONTROL アカウント]、および [!UICONTROL システム表示]（以下の節で説明）

![宛先 — 概要](../assets/ui/workspace/destinations-overview.png)

## [!UICONTROL 概要] {#overview}

この **[!UICONTROL 概要]** タブには、 [!UICONTROL 宛先] ダッシュボードに表示され、組織の宛先データに関連する主要指標を指定できます。 詳しくは、 [[!UICONTROL 宛先] ダッシュボードガイド](../../dashboards/guides/destinations.md).

>[!NOTE]
>
>組織がExperience Platformを初めて使用し、まだアクティブな宛先がない場合、 [!UICONTROL 宛先] ダッシュボードと [!UICONTROL 概要] タブが表示されません。 代わりに、「 [!UICONTROL 宛先] 左のナビゲーションから、 [[!UICONTROL カタログ] タブ](#catalog).

![](../../dashboards/images/destinations/dashboard-overview.png)

## [!UICONTROL カタログ] {#catalog}

この **[!UICONTROL カタログ]** タブには、 [!DNL Platform]を使用して、にデータを送信できます。

この [!DNL Platform] ユーザーインターフェイスには、宛先カタログページに対して複数の検索およびフィルターオプションが用意されています。

* ページ上の検索機能を使用して、特定の宛先を見つけます。
* を使用した宛先のフィルタリング [!UICONTROL カテゴリ] コントロール。
* 切り替え [!UICONTROL すべての宛先] および [!UICONTROL 宛先]. 次を選択した場合： **[!UICONTROL すべての宛先]**，すべて利用可能 [!DNL Platform] の宛先が表示されます。 次を選択した場合： **[!UICONTROL 宛先]**&#x200B;を使用すると、接続を確立した宛先のみを表示できます。
* 選択して表示 **[!UICONTROL 接続]** および/または **[!UICONTROL 拡張機能]**. この 2 つのカテゴリの違いについては、 [宛先のタイプとカテゴリ](../destination-types.md).

![カタログ](../assets/ui/workspace/catalog.png)

宛先カードには、 **[!UICONTROL 設定]** または **[!UICONTROL セグメントのアクティブ化]** コントロールと、より多くのオプションを表示するセカンダリコントロール。 以下に、これらのコントロールについて説明します。

| 制御 | 説明 |
|---------|----------|
| [!UICONTROL 設定] | 宛先への接続を作成できます。 |
| [!UICONTROL セグメントのアクティブ化] | 宛先への接続を確立したら、セグメントをアクティブ化できます。 |
| [!UICONTROL アカウントを表示] | 宛先に接続したアカウントを表示します。 |
| [!UICONTROL データフローを表示] | 宛先に存在するデータのアクティベーションフローを表示します。 |
| [!UICONTROL ドキュメントを表示] | その特定の宛先のドキュメントページへのリンクを開きます。詳細情報や設定に役立ちます。 |

{style=&quot;table-layout:auto&quot;}

![宛先カード上のコントロール](../assets/ui/workspace/destination-card-options.png)

カタログ内の宛先カードを選択して、右側のパネルを開きます。 ここでは、宛先の説明を確認できます。 右側のレールには、上の表で説明したのと同じコントロールが表示されます。これには、宛先の説明、宛先のカテゴリおよびタイプが含まれます。

![宛先カタログオプション](../assets/ui/workspace/destination-right-rail.png)

宛先カテゴリと各宛先の情報について詳しくは、 [宛先カタログ](../catalog/overview.md) および [宛先のタイプとカテゴリ](../destination-types.md).

## [!UICONTROL アカウント] {#accounts}

この **[!UICONTROL アカウント]** 「 」タブには、様々な宛先との接続を確立した場合の詳細が表示され、既存のアカウントの詳細を更新または削除できます。 各宛先アカウントで取得できるすべての情報については、次の表を参照してください。

>[!TIP]
>
> * 次の点で 3 つの点を選択 [!UICONTROL Platform] 列を参照し、 ![「セグメントをアクティブ化」ボタン](../assets/ui/workspace/add-data-symbol.png)**[!UICONTROL セグメントのアクティブ化&#x200B;]**」ボタンをクリックして、その宛先にセグメントを送信します。
> * 次の点で 3 つの点を選択 [!UICONTROL Platform] 列を参照し、 ![「詳細を編集」ボタン](../assets/ui/workspace/pencil-icon.png)**[!UICONTROL 詳細を編集&#x200B;]**ボタン [更新](update-accounts.md) 既存の宛先アカウントの詳細。
> * 次の点で 3 つの点を選択 [!UICONTROL Platform] 列を参照し、 ![削除ボタン](../assets/ui/workspace/delete-destination-symbol.png)**[!UICONTROL 削除&#x200B;]**ボタン [削除](delete-destination-account.md) 既存の宛先アカウント。


![「アカウント」タブ](../assets/ui/workspace/destination-account-options.png)

| 要素 | 説明 |
|---|---|
| [!UICONTROL プラットフォーム] | 接続を設定した宛先。 |
| [!UICONTROL 接続タイプ] | ストレージバケットまたは宛先へのアカウント接続タイプを表します。 宛先に応じて、認証オプションは次のとおりです。 <ul><li>電子メールマーケティングの宛先の場合：S3、FTP、Azure Blob のいずれかです。</li><li>リアルタイム広告の宛先の場合：サーバー間</li><li>Amazon S3 クラウドストレージの宛先：アクセスキー </li><li>SFTP クラウドストレージの宛先：SFTP の基本認証</li><li>OAuth 1 または OAuth 2 認証</li><li>Bearer トークン認証</li></ul> |
| [!UICONTROL ユーザー名] | [「宛先の接続」ウィザード](../catalog/email-marketing/overview.md#connect-destination)で選択したユーザー名。 |
| [!UICONTROL 宛先] | 宛先に対して作成された基本情報と接続された、一意に成功した宛先データフローの数を表します。 |
| [!UICONTROL 認証済み] | この宛先への接続が承認された日付。 |

{style=&quot;table-layout:auto&quot;}

## [!UICONTROL 参照] {#browse}

「**[!UICONTROL 参照]**」タブには、接続を確立した宛先が表示されます。を使用した宛先 **[!UICONTROL 有効/無効]** 切り替えをオンにして、宛先をそれぞれアクティブまたは非アクティブに設定します。 また、「 」を選択すると、データのフロー先を表示することもできます **[!UICONTROL セグメント]** > **[!UICONTROL 参照]** 検査するセグメントを選択します。 次の表に、 [!UICONTROL 参照] タブ：

>[!TIP]
>
> * 次の点で 3 つの点を選択 [!UICONTROL 名前] 列を参照し、 ![「セグメントをアクティブ化」ボタン](../assets/ui/workspace/add-data-symbol.png)**[!UICONTROL セグメントのアクティブ化&#x200B;]**」ボタンをクリックして、その宛先にセグメントを送信します。
> * 次の点で 3 つの点を選択 [!UICONTROL 名前] 列を参照し、 ![削除ボタン](../assets/ui/workspace/delete-destination-symbol.png)**[!UICONTROL 削除&#x200B;]**ボタン [削除](delete-destinations.md) 宛先への既存の接続。
> * 次の点で 3 つの点を選択 [!UICONTROL 名前] 列を参照し、 ![監視ボタンで表示](../assets/ui/workspace/monitoring-icon.png)**[!UICONTROL 監視で表示&#x200B;]**ボタンをクリックして、この宛先のアクティベーション情報を表示します。 [監視ダッシュボード](/help/dataflows/ui/monitor-destinations.md#monitoring-destinations-dashboard).
> * 次の点で 3 つの点を選択 [!UICONTROL 名前] 列を参照し、 ![アラートを購読 ](../assets/ui/workspace/alerts-icon.png)**[!UICONTROL アラートを購読&#x200B;]**ボタンをクリックして、宛先のデータフローアラートに登録します。 アラートを購読して、フロー実行のステータス、成功または失敗に関するメッセージを受け取ることができます。 詳しくは、 [コンテキスト内宛先アラートを購読](alerts.md) 宛先のデータフローアラートの詳細については、を参照してください。


![「参照」タブ](../assets/ui/workspace/browse-tab.png)

| 要素 | 説明 |
|---------|----------|
| 名前 | この宛先へのアクティベーションフローに指定した名前。同じ列には、次の 2 つのコントロールが含まれます。 [!UICONTROL 有効化 ] および [!UICONTROL 宛先の削除]. |
| [!UICONTROL 前回のフロー実行ステータス] | 最後のデータフロー実行のステータス。 詳しくは、 [宛先の詳細を表示](destination-details-page.md) データフローの実行の詳細を参照してください。 |
| [!UICONTROL 前回のフロー実行日] | 最後のデータフロー実行が発生した日時。 詳しくは、 [宛先の詳細を表示](destination-details-page.md) データフローの実行の詳細を参照してください。 |
| [!UICONTROL 宛先] | アクティベーションフローに対して選択した宛先プラットフォームです。 |
| [!UICONTROL 接続タイプ] | ストレージバケットまたは宛先への接続タイプを表します。 <ul><li>電子メールマーケティングの宛先の場合：S3、FTP、または [!DNL Azure Blob].</li><li>リアルタイム広告の宛先の場合：サーバー間.</li><li>ストリーミング先の場合：可能 [!DNL Azure Event Hubs] または [!DNL Amazon Kinesis].</li></ul> |
| [!UICONTROL ユーザー名] | 宛先フローに対して選択したアカウント資格情報。 |
| [!UICONTROL アクティベーションデータ] | この宛先に対してアクティブ化されるセグメントの数を示します。 このコントロールを選択して、アクティブ化されたセグメントの詳細を確認します。 参照： [アクティベーションデータ](/help/destinations/ui/destination-details-page.md#activation-data) 宛先の詳細ページで、アクティブ化されたセグメントに関する詳細を参照してください。 |
| [!UICONTROL 作成] | 宛先へのアクティベーションフローが作成された日時（UTC 時間）。アクティベーションフローを新しい順または古い順に並べ替えるには、上向き/下向き矢印記号を選択します。 |
| [!UICONTROL ステータス] | `Enabled` または `Disabled`。データがこの宛先に対してアクティブ化されているかどうかを示します。 |

目的の行をクリックすると、目的の行に関する詳細情報が右側のレールに表示されます。

![宛先行をクリック](../assets/ui/workspace/click-destination-row.png)

宛先名を選択して、この宛先に対してアクティブ化されたセグメントに関する情報を表示します。「**[!UICONTROL アクティベーションの編集]**」をクリックして、この宛先に送信されるセグメントを変更または追加します。

## [!UICONTROL システム表示] {#system-view}

この **[!UICONTROL システム表示]** 「 」タブには、Adobe Experience Platformで設定したアクティベーションフローの図が表示されます。

![Data-flows1](../assets/ui/workspace/data-flows1.png)

ページに表示される任意の宛先を選択し、「 **[!UICONTROL データフローを表示]** を参照して、各宛先に設定したすべての接続に関する情報を確認します。

![Data-flows2](../assets/ui/workspace/data-flows2.png)
