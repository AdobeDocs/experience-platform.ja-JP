---
title: Azure Blob 接続
description: Azure Blob Storage へのライブアウトバウンド接続を作成して、Adobe Experience Platform から CSV データファイルを定期的に書き出します。
exl-id: 8099849b-e3d2-48a5-902a-ca5a5ec88207
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 55%

---

# [!DNL Azure Blob] 接続

## 宛先の変更ログ {#changelog}

2023 年 7 月のExperience Platformリリースでは、 [!DNL Azure Blob] 宛先は、以下に示す新機能を提供します。

* [データセット書き出しのサポート](/help/destinations/ui/export-datasets.md)。
* 追加の[ファイル命名オプション](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)。
* 書き出されたファイルにカスタムファイルヘッダーを設定する機能（[マッピングステップの改善](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)による）
* [書き出された CSV データファイルの形式をカスタマイズする機能](/help/destinations/ui/batch-destinations-file-formatting-options.md)。

## 概要 {#overview}

[!DNL Azure Blob]（以下「[!DNL Blob]」）は、Microsoft のクラウド用オブジェクトストレージソリューションです。このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Blob] 宛先を作成する手順を説明します。

## に接続 [!UICONTROL Azure Blob] api または UI を介したストレージ {#connect-api-or-ui}

* に接続するには [!UICONTROL Azure Blob] ストレージの場所 Platform ユーザーインターフェイスを使用して、セクションを参照してください [宛先への接続](#connect) および [この宛先に対してオーディエンスをアクティブ化](#activate) 下。
* に接続するには [!UICONTROL Azure Blob] ストレージの場所をプログラムで読み取る [Flow Service API チュートリアルを使用して、ファイルベースの宛先に対するオーディエンスをアクティブ化します](../../api/activate-segments-file-based-destinations.md).

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効ながある場合 [!DNL Blob] 宛先の場合は、このドキュメントの残りの部分をスキップして、に関するチュートリアルに進むことができます。 [宛先へのオーディエンスのアクティブ化](../../ui/activate-batch-profile-destinations.md).

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
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
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

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_blob_rsa"
>title="RSA 公開鍵"
>abstract="必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式のキーの例については、以下のドキュメントリンクを参照してください。"

宛先を認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL 接続文字列]**：Blob ストレージのデータにアクセスするには、接続文字列が必要です。[!DNL Blob] 接続文字列のパターンは `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}` で始まります。
   * [!DNL Blob] 接続文字列の設定について詳しくは、Microsoftドキュメントの [Azure Storage アカウントの接続文字列を構成する](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-configure-connection-string#configure-a-connection-string-for-an-azure-storage-account)を参照してください。
* **[!UICONTROL 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式の暗号化キーの例については、以下の画像を参照してください。

  ![UI での正しい形式の PGP キーの例を示す画像](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

### 宛先の詳細の入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**：この宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーへのパス。
* **[!UICONTROL コンテナ]**：の名前を入力します [!DNL Azure Blob Storage] この宛先で使用されるコンテナ。
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

## データの正常な書き出しの検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Azure Blob] ストレージを確認し、書き出されたファイルに想定されるプロファイル母集団が含まれていることを確認してください。