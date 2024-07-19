---
title: Google Cloud Storage 接続
description: Google クラウドストレージに接続し、オーディエンスをアクティブ化する方法、またはデータセットを書き出す方法について説明します。
last-substantial-update: 2023-07-26T00:00:00Z
exl-id: ab274270-ae8c-4264-ba64-700b118e6435
source-git-commit: 679c1723965271b6a9c1b5b873cf8ac8de67458d
workflow-type: tm+mt
source-wordcount: '1228'
ht-degree: 62%

---

# [!DNL Google Cloud Storage] 接続

## 概要 {#overview}

[!DNL Google Cloud Storage] へのライブアウトバウンド接続を作成して、Adobe Experience Platform から独自のバケットにデータファイルを定期的に書き出します。

## API または UI を使用した [!DNL Google Cloud Storage] ストレージへの接続 {#connect-api-or-ui}

* Platform ユーザーインターフェイスを使用して [!DNL Google Cloud Storage] ストレージの場所に接続するには、以下の [ 宛先への接続 ](#connect) および [ この宛先に対するオーディエンスのアクティブ化 ](#activate) の節を参照してください。
* [!DNL Google Cloud Storage] ストレージの場所にプログラムで接続するには、[Flow Service API チュートリアルを使用した、ファイルベースの宛先に対するオーディエンスのアクティブ化 ](../../api/activate-segments-file-based-destinations.md) を参照してください。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform[ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性の選択画面で選択したように、セグメントのすべてのメンバーを該当するスキーマフィールドと共に書き出します。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## データセットを書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットの書き出しを設定する方法について詳しくは、次のチュートリアルを参照してください。

* [Platform ユーザーインターフェイスを使用したデータセットの書き出し ](/help/destinations/ui/export-datasets.md) 方法。
* [Flow Service API を使用してプログラムでデータセットを書き出す ](/help/destinations/api/export-datasets.md) 方法。

## 書き出されたデータのファイル形式 {#file-format}

*オーディエンスデータ* を書き出す際、Platform は、指定されたストレージの場所に `.csv`、`parquet` または `.json` ファイルを作成します。 ファイルについて詳しくは、Audience Activation チュートリアルの [ 書き出しでサポートされるファイル形式 ](../../ui/activate-batch-profile-destinations.md#supported-file-formats-export) の節を参照してください。

*データセット* を書き出す際、Platform は、指定されたストレージの場所に `.parquet` または `.json` ファイルを作成します。 ファイルについて詳しくは、データセットの書き出しチュートリアルの [ データセットの書き出しが成功したことを確認する ](../../ui/export-datasets.md#verify) の節を参照してください。

## [!DNL Google Cloud Storage] アカウントを接続するための前提条件の設定 {#prerequisites}

Platform を [!DNL Google Cloud Storage] に接続するには、最初に [!DNL Google Cloud Storage] アカウントの相互運用性を有効にする必要があります。相互運用性設定にアクセスするには、[!DNL Google Cloud Platform] を開き、ナビゲーションパネルの「**[!UICONTROL クラウドストレージ]**」オプションから「**[!UICONTROL 設定]**」を選択します。

![「クラウドストレージ」と「設定」がハイライト表示された Google Cloud Platform ダッシュボード。](../../../sources/images/tutorials/create/google-cloud-storage/nav.png)

**[!UICONTROL 設定]**&#x200B;ページが表示されます。ここから、[!DNL Google] プロジェクト ID に関する情報と [!DNL Google Cloud Storage] アカウントの詳細を確認できます。相互運用性設定にアクセスするには、上部のヘッダーから「**[!UICONTROL 相互運用性]**」を選択します。

![Google Cloud Platform ダッシュボードでハイライト表示された「相互運用性」タブ。](../../../sources/images/tutorials/create/google-cloud-storage/project-access.png)

**[!UICONTROL 相互運用性]**&#x200B;ページには、認証、アクセスキーおよびサービスアカウントに関連付けられたデフォルトプロジェクトに関する情報が含まれています。サービスアカウントの新しいアクセスキー ID と秘密アクセスキーを生成するには、「**[!UICONTROL サービスアカウントのキーを作成]**」を選択します。

![Google Cloud Platform ダッシュボードでハイライト表示された「サービスアカウントのキーを作成」コントロール。](../../../sources/images/tutorials/create/google-cloud-storage/interoperability.png)

新しく生成されたアクセスキー ID と秘密アクセスキーを使用して、[!DNL Google Cloud Storage] アカウントを Platform に接続できます。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](/help/destinations/ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL アクセスキー ID]**：[!DNL Google Cloud Storage] アカウントを Platform に認証するために使用される 61 文字の英数字の文字列。この値の取得方法について詳しくは、上記の[前提条件](#prerequisites)の節を参照してください。
* **[!UICONTROL 秘密アクセスキー]**：[!DNL Google Cloud Storage] アカウントを Platform に認証するために使用される 40 文字の base64 エンコード文字列。この値の取得方法について詳しくは、上記の[前提条件](#prerequisites)の節を参照してください。
* **[!UICONTROL 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式の暗号化キーの例については、以下の画像を参照してください。

  ![UI での正しい形式の PGP キーの例を示す画像](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

これらの値について詳しくは、[Google Cloud Storage の HMAC キー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)ガイドを参照してください。独自のアクセスキー ID と秘密アクセスキーを生成する手順については、[[!DNL Google Cloud Storage] ソースの概要](/help/sources/connectors/cloud-storage/google-cloud-storage.md)を参照してください。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL バケット名]**：この宛先が使用する [!DNL Google Cloud Storage] バケット名を入力します。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする保存先フォルダーのパス。
* **[!UICONTROL ファイルの種類]**：書き出したファイルに使用するExperience Platformの形式を選択します。 [!UICONTROL CSV] オプションを選択する場合、[ ファイル形式オプションを設定 ](../../ui/batch-destinations-file-formatting-options.md) することもできます。
* **[!UICONTROL 圧縮形式]**：書き出したファイルにExperience Platformで使用する圧縮タイプを選択します。
* **[!UICONTROL マニフェストファイルを含める]**：書き出しに、書き出しの場所や書き出しのサイズなどに関する情報を含んだマニフェスト JSON ファイルを含めたい場合は、このオプションをオンに切り替えます。 マニフェストには、形式 `manifest-<<destinationId>>-<<dataflowRunId>>.json` を使用して名前を付けます。 [ サンプル マニフェスト ファイル ](/help/destinations/assets/common/manifest-d0420d72-756c-4159-9e7f-7d3e2f8b501e-0ac8f3c0-29bd-40aa-82c1-f1b7e0657b19.json) を表示します。 マニフェストファイルには、次のフィールドが含まれています。
   * `flowRunId`：書き出されたファイルを生成した [ データフロー実行 ](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations)。
   * `scheduledTime`: ファイルが書き出された時間（UTC 単位）。
   * `exportResults.sinkPath`：書き出されたファイルが格納されるストレージの場所のパス。
   * `exportResults.name`：書き出すファイルの名前。
   * `size`：書き出されたファイルのサイズ（バイト単位）。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

### 必要な [!DNL Google Cloud Storage] 権限 {#required-google-cloud-storage-permission}

[!DNL Google Cloud Storage] ストレージの場所に正常に接続してデータを書き出すには、バケットに対して次の [!DNL Google Cloud Storage] 権限が必要です。

*`orgpolicy.policy.get`
*`resourcemanager.projects.get`
*`resourcemanager.projects.list`
*`storage.managedFolders.create`
*`storage.multipartUploads.abort`
*`storage.multipartUploads.create`
*`storage.multipartUploads.listParts`
*`storage.objects.create`
*`storage.objects.list`

詳しくは、[!DNL Google Cloud Storage] の [ アクセス制御と権限 ](https://cloud.google.com/storage/docs/access-control/iam-permissions) を参照してください。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[ プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。

### スケジュール設定

**[!UICONTROL スケジュール設定]**&#x200B;手順では、[!DNL Google Cloud Storage] 宛先の[書き出しスケジュールを設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)し、[書き出したファイルの名前を設定](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)することもできます。

### 属性と ID のマッピング {#map}

**[!UICONTROL マッピング]**&#x200B;手順では、プロファイルに書き出す属性および ID フィールドを選択できます。 また、書き出したファイル内のヘッダーを選択して、任意のわかりやすい名前に変更することもできます。詳しくは、「バッチの宛先をアクティベート」UI チュートリアルの[マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)を参照してください。

## データの正常な書き出しの検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Google Cloud Storage] バケットを確認し、書き出したファイルに、期待されたプロファイルの母集団が含まれていることを確認します。

## IP アドレスの許可リスト {#ip-address-allow-list}

許可リストにAdobe許可リストに加える IP を登録する必要がある場合は、[IP アドレス ](ip-address-allow-list.md) を参照してください。