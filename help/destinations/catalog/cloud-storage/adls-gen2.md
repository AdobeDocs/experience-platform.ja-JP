---
title: Azure Data Lake Storage Gen2 接続
description: Azure Data Lake Storage Gen2 に接続してオーディエンスをアクティブ化し、データセットを書き出す方法を説明します。
last-substantial-update: 2023-07-26T00:00:00Z
exl-id: d265a02d-c901-4b39-8714-fe9ecdbb5bb1
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '954'
ht-degree: 55%

---

# [!DNL Azure Data Lake Storage Gen2] 接続

## 概要 {#overview}

このページでは、[[!DNL Azure Data Lake Storage Gen2]](https://learn.microsoft.com/ja-jp/azure/storage/blobs/data-lake-storage-introduction)（[!DNL ADLS Gen2]）データレイクへのライブアウトバウンド接続を作成し、Experience Platform から定期的にデータファイルを書き出す方法を説明します。

## に接続 [!DNL ADLS Gen2] api または UI を介したストレージ {#connect-api-or-ui}

* に接続するには [!DNL ADLS Gen2] ストレージの場所 Platform ユーザーインターフェイスを使用して、セクションを参照してください [宛先への接続](#connect) および [この宛先に対してオーディエンスをアクティブ化](#activate) 下。
* に接続するには [!DNL ADLS Gen2] ストレージの場所をプログラムで読み取る [Flow Service API チュートリアルを使用して、ファイルベースの宛先に対するオーディエンスをアクティブ化します](../../api/activate-segments-file-based-destinations.md).

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性の選択画面で選択したように、該当するスキーマフィールド（例：PPID）と共に、セグメントのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## データセットを書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットの書き出しを設定する方法について詳しくは、次のチュートリアルを参照してください。

* 方法 [platform ユーザーインターフェイスを使用したデータセットの書き出し](/help/destinations/ui/export-datasets.md).
* 方法 [flow Service API を使用したプログラムによるデータセットの書き出し](/help/destinations/api/export-datasets.md).

## 書き出されたデータのファイル形式 {#file-format}

書き出し時 *オーディエンスデータ*、Platform は `.csv`, `parquet`、または `.json` ファイルは、指定したストレージの場所にあります。 ファイルの詳細については、を参照してください [書き出しでサポートされるファイル形式](../../ui/activate-batch-profile-destinations.md#supported-file-formats-export) Audience Activation チュートリアルの「」の節。

書き出し時 *データセット*、Platform は `.parquet` または `.json` ファイルは、指定したストレージの場所にあります。 ファイルの詳細については、を参照してください [データセットの正常な書き出しの確認](../../ui/export-datasets.md#verify) データセットの書き出しに関するチュートリアルの「」節。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](/help/destinations/ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。 

* **[!UICONTROL URL]**：[!DNL Azure Data Lake Storage Gen2] のエンドポイント。エンドポイントパターンは `abfss://<container>@<accountname>.dfs.core.windows.net` です。
* **[!UICONTROL テナント]**：アプリケーションを含んだテナント情報。
* **[!UICONTROL サービスプリンシパル ID]**：アプリケーションのクライアント ID。
* **[!UICONTROL サービスプリンシパルキー]**：アプリケーションのキー。
* **[!UICONTROL 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 正しい形式の暗号化キーの例については、以下の画像を参照してください。

  ![UI での正しい形式の PGP キーの例を示す画像](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

### 宛先の詳細の入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーへのパス。
* **[!UICONTROL ファイルタイプ]**：書き出したファイルでExperience Platformが使用するフォーマットを選択します。 を選択した場合 [!UICONTROL CSV] オプションを使用すると、次のこともできます [ファイル形式オプションの設定](../../ui/batch-destinations-file-formatting-options.md).
* **[!UICONTROL 圧縮フォーマット]**：書き出したファイルにExperience Platformで使用する圧縮タイプを選択します。
* **[!UICONTROL マニフェストファイルを含める]**：書き出しに、書き出しの場所、書き出しサイズなどに関する情報を含んだマニフェスト JSON ファイルを含める場合は、このオプションをオンに切り替えます。 マニフェストには、形式を使用して名前を付けます `manifest-<<destinationId>>-<<dataflowRunId>>.json`. を表示 [サンプルマニフェストファイル](/help/destinations/assets/common/manifest-d0420d72-756c-4159-9e7f-7d3e2f8b501e-0ac8f3c0-29bd-40aa-82c1-f1b7e0657b19.json). マニフェストファイルには、次のフィールドが含まれています。
   * `flowRunId`：です [データフローの実行](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) 書き出されたファイルを生成した。
   * `scheduledTime`：ファイルが書き出された時間（UTC 単位）。
   * `exportResults.sinkPath`：書き出したファイルを格納するストレージの場所のパス。
   * `exportResults.name`：書き出すファイルの名前。
   * `size`：書き出したファイルのサイズ（バイト単位）。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* エクスポートする *id*、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}

参照： [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md) この宛先に対してオーディエンスをアクティブ化する手順については、を参照してください。

### スケジュール設定 {#scheduling}

**[!UICONTROL スケジュール設定]**&#x200B;手順では、[!DNL Azure Data Lake Storage Gen2] 宛先の[書き出しスケジュールを設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)し、[書き出したファイルの名前を設定](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)することもできます。

### 属性と ID のマッピング {#map}

**[!UICONTROL マッピング]**&#x200B;手順では、プロファイルに書き出す属性および ID フィールドを選択できます。 また、書き出したファイル内のヘッダーを選択して、任意のわかりやすい名前に変更することもできます。詳しくは、「バッチの宛先をアクティベート」UI チュートリアルの[マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)を参照してください。

## データの正常な書き出しの検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Azure Data Lake Storage Gen2] ストレージを確認し、書き出されたファイルに想定されるプロファイル母集団が含まれていることを確認してください。
