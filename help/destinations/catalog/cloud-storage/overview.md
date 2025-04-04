---
keywords: クラウドストレージの宛先;クラウドストレージ
title: クラウドストレージの宛先の概要
description: Adobe Experience Platformは、オーディエンスをデータファイルとして、Amazon S3、AWS Kinesis、Azure Event Hubs または SFTP クラウドストレージの場所に配信できます。
exl-id: d29f0a6e-b323-4f78-bbd0-dee2f1e0fedb
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 38%

---

# クラウドストレージの宛先の概要 {#cloud-storage-destinations}

## 概要 {#overview}

Adobe Experience Platformは、オーディエンスをデータファイルとしてクラウドストレージの場所に配信できます。 これにより、CSV ファイル（[!DNL Amazon S3]、[!DNL Azure Blob]、[!DNL Azure Data Lake Storage Gen2]、[!DNL Data Landing Zone]、[!DNL Google Cloud Storage] および SFTP 用）経由で、オーディエンスとそのプロファイル属性を内部システムに送信できます。[!DNL Amazon Kinesis] と [!DNL Azure Event Hubs] の宛先の場合、データは [!DNL JSON] 形式で Experience Platform からストリーミングされます。

![アドビのクラウドストレージの宛先](../../assets/catalog/cloud-storage/cloud-storage-destinations.png)

## サポートされているクラウドストレージの宛先 {#supported-destinations}

Adobe Experience Platformは、次のクラウドストレージの宛先へのデータの書き出しをサポートしています。

* [Amazon Kinesis 接続](amazon-kinesis.md)
* [Amazon S3 接続](amazon-s3.md)
* [Azure Blob 接続](azure-blob.md)
* [Azure Data Lake Storage Gen2](adls-gen2.md)
* [Azure Event Hubs 接続](azure-event-hubs.md)
* [Data Landing Zone](data-landing-zone.md)
* [Google Cloud Storage](google-cloud-storage.md)
* [SFTP 接続](sftp.md)

## 新しいクラウドストレージの宛先への接続 {#connect-destination}

キャンペーン用にオーディエンスをクラウドストレージの宛先に送信するには、まずExperience Platformが宛先に接続する必要があります。 新しい宛先の設定について詳しくは、[宛先の作成に関するチュートリアル](../../ui/connect-destination.md)を参照してください。


## マクロを使用して、ストレージの場所にフォルダーを作成する {#use-macros}

>[!NOTE]
>
> この節で説明する機能は、すべてのクラウドストレージの宛先で使用できます。 ただし、[Amazon S3](amazon-s3.md) の宛先では、現在、`%SEGMENT_ID%` マクロと `%SEGMENT_NAME%` マクロのみがサポートされています。

ストレージの場所にあるオーディエンスファイルごとにカスタムフォルダーを作成するには、フォルダーパスの入力フィールドでマクロを使用します。 次に示すように、入力フィールドの末尾にマクロを挿入します。

![マクロを使用してストレージにフォルダーを作成する方法](../../assets/catalog/cloud-storage/workflow/macros-folder-path.png)

以下の例では、ID `25768be6-ebd5-45cc-8913-12fb3f348615` を持つサンプルオーディエンス `Luxury Audience` を参照しています。

**マクロ 1：`%SEGMENT_NAME%`**

入力： `acme/campaigns/2021/%SEGMENT_NAME%`
ストレージの場所のフォルダーパス： `acme/campaigns/2021/Luxury Audience`

**マクロ 2：`%SEGMENT_ID%`**

入力： `acme/campaigns/2021/%SEGMENT_ID%`
ストレージの場所のフォルダーパス： `acme/campaigns/2021/25768be6-ebd5-45cc-8913-12fb3f348615`

**マクロ 3：`%SEGMENT_NAME%/%SEGMENT_ID%`**

入力： `acme/campaigns/2021/%SEGMENT_NAME%/%SEGMENT_ID%`
ストレージの場所のフォルダーパス： `acme/campaigns/2021/Luxury Audience/25768be6-ebd5-45cc-8913-12fb3f348615`

**その他のマクロ**

上記の例と同様に、さらにマクロを使用して、フォルダーの場所にカスタムフォルダー構造を作成できます。

* ファイルの書き出し時間に基づいてカスタムフォルダー名を追加するには、`%DATETIME%` または `%TIMESTAMP%` を指定します。 最初のマクロの形式は `MMDDYYYY_HHMMSS` で、2 番目のマクロの形式は UNIX の 10 桁の形式です。
* 宛先データフローの名前に基づいてカスタムフォルダーを追加で `%DESTINATION_NAME%` ない。

## データの書き出しのタイプ {#export-type}

クラウドストレージの宛先では、次の書き出しタイプをサポートしています。
* **プロファイルベースの書き出**。 これはオーディエンスの個人に関する情報を書き出します。これらの詳細はパーソナライゼーションに必要で、属性、イベント、オーディエンスメンバーシップなどを含めることができます。
* **データセットの書き出し**。 この機能を使用すると、データセット全体をクラウドストレージの宛先に書き出すことができます。 機能について ](/help/destinations/ui/export-datasets.md) 詳細を参照 [。

## 次の手順 {#next-steps}

使用する [ サポートされているクラウド宛先 ](#supported-destinations) の 1 つを選択した後、[ 宛先への接続チュートリアル ](/help/destinations/ui/connect-destination.md) を読み、宛先への接続を確立する方法を学びます。 次に、ファイルベースの宛先へのアクティベーションチュートリアルを読んで、クラウドストレージの宛先へのデータの [ 書き出し ](/help/destinations/ui/activate-batch-profile-destinations.md) を開始する方法を学びます。
