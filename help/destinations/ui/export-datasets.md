---
title: （ベータ版）クラウドストレージ宛先へのデータセットの書き出し
type: Tutorial
description: Adobe Experience Platform から目的のクラウドストレージの場所にデータセットを書き出す方法を説明します。
exl-id: e89652d2-a003-49fc-b2a5-5004d149b2f4
source-git-commit: d9b59b8a331511e87171f3b9d1163d452ba469be
workflow-type: tm+mt
source-wordcount: '1425'
ht-degree: 88%

---

# （ベータ版）クラウドストレージ宛先へのデータセットの書き出し

>[!IMPORTANT]
>
>* データセットの書き出し機能は現在ベータ版で、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。
>* このベータ版機能では、Real-time Customer Data Platform の[製品説明](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)で定義されている第 1 世代データの書き出しをサポートしています。
>* この機能は、Real-Time CDP Prime および Ultimate パッケージを購入したお客様が利用できます。詳しくは、Adobe担当者にお問い合わせください。

この記事では、エクスポートに必要なワークフローについて説明します [データセット](/help/catalog/datasets/overview.md) Adobe Experience Platformから目的のクラウドストレージの場所 ( [!DNL Amazon S3]、SFTP の場所、または [!DNL Google Cloud Storage] Experience PlatformUI を使用。

また、Experience PlatformAPI を使用してデータセットを書き出すこともできます。 詳しくは、 [データセット API の書き出しチュートリアル](/help/destinations/api/export-datasets.md) を参照してください。

## サポートされる宛先 {#supported-destinations}

現在、以下のスクリーンショットでハイライト表示されているクラウドストレージの宛先にデータセットを書き出すことができます。

![データセットの書き出しをサポートする宛先](/help/destinations/assets/ui/export-datasets/destinations-supporting-dataset-exports.png)

* [[!DNL (Beta) Azure Data Lake Storage Gen2]](../../destinations/catalog/cloud-storage/adls-gen2.md)
* [[!DNL (Beta) Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md)
* [[!DNL (Beta) Google Cloud Storage]](../../destinations/catalog/cloud-storage/google-cloud-storage.md)
* [[!DNL (Beta) Amazon S3]](../../destinations/catalog/cloud-storage/amazon-s3.md#changelog)
* [[!DNL (Beta) Azure Blob]](../../destinations/catalog/cloud-storage/azure-blob.md#changelog)
* [[!DNL (Beta) SFTP]](../../destinations/catalog/cloud-storage/sftp.md#changelog)

## セグメントをアクティブ化するタイミングまたはデータセットを書き出すタイミング {#when-to-activate-segments-or-activate-datasets}

Experience Platform カタログ内の一部のファイルベース宛先では、セグメントのアクティベーションとデータセットの書き出しの両方をサポートしています。

* データを、オーディエンスの関心または選定別にグループ化されたプロファイルに構造化する場合は、セグメントのアクティブ化を検討してください。
* また、オーディエンスの関心や選定別にグループ化または構造化されていない未加工のデータセットを書き出そうとしている場合は、データセットの書き出しを検討します。 このデータは、レポート、データサイエンスワークフロー、コンプライアンス要件への適合、その他多くのユースケースに使用できます。

このドキュメントには、データセットの書き出しに必要な情報がすべて含まれています。クラウドストレージ宛先またはメールマーケティング宛先に対してセグメントをアクティブ化する場合は、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。

## 前提条件 {#prerequisites}

データセットをクラウドストレージ宛先に書き出すには、正常に[宛先に接続されている](./connect-destination.md)必要があります。まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。

### 必要な権限 {#permissions}

データセットを書き出すには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL データセット宛先の管理とアクティブ化]**&#x200B;の各[アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 必要な権限を取得するには、[アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせてください。

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

内 **[!UICONTROL スケジュール]** 手順では、データセットのエクスポートの開始日とエクスポートケイデンスを設定できます。

「**[!UICONTROL 増分ファイルの書き出し]**」オプションが自動的に選択されます。 これにより、書き出しがトリガーされます。最初のファイルがデータセットの完全なスナップショットになり、それ以降のファイルは前回の書き出し以降のデータセットへの増分追加になります。

>[!IMPORTANT]
>
>最初に書き出された増分ファイルには、データセット内の既存のデータがすべて含まれ、バックフィルとして機能します。

![「スケジュール設定」ステップが表示されているデータセット書き出しワークフロー](/help/destinations/assets/ui/export-datasets/export-incremental-datasets.png)

1. 「**[!UICONTROL 頻度]**」セレクターを使用して、書き出しの頻度を選択します。

   * **[!UICONTROL 毎日]**：増分ファイルの書き出しを、毎日 1 回、指定した時刻にスケジュールします。
   * **[!UICONTROL 毎時]**：増分ファイルの書き出しを、3 時間、6 時間、8 時間または 12 時間ごとにスケジュールします。

2. **[!UICONTROL 時間]**&#x200B;セレクターを使用して、ファイルが書き出される時刻を [!DNL UTC] 形式で指定します。

3. **[!UICONTROL 日付]**&#x200B;セレクターを使用して、書き出しが行われる間隔を選択します。なお、この機能のベータ版では、書き出しの終了日を設定できません。 詳しくは、[既知の制限事項](#known-limitations)の節を参照してください。

4. 「**[!UICONTROL 次へ]**」を選択して、スケジュールを保存し、**[!UICONTROL レビュー]**&#x200B;ステップに進みます。

>[!NOTE]
> 
>データセット書き出しの場合、ファイル名には事前に設定されたデフォルトの形式が使用され、これを変更することはできません。 書き出されたファイルの詳細と例については、[データセットの正常な書き出しの確認](#verify)の節を参照してください。

## レビュー {#review}

「**[!UICONTROL レビュー]**」ページには、選択内容の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを中断するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または「**[!UICONTROL 完了]**」を選択して選択内容を確定し、宛先へのデータセットの書き出しを開始します。

![レビューステップを表示するデータセット書き出しワークフロー](/help/destinations/assets/ui/export-datasets/review.png)

## データセットの正常な書き出しの確認 {#verify}

データセットを書き出す際、Experience Platform は、指定されたストレージの場所に `.json` または `.parquet` ファイルを保存します。指定した書き出しスケジュールに従って、新しいファイルがストレージの場所に格納されます。

Experience Platform は、指定されたストレージの場所にフォルダー構造を作成し、書き出されたデータセットファイルを格納します。 書き出しのたびに、次のパターンに従って新しいフォルダーが作成されます。

`folder-name-you-provided/datasetID/exportTime=YYYYMMDDHHMM`

デフォルトのファイル名はランダムに生成され、書き出されたファイルの名前は必ず一意になります。

### サンプルデータセットファイル {#sample-files}

これらのファイルがストレージの場所に存在すれば、書き出しは成功しています。書き出されたファイルの構造を理解するには、サンプルの [.parquet ファイル](../assets/common/part-00000-tid-253136349007858095-a93bcf2e-d8c5-4dd6-8619-5c662e261097-672704-1-c000.parquet)または [.json ファイル](../assets/common/part-00000-tid-4172098795867639101-0b8c5520-9999-4cff-bdf5-1f32c8c47cb9-451986-1-c000.json)をダウンロードできます。

#### 圧縮データセットファイル {#compressed-dataset-files}

内 [宛先ワークフローに接続](/help/destinations/ui/connect-destination.md#file-formatting-and-compression-options)次に示すように、書き出したデータセットファイルを選択して圧縮できます。

![宛先に接続してデータセットを書き出す際のファイルタイプと圧縮の選択。](/help/destinations/assets/ui/export-datasets/compression-format-datasets.gif)

圧縮時のファイル形式の違いに注意してください。

* 圧縮 JSON ファイルを書き出す場合、書き出されるファイル形式は次のようになります。 `json.gz`
* 圧縮 Parquet ファイルを書き出す場合、書き出されるファイル形式は次のようになります。 `gz.parquet`

## 宛先からのデータセットの削除 {#remove-dataset}

既存のデータフローからデータセットを削除するには、次の手順に従います。

1. [Experience Platform UI](https://experience.adobe.com/platform/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。上部のヘッダーから「**[!UICONTROL 参照]**」を選択して、既存の宛先データフローを表示します。

   ![宛先接続が表示され残りの部分がぼかされた宛先参照ビュー](../assets/ui/export-datasets/browse-dataset-connections.png)

   >[!TIP]
   > 
   >左上のフィルターアイコン ![フィルターアイコン](../assets/ui/edit-activation/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられた特定のデータフローを表示できます。

1. **[!UICONTROL アクティベーションデータ]**&#x200B;列から、データセットコントロールを選択して、この書き出しデータフローにマッピングされているすべてのデータセットを表示します。

   ![アクティベーションデータ列で強調表示されている使用可能なデータセットナビゲーションオプション](../assets/ui/export-datasets/go-to-datasets-data.png)

1. 宛先の&#x200B;**[!UICONTROL アクティベーションデータ]**&#x200B;ページが表示されます。 右側のパネルで「**[!UICONTROL データセットを削除]**」を選択すると、データセット削除の確認ダイアログが表示されます。

   ![右側のパネルに「データセットの削除」コントロールが表示されているデータセットを削除ダイアログ](../assets/ui/export-datasets/remove-dataset-control.png)

1. 確認ダイアログで、「**[!UICONTROL 削除]**」を選択すると、宛先への書き出しからデータセットが直ちに削除されます。

   ![データフローからのデータセットの削除を確認するオプションを表示するダイアログ](../assets/ui/export-datasets/remove-dataset-confirm.png)

## 既知の制限事項 {#known-limitations}

データセット書き出しのベータ版リリースでは、次の制限事項に注意してください。

* 現在、データセット宛先に対する管理権限とアクティベート権限を含む単一の権限（**[!UICONTROL データセット宛先の管理とアクティベート]**）が存在します。これらのコントロールは、今後、よりきめ細かい権限に分割される予定です。 データセットの書き出しに必要な権限の完全なリストについては、[必要な権限](#permissions)の節を参照してください。
* 現在、増分ファイルの書き出しのみ可能で、データセット書き出しでは終了日を選択できません。
* 書き出されるファイルの名前は現在、カスタマイズできません。
* 宛先に書き出されるデータセットの削除は、現在、UI で禁止されていません。 宛先に書き出されるデータセットは削除しないでください。 データセットを削除する場合は、まず、宛先データフローから[データセットを削除](#remove-dataset)します。
* データセット書き出しの監視指標は、現在、プロファイル書き出しの数値と混在しているので、実際の書き出し数値を反映していません。
