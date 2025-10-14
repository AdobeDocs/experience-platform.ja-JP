---
title: 自動データセット有効期限
description: Adobe Experience Platform UI でデータセットの有効期限をスケジュールする方法を説明します。
exl-id: 97db55e3-b5d6-40fd-94f0-2463fe041671
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 49%

---

# データセットの有効期限の自動化 {#dataset-expiration}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_scheduleDatasetExpiration_description"
>title="不要または期限切れの顧客レコードとデータセットの削除"
>abstract="<h2>説明</h2><p>規制への準拠に関係なく、Experience Platform データのライフサイクルを管理するために、消費者データを削除し、データセットの有効期限をスケジュールできます。 データ主体のリクエストを作成または管理するには、「データ主体のプライバシーリクエストに従う」ユースケースブロックを参照してください。</p>"

Adobe Experience Platform UI の [[!UICONTROL &#x200B; データライフサイクル &#x200B;] ワークスペース &#x200B;](./overview.md) では、データセットの有効期限をスケジュールできます。 データセットが有効期限に達すると、データレイク、ID サービスおよびリアルタイム顧客プロファイルは別個のプロセスを開始して、それぞれのサービスからデータセットの内容を削除します。3 つのサービスすべてからデータを削除すると、有効期限が完了とマークされます。

>[!WARNING]
>
>データセットの有効期限が切れるように設定されている場合、ダウンストリームワークフローに悪影響が及ばないよう、データをそのデータセットに取り込む可能性があるデータフローを手動で変更する必要があります。

このドキュメントでは、Experience Platform UI でデータセットの有効期限をスケジュール設定および自動化する方法を説明します。

>[!NOTE]
>
>データセットの有効期限は、現在、Adobe Experience Platform Edge Networkからデータを削除しません。 ただし、データセットの有効期限が切れた後も、データがEdge Network内に残る可能性はありません。 これは、データセットの有効期限に関する 15 日間のサービス使用許諾契約が、破棄される前にEdge Network内にデータが存在する 14 日間の期間と重なるためです。

Advanced Data Lifecycle Management では、[&#x200B; データセット有効期限エンドポイント &#x200B;](../api/dataset-expiration.md) を介したデータセット削除、および [workorder エンドポイント &#x200B;](../api/workorder.md) を介したプライマリ ID を使用した ID 削除（行レベルデータ）をサポートしています。 また、Experience Platform UI を使用して、データセットの有効期限や [&#x200B; レコードの削除 &#x200B;](./record-delete.md) を管理することもできます。 詳しくは、リンクされたドキュメントを参照してください。

>[!NOTE]
>
>データ ライフサイクルはバッチ削除をサポートしていません。

## データセット有効期限のスケジュール設定 {#schedule-dataset-expiration}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_scheduleDatasetExpiration_instructions"
>title="手順"
>abstract="<ul><li>左側のナビゲーションで「<a href="https://experienceleague.adobe.com/docs/experience-platform/hygiene/ui/overview.html?lang=ja">データライフサイクル</a>」、「<b>リクエストを作成</b>」の順に選択します。</li><li>レコードを削除する場合：</li>   <li>「<b>レコード</b>」を選択します。</li>   <li>レコードを削除する特定のデータセットを選択するか、すべてのデータセットからレコードを削除するオプションを選択します。</li>   <li>レコードを削除する消費者の ID を指定します。「<b>ID を追加</b>」を選択して ID を 1 つずつ指定するか、「<b>ファイルを選択</b>」を選択して ID の JSON ファイルをアップロードします。</li>   <li>必要に応じて、「<b>テンプレート</b>」を選択して、JSON ファイルの予想される形式を表示します。</li><li><a href="https://experienceleague.adobe.com/docs/experience-platform/hygiene/ui/dataset-expiration.html?lang=ja#schedule-dataset-expiration">データセットの有効期限をスケジュール</a>する場合の手順については、ドキュメントを参照してください。</li></ul>"

リクエストを作成するには、ワークスペースのメインページから **[!UICONTROL リクエストを作成]** を選択します。

>[!IMPORTANT]
>
>Real-Time CDP、Adobe Journey Optimizer、Customer Journey Analyticsの各ユーザーには、20 件の保留中スケジュール済みデータセット有効期限作業指示があります。 Healthcare Shield およびプライバシーとセキュリティシールドのユーザーには、保留中のスケジュールされたデータセット有効期限作業指示が 50 件あります。 つまり、一度に 20 個または 50 個のデータセットを削除するようにスケジュールできます。<br> 例えば、スケジュールされたデータセット有効期限が 20 あり、1 つのデータセットが明日削除される予定の場合、そのデータセットが削除されるまで、それ以上の有効期限を設定することはできません。

![&#x200B; リクエストを作成  がハイライト表示された [!UICONTROL &#x200B; データライフサイクル &#x200B;] ワークスペース &#x200B;](../images/ui/ttl/create-request-button.png)

リクエスト作成ワークフローが表示されます。 「[!UICONTROL &#x200B; リクエストされたアクション &#x200B;]」セクションで、**[!UICONTROL データセットを削除]** を選択して、データセットの有効期限スケジュール設定のコントロールを更新します。

![&#x200B; 「データセットを削除 [!UICONTROL &#x200B; オプションがハイライト表示されたリクエスト作成ワークフロ &#x200B;]。](../images/ui/ttl/dataset-selected.png)

### 日付およびデータセットの選択 {#select-date-and-dataset}

「**[!UICONTROL リクエストされたアクション]**」セクションで、データセットを削除する日付を選択します。手動で日付を入力（`mm/dd/yyyy` の形式）するか、カレンダーアイコン（![A カレンダーアイコン）を選択します。](/help/images/icons/calendar.png)）を選択して、ダイアログから日付を選択します。

![&#x200B; データセット用に有効期限が選択されハイライト表示されたカレンダーダイアログ &#x200B;](../images/ui/ttl/select-date.png)

次に、「**[!UICONTROL データセットの詳細]** で、データベースアイコン（![&#x200B; データベースアイコンを選択します。](/help/images/icons/database.png)）を選択して、データセット選択ダイアログを開きます。 リストから有効期限を適用するデータセットを選択して、「**[!UICONTROL 完了]**」を選択します。

[ データセットが選択され ] 完了 ![[!UICONTROL &#x200B; がハイライト表示された [!UICONTROL &#x200B; データセットを選択 &#x200B;] ダイアログ &#x200B;]](../images/ui/ttl/select-dataset.png)

>[!NOTE]
>
>現在のサンドボックスに属するデータセットのみが表示されます。

### リクエストの送信 {#submit-request}

「[!UICONTROL データセットの詳細]」セクションには、選択したデータセットのプライマリ ID とスキーマが表示されます。**[!UICONTROL リクエスト設定]**&#x200B;で、リクエストの名前とオプションの説明を入力し、「**[!UICONTROL 送信]**」を続行します。

![ 「リクエスト設定 [!UICONTROL &#x200B; と「送信 &#x200B;] ボタンがハイライト表示された、完了したデータセット有効期限リク ] スト (../images/ui/ttl/submit.png)

[!UICONTROL &#x200B; リクエストを確認 &#x200B;] ダイアログが表示されます。 データセット名とデータセットが削除される日付を確認するよう求められます。 「**[!UICONTROL 送信]**」をクリックして続行します。

リクエストが送信されると、作業指示が作成され、[!UICONTROL &#x200B; データライフサイクル &#x200B;] ワークスペースのメインタブに表示されます。 ここから、リクエストを処理する作業指示のステータスを監視できます。

>[!NOTE]
>
>データセットの有効期限が実行されるとどのように処理されるかの詳細については、[タイムラインと透明性](../home.md#dataset-expiration-transparency)の概要に関する節を参照してください。

## データセット有効期限の編集またはキャンセル {#edit-or-cancel}

データセットの有効期限を編集またはキャンセルするには、ワークスペースのメインページで&#x200B;**[!UICONTROL データセット]**&#x200B;を選択して、リストからデータセットの有効期限を選択します。

データセット有効期限の詳細ページで、右側のパネルにスケジュール設定された削除を編集またはキャンセルするためのコントロールが表示されます。

## 次の手順

このドキュメントでは、Experience Platform UI でデータセットの有効期限をスケジュール設定する方法について説明しました。UI で他のデータ最小化タスクを実行する方法について詳しくは、[&#x200B; データライフサイクル UI の概要 &#x200B;](./overview.md) を参照してください。

Data Hygiene API を使用したデータセット有効期限のスケジュール設定方法については、[&#x200B; データセット有効期限のエンドポイントガイド &#x200B;](../api/dataset-expiration.md) を参照してください。
