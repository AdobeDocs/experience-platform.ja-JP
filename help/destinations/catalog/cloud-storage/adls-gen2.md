---
title: （ベータ版）Azure Data Lake Storage Gen2 接続
description: Azure Data Lake Storage Gen2 に接続してセグメントをアクティブ化し、データセットを書き出す方法を説明します。
exl-id: d265a02d-c901-4b39-8714-fe9ecdbb5bb1
source-git-commit: f841b27a2d2700b0b68a386b89d1a5c62d3910ff
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 39%

---

# （ベータ版） [!DNL Azure Data Lake Storage Gen2] 接続

>[!IMPORTANT]
>
>この宛先は現在ベータ版で、限られた数のお客様のみが利用できます。 [!DNL Azure Data Lake Storage Gen2] 接続へのアクセスをリクエストするには、アドビ担当者に連絡し、[!DNL Organization ID] を提供します。

## 概要 {#overview}

このページを読んで、 [[!DNL Azure Data Lake Storage Gen2]](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction) ([!DNL ADLS Gen2]) データレイクを使用し、データファイルを定期的にExperience Platformから書き出します。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、該当するスキーマフィールド（PPID など）と共に、 [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳細を表示 [バッチファイルベースの宛先](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

## 前提条件 {#prerequisites}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](/help/destinations/ui/connect-destination.md)の手順に従ってください。宛先設定ワークフローで、以下の 2 つのセクションにリストするフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

* **[!UICONTROL URL]**:のエンドポイント [!DNL Azure Data Lake Storage Gen2]. エンドポイントパターンは次のとおりです。 `https://<accountname>.dfs.core.windows.net`.
* **[!UICONTROL テナント]**:アプリケーションを含むテナント情報。
* **[!UICONTROL サービスプリンシパル ID]**:アプリケーションのクライアント ID。
* **[!UICONTROL サービスプリンシパルキー]**:アプリケーションのキー。
* **[!UICONTROL 暗号化キー]**:必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 以下の画像で、正しく書式設定された暗号化キーの例を参照してください。

   ![UI での正しく書式設定された PGP キーの例を示す画像](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。


* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする保存先フォルダーのパスを入力します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

### スケジュール設定 {#scheduling}

内 **[!UICONTROL スケジュール]** ステップ [書き出しスケジュールの設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling) の [!DNL Azure Data Lake Storage Gen2] 宛先と [書き出したファイルの名前を設定する](/help/destinations/ui/activate-batch-profile-destinations.md#file-names).

### 属性と ID のマッピング {#map}

内 **[!UICONTROL マッピング]** 手順では、プロファイルに書き出す属性および id フィールドを選択できます。 また、書き出したファイルのヘッダーを、任意のわかりやすい名前に変更するように選択することもできます。 詳しくは、 [マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) （「バッチ保存先 UI のアクティブ化」チュートリアル）。

## （ベータ版）データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しがサポートされます。 データセットのエクスポートを設定する方法について詳しくは、 [データセットの書き出しチュートリアル](/help/destinations/ui/export-datasets.md).

## データエクスポートの成功を検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、 [!DNL Azure Data Lake Storage Gen2] ストレージに保存し、書き出したファイルに、期待されたプロファイルの母集団が含まれていることを確認します。
