---
title: クラウドストレージの宛先へのデータセットの書き出し
type: Tutorial
description: Adobe Experience Platform から目的のクラウドストレージの場所にデータセットを書き出す方法を説明します。
exl-id: e89652d2-a003-49fc-b2a5-5004d149b2f4
source-git-commit: 69a1ae08fefebb7fed54564ed06f42af523d2903
workflow-type: tm+mt
source-wordcount: '2656'
ht-degree: 25%

---

# クラウドストレージの宛先へのデータセットの書き出し

>[!AVAILABILITY]
>
>この機能は、Real-Time CDP PrimeまたはUltimate パッケージ、Adobe Journey Optimizer、Customer Journey Analyticsを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。

>[!IMPORTANT]
>
>**アクション項目**:[2024 年 9 月リリースのExperience Platform](/help/release-notes/latest/latest.md#destinations) では、データセットデータフローの書き出し `endTime` 日を設定するオプションが導入されました。 Adobeでは、*2024 年 11 月 1 日（PT）より前* に作成されたすべてのデータセット書き出しデータフローに対して、2025 年 9 月 1 日（PT）のデフォルトの終了日が導入されました。
>
>これらのデータフローのいずれについても、終了日より前に、データフローの終了日を手動で更新する必要があります。さもないと、書き出しはその日に停止します。 Experience Platform UI を使用して、2025 年 9 月 1 日（PT）に停止に設定されるデータフローを確認します。
>
>データセット書き出しデータフローの終了日の編集方法については、[ スケジュールの節 ](#scheduling) を参照してください。

この記事では、Adobe Experience Platform Experience Platform UI を使用して、目的のクラウドストレージの場所（[、SFTP の場所、](/help/catalog/datasets/overview.md) など）に [!DNL Amazon S3] データセット [!DNL Google Cloud Storage] を書き出すために必要なワークフローについて説明します。

Experience Platform API を使用してデータセットを書き出すこともできます。 詳しくは、[ データセット API の書き出しチュートリアル ](/help/destinations/api/export-datasets.md) を参照してください。

## 書き出すことができるデータセット {#datasets-to-export}

書き出すことができるデータセットは、Experience Platform アプリケーション（Real-Time CDP、Adobe Journey Optimizer）、層（PrimeまたはUltimate）、購入したアドオン（例：Data Distiller）によって異なります。

次の表を使用して、アプリケーション、製品層、購入したアドオンに応じて、書き出すことができるデータセットのタイプを理解します。

<table>
<thead>
  <tr>
    <th>アプリケーション / アドオン</th>
    <th>層</th>
    <th>書き出すことができるデータセット</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td rowspan="2">Real-Time CDP</td>
    <td>Prime</td>
    <td>ソース、Web SDK、Mobile SDK、Analytics Data Connector およびAudience Managerを使用してデータを取り込みまたは収集した後、Experience Platform UI で作成されたプロファイルおよびエクスペリエンスイベントデータセット。</td>
  </tr>
  <tr>
    <td>Ultimate</td>
    <td><ul><li>ソース、Web SDK、Mobile SDK、Analytics Data Connector およびAudience Managerを使用してデータを取り込みまたは収集した後、Experience Platform UI で作成されたプロファイルおよびエクスペリエンスイベントデータセット。</li><li> <a href="https://experienceleague.adobe.com/docs/experience-platform/dashboards/query.html#profile-attribute-datasets"> システム生成プロファイルスナップショットデータセット </a>。</li></td>
  </tr>
  <tr>
    <td rowspan="2">Adobe Journey Optimizer</td>
    <td>Prime</td>
    <td><a href="https://experienceleague.adobe.com/docs/journey-optimizer/using/data-management/datasets/export-datasets.html#datasets"> Adobe Journey Optimizer</a> ドキュメントを参照してください。</td>
  </tr>
  <tr>
    <td>Ultimate</td>
    <td><a href="https://experienceleague.adobe.com/docs/journey-optimizer/using/data-management/datasets/export-datasets.html#datasets"> Adobe Journey Optimizer</a> ドキュメントを参照してください。</td>
  </tr>
  <tr>
    <td>Customer Journey Analytics</td>
    <td>すべて</td>
    <td> ソース、Web SDK、Mobile SDK、Analytics Data Connector およびAudience Managerを使用してデータを取り込みまたは収集した後、Experience Platform UI で作成されたプロファイルおよびエクスペリエンスイベントデータセット。</td>
  </tr>
  <tr>
    <td>Data Distiller</td>
    <td>Data Distiller（アドオン）</td>
    <td>クエリサービスを通じて作成された派生データセット。</td>
  </tr>
</tbody>
</table>

## ビデオチュートリアル {#video-tutorial}

このページで説明されているワークフローのエンドツーエンドの説明、データセットの書き出し機能を使用するメリット、推奨されるユースケースについては、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3424392/)

## サポートされる宛先 {#supported-destinations}

現在、スクリーンショットでハイライト表示され、以下に示されているクラウドストレージの宛先にデータセットを書き出すことができます。

![ データセット書き出しをサポートする宛先を示した宛先カタログページ ](/help/destinations/assets/ui/export-datasets/destinations-supporting-dataset-exports.png)

* [[!DNL Azure Data Lake Storage Gen2]](../../destinations/catalog/cloud-storage/adls-gen2.md)
* [[!DNL Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md)
* [[!DNL Google Cloud Storage]](../../destinations/catalog/cloud-storage/google-cloud-storage.md)
* [[!DNL Amazon S3]](../../destinations/catalog/cloud-storage/amazon-s3.md#changelog)
* [[!DNL Azure Blob]](../../destinations/catalog/cloud-storage/azure-blob.md#changelog)
* [[!DNL SFTP]](../../destinations/catalog/cloud-storage/sftp.md#changelog)

## オーディエンスをアクティブ化するか、データセットを書き出すタイミング {#when-to-activate-audiences-or-activate-datasets}

Experience Platform カタログ内の一部のファイルベース宛先では、オーディエンスのアクティベーションとデータセットの書き出しの両方をサポートしています。

* データを、オーディエンスの関心または選定別にグループ化されたプロファイルに構造化する場合は、オーディエンスのアクティブ化を検討してください。
* また、オーディエンスの関心や選定別にグループ化または構造化されていない未加工のデータセットを書き出そうとしている場合は、データセットの書き出しを検討します。 このデータは、レポート、データサイエンスワークフロー、その他の多くのユースケースに使用できます。 例えば、管理者、データエンジニアまたはアナリストは、Experience Platformからデータをエクスポートしてデータウェアハウスと同期したり、BI 分析ツールや外部 Cloud ML ツールで使用したり、システムに長期保存のニーズに応じて保存したりできます。

このドキュメントには、データセットの書き出しに必要な情報がすべて含まれています。クラウドストレージ宛先またはメールマーケティング宛先に対して *オーディエンス* をアクティブ化する場合は、[ バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化 ](/help/destinations/ui/activate-batch-profile-destinations.md) を参照してください。

## 前提条件 {#prerequisites}

データセットを書き出すには、次の前提条件に注意してください。

* データセットをクラウドストレージ宛先に書き出すには、正常に[宛先に接続されている](./connect-destination.md)必要があります。まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。
* リアルタイム顧客プロファイルで使用するには、プロファイルデータセットを有効にする必要があります。 このオプションを有効にする方法については、[ 詳細情報 ](/help/ingestion/tutorials/ingest-batch-data.md#enable-for-profile) を参照してください。

### 必要な権限 {#permissions}

データセットを書き出すには、**[!UICONTROL View Destinations]**、**[!UICONTROL View Datasets]**、**[!UICONTROL Manage and Activate Dataset Destinations]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 必要な権限を取得するには、[アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせてください。

データセットの書き出しに必要な権限があることと、宛先でデータセットの書き出しがサポートされていることを確認するには、宛先カタログを参照します。 宛先に **[!UICONTROL Activate]** または **[!UICONTROL Export datasets]** コントロールがある場合、適切な権限を持っています。

## 宛先の選択 {#select-destination}

データセットを書き出すことができる宛先を選択するには、次の手順に従います。

1. **[!UICONTROL Connections > Destinations]** に移動して、「**[!UICONTROL Catalog]**」タブを選択します。

   ![カタログコントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/catalog-tab.png)

1. データセットを書き出す宛先に対応するカードで、「**[!UICONTROL Activate]**」または「**[!UICONTROL Export datasets]**」を選択します。

   ![「アクティブ化」コントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/activate-button.png)

1. 「**[!UICONTROL Data type Datasets]**」を選択し、データセットを書き出す宛先接続を選択して、「**[!UICONTROL Next]**」を選択します。

>[!TIP]
> 
>データセットを書き出す新しい宛先を設定する場合は、「**[!UICONTROL Configure new destination]**」を選択して [ 宛先に接続 ](/help/destinations/ui/connect-destination.md) ワークフローをトリガーします。

![「データセット」コントロールがハイライト表示された宛先のアクティベーションワークフロー](/help/destinations/assets/ui/export-datasets/select-datatype-datasets.png)

1. **[!UICONTROL Select datasets]** ビューが表示されます。 次の節に進んで、書き出す[データセットを選択](#select-datasets)します。

## データセットの選択 {#select-datasets}

データセット名の左側にあるチェックボックスを使用して、宛先に書き出すデータセットを選択し、「**[!UICONTROL Next]**」を選択します。

![書き出すデータセットを選択できる「データセットを選択」ステップが表示されているデータセット書き出しワークフロー](/help/destinations/assets/ui/export-datasets/select-datasets.png)

## データセット書き出しのスケジュール設定 {#scheduling}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_datasets_exportoptions"
>title="データセットのファイル書き出しオプション"
>abstract="「**増分ファイルの書き出し**」を選択すると、前回の書き出し以降にデータセットに追加されたデータのみを書き出すことができます。<br> 最初の増分ファイル書き出しには、データセット内のすべてのデータが含まれ、バックフィルとして機能します。 それ以後の増分ファイルには、最初の書き出し以降にデータセットに追加されたデータのみが含まれます。<br>各書き出しで各データセットの完全なメンバーシップを書き出すには、「**完全ファイルを書き出し**」を選択します。 "

>[!CONTEXTUALHELP]
>id="dataset_dataflow_needs_schedule_end_date_header"
>title="このデータフローの終了日を更新"
>abstract="このデータフローの終了日を更新"

>[!CONTEXTUALHELP]
>id="dataset_dataflow_needs_schedule_end_date_body"
>title="このデータフロー本文の終了日を更新"
>abstract="この宛先が最近更新されたので、データフローには終了日が必要になりました。アドビでは、デフォルトの終了日を 2025年9月1日（PT）に設定しています。希望の終了日に更新してください。更新しない場合、データの書き出しがデフォルトの日付で停止します。"

**[!UICONTROL Scheduling]** の手順を使用して、次の操作を行います。

* 開始日と終了日、およびデータセット書き出しの書き出しケイデンスを設定します。
* 書き出したデータセットファイルで、データセットの完全なメンバーシップを書き出すか、書き出し発生のたびにメンバーシップに対する増分変更のみを書き出すかを設定します。
* データセットを書き出すストレージの場所のフォルダーパスをカスタマイズします。 詳しくは、書き出しフォルダーパスの編集 [ 方法を参照してください ](#edit-folder-path)。

ページの **[!UICONTROL Edit schedule]** コントロールを使用して、書き出しの書き出しケイデンスを編集し、完全ファイルと増分ファイルのどちらを書き出すかを選択します。

![ スケジュール設定ステップでハイライト表示されたスケジュール管理を編集 ](/help/destinations/assets/ui/export-datasets/edit-schedule-control-highlight.png)

デフォルトでは、「**[!UICONTROL Export incremental files]**」オプションが選択されています。 これにより、データセットの完全なスナップショットを表す 1 つまたは複数のファイルの書き出しがトリガーされます。 以降のファイルは、前回の書き出し以降のデータセットへの増分追加です。 **[!UICONTROL Export full files]** を選択することもできます。 この場合、データセットの 1 回限りの完全書き出しの頻度 **[!UICONTROL Once]** を選択します。

>[!IMPORTANT]
>
>最初の増分ファイル書き出しには、データセット内の既存のデータがすべて含まれ、バックフィルとして機能します。 書き出しには、1 つまたは複数のファイルを含めることができます。

![「スケジュール設定」ステップが表示されているデータセット書き出しワークフロー](/help/destinations/assets/ui/export-datasets/export-incremental-datasets.png)

1. **[!UICONTROL Frequency]** セレクターを使用して、書き出しの頻度を選択します。

   * **[!UICONTROL Daily]**：増分ファイルの書き出しを、毎日 1 回指定した時刻にスケジュールします。
   * **[!UICONTROL Hourly]**：増分ファイルの書き出しを、3 時間、6 時間、8 時間または 12 時間ごとにスケジュールします。

2. **[!UICONTROL Time]** セレクターを使用して、ファイルが書き出され [!DNL UTC] 時刻を形式を指定します。

3. **[!UICONTROL Date]** セレクターを使用して、書き出しが行われる間隔を選択します。

4. 「**[!UICONTROL Save]**」を選択して、スケジュールを保存し、**[!UICONTROL Review]** の手順に進みます。

>[!NOTE]
> 
>データセット書き出しの場合、ファイル名には事前に設定されたデフォルトの形式が使用され、これを変更することはできません。 書き出されたファイルの詳細と例については、[データセットの正常な書き出しの確認](#verify)の節を参照してください。

## フォルダーパスの編集 {#edit-folder-path}

>[!CONTEXTUALHELP]
>id="destinations_folder_name_template"
>title="フォルダーパスの編集"
>abstract="提供されている複数のマクロを使用して、データセットが書き出されるフォルダーパスをカスタマイズします。"

>[!CONTEXTUALHELP]
>id="destinations_folder_name_template_preview"
>title="データセットフォルダーパスのプレビュー"
>abstract="このウィンドウで追加したマクロに基づいて、ストレージの場所に作成されるフォルダー構造のプレビューを取得します。"

「**[!UICONTROL Edit folder path]**」を選択して、書き出されたデータセットが格納されるストレージの場所のフォルダー構造をカスタマイズします。

![ スケジュール設定ステップでハイライト表示された「フォルダーパスを編集」コントロール ](/help/destinations/assets/ui/export-datasets/edit-folder-path.png)

使用可能な複数のマクロを使用して、目的のフォルダー名をカスタマイズできます。 マクロをダブルクリックしてフォルダーパスに追加し、マクロ間で `/` を使用してフォルダーを区切ります。

![ カスタムフォルダーモーダルウィンドウでハイライト表示されたマクロの選択。](/help/destinations/assets/ui/export-datasets/custom-folder-path-macros.png)

目的のマクロを選択すると、ストレージの場所に作成されるフォルダー構造のプレビューを確認できます。 フォルダー構造の最初のレベルは、データセットを書き出すために **[!UICONTROL Folder path]** 宛先に接続 [ した際に示した ](/help/destinations/ui/connect-destination.md##set-up-connection-parameters) を表します。

![ カスタムフォルダーモーダルウィンドウでハイライト表示されたフォルダーパスのプレビュー。](/help/destinations/assets/ui/export-datasets/custom-folder-path-preview.png)

## レビュー {#review}

**[!UICONTROL Review]** のページには、選択内容の概要が表示されます。 「**[!UICONTROL Cancel]**」を選択してフローを中断する **[!UICONTROL Back]**、設定を変更する、または「**[!UICONTROL Finish]**」を選択して選択内容を確認し、宛先へのデータセットの書き出しを開始します。

![レビューステップを表示するデータセット書き出しワークフロー](/help/destinations/assets/ui/export-datasets/review.png)

## データセットの正常な書き出しの確認 {#verify}

データセットを書き出す際、Experience Platformは、指定されたストレージの場所に 1 つまたは複数の `.json` ファイルまたは `.parquet` ファイルを作成します。 指定した書き出しスケジュールに従って、新しいファイルがストレージの場所に格納されます。

Experience Platform は、指定されたストレージの場所にフォルダー構造を作成し、書き出されたデータセットファイルを格納します。 デフォルトのフォルダー書き出しパターンを以下に示しますが、[ 好みのマクロを使用してフォルダー構造をカスタマイズする ](#edit-folder-path) ことができます。

>[!TIP]
> 
>このフォルダー構造の最初のレベル（`folder-name-you-provided`）は、データセットを書き出すために **[!UICONTROL Folder path]** 宛先に接続 [ した際に指定した ](/help/destinations/ui/connect-destination.md##set-up-connection-parameters) を表します。

`folder-name-you-provided/datasetID/exportTime=YYYYMMDDHHMM`

デフォルトのファイル名はランダムに生成され、書き出されたファイルの名前は必ず一意になります。

### サンプルデータセットファイル {#sample-files}

これらのファイルがストレージの場所に存在すれば、書き出しは成功しています。書き出されたファイルの構造を理解するには、サンプルの [.parquet ファイル](../assets/common/part-00000-tid-253136349007858095-a93bcf2e-d8c5-4dd6-8619-5c662e261097-672704-1-c000.parquet)または [.json ファイル](../assets/common/part-00000-tid-4172098795867639101-0b8c5520-9999-4cff-bdf5-1f32c8c47cb9-451986-1-c000.json)をダウンロードできます。

#### 圧縮されたデータセットファイル {#compressed-dataset-files}

[ 宛先に接続ワークフロー ](/help/destinations/ui/connect-destination.md#file-formatting-and-compression-options) では、以下に示すように、圧縮する書き出されたデータセットファイルを選択できます。

![ データセットを書き出す宛先に接続する際のファイルタイプと圧縮の選択。](/help/destinations/assets/ui/export-datasets/compression-format-datasets.gif)

2 つのファイルタイプを圧縮した場合、ファイル形式に違いがあることに注意してください。

* 圧縮された JSON ファイルを書き出す場合、書き出されるファイルの形式は `json.gz` です。 書き出される JSON の形式は NDJSON であり、ビッグデータエコシステムの標準の交換形式です。 Adobeでは、書き出されたファイルを読み取るために NDJSON 互換のクライアントを使用することをお勧めします。
* 圧縮 Parquet ファイルをエクスポートする場合、エクスポートされるファイル形式は `gz.parquet` です

JSON ファイルへの書き出しはサポートされています *圧縮モードのみ*。 Parquet ファイルへの書き出しは、圧縮および非圧縮モードでサポートされます。

## 宛先からのデータセットの削除 {#remove-dataset}

既存のデータフローからデータセットを削除するには、次の手順に従います。

1. [Experience Platform UI にログインし ](https://experience.adobe.com/platform/) 左側のナビゲーションバーから「**[!UICONTROL Destinations]**」を選択します。 上部のヘッダーから「**[!UICONTROL Browse]**」を選択して、既存の宛先データフローを表示します。

   ![宛先接続が表示され残りの部分がぼかされた宛先参照ビュー](../assets/ui/export-datasets/browse-dataset-connections.png)

   >[!TIP]
   > 
   >左上のフィルターアイコン ![フィルターアイコン](/help/images/icons/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられた特定のデータフローを表示できます。

2. **[!UICONTROL Activation data]** 列から、データセットコントロールを選択して、この書き出しデータフローにマッピングされているすべてのデータセットを表示します。

   ![アクティベーションデータ列で強調表示されている使用可能なデータセットナビゲーションオプション](../assets/ui/export-datasets/go-to-datasets-data.png)

3. 宛先の **[!UICONTROL Activation data]** ページが表示されます。 データセットリストの左側にあるチェックボックスを使用して、削除するデータセットを選択し、右側のパネルで「**[!UICONTROL Remove datasets]**」を選択して、データセット削除の確認ダイアログをトリガーします。

   ![右側のパネルに「データセットの削除」コントロールが表示されているデータセットを削除ダイアログ](../assets/ui/export-datasets/bulk-remove-datasets.png)

4. 確認ダイアログで、「**[!UICONTROL Remove]**」を選択すると、宛先への書き出しからデータセットが直ちに削除されます。

   ![データフローからのデータセットの削除を確認するオプションを表示するダイアログ](../assets/ui/export-datasets/remove-dataset-confirm.png)

## データセット書き出し権限 {#licensing-entitlement}

1 年にエクスポートできるExperience Platform アプリケーションのデータの量については、製品説明ドキュメントを参照してください。 例えば、Real-Time CDPの製品説明を [ こちら ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html) で確認できます。

様々なアプリケーションのデータ書き出し権限は追加的ではないことに注意してください。 例えば、Real-Time CDP UltimateとAdobe Journey Optimizer Ultimateを購入した場合、製品の説明に従って、プロファイルの書き出し権限は 2 つの権限のうち大きい方になります。 ボリューム使用権限は、ライセンス済みプロファイルの合計数を取得し、Real-Time CDP Ultimateの場合は 500 KB、Real-Time CDP Primeの場合は 700 KB を乗じて、使用資格のあるデータのボリュームを判断することで計算されます。

一方、Data Distillerなどのアドオンを購入した場合、データ書き出し制限は、製品層とアドオンの合計を表します。

[ ライセンス使用状況ダッシュボード ](/help/landing/license-usage-and-guardrails/license-usage-dashboard.md) で、契約上の制限に照らしてプロファイルの書き出しを表示および追跡できます。

## 既知の制限事項 {#known-limitations}

データセット書き出しの一般リリースについては、次の制限事項に注意してください。

* Experience Platformでは、小さなデータセットでも、複数のファイルを書き出す場合があります。 データセットの書き出しは、システム間の統合を目的として設計され、パフォーマンスに最適化されているため、書き出されるファイルの数をカスタマイズすることはできません。
* 書き出すファイルの名前は現在、カスタマイズできません。
* 宛先に書き出されるデータセットの削除は、現在、UI で禁止されていません。 宛先に書き出されるデータセットは削除しないでください。 データセットを削除する場合は、まず、宛先データフローから[データセットを削除](#remove-dataset)します。
* データセット書き出しの監視指標は、現在、プロファイル書き出しの数値と混在しているので、実際の書き出し数値を反映していません。
* タイムスタンプが 365 日より古いデータは、データセットの書き出しから除外されます。 詳しくは、[ スケジュールされたデータセット書き出しのガードレール ](/help/destinations/guardrails.md#guardrails-for-scheduled-dataset-exports) を参照してください

## よくある質問 {#faq}

**フォルダーパスとして `/` に保存するだけの場合、フォルダーのないファイルを生成することはできますか？ また、フォルダーパスが不要な場合、名前が重複するファイルはどのようにフォルダーまたは場所に生成されますか？**

+++回答
2024 年 9 月のリリース以降、フォルダー名をカスタマイズし、`/` を使用して同じフォルダー内のすべてのデータセットのファイルを書き出すこともできます。 異なるデータセットに属するシステム生成ファイル名が同じフォルダーに混在するので、Adobeは複数のデータセットを書き出す宛先に対してはこの方法を推奨しません。
+++

**マニフェストファイルを 1 つのフォルダーに、データファイルを別のフォルダーにルーティングできますか？**

+++回答
いいえ。マニフェストファイルを別の場所にコピーする機能はありません。
+++

**ファイル配信のシーケンスやタイミングを制御することはできますか？**

+++回答
書き出しをスケジュールするためのオプションがあります。 ファイルのコピーを遅延または順序付けするオプションはありません。 作成されたらすぐに、ストレージの場所にコピーされます。
+++

**マニフェストファイルにはどのような形式がありますか？**

+++回答
マニフェストファイルは.json 形式です。
+++

**マニフェストファイルに対して API は使用できますか？**

+++回答
マニフェストファイルに使用できる API はありませんが、書き出しを構成するファイルのリストが含まれています。
+++

**マニフェストファイル（レコード数）に詳細を追加することはできますか？ その場合、方法を教えてください。**

+++回答
マニフェストファイルに情報を追加する可能性はありません。 レコード数は、`flowRun` エンティティを介して使用できます（API 経由でクエリ可能）。 詳しくは、宛先の監視を参照してください。
+++

**データファイルはどのように分割されますか？ ファイルあたりのレコード数**

+++回答
データファイルは、Experience Platform データレイクのデフォルトのパーティションごとに分割されます。 データセットが大きいほど、パーティション数は多くなります。 デフォルトのパーティション化は、読み取り用に最適化されているので、ユーザーは設定できません。
+++

**しきい値（ファイルあたりのレコード数）を設定できますか？**

+++回答
いいえ、できません。
+++

**最初の送信が無効な場合、データセットを再送信するにはどうすればよいですか？**

+++回答
ほとんどのタイプのシステムエラーでは、再試行は自動的に行われます。
+++
