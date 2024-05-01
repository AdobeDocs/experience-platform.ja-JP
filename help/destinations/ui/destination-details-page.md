---
keywords: 宛先；宛先；宛先の詳細ページ；宛先の詳細ページ
title: 宛先の詳細を表示
description: 個々の宛先の詳細ページには、宛先の詳細の概要が表示されます。 宛先の詳細には、宛先名、ID、宛先にマッピングされたオーディエンス、アクティブ化を編集したり、データフローを有効または無効にしたりするためのコントロールが含まれます。
exl-id: e44e2b2d-f477-4516-8a47-3e95c2d85223
source-git-commit: 9d3b6409013edc38ef41dd2a184ccbdcf7ab9edd
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 9%

---

# 宛先の詳細を表示

## 概要 {#overview}

Adobe Experience Platformのユーザーインターフェイスでは、宛先の属性とアクティビティを表示およびモニタリングできます。 これらの詳細には、宛先の名前と ID、宛先をアクティブ化または無効にするコントロールなどが含まれます。 詳細には、アクティブ化されたプロファイルレコードの指標、アクティブ化された ID、失敗および除外された ID、データフロー実行の履歴も含まれます。

>[!NOTE]
>
>宛先の詳細ページ [!UICONTROL 宛先] のワークスペース [!DNL Platform] [!DNL UI]. を参照してください。 [[!UICONTROL 宛先] workspace の概要](./destinations-workspace.md) を参照してください。

## 宛先の詳細を表示 {#view-details}

既存の宛先に関する詳細を表示するには、次の手順に従います。

1. [Experience Platform UI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。を選択 **[!UICONTROL 参照]** 上部のヘッダーから、既存の宛先を表示します。

   ![宛先の参照](../assets/ui/details-page/browse-destinations.png)

1. 左上のフィルターアイコン ![フィルターアイコン](../assets/ui/details-page/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられた特定のデータフローを表示できます。

   ![宛先のフィルタリング](../assets/ui/details-page/filter-destinations.png)

1. 表示する宛先の名前を選択します。

   ![宛先を選択](../assets/ui/details-page/destination-select.png)

1. 宛先の詳細ページが表示され、使用可能なコントロールが表示されます。

   ![宛先の詳細](../assets/ui/details-page/destination-details.png)

## 右側のパネル {#right-rail}

右側のパネルには、選択した宛先に関する基本情報が表示されます。

![右パネル](../assets/ui/details-page/right-sidebar.png)

右側のパネルに表示されるコントロールと詳細を次の表に示します。

| 右側のパネル項目 | 説明 |
| --- | --- |
| [!UICONTROL オーディエンスをアクティベート] | このコントロールを選択して、宛先にマッピングされるオーディエンスの編集、書き出しスケジュールの更新、マッピングされた属性および ID の追加および削除を行います。 ガイドを参照してください [オーディエンスストリーミング宛先に対するオーディエンスデータのアクティブ化](./activate-segment-streaming-destinations.md), [バッチプロファイルベースの宛先に対するオーディエンスデータのアクティブ化](./activate-batch-profile-destinations.md)、および [ストリーミングプロファイルベースの宛先に対するオーディエンスデータのアクティブ化](./activate-streaming-profile-destinations.md) を参照してください。 |
| [!UICONTROL 削除] | このデータフローを削除でき、以前にアクティブ化されたオーディエンスが存在する場合は、そのオーディエンスのマッピングを解除できます。 |
| [!UICONTROL 宛先名] | このフィールドを編集して、宛先の名前を更新できます。 |
| [!UICONTROL 説明] | このフィールドを編集して、オプションの説明を宛先に更新または追加できます。 |
| [!UICONTROL 宛先] | オーディエンスの宛先プラットフォームを表します。を参照してください。 [宛先カタログ](../catalog/overview.md) を参照してください。 |
| [!UICONTROL ステータス] | 宛先が有効か無効かを示します。 |
| [!UICONTROL マーケティングアクション] | データガバナンスの目的でこの宛先に適用されるマーケティングアクション（ユースケース）を示します。 |
| [!UICONTROL カテゴリ] | 宛先のタイプを示します。 を参照してください。 [宛先カタログ](../catalog/overview.md) を参照してください。 |
| [!UICONTROL 接続タイプ] | オーディエンスを宛先に送信する際に使用するフォームを示します。 使用可能な値は次のとおりです [!UICONTROL Cookie] および [!UICONTROL プロファイルベース]. |
| [!UICONTROL 頻度] | オーディエンスが宛先に送信される頻度を示します。使用可能な値は次のとおりです [!UICONTROL ストリーミング] および [!UICONTROL バッチ]. |
| [!UICONTROL ID] | 次のように、宛先によって受け入れられる ID 名前空間を表します `GAID`, `IDFA`、または `email`. 受け入れ可能な ID 名前空間について詳しくは、 [id 名前空間の概要](../../identity-service/features/namespaces.md). |
| [!UICONTROL 作成者] | この宛先を作成したユーザーを示します。 |
| [!UICONTROL 作成日] | この宛先が作成された際の UTC 日時を示します。 |

{style="table-layout:auto"}

## [!UICONTROL Enabled]/[!UICONTROL Disabled] 切り替え {#enabled-disabled-toggle}

を使用できます **[!UICONTROL Enabled]/[!UICONTROL Disabled]** 宛先へのすべてのデータ書き出しの開始と一時停止を切り替えます。

![データフローの有効化または無効化の切り替え](../assets/ui/details-page/enable-disable.png)

## [!UICONTROL データフローの実行] {#dataflow-runs}

この [!UICONTROL データフローの実行] タブは、データフローのバッチ宛先およびストリーミング宛先への実行に関する指標データを提供します。 こちらを参照してください [データフローの監視](monitor-dataflows.md) を参照してください。

>[!NOTE]
>
>* 宛先モニタリング機能は、現在、Experience Platformのすべての宛先でサポートされています *例外* この [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md), [カスタムパーソナライゼーション](/help/destinations/catalog/personalization/custom-personalization.md) および [Experience Cloudオーディエンス](/help/destinations/catalog/adobe/experience-cloud-audiences.md) 宛先。
>* の場合 [AmazonKinesis](/help/destinations/catalog/cloud-storage/amazon-kinesis.md), [Azure Event Hubs](/help/destinations/catalog/cloud-storage/azure-event-hubs.md)、および [HTTP API](/help/destinations/catalog/streaming/http-destination.md) 宛先、除外された id、失敗した id およびアクティブ化された id に関連する指標が推定されます。 アクティベーションデータの量が多いほど、指標の精度が高くなります。

![データフロー実行ビュー](../assets/ui/details-page/dataflow-runs.png)

### データフロー実行時間 {#dataflow-runs-duration}

ストリーミング宛先とファイルベースの宛先では、データフロー実行の表示時間に違いがあります。

### ストリーミングの宛先 {#streaming}

一方、は **[!UICONTROL 処理期間]** 以下の画像に示すように、ほとんどのストリーミングデータフロー実行で示される処理時間は約 4 時間であり、データフロー実行の実際の処理時間ははるかに短くなります。 データフロー実行ウィンドウは、Experience Platformが宛先への呼び出しを再試行する必要がある場合に長い間開いたままになり、同じ期間に到着する遅延データを見逃さないようにします。

![ストリーミング宛先の「処理時間」列がハイライト表示されたデータフロー実行ページの画像。](/help/destinations/assets/ui/details-page/processing-time-dataflow-run-streaming.png)

詳しくは、以下を参照してください [データフローはストリーミング宛先に対して実行されます](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-streaming-destinations) 監視ドキュメントの

### ファイルベースの宛先 {#file-based}

データフローをファイルベースの宛先に対して実行する場合、 **[!UICONTROL 処理期間]** エクスポートされるデータのサイズとシステムの負荷によって異なります。 また、データフローがファイルベースの宛先に対して実行されるは、オーディエンスごとに分類されます。

![ファイルベースの宛先用に「処理時間」列がハイライト表示されたデータフロー実行ページの画像。](/help/destinations/assets/ui/details-page/processing-time-dataflow-run-file-based.png)

詳しくは、以下を参照してください [データフローをバッチ（ファイルベース）の宛先に対して実行](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) 監視ドキュメントの

## [!UICONTROL アクティベーションデータ] {#activation-data}

この [!UICONTROL アクティベーションデータ] タブには、宛先にマッピングされたオーディエンスのリストが表示されます。これには、オーディエンスの開始日と終了日（該当する場合）やデータ書き出しに関連するその他の情報（書き出しタイプ、スケジュール、頻度など）が含まれます。 特定のオーディエンスに関する詳細を表示するには、リストから名前を選択します。

>[!TIP]
>
>宛先にマッピングされた属性および ID に関する詳細を表示および編集するには、以下を選択します **[!UICONTROL オーディエンスをアクティベート]** が含まれる [右側のパネル](#right-rail).

![有効化データビューバッチ宛先](../assets/ui/details-page/activation-data-batch.png)

![アクティブ化データビューストリーミング宛先](../assets/ui/details-page/activation-data-streaming.png)

### [!BADGE ベータ版]{type=Informative} アクティベーションフローから複数のオーディエンスを削除 {#bulk-remove}

>[!NOTE]
>
この機能はベータ版で、一部のお客様のみご利用いただけます。 この機能へのアクセス権をリクエストするには、Adobe担当者にお問い合わせください。

既存のアクティベーションフローから複数のオーディエンスを削除するには、オーディエンスを選択してから選択します **[!UICONTROL オーディエンスを削除]**.

![「オーディエンスを削除」オプションをハイライト表示したアクティベーションデータ画面。](../assets/ui/details-page/bulk-remove-audiences.png)

### [!BADGE ベータ版]{type=Informative} 複数のファイルをオンデマンドでバッチ宛先に書き出す {#bulk-export}

>[!NOTE]
>
この機能はベータ版で、一部のお客様のみご利用いただけます。 この機能へのアクセス権をリクエストするには、Adobe担当者にお問い合わせください。

次のことができます [オンデマンドでの複数ファイルの書き出し](../ui/export-file-now.md) から **[!UICONTROL アクティベーションデータ]** ページ。 それには、ファイルをオンデマンドで書き出すオーディエンスを選択し、 **[!UICONTROL 今すぐファイルを書き出し]** 1 回限りの書き出しをトリガーにするコントロール。これにより、選択した各オーディエンスのファイルがバッチ宛先に配信されます。

![「今すぐファイルを書き出し」ボタンを強調表示した画像。](../assets/ui/details-page/bulk-export-file-now.png)

### [!BADGE ベータ版]{type=Informative} バッチ宛先に書き出された複数のオーディエンスのアクティベーションスケジュールを編集します {#bulk-edit-schedule}

>[!NOTE]
>
この機能はベータ版で、一部のお客様のみご利用いただけます。 この機能へのアクセス権をリクエストするには、Adobe担当者にお問い合わせください。

複数のオーディエンスの既存のアクティベーションスケジュールを同時に編集するには、目的のオーディエンスを選択してから、以下を選択します。 **[!UICONTROL スケジュールを編集]**. 書き出しスケジュールを定義または編集する方法について詳しくは、 [オーディエンスの書き出しをスケジュール](../ui/activate-batch-profile-destinations.md#scheduling) セクション。

![複数のオーディエンスのアクティベーションスケジュールを編集するオプションをハイライト表示したアクティベーションデータ画面。](../assets/ui/details-page/bulk-edit-schedule.png)

>[!NOTE]
>
オーディエンスの詳細ページの詳細については、 [セグメント化 UI の概要](../../segmentation/ui/overview.md#segment-details).
