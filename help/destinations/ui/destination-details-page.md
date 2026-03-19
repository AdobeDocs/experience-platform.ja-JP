---
keywords: 宛先；宛先；宛先の詳細ページ；宛先の詳細ページ
title: 宛先の詳細を表示
description: 個々の宛先の詳細ページには、宛先の詳細の概要が表示されます。 宛先の詳細には、宛先名、ID、宛先にマッピングされたオーディエンス、アクティブ化を編集したり、データフローを有効または無効にしたりするためのコントロールが含まれます。
exl-id: e44e2b2d-f477-4516-8a47-3e95c2d85223
source-git-commit: 2dd4ae4146f7c1c5228e22d24ff2ba31010adedb
workflow-type: tm+mt
source-wordcount: '1222'
ht-degree: 6%

---

# 宛先の詳細を表示

## 概要 {#overview}

Adobe Experience Platformのユーザーインターフェイスでは、宛先の属性とアクティビティを表示およびモニタリングできます。 これらの詳細には、宛先の名前と ID、宛先をアクティブ化または無効にするコントロールなどが含まれます。 詳細には、アクティブ化されたプロファイルレコードの指標、アクティブ化された ID、失敗および除外された ID、データフロー実行の履歴も含まれます。

>[!NOTE]
>
>宛先の詳細ページは、[!UICONTROL Destinations] [!DNL Experience Platform] の [!DNL UI] ワークスペースの一部です。 詳しくは、[[!UICONTROL Destinations] Workspace の概要 &#x200B;](./destinations-workspace.md) 参照してください。

## 宛先の詳細を表示 {#view-details}

既存の宛先に関する詳細を表示するには、次の手順に従います。 宛先の宛先 ID、宛先を作成したユーザー、作成時などの情報を確認できます。

1. [Experience Platform UI にログインし &#x200B;](https://platform.adobe.com/) 左側のナビゲーションバーから「**[!UICONTROL Destinations]**」を選択します。 上部のヘッダーから「**[!UICONTROL Browse]**」を選択すると、既存の宛先が表示されます。

   ![&#x200B; 宛先の参照 &#x200B;](../assets/ui/details-page/browse-destinations.png)

2. 左上のフィルターアイコン ![フィルターアイコン](../../images/icons/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられた特定のデータフローを表示できます。

   ![&#x200B; 宛先のフィルタリング &#x200B;](../assets/ui/details-page/filter-destinations.png)

3. 詳細情報を表示する宛先の行を選択します。 これにより、宛先 ID、宛先接続を作成したユーザー、その他の情報など、宛先に関する情報を含む適切なパネルが表示されます。

   ![&#x200B; 右側のパネルの宛先 ID](../assets/ui/details-page/right-rail-info-including-destination-id.png)

4. または、表示する *宛先の名前* を選択して、宛先に関する他の情報を表示することもできます。

   ![宛先を選択](../assets/ui/details-page/destination-select.png)

5. 宛先の詳細ページが右側のパネルに表示され、使用可能なコントロールが表示されます。

   ![&#x200B; 宛先の詳細 &#x200B;](../assets/ui/details-page/destination-details.png)

## 右側のパネル {#right-rail}

右側のパネルには、選択した宛先に関する基本情報が表示されます。

![右パネル](../assets/ui/details-page/right-sidebar.png)

右側のパネルに表示されるコントロールと詳細を次の表に示します。

| 右側のパネル項目 | 説明 |
| --- | --- |
| [!UICONTROL Activate audiences] | このコントロールを選択して、宛先にマッピングされるオーディエンスの編集、書き出しスケジュールの更新、マッピングされた属性および ID の追加および削除を行います。 詳しくは、[&#x200B; オーディエンスストリーミング宛先に対するオーディエンスデータのアクティブ化 &#x200B;](./activate-segment-streaming-destinations.md)、[&#x200B; バッチプロファイルベースの宛先に対するオーディエンスデータのアクティブ化 &#x200B;](./activate-batch-profile-destinations.md) および [&#x200B; ストリーミングプロファイルベースの宛先に対するオーディエンスデータのアクティブ化 &#x200B;](./activate-streaming-profile-destinations.md) に関するガイドを参照してください。 |
| [!UICONTROL Delete] | このデータフローを削除でき、以前にアクティブ化されたオーディエンスが存在する場合は、そのオーディエンスのマッピングを解除できます。 |
| [!UICONTROL Destination name] | このフィールドを編集して宛先の名前を更新できます。 |
| [!UICONTROL Description] | このフィールドを編集して、宛先を更新したり、オプションで説明を追加したりできます。 |
| [!UICONTROL Destination] | オーディエンスの宛先プラットフォームを表します。詳しくは、[&#x200B; 宛先カタログ &#x200B;](../catalog/overview.md) を参照してください。 |
| [!UICONTROL Status] | 宛先が有効か無効かを示します。 |
| [!UICONTROL Marketing actions] | データガバナンスの目的でこの宛先に適用されるマーケティングアクション（ユースケース）を示します。 |
| [!UICONTROL Category] | 宛先のタイプを示します。 詳しくは、[&#x200B; 宛先カタログ &#x200B;](../catalog/overview.md) を参照してください。 |
| [!UICONTROL Connection type] | オーディエンスを宛先に送信する際に使用するフォームを示します。 使用可能な値は [!UICONTROL Cookie] および [!UICONTROL Profile-based] です。 |
| [!UICONTROL Frequency] | オーディエンスが宛先に送信される頻度を示します。使用可能な値は [!UICONTROL Streaming] および [!UICONTROL Batch] です。 |
| [!UICONTROL Identity] | 宛先によって受け入れられる ID 名前空間（`GAID`、`IDFA`、`email` など）を表します。 受け入れ可能な ID 名前空間について詳しくは、[ID 名前空間の概要 &#x200B;](../../identity-service/features/namespaces.md) を参照してください。 |
| [!UICONTROL Created by] | この宛先を作成したユーザーを示します。 |
| [!UICONTROL Created] | この宛先が作成された際の UTC 日時を示します。 |

{style="table-layout:auto"}

## [!UICONTROL Enabled]/[!UICONTROL Disabled] 切り替え {#enabled-disabled-toggle}

**[!UICONTROL Enabled]/[!UICONTROL Disabled]** 切り替えスイッチを使用して、宛先へのすべてのデータ書き出しを開始および一時停止できます。

![&#x200B; データフローの有効/無効の切り替え &#x200B;](../assets/ui/details-page/enable-disable.png)

## [!UICONTROL Dataflow runs] {#dataflow-runs}

「[!UICONTROL Dataflow runs]」タブには、データフローのバッチ宛先およびストリーミング宛先への実行に関する指標データが表示されます。 詳細と指標の定義については、[&#x200B; データフローの監視 &#x200B;](monitor-dataflows.md) を参照してください。

>[!NOTE]
>
>* 宛先モニタリング機能は、現在、Experience Platformのすべての宛先 ** Adobe Targetを除く [、](/help/destinations/catalog/personalization/adobe-target-connection.md) カスタムパーソナライゼーション [&#x200B; および &#x200B;](/help/destinations/catalog/personalization/custom-personalization.md)6&rbrace;Experience Cloud オーディエンス [&#x200B; の宛先でサポートされています。](/help/destinations/catalog/adobe/experience-cloud-audiences.md)
>* [Amazon Kinesis](/help/destinations/catalog/cloud-storage/amazon-kinesis.md)、[Azure Event Hubs](/help/destinations/catalog/cloud-storage/azure-event-hubs.md) および [HTTP API](/help/destinations/catalog/streaming/http-destination.md) の宛先については、除外された ID、失敗した ID およびアクティブ化された ID に関連する指標が予測されます。 アクティベーションデータの量が多いほど、指標の精度が高くなります。

![&#x200B; データフロー実行ビュー &#x200B;](../assets/ui/details-page/dataflow-runs.png)

### データフロー実行時間 {#dataflow-runs-duration}

ストリーミング宛先とファイルベースの宛先では、データフロー実行の表示時間に違いがあります。

### ストリーミングの宛先 {#streaming}

以下の画像に示すように、ほとんどのストリーミングデータフロー実行で示される **[!UICONTROL Processing duration]** は約 4 時間ですが、データフロー実行の実際の処理時間ははるかに短くなります。 データフロー実行ウィンドウは、Experience Platformが宛先への呼び出しを再試行する必要がある場合に長い間開いたままになり、同じ時間枠で到着する遅延データを見逃さないようにします。

![&#x200B; ストリーミング宛先の「処理時間」列がハイライト表示されたデータフロー実行ページの画像 &#x200B;](/help/destinations/assets/ui/details-page/processing-time-dataflow-run-streaming.png)

詳しくは、監視ドキュメントの [&#x200B; ストリーミング宛先へのデータフロー実行 &#x200B;](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-streaming-destinations) を参照してください。

### ファイルベースの宛先 {#file-based}

データフローをファイルベースの宛先に対して実行する場合、**[!UICONTROL Processing duration]** は、書き出されるデータのサイズとシステムの読み込みによって異なります。 また、データフローがファイルベースの宛先に対して実行されるは、オーディエンスごとに分類されます。

![&#x200B; ファイルベースの宛先用に「処理時間」列がハイライト表示されたデータフロー実行ページの画像 &#x200B;](../assets/ui/details-page/processing-time-dataflow-run-file-based.png)

詳しくは、監視ドキュメントの [&#x200B; バッチ（ファイルベース）宛先に対するデータフローの実行 &#x200B;](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) を参照してください。

## [!UICONTROL Activation data] {#activation-data}

「**[!UICONTROL Activation data]**」タブには、宛先にマッピングされたオーディエンスのリストが表示されます。これには、オーディエンスの開始日と終了日（該当する場合）やデータ書き出しに関連するその他の情報（書き出しタイプ、スケジュール、頻度など）が含まれます。 特定のオーディエンスに関する詳細を表示するには、リストから名前を選択します。

>[!TIP]
>
>宛先にマッピングされた属性と ID に関する詳細を表示および編集するには、**[!UICONTROL Activate audiences]** 右側のパネル [&#x200B; で「](#right-rail)」を選択します。

>[!BEGINSHADEBOX]

ファイルベースの宛先の「**[!UICONTROL Activation data]**」タブ

![&#x200B; 有効化データビューのバッチ宛先 &#x200B;](../assets/ui/details-page/activation-data-batch.png)

>[!ENDSHADEBOX]


>[!BEGINSHADEBOX]

ストリーミング宛先の「**[!UICONTROL Activation data]**」タブ。

![&#x200B; アクティベーションデータビューストリーミング宛先 &#x200B;](../assets/ui/details-page/activation-data-streaming.png)

>[!ENDSHADEBOX]

### アクティブ化されたオーディエンスをフィルター {#filter-audiences}

宛先に対してアクティブ化されたオーディエンスのリストをフィルタリングするには、検索ボックスにオーディエンス名を入力します。 検索結果に合わせて、オーディエンスのリストが自動的に更新されます。

![&#x200B; オーディエンスをフィルタリングするための検索ボックス。](../assets/ui/details-page/filter-audiences.png)

### アクティベーションフローから複数のオーディエンスを削除 {#bulk-remove}

既存のアクティベーションフローから複数のオーディエンスを削除するには、オーディエンスを選択してから **[!UICONTROL Remove audiences]** を選択します。

![&#x200B; 「オーディエンスを削除」オプションを強調表示したアクティベーションデータ画面 &#x200B;](../assets/ui/details-page/bulk-remove-audiences.png)

### オンデマンドでの複数ファイルのバッチ宛先への書き出し {#bulk-export}

[&#x200B; ページから &#x200B;](../ui/export-file-now.md) オンデマンドで複数のファイルを書き出す **[!UICONTROL Activation data]** ことができます。 これを行うには、オンデマンドでファイルを書き出すオーディエンスを選択し、**[!UICONTROL Export file now]** コントロールを選択して、1 回限りの書き出しをトリガーにします。これにより、選択した各オーディエンスのファイルがバッチ宛先に配信されます。

![&#x200B; 「今すぐファイルを書き出し」ボタンをハイライト表示した画像。](../assets/ui/details-page/bulk-export-file-now.png)

### バッチ宛先に書き出された複数のオーディエンスのアクティベーションスケジュールを編集します {#bulk-edit-schedule}

複数のオーディエンスの既存のアクティベーションスケジュールを同時に編集するには、目的のオーディエンスを選択してから、「**[!UICONTROL Edit schedule]**」を選択します。 書き出しスケジュールを定義または編集する方法について詳しくは、[&#x200B; オーディエンスの書き出しをスケジュール &#x200B;](../ui/activate-batch-profile-destinations.md#scheduling) の節を参照してください。

![&#x200B; 複数のオーディエンスのアクティベーションスケジュールを編集するオプションをハイライト表示したアクティベーションデータ画面。](../assets/ui/details-page/bulk-edit-schedule.png)

>[!NOTE]
>
>オーディエンスの詳細ページの詳細については、[&#x200B; オーディエンスポータルの概要 &#x200B;](../../segmentation/ui/audience-portal.md#segment-details) を参照してください。

### バッチ宛先に書き出された複数のオーディエンスのファイル名を編集します {#bulk-edit-file-names}

複数のオーディエンスの書き出されたファイル名を同時に編集するには、目的のオーディエンスを選択してから、「**[!UICONTROL Edit file name]**」を選択します。 ファイル名の定義または編集方法について詳しくは、[&#x200B; ファイル名の設定 &#x200B;](../ui/activate-batch-profile-destinations.md#configure-file-names) の節を参照してください。

![&#x200B; 複数のオーディエンスのファイル名を編集するオプションをハイライト表示したアクティベーションデータ画面。](../assets/ui/details-page/bulk-edit-file-name.png)