---
keywords: クラウドストレージの宛先;クラウドストレージ
title: クラウドストレージの宛先の概要
description: Adobe Experience Platform は、セグメントをデータファイルとして Amazon S3、AWS Kinesis、Azure Event Hubs、または SFTP クラウドストレージの場所に配信できます。
exl-id: d29f0a6e-b323-4f78-bbd0-dee2f1e0fedb
source-git-commit: 4a4c82cc4528fe07bbdb75ae9f795bdbab48c089
workflow-type: ht
source-wordcount: '309'
ht-degree: 100%

---

# クラウドストレージの宛先の概要 {#cloud-storage-destinations}

## 概要 {#overview}

Adobe Experience Platform は、セグメントをデータファイルとしてクラウドストレージの場所に配信できます。 これにより、CSV ファイル（[!DNL Amazon S3]、[!DNL Azure Blob]、[!DNL Azure Data Lake Storage Gen2]、[!DNL Data Landing Zone]、[!DNL Google Cloud Storage] および SFTP 用）経由で、オーディエンスとそのプロファイル属性を内部システムに送信できます。[!DNL Amazon Kinesis] と [!DNL Azure Event Hubs] の宛先の場合、データは [!DNL JSON] 形式で Experience Platform からストリーミングされます。

![アドビのクラウドストレージの宛先](../../assets/catalog/cloud-storage/cloud-storage-destinations.png)

## サポートされているクラウドストレージの宛先 {#supported-destinations}

Adobe Experience Platform は、次のクラウドストレージの宛先をサポートしています。

* [Amazon Kinesis 接続](amazon-kinesis.md)
* [Amazon S3 接続](amazon-s3.md)
* [Azure Blob 接続](azure-blob.md)
* [（ベータ版）Azure Data Lake Storage Gen2](adls-gen2.md)
* [Azure Event Hubs 接続](azure-event-hubs.md)
* [（ベータ版）データランディングゾーン](data-landing-zone.md)
* [（ベータ版）Google Cloud Storage](google-cloud-storage.md)
* [SFTP 接続](sftp.md)

## 新しいクラウドストレージの宛先への接続 {#connect-destination}

キャンペーン用に、セグメントをクラウドストレージの宛先に送信するには、まずは Platform が宛先に接続する必要があります。新しい宛先の設定について詳しくは、[宛先の作成に関するチュートリアル](../../ui/connect-destination.md)を参照してください。


## マクロを使用して、ストレージの場所にフォルダーを作成する {#use-macros}

>[!NOTE]
>
> この節で説明する機能は、現在、[Amazon S3](amazon-s3.md) の宛先でのみ使用できます。

ストレージの場所にあるセグメントファイルごとにカスタムフォルダーを作成するには、フォルダーパスの入力フィールドでマクロを使用します。 次に示すように、入力フィールドの末尾にマクロを挿入します。

![マクロを使用してストレージにフォルダーを作成する方法](../../assets/catalog/cloud-storage/workflow/macros-folder-path.png)

以下の例では、ID `25768be6-ebd5-45cc-8913-12fb3f348615` を持つサンプルセグメント `Luxury Audience` を参照しています。

**マクロ 1：`%SEGMENT_NAME%`**

入力： `acme/campaigns/2021/%SEGMENT_NAME%`
ストレージの場所のフォルダーパス： `acme/campaigns/2021/Luxury Audience`

**マクロ 2：`%SEGMENT_ID%`**

入力： `acme/campaigns/2021/%SEGMENT_ID%`
ストレージの場所のフォルダーパス： `acme/campaigns/2021/25768be6-ebd5-45cc-8913-12fb3f348615`

**マクロ 3：`%SEGMENT_NAME%/%SEGMENT_ID%`**

入力： `acme/campaigns/2021/%SEGMENT_NAME%/%SEGMENT_ID%`
ストレージの場所のフォルダーパス： `acme/campaigns/2021/Luxury Audience/25768be6-ebd5-45cc-8913-12fb3f348615`

## データの書き出しのタイプ {#export-type}

クラウドストレージの宛先は、**プロファイルベースの書き出し**&#x200B;をサポートします。これはオーディエンスの個人に関する情報を書き出します。これらの詳細はパーソナライゼーションに必要で、属性、イベント、セグメントのメンバーシップなどを含めることができます。