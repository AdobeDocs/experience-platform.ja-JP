---
title: （ベータ版）Azure Data Lake Storage Gen2 接続
description: Azure Data Lake Storage Gen2 に接続してセグメントをアクティブ化し、データセットを書き出す方法を説明します。
exl-id: d265a02d-c901-4b39-8714-fe9ecdbb5bb1
source-git-commit: 010818b56154067402a7cd66f489dd2080142e53
workflow-type: tm+mt
source-wordcount: '680'
ht-degree: 90%

---

# （ベータ版）[!DNL Azure Data Lake Storage Gen2] 接続

>[!IMPORTANT]
>
>この宛先は現在ベータ版で、一部のお客様のみご利用いただけます。[!DNL Azure Data Lake Storage Gen2] 接続へのアクセスをリクエストするには、アドビ担当者に連絡し、[!DNL Organization ID] を提供します。

## 概要 {#overview}

このページでは、[[!DNL Azure Data Lake Storage Gen2]](https://learn.microsoft.com/ja-jp/azure/storage/blobs/data-lake-storage-introduction)（[!DNL ADLS Gen2]）データレイクへのライブアウトバウンド接続を作成し、Experience Platform から定期的にデータファイルを書き出す方法を説明します。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性の選択画面で選択したように、該当するスキーマフィールド（例：PPID）と共に、セグメントのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](/help/destinations/ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。 

* **[!UICONTROL URL]**：[!DNL Azure Data Lake Storage Gen2] のエンドポイント。エンドポイントパターンは `abfss://<container>@<accountname>.dfs.core.windows.net` です。
* **[!UICONTROL テナント]**：アプリケーションを含んだテナント情報。
* **[!UICONTROL サービスプリンシパル ID]**：アプリケーションのクライアント ID。
* **[!UICONTROL サービスプリンシパルキー]**：アプリケーションのキー。
* **[!UICONTROL 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 以下の画像で、正しく書式設定された暗号化キーの例を参照してください。

   ![UI での正しく書式設定された PGP キーの例を示す画像](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーへのパス。
* **[!UICONTROL ファイルタイプ]**:書き出すファイルに使用する形式Experience Platformを選択します。 選択時に、 [!UICONTROL CSV] オプションを選択する場合は、 [ファイル形式設定オプションの設定](../../ui/batch-destinations-file-formatting-options.md).
* **[!UICONTROL 圧縮形式]**:書き出したファイルにExperience Platformが使用する圧縮タイプを選択します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントを有効化する手順については、[プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md)を参照してください。

### スケジュール設定 {#scheduling}

**[!UICONTROL スケジュール設定]**&#x200B;手順では、[!DNL Azure Data Lake Storage Gen2] 宛先の[書き出しスケジュールを設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)し、[書き出したファイルの名前を設定](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)することもできます。

### 属性と ID のマッピング {#map}

**[!UICONTROL マッピング]**&#x200B;手順では、プロファイルに書き出す属性および ID フィールドを選択できます。 また、書き出したファイル内のヘッダーを選択して、任意のわかりやすい名前に変更することもできます。詳しくは、「バッチの宛先をアクティベート」UI チュートリアルの[マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)を参照してください。

## （ベータ版）データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットの書き出し設定方法について詳しくは、[データセットの書き出しチュートリアル](/help/destinations/ui/export-datasets.md)を参照してください。

## データの正常な書き出しの検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Azure Data Lake Storage Gen2] ストレージを確認し、書き出されたファイルに想定されるプロファイル母集団が含まれていることを確認してください。
