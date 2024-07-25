---
title: クラウドストレージの宛先へのデータセットの書き出し
type: Tutorial
description: Adobe Experience Platform から目的のクラウドストレージの場所にデータセットを書き出す方法を説明します。
exl-id: e89652d2-a003-49fc-b2a5-5004d149b2f4
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '1891'
ht-degree: 47%

---

# クラウドストレージの宛先へのデータセットの書き出し

>[!AVAILABILITY]
>
>* この機能は、Real-Time CDP Prime または Ultimate パッケージ、Adobe Journey OptimizerまたはCustomer Journey Analyticsを購入したお客様が利用できます。 詳しくは、Adobe担当者にお問い合わせください。

この記事では、Experience PlatformUI を使用して、Adobe Experience Platformから目的のクラウドストレージの場所（[!DNL Amazon S3]、SFTP の場所、[!DNL Google Cloud Storage] など）に [ データセット ](/help/catalog/datasets/overview.md) を書き出すために必要なワークフローについて説明します。

Experience PlatformAPI を使用してデータセットを書き出すこともできます。 詳しくは、[ データセット API の書き出しチュートリアル ](/help/destinations/api/export-datasets.md) を参照してください。

## 書き出すことができるデータセット {#datasets-to-export}

書き出すことができるデータセットは、Experience Platformアプリケーション（Real-Time CDP、Adobe Journey Optimizer）、層（Prime または Ultimate）、購入したアドオン（例：Data Distiller）によって異なります。

アプリケーション、製品層、購入したアドオンに応じて、書き出すことができるデータセットのタイプを次の表から理解します。

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
    <td>ソース、Web SDK、Mobile SDK、Analytics Data Connector およびAudience Managerからデータを取り込んだり収集した後、Experience PlatformUI で作成されたプロファイルおよびエクスペリエンスイベントデータセット。</td>
  </tr>
  <tr>
    <td>Ultimate</td>
    <td><ul><li>ソース、Web SDK、Mobile SDK、Analytics Data Connector およびAudience Managerからデータを取り込んだり収集した後、Experience PlatformUI で作成されたプロファイルおよびエクスペリエンスイベントデータセット。</li><li> <a href="https://experienceleague.adobe.com/docs/experience-platform/dashboards/query.html#profile-attribute-datasets"> システム生成プロファイルスナップショットデータセット </a>。</li></td>
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
    <td> ソース、Web SDK、Mobile SDK、Analytics Data Connector およびAudience Managerからデータを取り込んだり収集した後、Experience PlatformUI で作成されたプロファイルおよびエクスペリエンスイベントデータセット。 <br> <p> <b> 可用性に関するメモ：</b> データセットをクラウドに書き出す機能は、リリースの限定的テスト段階にあり、お使いの環境ではまだ使用できない可能性があります。 機能が一般入手可能になったら、このメモは削除されます。 Customer Journey Analyticsリリースプロセスについて詳しくは、Customer Journey Analytics機能リリース <a href="https://experienceleague.adobe.com/docs/analytics-platform/using/releases/releases.html"> 参照してください </a> </p> </td>
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

Experience Platformカタログ内の一部のファイルベース宛先では、オーディエンスのアクティベーションとデータセットの書き出しの両方をサポートしています。

* データを、オーディエンスの関心または選定別にグループ化されたプロファイルに構造化する場合は、オーディエンスのアクティブ化を検討してください。
* また、オーディエンスの関心や選定別にグループ化または構造化されていない未加工のデータセットを書き出そうとしている場合は、データセットの書き出しを検討します。 このデータは、レポート、データサイエンスワークフロー、その他の多くのユースケースに使用できます。 例えば、管理者、データエンジニアまたはアナリストは、Experience Platformからデータをエクスポートしてデータウェアハウスと同期したり、BI 分析ツールや外部 Cloud ML ツールで使用したり、システムに保存して長期的なストレージのニーズに対応したりできます。

このドキュメントには、データセットの書き出しに必要な情報がすべて含まれています。クラウドストレージ宛先またはメールマーケティング宛先に対して *オーディエンス* をアクティブ化する場合は、[ バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化 ](/help/destinations/ui/activate-batch-profile-destinations.md) を参照してください。

## 前提条件 {#prerequisites}

データセットをクラウドストレージ宛先に書き出すには、正常に[宛先に接続されている](./connect-destination.md)必要があります。まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。

### 必要な権限 {#permissions}

データセットを書き出すには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL データセットの表示]** および **[!UICONTROL データセットの宛先の管理とアクティブ化]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 必要な権限を取得するには、[アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせてください。

データセットの書き出しに必要な権限があることと、宛先でデータセットの書き出しがサポートされていることを確認するには、宛先カタログを参照します。 宛先に「**[!UICONTROL アクティブ化]**」または「**[!UICONTROL データセットを書き出し]**」コントロールがある場合、適切な権限を持っています。

## 宛先の選択 {#select-destination}

データセットを書き出すことができる宛先を選択するには、次の手順に従います。

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![カタログコントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/catalog-tab.png)

1. データセットを書き出す宛先に対応するカードで、「**[!UICONTROL アクティブ化]**」または「**[!UICONTROL データセットを書き出し]**」を選択します。

   ![「アクティブ化」コントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/activate-button.png)

1. 「**[!UICONTROL データタイプデータセット]**」を選択し、データセットを書き出す宛先接続を選択して、「**[!UICONTROL 次へ]**」を選択します。 

>[!TIP]
> 
>データセットを書き出す新しい宛先を設定する場合は、「**[!UICONTROL 新しい宛先を設定]**」を選択して、[宛先に接続](/help/destinations/ui/connect-destination.md)ワークフローをトリガーします。

![「データセット」コントロールがハイライト表示された宛先のアクティベーションワークフロー](/help/destinations/assets/ui/export-datasets/select-datatype-datasets.png)

1. **[!UICONTROL データセットを選択]**&#x200B;ビューが表示されます。 次の節に進んで、書き出す[データセットを選択](#select-datasets)します。

## データセットの選択 {#select-datasets}

データセット名の左側にあるチェックボックスを使用して、宛先に書き出すデータセットを選択し、「**[!UICONTROL 次へ]**」を選択します。

![書き出すデータセットを選択できる「データセットを選択」ステップが表示されているデータセット書き出しワークフロー](/help/destinations/assets/ui/export-datasets/select-datasets.png)

## データセット書き出しのスケジュール設定 {#scheduling}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_datasets_exportoptions"
>title="データセットのファイル書き出しオプション"
>abstract="「**増分ファイルの書き出し**」を選択すると、前回の書き出し以降にデータセットに追加されたデータのみを書き出すことができます。<br> 最初の増分ファイル書き出しには、データセット内のすべてのデータが含まれ、バックフィルとして機能します。 以後の増分ファイルには、最初の書き出し以降にデータセットに追加されたデータのみが含まれます。"

**[!UICONTROL スケジュール設定]** ステップでは、データセット書き出しの開始日と書き出しケイデンスを設定できます。

「**[!UICONTROL 増分ファイルの書き出し]**」オプションが自動的に選択されます。 これにより、データセットの完全なスナップショットを表す 1 つまたは複数のファイルの書き出しがトリガーされます。 以降のファイルは、前回の書き出し以降のデータセットへの増分追加です。

>[!IMPORTANT]
>
>最初の増分ファイル書き出しには、データセット内の既存のデータがすべて含まれ、バックフィルとして機能します。 書き出しには、1 つまたは複数のファイルを含めることができます。

![「スケジュール設定」ステップが表示されているデータセット書き出しワークフロー](/help/destinations/assets/ui/export-datasets/export-incremental-datasets.png)

1. 「**[!UICONTROL 頻度]**」セレクターを使用して、書き出しの頻度を選択します。

   * **[!UICONTROL 毎日]**：増分ファイルの書き出しを、毎日 1 回、指定した時刻にスケジュールします。
   * **[!UICONTROL 毎時]**：増分ファイルの書き出しを、3 時間、6 時間、8 時間または 12 時間ごとにスケジュールします。

2. **[!UICONTROL 時間]**&#x200B;セレクターを使用して、ファイルが書き出される時刻を [!DNL UTC] 形式で指定します。

3. **[!UICONTROL 日付]**&#x200B;セレクターを使用して、書き出しが行われる間隔を選択します。現在、書き出しの終了日は設定できません。 詳しくは、[既知の制限事項](#known-limitations)の節を参照してください。

4. 「**[!UICONTROL 次へ]**」を選択して、スケジュールを保存し、**[!UICONTROL レビュー]**&#x200B;ステップに進みます。

>[!NOTE]
> 
>データセット書き出しの場合、ファイル名には事前に設定されたデフォルトの形式が使用され、これを変更することはできません。 書き出されたファイルの詳細と例については、[データセットの正常な書き出しの確認](#verify)の節を参照してください。

## レビュー {#review}

「**[!UICONTROL レビュー]**」ページには、選択内容の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを中断するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または「**[!UICONTROL 完了]**」を選択して選択内容を確定し、宛先へのデータセットの書き出しを開始します。

![レビューステップを表示するデータセット書き出しワークフロー](/help/destinations/assets/ui/export-datasets/review.png)

## データセットの正常な書き出しの確認 {#verify}

データセットを書き出す際、Experience Platformは、指定されたストレージの場所に 1 つまたは複数の `.json` ファイルまたは `.parquet` ファイルを作成します。 指定した書き出しスケジュールに従って、新しいファイルがストレージの場所に格納されます。

Experience Platform は、指定されたストレージの場所にフォルダー構造を作成し、書き出されたデータセットファイルを格納します。 書き出しのたびに、次のパターンに従って新しいフォルダーが作成されます。

`folder-name-you-provided/datasetID/exportTime=YYYYMMDDHHMM`

デフォルトのファイル名はランダムに生成され、書き出されたファイルの名前は必ず一意になります。

### サンプルデータセットファイル {#sample-files}

これらのファイルがストレージの場所に存在すれば、書き出しは成功しています。書き出されたファイルの構造を理解するには、サンプルの [.parquet ファイル](../assets/common/part-00000-tid-253136349007858095-a93bcf2e-d8c5-4dd6-8619-5c662e261097-672704-1-c000.parquet)または [.json ファイル](../assets/common/part-00000-tid-4172098795867639101-0b8c5520-9999-4cff-bdf5-1f32c8c47cb9-451986-1-c000.json)をダウンロードできます。

#### 圧縮されたデータセットファイル {#compressed-dataset-files}

[ 宛先に接続ワークフロー ](/help/destinations/ui/connect-destination.md#file-formatting-and-compression-options) では、以下に示すように、圧縮する書き出されたデータセットファイルを選択できます。

![ データセットを書き出す宛先に接続する際のファイルタイプと圧縮の選択。](/help/destinations/assets/ui/export-datasets/compression-format-datasets.gif)

2 つのファイルタイプを圧縮した場合、ファイル形式に違いがあることに注意してください。

* 圧縮された JSON ファイルを書き出す場合、書き出されるファイルの形式は `json.gz` です
* 圧縮 Parquet ファイルをエクスポートする場合、エクスポートされるファイル形式は `gz.parquet` です

## 宛先からのデータセットの削除 {#remove-dataset}

既存のデータフローからデータセットを削除するには、次の手順に従います。

1. [Experience Platform UI](https://experience.adobe.com/platform/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。上部のヘッダーから「**[!UICONTROL 参照]**」を選択して、既存の宛先データフローを表示します。

   ![宛先接続が表示され残りの部分がぼかされた宛先参照ビュー](../assets/ui/export-datasets/browse-dataset-connections.png)

   >[!TIP]
   > 
   >左上のフィルターアイコン ![フィルターアイコン](/help/images/icons/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられた特定のデータフローを表示できます。

2. **[!UICONTROL アクティベーションデータ]**&#x200B;列から、データセットコントロールを選択して、この書き出しデータフローにマッピングされているすべてのデータセットを表示します。

   ![アクティベーションデータ列で強調表示されている使用可能なデータセットナビゲーションオプション](../assets/ui/export-datasets/go-to-datasets-data.png)

3. [!BADGE Beta] 宛先の **[!UICONTROL アクティベーションデータ]** ページが表示されます。 データセットリストの左側にあるチェックボックスを使用して削除するデータセットを選択し、右側のパネルで「**[!UICONTROL データセットを削除]**」を選択してデータセット削除の確認ダイアログをトリガーします。

   >[!NOTE]
   >
この機能はベータ版で、一部のお客様のみご利用いただけます。 この機能へのアクセス権をリクエストするには、Adobe担当者にお問い合わせください。

   ![右側のパネルに「データセットの削除」コントロールが表示されているデータセットを削除ダイアログ](../assets/ui/export-datasets/bulk-remove-datasets.png)

4. 確認ダイアログで、「**[!UICONTROL 削除]**」を選択すると、宛先への書き出しからデータセットが直ちに削除されます。

   ![データフローからのデータセットの削除を確認するオプションを表示するダイアログ](../assets/ui/export-datasets/remove-dataset-confirm.png)

## データセット書き出し権限 {#licensing-entitlement}

Experience Platformの説明文書を参照して、1 年にエクスポートできるデータの量を確認してください。 例えば、Real-Time CDPの製品説明を [ こちら ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html) で確認できます。

様々なアプリケーションのデータ書き出し権限は追加的ではないことに注意してください。 例えば、Real-Time CDP Ultimate とAdobe Journey Optimizer Ultimate を購入した場合、製品の説明に従って、プロファイルの書き出し権限は 2 つの権限のうち大きい方になります。 ボリューム使用権限は、ライセンス済みプロファイルの合計数を取得し、Real-Time CDP Prime の場合は 500 KB、Real-Time CDP Ultimate の場合は 700 KB を乗じて、使用資格のあるデータのボリュームを判断することで計算されます。

一方、Data Distillerなどのアドオンを購入した場合、データ書き出し制限は、製品層とアドオンの合計を表します。

ライセンスダッシュボードで、契約上の制限に照らしてプロファイルの書き出しを表示および追跡できます。

## 既知の制限事項 {#known-limitations}

データセット書き出しの一般リリースについては、次の制限事項に注意してください。

* 現在、増分ファイルの書き出しのみ可能で、データセット書き出しでは終了日を選択できません。
* 書き出されるファイルの名前は現在、カスタマイズできません。
* API を使用して作成したデータセットは、現在、書き出しには使用できません。
* 宛先に書き出されるデータセットの削除は、現在、UI で禁止されていません。 宛先に書き出されるデータセットは削除しないでください。 データセットを削除する場合は、まず、宛先データフローから[データセットを削除](#remove-dataset)します。
* データセット書き出しの監視指標は、現在、プロファイル書き出しの数値と混在しているので、実際の書き出し数値を反映していません。
* タイムスタンプが 365 日より古いデータは、データセットの書き出しから除外されます。 詳しくは、[ スケジュールされたデータセット書き出しのガードレール ](/help/destinations/guardrails.md#guardrails-for-scheduled-dataset-exports) を参照してください
