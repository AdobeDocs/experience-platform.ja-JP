---
keywords: Platform;宛先;宛先ワークスペース;ワークスペース;UI;宛先UI;カタログ;宛先カタログ;
title: 宛先ワークスペース
description: 宛先ワークスペースは、「概要」、「カタログ」、「参照」、「アカウント」、「システム表示」の 5 つのセクションで構成されます。 以下の節で説明します。
exl-id: 0f46f08d-0fe3-441d-933a-86bc146c0f19
source-git-commit: 69e1f065cb3b302c4b144f39c84179075379f648
workflow-type: tm+mt
source-wordcount: '1217'
ht-degree: 100%

---

# 宛先ワークスペース {#destinations-workspace}

Adobe Experience Platform で、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択して、[!UICONTROL 宛先]ワークスペースにアクセスします。

[!UICONTROL 宛先]ワークスペースは、「[!UICONTROL 概要]」、「[!UICONTROL カタログ]」、 「[!UICONTROL 参照]」、「[!UICONTROL アカウント]」、「[!UICONTROL システム表示]」の 5 つのセクションで構成されます。これらは、以下の節で説明します。

![3 つのウィジェットが表示されている宛先の概要ダッシュボード。](../assets/ui/workspace/destinations-overview.png)

## [!UICONTROL 概要] {#overview}

「**[!UICONTROL 概要]**」タブには、[!UICONTROL 宛先]ダッシュボードが表示され、組織の宛先データに関連する主要指標が提供されます。詳しくは、[[!UICONTROL 宛先]ダッシュボードガイド](../../dashboards/guides/destinations.md)を参照してください。

>[!NOTE]
>
>Experience Platform を初めて使用し、まだアクティブな宛先がない組織の場合、[!UICONTROL 宛先]ダッシュボードと「[!UICONTROL 概要]」タブは表示されません。 代わりに、左側のナビゲーションから「[!UICONTROL 宛先]」を選択すると、「[[!UICONTROL カタログ]」タブ](#catalog)が表示されます

![宛先ダッシュボードの「概要」タブ。](../../dashboards/images/destinations/dashboard-overview.png)

## [!UICONTROL カタログ] {#catalog}

「**[!UICONTROL カタログ]**」タブには、データを送信できる [!DNL Platform] で使用可能なすべての宛先のリストが表示されます。

[!DNL Platform] ユーザーインターフェイスには、宛先カタログページに対して複数の検索およびフィルターオプションが用意されています。

* ページの検索機能を使用して、特定の宛先を見つけます。
* [!UICONTROL カテゴリ]コントロールを使用した宛先のフィルタリング。
* [!UICONTROL すべての宛先]と[!UICONTROL 宛先]を切り替えます。**[!UICONTROL すべての宛先]**&#x200B;を選択すると、使用可能なすべての [!DNL Platform] の宛先が表示されます。 **[!UICONTROL 宛先]**&#x200B;を選択すると、接続を確立した宛先のみを表示できます。
* 選択して、**[!UICONTROL 接続]**&#x200B;および／または&#x200B;**[!UICONTROL 拡張機能]**&#x200B;のタイプを表示します。 2 つのカテゴリの違いを理解するには、[宛先のタイプとカテゴリ](../destination-types.md)を参照してください。

![広告およびクラウドストレージの宛先をいくつか表示している宛先カタログ。](../assets/ui/workspace/catalog.png)

宛先カードには、プライマリとセカンダリのコントロールオプションが含まれます。 プライマリ制御には、「[!UICONTROL 設定]」、「[!UICONTROL アクティブ化]」、「[!UICONTROL セグメントのアクティブ化]」または「[!UICONTROL データセットを書き出し]」が含まれます。 セカンダリ制御を使用すると、オプションを表示できます。 これらの制御については、以下で説明します。

| コントロール | 説明 |
|---------|----------|
| [!UICONTROL 設定] | 宛先への接続を作成できます。 |
| [!UICONTROL アクティブ化] | 宛先への接続を確立したら、セグメントをアクティブ化したり、この宛先にデータセットを書き出したりできます。 |
| [!UICONTROL セグメントのアクティブ化] | 宛先への接続を確立したら、この宛先へのセグメントをアクティブ化できます。 |
| [!UICONTROL データセットを書き出し] | 宛先への接続を確立したら、この宛先にデータセットを書き出すことができます。 |
| [!UICONTROL アカウントを表示] | 宛先に接続したアカウントを表示します。 |
| [!UICONTROL データフローを表示] | 宛先に存在するデータのアクティベーションフローを表示します。 |
| [!UICONTROL ドキュメントを表示] | その特定の宛先のドキュメントページへのリンクを開きます。詳細の確認や設定に役立ちます。 |

{style="table-layout:auto"}

![宛先カード上のコントロール](../assets/ui/workspace/destination-card-options.png)

カタログ内の宛先カードを選択して、右側のパネルを開きます。ここでは、宛先の説明を確認できます。 右側のパネルには、上の表で説明したのと同じコントロールが表示されます。これには、宛先の説明、宛先のカテゴリおよびタイプが含まれます。

![宛先カタログオプション](../assets/ui/workspace/destination-right-rail.png)

宛先カテゴリと各宛先の情報について詳しくは、[宛先カタログ](../catalog/overview.md)と[宛先のタイプとカテゴリ](../destination-types.md)を参照してください。

## [!UICONTROL アカウント] {#accounts}

「**[!UICONTROL アカウント]**」タブには、様々な宛先との接続を確立した場合の詳細が表示され、既存のアカウントの詳細を更新または削除できます。 各宛先のアカウントについて取得できるすべての情報については、次の表を参照してください。

>[!TIP]
>
> * [!UICONTROL プラットフォーム]列の省略記号（`...`）を選択し、![コントロールのアクティブ化](../assets/ui/workspace/add-data-symbol.png)**[!UICONTROL アクティブ化&#x200B;]**／**[!UICONTROL &#x200B;セグメントのアクティブ化&#x200B;]**／**[!UICONTROL &#x200B;データセットを書き出し&#x200B;]**のコントロールを使用して、セグメントやデータセットをその宛先に書き出すことができます。
> * [!UICONTROL プラットフォーム]列の省略記号（`...`）を選択し、![詳細を編集コントロール](../assets/ui/workspace/pencil-icon.png)**[!UICONTROL 詳細を編集&#x200B;]**コントロールを使用して、既存の宛先アカウントの詳細を[更新](update-accounts.md)します。
> * [!UICONTROL プラットフォーム]列の省略記号（`...`）を選択し、![削除コントロール](../assets/ui/workspace/delete-destination-symbol.png)**[!UICONTROL 削除&#x200B;]**コントロールを使用して、既存の宛先アカウントを[削除](delete-destination-account.md)します。


![「アカウント」タブ](../assets/ui/workspace/destination-account-options.png)

| 要素 | 説明 |
|---|---|
| [!UICONTROL プラットフォーム] | 接続を設定した宛先。 |
| [!UICONTROL 接続タイプ] | ストレージバケットまたは宛先へのアカウント接続タイプを表します。宛先に応じて、認証オプションは次のとおりです。 <ul><li>メールマーケティングの宛先の場合：S3、FTP、Azure Blob のいずれかです。</li><li>リアルタイム広告の宛先の場合：サーバー間</li><li>Amazon S3 クラウドストレージの宛先：アクセスキー </li><li>SFTP クラウドストレージの宛先：SFTP の基本認証</li><li>OAuth 1 または OAuth 2 認証</li><li>ベアラートークン認証</li></ul> |
| [!UICONTROL ユーザー名] | [「宛先の接続」ウィザード](../catalog/email-marketing/overview.md#connect-destination)で選択したユーザー名。 |
| [!UICONTROL 宛先] | 宛先に対して作成された基本情報に接続された、一意の成功した宛先データフローの数を表します。 |
| [!UICONTROL 認証済み] | この宛先への接続が承認された日付。 |

{style="table-layout:auto"}

## [!UICONTROL 参照] {#browse}

「**[!UICONTROL 参照]**」タブには、接続を確立した宛先が表示されます。宛先の切替スイッチを&#x200B;**[!UICONTROL 有効／無効]**&#x200B;にすると、それぞれアクティブまたは非アクティブに設定されます。また、**[!UICONTROL セグメント]**／**[!UICONTROL 参照]**&#x200B;を選択し、検査するセグメントを選択すると、データのフロー先を表示することもできます。「[!UICONTROL 参照]」タブで各宛先に対して提供されるすべての情報については、次の表を参照してください。

>[!TIP]
>
> * [!UICONTROL 名前]列の省略記号（`...`）を選択し、![セグメントコントロールのアクティブ化](../assets/ui/workspace/add-data-symbol.png)**[!UICONTROL アクティブ化&#x200B;]**コントロールを使用して、セグメントやデータセットをその宛先に書き出すことができます。
> * [!UICONTROL 名前]列の省略記号（`...`）を選択し、![削除コントロール](../assets/ui/workspace/delete-destination-symbol.png)**[!UICONTROL 削除&#x200B;]**コントロールを使用して、既存の宛先への接続を[削除](delete-destinations.md)できます。
> * [!UICONTROL 名前]列の省略記号（`...`）を選択し、![モニタリングで表示コントロール](../assets/ui/workspace/monitoring-icon.png)**[!UICONTROL モニタリングで表示&#x200B;]**コントロールを使用して、[モニタリングダッシュボード](/help/dataflows/ui/monitor-destinations.md#monitoring-destinations-dashboard)にこの宛先のアクティブ化情報を表示できます。
> * [!UICONTROL 名前]列で省略記号（`...`）を選択し、![アラートを購読](../assets/ui/workspace/alerts-icon.png)**[!UICONTROL アラートを購読&#x200B;]**コントロールを使用して、宛先データフローアラートを購読できます。アラートを購読して、フロー実行のステータス、成功または失敗に関するメッセージを受け取ることができます。 宛先データフローアラートの詳細については、[コンテキスト内の宛先アラートを購読](alerts.md)を参照してください。


![「参照」タブ](../assets/ui/workspace/browse-tab.png)

| 要素 | 説明 |
|---------|----------|
| 名前 | この宛先へのアクティベーションフローに指定した名前。同じ列には、[!UICONTROL アクティブ化]と[!UICONTROL 宛先を削除]という 2 つのコントロールがあります。 |
| [!UICONTROL 前回のフロー実行ステータス] | 前回のデータフロー実行のステータス。データフロー実行について詳しくは、[宛先の詳細を表示](destination-details-page.md)を参照してください。 |
| [!UICONTROL 前回のフロー実行日] | 前回のデータフローが実行された日時。データフローの実行について詳しくは、[宛先の詳細を表示](destination-details-page.md)を参照してください。 |
| [!UICONTROL 宛先] | アクティベーションフローに対して選択した宛先プラットフォームです。 |
| [!UICONTROL 接続タイプ] | ストレージバケットまたは宛先への接続タイプを表します。 <ul><li>メールマーケティングの宛先の場合、S3、FTP または [!DNL Azure Blob] のいずれかです。</li><li>リアルタイム広告の宛先の場合：サーバー間。</li><li>ストリーミング宛先の場合、[!DNL Azure Event Hubs] または [!DNL Amazon Kinesis] のいずれかです。</li></ul> |
| [!UICONTROL ユーザー名] | 宛先フローに対して選択したアカウント資格情報。 |
| [!UICONTROL アクティベーションデータ] | この宛先に対してアクティブ化されているセグメントの数を示します。このコントロールを選択すると、アクティブ化されたセグメントの詳細が表示されます。アクティブ化されたセグメントについて詳しくは、宛先詳細ページの[アクティベーションデータ](/help/destinations/ui/destination-details-page.md#activation-data)を参照してください。 |
| [!UICONTROL 作成日] | 宛先へのアクティベーションフローが作成された日時（UTC 時間）。上下の矢印記号を選択すると、アクティベーションフローを新しい順または古い順に並べ替えることができます。 |
| [!UICONTROL ステータス] | `Enabled` または `Disabled`。データがこの宛先に対してアクティブ化されているかどうかを示します。 |

目的の行をクリックすると、目的の行に関する詳細情報が右側のレールに表示されます。

![宛先行をクリック](../assets/ui/workspace/click-destination-row.png)

宛先名を選択して、この宛先に対してアクティブ化されたセグメントに関する情報を表示します。「**[!UICONTROL アクティベーションの編集]**」をクリックして、この宛先に送信されるセグメントを変更または追加します。

## [!UICONTROL システム表示] {#system-view}

「**[!UICONTROL システム表示]**」タブには、Adobe Experience Platform で設定したアクティベーションフロー図が表示されます。

![Data-flows1](../assets/ui/workspace/data-flows1.png)

ページに表示される任意の宛先を選択し、「**[!UICONTROL データフローを表示]**」をクリックして、各宛先に設定したすべての接続に関する情報を表示します。

![Data-flows2](../assets/ui/workspace/data-flows2.png)
