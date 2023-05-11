---
title: データセット有効期限の管理
description: Adobe Experience Platform UI でデータセットの有効期限をスケジュールする方法を説明します。
exl-id: 97db55e3-b5d6-40fd-94f0-2463fe041671
source-git-commit: a1628df7d0eefc795d1eaeefce842a65c7133322
workflow-type: tm+mt
source-wordcount: '736'
ht-degree: 100%

---

# データセット有効期限の管理 {#dataset-expiration}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_scheduleDatasetExpiration_description"
>title="不要または期限切れの顧客レコードとデータセットの削除"
>abstract="<h2>説明</h2><p>規制への準拠に関係なく、Experience Platform データのライフサイクルを管理するために、消費者データを削除し、データセットの有効期限をスケジュールできます。 データ主体のリクエストを作成または管理するには、「データ主体のプライバシーリクエストに従う」ユースケースブロックを参照してください。</p>"

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は現在、**Adobe Healthcare Shield** または **Adobe Privacy &amp; Security Shield** を購入した組織でのみご利用いただけます。これらの機能は今後、一般リリースが予定されています。 今後のリリースの詳細情報については、アドビサービス担当者にお問い合わせください。 ただし、すぐに[[!UICONTROL データセット] UI でデータセットを削除できます](../../catalog/datasets/user-guide.md#delete)。

Adobe Experience Platform UI の[[!UICONTROL データハイジーン]ワークスペース](./overview.md)では、データセットの有効期限をスケジュールできます。データセットが有効期限に達すると、データレイク、ID サービスおよびリアルタイム顧客プロファイルは別個のプロセスを開始して、それぞれのサービスからデータセットの内容を削除します。3 つのサービスすべてからデータを削除すると、有効期限が完了とマークされます。

>[!WARNING]
>
>データセットの有効期限が切れるように設定されている場合、ダウンストリームワークフローに悪影響が及ばないよう、データをそのデータセットに取り込む可能性があるデータフローを手動で変更する必要があります。

このドキュメントでは、Platform UI のデータセット有効期限をスケジュール設定および管理する方法を説明します。

## データセット有効期限のスケジュール設定 {#schedule-dataset-expiration}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_scheduleDatasetExpiration_instructions"
>title="手順"
>abstract="<ul><li>左側のナビゲーションで「<a href="https://experienceleague.adobe.com/docs/experience-platform/hygiene/ui/overview.html?lang=ja">データハイジーン</a>」を選択し、「<b>リクエストを作成</b>」を選択します。</li><li>レコードを削除する場合：</li>   <li>「<b>レコード</b>」を選択します。</li>   <li>レコードを削除する特定のデータセットを選択するか、すべてのデータセットからレコードを削除するオプションを選択します。</li>   <li>レコードを削除する消費者の ID を指定します。「<b>ID を追加</b>」を選択して ID を 1 つずつ指定するか、「<b>ファイルを選択</b>」を選択して ID の JSON ファイルをアップロードします。</li>   <li>必要に応じて、「<b>テンプレート</b>」を選択して、JSON ファイルの予想される形式を表示します。</li><li><a href="https://experienceleague.adobe.com/docs/experience-platform/hygiene/ui/dataset-expiration.html?lang=ja#schedule-dataset-expiration">データセットの有効期限をスケジュール</a>する場合の手順については、ドキュメントを参照してください。</li></ul>"

新しいリクエストを作成するには、ワークスペースのメインページから「**[!UICONTROL リクエストを作成]**」を選択します。

![「[!UICONTROL リクエストを作成]」ボタンが選択されていることを示す画像](../images/ui/ttl/create-request-button.png)

リクエスト作成ダイアログが表示されます。「**[!UICONTROL リクエストされたアクション]**」セクションで、**[!UICONTROL データセットを削除]**&#x200B;を選択して、 データセットの有効期限スケジュール設定で使用可能なコントロールを更新します。

![「[!UICONTROL リクエストを作成]」ボタンが選択されていることを示す画像](../images/ui/ttl/dataset-selected.png)

### 日付およびデータセットの選択

リクエスト作成ダイアログが表示されます。「**[!UICONTROL リクエストされたアクション]**」セクションで、データセットを削除する日付を選択します。手動で日付を入力（`mm/dd/yyyy` 形式）するか、カレンダーアイコン（![カレンダーアイコンの画像](../images/ui/ttl/calendar-icon.png)）を選択して、ダイアログから日付を選択します。

![データセットの有効期限が設定されていることを示す画像](../images/ui/ttl/select-date.png)

次に、「**[!UICONTROL データセットの詳細]**」で、データベースアイコン（![データベースアイコンの画像](../images/ui/ttl/database-icon.png)）を選択して、データセット選択ダイアログを開きます。リストから有効期限を適用するデータセットを選択して、「**[!UICONTROL 完了]**」を選択します。

![データセットが選択されていることを示す画像](../images/ui/ttl/select-dataset.png)

>[!NOTE]
>現在のサンドボックスに属するデータセットのみが表示されます。

### リクエストの送信

「[!UICONTROL データセットの詳細]」セクションには、選択したデータセットのプライマリ ID とスキーマが表示されます。**[!UICONTROL リクエスト設定]**&#x200B;で、リクエストの名前とオプションの説明を入力し、「**[!UICONTROL 送信]**」を続行します。

![「[!UICONTROL 送信]」ボタンが選択されていることを示す画像](../images/ui/ttl/submit.png)

データセットが削除される日付を確認するよう求められます。「**[!UICONTROL 送信]**」をクリックして続行します。

リクエストが送信されると、作業指示が作成され、[!UICONTROL データハイジーン]ワークスペースのメインタブに表示されます。ここから、リクエストを処理する作業指示のステータスを監視できます。

>[!NOTE]
>データセットの有効期限が実行されるとどのように処理されるかの詳細については、[タイムラインと透明性](../home.md#dataset-expiration-transparency)の概要に関する節を参照してください。

## データセット有効期限の編集またはキャンセル

データセットの有効期限を編集またはキャンセルするには、ワークスペースのメインページで&#x200B;**[!UICONTROL データセット]**&#x200B;を選択して、リストからデータセットの有効期限を選択します。

データセット有効期限の詳細ページで、右側のパネルにスケジュール設定された削除を編集またはキャンセルするためのコントロールが表示されます。

## 次の手順

このドキュメントでは、Experience Platform UI でデータセットの有効期限をスケジュール設定する方法について説明しました。UI で他のデータハイジーンタスクを実行する方法について詳しくは、[データハイジーン UI の概要](./overview.md)を参照してください。

Data Hygiene API を使用したデータセット有効期限のスケジュール設定方法については、[データセット有効期限のエンドポイントガイド](../api/dataset-expiration.md)を参照してください。
