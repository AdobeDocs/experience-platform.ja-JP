---
title: データセット有効期限の管理
description: Adobe Experience Platform UI でデータセットの有効期限をスケジュールする方法を説明します。
exl-id: 97db55e3-b5d6-40fd-94f0-2463fe041671
source-git-commit: 7931c8fe4a1ca5d255a80e7e6b0deb976d53c3de
workflow-type: tm+mt
source-wordcount: '819'
ht-degree: 52%

---

# データセット有効期限の管理 {#dataset-expiration}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_scheduleDatasetExpiration_description"
>title="不要または期限切れの顧客レコードとデータセットの削除"
>abstract="<h2>説明</h2><p>規制への準拠に関係なく、Experience Platform データのライフサイクルを管理するために、消費者データを削除し、データセットの有効期限をスケジュールできます。 データ主体のリクエストを作成または管理するには、「データ主体のプライバシーリクエストに従う」使用例ブロックを参照してください。</p>"

The [[!UICONTROL データのライフサイクル] workspace](./overview.md) Adobe Experience Platform UI では、データセットの有効期限をスケジュールできます。 データセットが有効期限に達すると、データレイク、ID サービスおよびリアルタイム顧客プロファイルは別個のプロセスを開始して、それぞれのサービスからデータセットの内容を削除します。3 つのサービスすべてからデータを削除すると、有効期限が完了とマークされます。

>[!WARNING]
>
>データセットの有効期限が切れるように設定されている場合、ダウンストリームワークフローに悪影響が及ばないよう、データをそのデータセットに取り込む可能性があるデータフローを手動で変更する必要があります。

このドキュメントでは、Platform UI のデータセット有効期限をスケジュール設定および管理する方法を説明します。

>[!NOTE]
>
>現在、データセットの有効期限によってAdobe Experience Platform Edge ネットワークからデータが削除されることはありません。 ただし、データセットの有効期限が切れるように設定した後も、データが Edge ネットワーク内に残る可能性はありません。 これは、データセットの有効期限に関する 14 日間のサービスライセンス契約が、破棄される前に Edge ネットワーク内にデータが存在する 14 日間の期間と一致するためです。

## データセット有効期限のスケジュール設定 {#schedule-dataset-expiration}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_scheduleDatasetExpiration_instructions"
>title="手順"
>abstract="<ul><li>選択 <a href="https://experienceleague.adobe.com/docs/experience-platform/hygiene/ui/overview.html?lang=ja">データのライフサイクル</a> 左側のナビゲーションで、「 <b>リクエストを作成</b>.</li><li>レコードを削除する場合：</li>   <li>「<b>レコード</b>」を選択します。</li>   <li>レコードを削除する特定のデータセットを選択するか、すべてのデータセットからレコードを削除するオプションを選択します。</li>   <li>レコードを削除する消費者の ID を指定します。「<b>ID を追加</b>」を選択して ID を 1 つずつ指定するか、「<b>ファイルを選択</b>」を選択して ID の JSON ファイルをアップロードします。</li>   <li>必要に応じて、「<b>テンプレート</b>」を選択して、JSON ファイルの予想される形式を表示します。</li><li><a href="https://experienceleague.adobe.com/docs/experience-platform/hygiene/ui/dataset-expiration.html?lang=ja#schedule-dataset-expiration">データセットの有効期限をスケジュール</a>する場合の手順については、ドキュメントを参照してください。</li></ul>"

リクエストを作成するには、「 **[!UICONTROL リクエストを作成]** をワークスペースのメインページから削除します。

>[!IMPORTANT]
>
同時にスケジュールされたデータセットの有効期限は最大 20 個までにすることができます。 つまり、一度に 20 個のデータセットの削除をスケジュールできます。 これらの有効期限を設定する時間や年に制限はありません。 例えば、データセットの有効期限が 20 回スケジュールされていて、あす 1 つのデータセットが削除される予定の場合、そのデータセットが削除されるまでは、それ以上の期限を設定することはできません。

![The [!UICONTROL データのライフサイクル] ワークスペース [!UICONTROL リクエストを作成] ハイライト表示されました。](../images/ui/ttl/create-request-button.png)

リクエスト作成ワークフローが表示されます。 の下 [!UICONTROL 要求されたアクション] セクション、選択 **[!UICONTROL データセットを削除]** をクリックして、データセットの有効期限のスケジュール設定に関するコントロールを更新します。

![リクエスト作成ワークフローで、 [!UICONTROL データセットを削除] オプションがハイライト表示されました。](../images/ui/ttl/dataset-selected.png)

### 日付およびデータセットの選択 {#select-date-and-dataset}

「**[!UICONTROL リクエストされたアクション]**」セクションで、データセットを削除する日付を選択します。日付は手動で（形式で）入力できます `mm/dd/yyyy`) またはカレンダーアイコン (![カレンダーアイコン。](../images/ui/ttl/calendar-icon.png)) をクリックして、ダイアログから日付を選択します。

![データセットの有効期限が選択され、強調表示されたカレンダーダイアログ。](../images/ui/ttl/select-date.png)

次の、以下 **[!UICONTROL データセットの詳細]**、データベースアイコン (![データベースアイコン。](../images/ui/ttl/database-icon.png)) をクリックして、データセット選択ダイアログを開きます。 リストから有効期限を適用するデータセットを選択して、「**[!UICONTROL 完了]**」を選択します。

![The [!UICONTROL データセットを選択] データセットを選択したダイアログと [!UICONTROL 完了] ハイライト表示されました。](../images/ui/ttl/select-dataset.png)

>[!NOTE]
>
現在のサンドボックスに属するデータセットのみが表示されます。

### リクエストの送信 {#submit-request}

「[!UICONTROL データセットの詳細]」セクションには、選択したデータセットのプライマリ ID とスキーマが表示されます。**[!UICONTROL リクエスト設定]**&#x200B;で、リクエストの名前とオプションの説明を入力し、「**[!UICONTROL 送信]**」を続行します。

![データセットの有効期限リクエストが完了し、 [!UICONTROL リクエスト設定] および [!UICONTROL 送信] ボタンをハイライト表示します。](../images/ui/ttl/submit.png)

A [!UICONTROL リクエストを確認] ダイアログが表示されます。 データセットの名前と、データセットの削除日を確認するよう求められます。 「**[!UICONTROL 送信]**」をクリックして続行します。

リクエストが送信されると、作業指示が作成され、 [!UICONTROL データのライフサイクル] ワークスペース。 ここから、リクエストを処理する作業指示のステータスを監視できます。

>[!NOTE]
>
データセットの有効期限が実行されるとどのように処理されるかの詳細については、[タイムラインと透明性](../home.md#dataset-expiration-transparency)の概要に関する節を参照してください。

## データセット有効期限の編集またはキャンセル {#edit-or-cancel}

データセットの有効期限を編集またはキャンセルするには、ワークスペースのメインページで&#x200B;**[!UICONTROL データセット]**&#x200B;を選択して、リストからデータセットの有効期限を選択します。

データセット有効期限の詳細ページで、右側のパネルにスケジュール設定された削除を編集またはキャンセルするためのコントロールが表示されます。

## 次の手順

このドキュメントでは、Experience Platform UI でデータセットの有効期限をスケジュール設定する方法について説明しました。UI での他のデータ最小化タスクの実行方法について詳しくは、 [データライフサイクル UI の概要](./overview.md).

データ衛生 API を使用してデータセット有効期限をスケジュールする方法については、 [データセット有効期限エンドポイントガイド](../api/dataset-expiration.md).
